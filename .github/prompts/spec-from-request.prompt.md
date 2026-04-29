---
description: "Convert a raw user request into a v0.1-style functional spec draft (FR-### entries) with acceptance criteria for the English teaching automation project."
name: "Spec from Request"
agent: "project-planner"
argument-hint: "원시 요구사항 또는 사용자 요청 텍스트"
---
다음 요청을 v0.1 기능명세 스타일의 초안으로 변환합니다.

## 입력
- 원시 요청: ${input:request:변환할 요구사항을 붙여넣으세요}

## 작업
1. 요청을 한 문장으로 재진술합니다.
2. v0.1 범위 안에 있는지 확인합니다. 범위 밖이면 명확히 표시하고 보류 사유를 적습니다.
3. 필요한 기능을 `FR-###` 항목으로 분해합니다. 각 항목은 다음 절을 포함합니다.
   - 목적
   - 입력
   - 처리
   - 출력
   - 예외 처리
   - 우선순위 (P0 / P1 / P2)
   - 수용 기준
4. 관련 QA 규칙이 필요하면 `QA-RULE-###` 후보를 함께 제시합니다.
5. 관련된 MCP tool 후보 이름을 제안합니다 (snake_case).

## 출력 형식
- **Restated request**
- **Scope check**
- **Proposed FR entries** (각 항목 위 구조 그대로)
- **Proposed QA rules** (있다면)
- **Proposed MCP tools** (있다면)
- **Open questions**

본문은 한국어로 작성하고, 식별자와 enum 이름은 영어로 유지합니다.
