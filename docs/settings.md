# Claude Code settings.json 가이드

## 파일 위치

| 범위 | 경로 | 설명 |
|------|------|------|
| 전역 | `~/.claude/settings.json` | 모든 프로젝트에 적용 |
| 프로젝트 | `.claude/settings.json` | 해당 프로젝트에만 적용 |
| 로컬(비공유) | `.claude/settings.local.json` | git 무시, 개인 설정용 |

> 우선순위: 프로젝트 로컬 > 프로젝트 > 전역

---

## 주요 설정 옵션

### 권한 (permissions)

```json
{
  "permissions": {
    "allow": [
      "Bash(git *)",
      "Bash(npm run *)",
      "Read(**)",
      "Edit(**)"
    ],
    "deny": [
      "Bash(rm -rf *)"
    ]
  }
}
```

- `allow` / `deny` 배열로 도구 사용 허용/차단
- 패턴 문법: `도구명(인자패턴)` 형태
- 와일드카드 `*` 사용 가능

### 모델 설정

```json
{
  "model": "claude-opus-4-5",
  "smallModel": "claude-haiku-4-5-20251001"
}
```

- `model`: 기본 모델 지정
- `smallModel`: 보조 작업(요약 등)에 쓰는 경량 모델

### 환경 변수

```json
{
  "env": {
    "NODE_ENV": "development",
    "MY_API_KEY": "..."
  }
}
```

세션 시작 시 자동으로 설정되는 환경 변수.

### API 키 / 엔드포인트

```json
{
  "apiKeyHelper": "cat ~/.secrets/anthropic_key",
  "anthropicBaseUrl": "https://api.anthropic.com"
}
```

- `apiKeyHelper`: 명령어 실행 결과를 API 키로 사용 (비밀 관리에 유용)

---

## 전체 예시

```json
{
  "model": "claude-sonnet-4-6",
  "permissions": {
    "allow": [
      "Bash(git *)",
      "Bash(npm *)",
      "Bash(pnpm *)",
      "Read(**)",
      "Edit(**)",
      "Write(**)"
    ],
    "deny": [
      "Bash(rm -rf *)",
      "Bash(sudo *)"
    ]
  },
  "env": {
    "NODE_ENV": "development"
  }
}
```

---

## 팁

- `.claude/settings.local.json`은 `.gitignore`에 추가해서 개인 API 키나 로컬 경로가 커밋되지 않도록 할 것
- `allow`에 없는 도구는 실행 시마다 사용자 승인 요청
- 팀 공유 설정은 `.claude/settings.json`, 개인 오버라이드는 `settings.local.json`으로 분리하는 게 좋음
