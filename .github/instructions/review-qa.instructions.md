---
description: "Use when reviewing code, specs, prompts, generated worksheets, questions, answers, or rewrites. Defines QA gate priorities, traceability checks, and educational content quality criteria for the English teaching automation project."
---
# Review & QA Rules

이 지침은 코드, 명세, 프롬프트, 생성 산출물(재작성/문항/해설/워크시트/chunk) 검수에 적용됩니다.

## Review Priorities (in order)

1. **Correctness** — 명세된 입력/출력과 일치하는가.
2. **Source fidelity** — 원문 변형 금지 옵션이 켜진 경우 원문이 보존되는가.
3. **Traceability** — 산출물이 source 와 생성 옵션을 추적할 수 있는가.
4. **QA rule coverage** — 해당 산출물 유형에 정의된 QA 규칙을 모두 통과했는가.
5. **Layer hygiene** — tool/service/repository/qa/exporter 경계가 지켜졌는가.
6. **Readability** — 동료 교사가 검수 가능한 형태인가.

## Required QA Checks by Artifact

- **Rewrite**: 원문 핵심 정보 누락 여부, 논리 순서 유지, 목표 수준 이탈, 변경 diff 추적.
- **Chunk**: 문법 판단 단위 적절성, chunk 번호 보존, 과도한 의미 단위 분리 경고.
- **Question**: 정답 중복 가능성, 오답의 명백성, 출처 sentence/passage 연결.
- **Answer / Explanation**: 정답표와 해설 일치, 정답 확신도 표시.
- **Worksheet**: 학생용/교사용 분리, 정답지 분리, 힌트 옵션 반영.

## Code Review Checklist

- Tool handler 가 비즈니스 로직, SQL, 파일 I/O 를 포함하지 않는다.
- Service 가 FastMCP 를 import 하지 않는다.
- Repository 가 비즈니스 분기를 수행하지 않는다.
- 새 의존성이 정당화되어 있고 `pyproject.toml` 에 버전 범위가 명시되어 있다.
- 외부 네트워크 호출이 새로 도입되지 않았다.

## Reporting Format

리뷰 결과는 다음 구조로 보고합니다.

- **Status**: pass / pass-with-warnings / fail.
- **Errors**: 차단성 문제. 각 항목은 위치, 위반 규칙 ID(가능한 경우), 권장 조치.
- **Warnings**: 차단성은 아니나 추적이 필요한 항목.
- **Notes**: 후속 단계 제안.

## Anti-patterns

- "전반적으로 좋아 보입니다" 같은 막연한 승인.
- 명세에 없는 개인 취향 기반 코멘트.
- 차단성 오류와 사소한 스타일을 같은 우선순위로 보고.
- 산출물을 바로 수정하면서 리뷰만 끝낸 척 하기 — 수정은 별도 작업으로 명확히 분리합니다.
