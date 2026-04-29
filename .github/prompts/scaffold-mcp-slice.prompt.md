---
description: "Scaffold a single thin MCP slice (one tool, service, repository, QA rule, or exporter) for the FastMCP English teaching automation project. Implements one layer at a time against an approved spec."
name: "Scaffold MCP Slice"
agent: "fastmcp-builder"
argument-hint: "대상 layer 와 기능명 (예: tool register_source_document, service parsing_service.split_passages)"
---
승인된 명세를 바탕으로 단일 thin slice 를 스캐폴딩합니다.

## 입력
- 대상: ${input:target:layer 와 기능명을 적으세요. 예) tool register_source_document}
- 참고 명세 또는 FR ID: ${input:spec:관련 FR-### 또는 명세 위치}

## 사전 조건
- 대상 layer 는 다음 중 하나여야 합니다: `tool`, `service`, `repository`, `qa-rule`, `exporter`, `domain`.
- 명세 또는 수용 기준이 없으면 작업을 멈추고 `project-planner` 로 위임을 권고합니다.

## 작업
1. 대상 파일 경로를 결정합니다.
   - tool: `app/mcp/tools/<area>_tools.py` 와 `app/mcp/registry.py`
   - service: `app/services/<name>_service.py`
   - repository: `app/repositories/<name>_repository.py`
   - qa-rule: `app/qa/rules/<area>_rules.py` 와 `app/qa/qa_service.py`
   - exporter: `app/exporters/<name>_exporter.py`
   - domain: `app/domain/{enums,models,schemas}/<name>.py`
2. `.github/instructions/python-fastmcp.instructions.md` 의 layering 규칙을 준수합니다.
3. 한 slice 안에서 다른 layer 를 동시에 변경하지 않습니다. 의존하는 layer 가 비어 있으면 placeholder 가 아니라 별도 slice 로 분리하라고 보고합니다.
4. 최소 단위 테스트를 `tests/` 아래에 추가합니다.
5. 가능한 경우 lint 또는 test 를 실행합니다.

## 출력 형식
- **Slice implemented**
- **Files changed** (워크스페이스 상대 링크)
- **Public API**
- **Tests**
- **Verification run**
- **Next handoff** (보통 `qa-reviewer`)
