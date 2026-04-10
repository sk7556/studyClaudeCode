# CLAUDE.md 작성법

`CLAUDE.md`는 프로젝트 루트에 두는 파일로, Claude Code가 세션 시작 시 자동으로 읽어 컨텍스트로 활용합니다.

---

## 파일 위치

```
~/CLAUDE.md               # 전역 (모든 프로젝트 공통)
프로젝트루트/CLAUDE.md    # 프로젝트별
하위폴더/CLAUDE.md        # 서브 디렉토리용 (해당 폴더 작업 시 로드)
```

---

## 핵심 원칙

**목적**: Claude에게 이 프로젝트의 맥락, 규칙, 주의사항을 알려주는 파일

- 짧고 명확하게 작성 (길수록 중요도 희석)
- Claude가 모르면 실수할 만한 것 위주로 작성
- 코드 스타일, 아키텍처 결정, 금지 사항 등을 포함

---

## 기본 구조

```markdown
# 프로젝트명

## 개요
[한 두 문장으로 이 프로젝트가 무엇인지]

## 기술 스택
- Language: TypeScript
- Framework: Next.js 14
- DB: PostgreSQL + Prisma

## 자주 사용하는 명령어
\`\`\`bash
npm run dev      # 개발 서버
npm run test     # 테스트
npm run build    # 빌드
\`\`\`

## 코드 컨벤션
- [규칙 1]
- [규칙 2]

## 중요 주의사항
- [절대 하면 안 되는 것]
- [실수하기 쉬운 것]

## 아키텍처
[폴더 구조나 레이어 설명]
```

---

## 실전 예시

```markdown
# E-Commerce API

## 개요
Node.js + Express 기반 쇼핑몰 백엔드 API.
결제는 Stripe, 인증은 JWT 사용.

## 자주 사용하는 명령어
\`\`\`bash
npm run dev          # ts-node-dev로 개발 서버
npm run test         # jest
npm run migrate      # prisma migrate dev
npm run seed         # 테스트 데이터 시딩
\`\`\`

## 코드 규칙
- 모든 에러는 AppError 클래스로 throw
- 컨트롤러는 비즈니스 로직 없이 서비스 위임만
- DB 쿼리는 반드시 서비스 레이어에서만
- any 타입 사용 금지

## 주의사항
- `src/config/secrets.ts`는 절대 수정 금지 (운영 설정)
- 마이그레이션 파일 직접 수정 금지, migrate dev 사용
- `main` 브랜치에 직접 push 금지

## 폴더 구조
src/
├── controllers/   # 요청/응답 처리
├── services/      # 비즈니스 로직
├── repositories/  # DB 접근
├── middlewares/   
└── types/         # 공통 타입
```

---

## 작성 팁

**포함하면 좋은 것**
- 빌드/테스트/배포 명령어
- 코드 스타일 가이드
- 아키텍처 결정 이유 (왜 이렇게 구조화했는지)
- 건드리면 안 되는 파일/설정
- 환경변수 설명 (값 말고 어디서 구하는지)
- 자주 발생하는 함정/주의사항

**포함하지 않아도 되는 것**
- 코드에서 이미 명확한 것
- 업데이트가 자주 필요한 상세 내용
- 일반적인 Git/코딩 규칙 (Claude가 이미 앎)

---

## 자동 로드 범위

Claude Code는 다음 순서로 CLAUDE.md를 자동 로드합니다:

1. `~/CLAUDE.md` (전역)
2. 현재 프로젝트 루트의 `CLAUDE.md`
3. 작업 중인 파일의 상위 폴더들의 `CLAUDE.md`

모든 내용이 컨텍스트에 포함되므로 너무 길게 쓰면 비효율적입니다.
