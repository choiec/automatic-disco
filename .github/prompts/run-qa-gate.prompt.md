---
description: "Run the project's QA gate over a target artifact (code slice, spec, prompt, rewrite, chunk set, question set, answer set, or worksheet) and report findings using the standard review format."
name: "Run QA Gate"
agent: "qa-reviewer"
argument-hint: "검수 대상 경로 또는 산출물 식별자"
---
대상 산출물에 대해 프로젝트 QA gate 를 실행합니다.

## 입력
- 대상: ${input:target:검수할 파일 경로, FR ID, 또는 산출물 식별자}
- 산출물 유형(선택): ${input:artifact_type:code / spec / prompt / rewrite / chunk / question / answer / worksheet 중 하나}

## 작업
1. 대상의 유형을 확정합니다. 모호하면 한 줄로 가정한 뒤 진행합니다.
2. `.github/instructions/review-qa.instructions.md` 에서 해당 유형에 적용되는 체크리스트를 로드합니다.
3. 코드라면 `.github/instructions/python-fastmcp.instructions.md` 의 layering 규칙도 함께 점검합니다.
4. 교육용 생성 산출물이면 다음을 우선 점검합니다.
   - 원문 충실도 (변형 금지 옵션 위반 여부)
   - traceability (source 와 생성 옵션 기록)
   - 해당 유형의 QA 규칙 모두 통과 여부
5. 발견 사항을 Error / Warning / Note 로 분류합니다.

## 출력 형식
- **Status**: pass / pass-with-warnings / fail
- **Artifact**: 유형과 경로
- **Errors**
- **Warnings**
- **Notes**
- **Handoff**: 후속 작업이 필요하면 agent 와 한 줄 brief, 아니면 "no handoff needed"
