# 실전 팁 & 패턴

실제 Claude Code를 사용하면서 발견한 유용한 패턴과 팁을 기록합니다.

---

## 효율적인 사용 패턴

### 큰 작업은 계획 먼저

```
먼저 어떻게 구현할지 계획만 세워줘. 코드는 짜지 말고.
```

→ 계획 확인 후 승인하면 구현 진행. 방향이 틀렸을 때 낭비 방지.

### 파일 범위 명시

```
src/auth/ 폴더만 보고 JWT 갱신 로직 설명해줘
```

→ 불필요한 파일 탐색 줄이고 응답 속도 향상.

### 단계적 작업 분리

```
1단계: 타입 정의만
2단계: 함수 구현
3단계: 테스트 작성
```

→ 각 단계별로 검토 가능, 오류 범위 축소.

---

## Bash 권한 설정 패턴

### 프로젝트별 허용 명령어 화이트리스트

```json
{
  "permissions": {
    "allow": [
      "Bash(git log *)",
      "Bash(git diff *)",
      "Bash(git status)",
      "Bash(git add *)",
      "Bash(git commit *)",
      "Bash(npm run *)",
      "Bash(npx tsc *)",
      "Bash(npx jest *)"
    ]
  }
}
```

패턴이 구체적일수록 승인 팝업 없이 자동 실행됨.

### 읽기 전용 모드

```json
{
  "permissions": {
    "allow": ["Read(**)", "Bash(git log *)", "Bash(git diff *)"],
    "deny": ["Edit(**)", "Write(**)", "Bash(*)"]
  }
}
```

코드 분석/설명만 할 때 사용. 실수로 수정되는 것 방지.

---

## 반복 작업 자동화

### 훅으로 저장 시 자동 린트

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "cd $CLAUDE_PROJECT_ROOT && npx eslint --fix $(echo $CLAUDE_TOOL_INPUT | jq -r '.file_path // empty') 2>/dev/null || true"
          }
        ]
      }
    ]
  }
}
```

### 작업 완료 후 테스트 실행

```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "cd $CLAUDE_PROJECT_ROOT && npm test -- --passWithNoTests 2>&1 | tail -5"
          }
        ]
      }
    ]
  }
}
```

---

## 컨텍스트 관리

### /clear 활용

긴 대화 후 새 주제 시작 시 `/clear`로 컨텍스트 초기화.
→ 이전 작업 내용이 섞여 오답이 나오는 것 방지.

### 중요 파일 먼저 읽기

```
시작 전에 package.json, tsconfig.json 읽어봐
```

→ 프로젝트 구조 파악 후 더 정확한 응답.

---

## 디버깅 패턴

### 에러 메시지 전체 붙여넣기

```
이 에러 고쳐줘:

Error: Cannot read properties of undefined (reading 'map')
    at UserList (src/components/UserList.tsx:23:18)
    ...스택트레이스 전체...
```

→ 요약하지 말고 전체 스택트레이스 제공이 더 정확.

### 재현 코드 요청

```
버그 재현하는 최소 코드 예시 만들어줘
```

→ 문제를 격리하면 원인 파악이 쉬워짐.

---

## 모델 선택 가이드

| 작업 | 추천 모델 |
|------|-----------|
| 복잡한 아키텍처 설계 | claude-opus-4-6 |
| 일반 코딩/리팩토링 | claude-sonnet-4-6 |
| 빠른 질문/간단한 수정 | claude-haiku-4-5 |

---

## 주의사항 & 안티패턴

**피해야 할 것**
- 파일 수정 없이 "고쳐줘"만 반복 → 구체적 파일/함수 지정
- 한 번에 너무 많은 작업 요청 → 단계별 분리
- 승인 없이 배포/push 허용 → permissions deny 설정

**좋은 습관**
- 중요한 변경 전 git commit
- 의도한 변경인지 diff 확인
- CLAUDE.md에 "하지 말아야 할 것" 명시
