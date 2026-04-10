# MCP 서버 가이드

MCP(Model Context Protocol)는 Claude에 외부 도구와 데이터 소스를 연결하는 프로토콜입니다.

---

## 설정 위치

```
~/.claude.json          # 전역 MCP 서버 설정
.claude/settings.json   # 프로젝트별 MCP 서버 설정
```

---

## 설정 방법

### stdio 방식 (로컬 프로세스)

```json
{
  "mcpServers": {
    "my-server": {
      "type": "stdio",
      "command": "node",
      "args": ["/path/to/mcp-server/index.js"],
      "env": {
        "API_KEY": "..."
      }
    }
  }
}
```

### SSE 방식 (원격 서버)

```json
{
  "mcpServers": {
    "remote-server": {
      "type": "sse",
      "url": "https://my-mcp-server.example.com/sse",
      "headers": {
        "Authorization": "Bearer TOKEN"
      }
    }
  }
}
```

---

## 자주 쓰는 MCP 서버

### GitHub MCP

```json
{
  "mcpServers": {
    "github": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_..."
      }
    }
  }
}
```

제공 도구: 이슈 조회/생성, PR 관리, 파일 읽기/쓰기, 코드 검색

### Filesystem MCP

```json
{
  "mcpServers": {
    "filesystem": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/allowed/path1",
        "/allowed/path2"
      ]
    }
  }
}
```

### Slack MCP

```json
{
  "mcpServers": {
    "slack": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-slack"],
      "env": {
        "SLACK_BOT_TOKEN": "xoxb-...",
        "SLACK_TEAM_ID": "T..."
      }
    }
  }
}
```

### PostgreSQL MCP

```json
{
  "mcpServers": {
    "postgres": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-postgres",
        "postgresql://user:password@localhost/dbname"
      ]
    }
  }
}
```

---

## CLI로 MCP 서버 관리

```bash
# 서버 추가
claude mcp add my-server -e API_KEY=xxx -- node /path/to/server.js

# 서버 목록
claude mcp list

# 서버 제거
claude mcp remove my-server

# 특정 스코프로 추가 (local/project/user)
claude mcp add my-server --scope project -- npx server-package
```

---

## 팁

- MCP 서버는 세션 시작 시 자동 연결됨
- 연결 실패해도 Claude Code 자체는 정상 동작
- `/mcp` 커맨드로 현재 연결된 서버와 도구 목록 확인 가능
- 민감한 토큰은 `env` 섹션보다 `apiKeyHelper`나 시스템 환경변수 사용 권장
