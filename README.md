# Claude Code 스터디

Claude Code 사용법, 세팅, 팁을 기록하고 공유하는 레포지토리입니다.

## 목차

| 문서 | 설명 |
|------|------|
| [기본 설정 (settings.json)](docs/settings.md) | Claude Code 설정 파일 구조와 옵션 |
| [훅 (Hooks)](docs/hooks.md) | 이벤트 기반 자동화 훅 설정 |
| [MCP 서버](docs/mcp-servers.md) | Model Context Protocol 서버 연동 |
| [슬래시 커맨드](docs/slash-commands.md) | 커스텀 슬래시 커맨드 만들기 |
| [CLAUDE.md 작성법](docs/claude-md.md) | 프로젝트별 AI 지시사항 파일 |
| [실전 팁 & 패턴](docs/tips-and-patterns.md) | 실제 사용하면서 찾은 유용한 패턴들 |

## 빠른 시작

```bash
# Claude Code 설치
npm install -g @anthropic-ai/claude-code

# 실행
claude
```

## 폴더 구조

```
studyClaudeCode/
├── README.md               # 이 파일
├── .claude/
│   └── settings.json       # 예시 설정 파일
├── docs/
│   ├── settings.md         # settings.json 가이드
│   ├── hooks.md            # Hooks 가이드
│   ├── mcp-servers.md      # MCP 서버 가이드
│   ├── slash-commands.md   # 슬래시 커맨드 가이드
│   ├── claude-md.md        # CLAUDE.md 작성법
│   └── tips-and-patterns.md # 팁 & 패턴
└── examples/               # 예시 설정 파일들
```

## 참고 링크

- [Claude Code 공식 문서](https://docs.anthropic.com/en/docs/claude-code)
- [피드백/이슈](https://github.com/anthropics/claude-code/issues)
