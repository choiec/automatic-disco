# Project Guidelines

이 문서는 본 워크스페이스의 모든 에이전트 작업에 항상 적용되는 공통 규칙입니다.
프로젝트 주제는 **로컬 MCP 서버 기반 영어 수업 준비 자동화 플랫폼** 이며, 이 하네스는 그 개발 작업을 보조합니다.

## Mission

- 영어 원문 1개를 중심으로 분석, 시험형 chunking, 수준별 재작성, 문제/정답/해설, 워크시트, QA, 검색까지 잇는 **로컬 자동화 파이프라인**을 단계적으로 구현합니다.
- 클라우드 저장소(OneDrive/SharePoint)에 의존하지 않고 **로컬 파일 시스템 + SQLite**로 자료와 메타데이터를 관리합니다.

## Tech Baseline

- 언어: Python (3.11 이상 권장).
- MCP 프레임워크: **FastMCP**. 의존성은 minor 범위로 pinning 합니다.
- 저장소: 로컬 파일 시스템 + SQLite. 외부 클라우드 의존 금지.
- Transport: 개발 기본은 `stdio`, 디버깅용 보조는 `streamable-http`.
- 프로토콜 참고: 공식 MCP Python SDK 문서를 보조 레퍼런스로 사용합니다.

## Architecture Principles

- **Layered architecture** 를 유지합니다.
  - `app/mcp/tools/*` 는 입력 검증, 서비스 호출, 응답 envelope 반환만 담당하는 얇은 진입점.
  - `app/services/*` 는 업무 로직. FastMCP 를 직접 import 하지 않습니다.
  - `app/repositories/*` 는 SQLite + 파일 저장만 담당. SQL 문자열은 repository 내부에만 둡니다.
  - `app/domain/*` 은 enum, model, schema 만 둡니다.
  - `app/qa/*` 는 검수 규칙을 service 와 분리해서 둡니다.
  - `app/exporters/*` 는 Markdown/JSON 등 산출물 렌더링 전담.
- Tool 등록은 import side effect 가 아니라 `app/mcp/registry.py` 에서 명시적으로 호출합니다.
- 모든 산출물은 원문과의 **lineage(출처/생성옵션)** 를 유지해야 합니다.

## Workflow Principles

- **Spec-first**: 새 기능은 기능명세 또는 수용기준이 먼저 합의된 뒤 구현합니다.
- **Thin slice 우선**: 한 번에 한 개의 도메인 단위(tool 1개 또는 service 1개)만 추가합니다.
- **QA gate 필수**: 재작성, 문항, 해설 같은 생성 결과물은 QA 레이어를 통과해야 산출물로 간주합니다.
- **Traceability 유지**: 어떤 원문에서 어떤 옵션으로 생성되었는지 항상 추적 가능해야 합니다.
- **Local-first**: 외부 네트워크 호출은 기본적으로 도입하지 않습니다.

## Implementation Discipline

- 요청되지 않은 리팩터링, docstring 보강, 타입 보강, 광범위한 정리는 하지 않습니다.
- Tool handler 안에 비즈니스 로직, SQL, 파일 I/O 를 직접 넣지 않습니다.
- 새 의존성은 꼭 필요할 때만 추가하고, 추가 시 `pyproject.toml` 에 버전 범위를 명시합니다.
- 시스템 경계가 아닌 곳에서 과도한 입력 검증을 추가하지 않습니다.

## Communication

- 사용자 응답은 한국어 실무 문체를 기본으로 합니다.
- 파일/심볼 인용은 워크스페이스 상대 경로의 마크다운 링크를 사용합니다.
- 모호한 요구는 추정해 진행하지 말고, 짧게 확인 질문을 먼저 합니다.

## Out of Scope (이 단계 한정)

- OneDrive/SharePoint, 외부 LMS 연동.
- 모바일 앱, 멀티테넌트, 권한 시스템.
- OCR/OMR 자동 채점, 대시보드/BI.
- 이 하네스 자체로 FastMCP 런타임을 구현하지 않습니다. 구현 작업은 별도 요청 시 시작합니다.
