# sources/cv

영문 CV 등 정형 이력서 보관소. 자소서(resumes)와 달리 사실의 시점·소속·직함·학사
정보가 가장 깔끔하게 정돈된 1차 출처다.

## 규칙
- 절대 수정하지 않는다 (원본 보존)
- 파일명: `{형식}__{이름}__{YYYY_월}.{ext}` 권장 (예: `CV__Jinseok_Park__2026_April_.pdf`)
- 새 버전이 생기면 파일을 별도로 추가하고 기존 파일은 그대로 둔다 (덮어쓰기 금지)

## ingest 가이드
- **profile.md** — 학력(학사 포함), 경력 시점·소속·직함을 CV 기준으로 정확화
- **experiences.md** — 각 경험 카드의 기간(연/월)을 CV 기준으로 보강.
  CV에만 있던 경력(예: Qualcomm 계약직)이 있으면 새 카드로 추가
- **skills.md** — 수상의 정확한 명칭/연도(예: "Naver Ph.D. Fellowship 2018", "D'LIVE Scholarship 2018"),
  발표(예: Naver Techtalk), Reviewer 경력 등 anchor 보강
- **영문 표현 보존** — 자소서 영문 버전이나 LinkedIn 등에 그대로 쓰일 수 있는
  영문 직함(예: "Team Lead, Gleo Multimodal Team at 42dot")은 wiki 항목에 영문 원문을 같이 남긴다

## 메타 예시 (사이드카 .md, 선택)
```
<!--
date: 2026-04
language: en
purpose: 영문 직함·시점·학사 정보의 1차 출처
-->
```
