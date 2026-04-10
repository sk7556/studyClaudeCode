# 슬래시 커맨드 가이드

슬래시 커맨드는 자주 사용하는 프롬프트를 `/커맨드명`으로 빠르게 호출하는 기능입니다.

---

## 파일 위치

```
.claude/commands/커맨드명.md      # 프로젝트 공유용
~/.claude/commands/커맨드명.md   # 개인 전역용
```

파일명이 곧 커맨드 이름입니다. `review.md` → `/review`

---

## 기본 예시

### `.claude/commands/review.md`

```markdown
코드 리뷰를 진행해주세요.

다음 항목을 확인해주세요:
1. 버그 가능성
2. 성능 문제
3. 보안 취약점
4. 코드 가독성

$ARGUMENTS
```

사용: `/review src/utils.ts`

### `.claude/commands/commit.md`

```markdown
변경된 파일들을 확인하고, 적절한 커밋 메시지를 작성한 후 커밋해주세요.

- Conventional Commits 형식 사용 (feat/fix/refactor/docs/chore 등)
- 한국어로 작성
- 변경 이유를 본문에 포함
```

사용: `/commit`

### `.claude/commands/test.md`

```markdown
$ARGUMENTS 파일에 대한 단위 테스트를 작성해주세요.

요구사항:
- 엣지 케이스 포함
- 기존 테스트 스타일 유지
- 테스트 커버리지 80% 이상 목표
```

사용: `/test src/auth/login.ts`

---

## $ARGUMENTS 활용

`$ARGUMENTS`는 커맨드 뒤에 입력한 텍스트로 치환됩니다.

```
/review src/api.ts --focus security
```

→ `$ARGUMENTS` = `src/api.ts --focus security`

---

## 하위 폴더로 구조화

```
.claude/commands/
├── git/
│   ├── commit.md     → /git:commit
│   └── pr.md         → /git:pr
├── code/
│   ├── review.md     → /code:review
│   └── refactor.md   → /code:refactor
└── docs/
    └── update.md     → /docs:update
```

---

## 유용한 커맨드 아이디어

| 커맨드 | 용도 |
|--------|------|
| `/review` | 코드 리뷰 |
| `/commit` | 스마트 커밋 |
| `/test` | 테스트 작성 |
| `/pr` | PR 설명 작성 |
| `/debug` | 버그 디버깅 |
| `/refactor` | 리팩토링 |
| `/explain` | 코드 설명 |
| `/perf` | 성능 분석 |

---

## 팁

- 팀 공통 커맨드: `.claude/commands/`에 커밋
- 개인 커맨드: `~/.claude/commands/`에 저장
- 마크다운 형식이므로 코드블록, 목록 등 자유롭게 사용 가능
