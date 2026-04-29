---
description: "Use when authoring or updating specs, planning documents, system design notes, WBS, MCP tool I/O specs, QA rules, or any markdown docs for the English teaching automation project."
applyTo: "docs/**, **/*.md"
---
# Spec & Documentation Rules

이 지침은 기능명세, 시스템 설계, MCP tool 명세, WBS, QA 규칙 같은 문서를 작성할 때 적용됩니다. 루트의 `AGENTS.md` 는 예외이며 별도 always-on 지침으로 관리합니다.

## Document Identity

- 모든 문서 상단에는 **문서명, 버전, 목적, 대상 독자, 전제 조건** 을 명시합니다.
- 버전은 `v0.1` 같은 시맨틱 표기를 사용하고, 큰 변경이 있을 때만 올립니다.

## Structure

- 기능명세는 `FR-###` 형식의 ID 로 관리하고 다음 절을 포함합니다: 목적, 입력, 처리, 출력, 예외 처리, 우선순위(P0/P1/P2), 수용 기준.
- QA 규칙은 `QA-RULE-###` ID 로 관리합니다.
- 수용 테스트는 `AT-###` ID 로 관리합니다.
- MCP tool 명세는 tool name, input schema, output schema, 예외 코드, 호출 순서를 포함합니다.

## Scope Discipline

- 한 문서는 한 가지 관심사만 다룹니다. 기획서, 기능명세, 시스템 설계, WBS 를 한 파일에 섞지 않습니다.
- v0.1 범위에서 명시적으로 제외된 항목(OneDrive/SharePoint 연동, OCR/OMR 자동 채점, 대시보드 등)은 새 문서에서 다시 끌어오지 않습니다.

## Writing Style

- 본문은 한국어 실무 문체를 기본으로 합니다.
- 영어 고유명사(FastMCP, passage, chunk, CEFR 등)는 번역하지 않고 그대로 씁니다.
- 마케팅 어조, 장식적 형용사, 과장 표현을 피합니다.
- 표, 목록, 코드 블록을 활용해 검색성과 가독성을 확보합니다.

## Linking & Traceability

- 다른 문서나 코드를 인용할 때는 워크스페이스 상대 경로의 마크다운 링크를 사용합니다.
- 기능, QA 규칙, tool 사이의 관계는 ID 로 상호 참조해 traceability 를 유지합니다.

## Anti-patterns

- 같은 내용을 여러 문서에 복사: 한 곳에 두고 링크합니다.
- 범위 밖 기능을 "참고용" 으로 다시 끌어오기.
- 의사결정 없는 장황한 배경 서술.
- 이모지 또는 장식 기호의 과도한 사용.
