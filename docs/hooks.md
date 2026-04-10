# Claude Code Hooks 가이드

Hooks는 Claude Code의 특정 이벤트 발생 시 자동으로 쉘 명령어를 실행하는 기능입니다.

---

## 훅 종류

| 훅 이름 | 트리거 시점 |
|---------|------------|
| `PreToolUse` | 도구 실행 **전** |
| `PostToolUse` | 도구 실행 **후** |
| `Notification` | Claude가 알림을 보낼 때 |
| `SessionStart` | 세션 시작 시 |
| `Stop` | Claude 응답 완료 후 |

---

## 설정 방법

`settings.json`의 `hooks` 섹션에 추가:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "npm run lint --fix"
          }
        ]
      }
    ]
  }
}
```

### matcher 패턴

- 특정 도구: `"Edit"`, `"Bash"`, `"Write"`
- OR 조건: `"Edit|Write"`
- 모든 도구: `""` (빈 문자열)

---

## 실용 예시

### 1. 파일 수정 후 자동 포맷

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "prettier --write ."
          }
        ]
      }
    ]
  }
}
```

### 2. Bash 실행 전 로깅

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "echo \"[$(date)] Running bash command\" >> ~/claude-audit.log"
          }
        ]
      }
    ]
  }
}
```

### 3. 세션 시작 시 환경 확인

```json
{
  "hooks": {
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "node --version && npm --version"
          }
        ]
      }
    ]
  }
}
```

### 4. 응답 완료 후 알림 (macOS)

```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "osascript -e 'display notification \"Claude 작업 완료\" with title \"Claude Code\"'"
          }
        ]
      }
    ]
  }
}
```

### 5. 위험한 명령 차단 (PreToolUse로 감사)

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "echo \"$CLAUDE_TOOL_INPUT\" | grep -q 'rm -rf' && echo 'BLOCKED' && exit 2 || exit 0"
          }
        ]
      }
    ]
  }
}
```

> exit code 2를 반환하면 도구 실행이 차단됩니다.

---

## 훅 환경변수

훅 실행 시 다음 환경변수가 제공됩니다:

| 변수 | 내용 |
|------|------|
| `CLAUDE_TOOL_NAME` | 실행된 도구 이름 |
| `CLAUDE_TOOL_INPUT` | 도구 입력값 (JSON) |
| `CLAUDE_TOOL_OUTPUT` | 도구 출력값 (PostToolUse만) |
| `CLAUDE_SESSION_ID` | 현재 세션 ID |

---

## 주의사항

- 훅 명령어는 동기적으로 실행됨 (블로킹)
- 긴 작업은 백그라운드로 실행: `command &`
- exit code 0 = 성공, 1 = 경고(진행), 2 = 차단
