---
description: "Use when writing or modifying Python code for the FastMCP-based English teaching automation server. Covers layered architecture, tool registration, services, repositories, QA layer, and local-first storage rules."
applyTo: "app/**, tests/**, scripts/**, pyproject.toml"
---
# Python / FastMCP Implementation Rules

이 지침은 FastMCP 기반 로컬 MCP 서버 코드 작업에 적용됩니다.

## Layering

- `app/mcp/tools/*` 는 다음 3가지만 합니다.
  1. 입력 검증
  2. `container` 의 service 호출
  3. `app/mcp/common/envelopes.py` 의 `ok_response` / `error_response` 반환
- `app/services/*` 에 업무 로직을 둡니다. 여기서는 FastMCP 를 import 하지 않습니다.
- `app/repositories/*` 는 저장과 조회만 합니다. SQL 문자열은 repository 외부로 노출하지 않습니다.
- `app/domain/*` 은 enum, model, schema 만 둡니다. I/O 로직 금지.
- QA 는 `app/qa/*` 로 분리하고, 생성 service 와 호출만 연결합니다.
- 산출물 렌더링은 `app/exporters/*` 로 분리합니다.

## Tool Registration

- 새 tool 은 `app/mcp/tools/<area>_tools.py` 에 추가하고, 같은 파일의 `register_*_tools(mcp, container)` 함수 안에서 `@mcp.tool` 로 등록합니다.
- 파일 import 시 자동 등록되는 side effect 패턴을 만들지 않습니다.
- 등록은 `app/mcp/registry.py` 의 `register_all_tools` 에서 명시적으로 호출합니다.

## Server Entry

- `FastMCP(...)` 인스턴스는 `app/mcp/server.py` 의 `create_mcp_server(settings)` 에서 1회 생성합니다.
- transport 는 `settings.mcp_transport` 로 결정하며, 개발 기본은 `stdio`, 디버깅용 보조는 `streamable-http` 입니다.
- `app/main.py` 는 `bootstrap → server → registry → run` 순으로 실행합니다.

## Dependency Injection

- 전역 싱글턴을 직접 참조하지 말고, `app/bootstrap.py` 에서 만든 `AppContainer` 를 통해 service 와 repository 를 주입받습니다.
- `Context` 와 `lifespan` 은 progress/logging, DB 연결 초기화 같은 한정된 용도에만 사용합니다.

## Storage

- 원문 파일과 파생 산출물은 `data/` 하위에 저장합니다.
- 검색, 관계, lineage 정보는 SQLite 에 저장합니다.
- 외부 클라우드 호출 또는 사용자 홈 외부 임의 경로 접근을 추가하지 않습니다.

## QA & Traceability

- 재작성, 문항, 해설, chunk 등 생성 결과물은 반드시 대응되는 QA 규칙을 통과한 뒤 산출물로 저장합니다.
- 모든 파생 산출물은 `lineage_repository` 를 통해 source 와 생성 옵션을 기록합니다.

## Style

- Python 3.11+ 문법을 사용합니다 (`X | None` 등).
- 타입 힌트는 tool 시그니처와 service public API 에 우선 적용합니다.
- 요청되지 않은 광범위한 리팩터링, docstring 보강, 주석 추가는 하지 않습니다.

## Anti-patterns

- Tool handler 안에서 직접 SQL 실행 또는 파일 쓰기.
- service 가 FastMCP 또는 MCP 응답 envelope 를 직접 반환.
- repository 가 비즈니스 분기를 수행.
- 한 PR 에서 여러 layer 를 동시에 크게 변경.
