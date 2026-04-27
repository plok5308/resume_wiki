# sources/jobs

generate 단계에서 입력으로 쓰는 채용 공고 보관소.

> **이 폴더는 wiki에 ingest되지 않는다.**
> 공고는 "나"의 영속적 지식이 아니라 한 번 자소서를 만들 때 쓰고 마는 일회성 입력이다.
> wiki는 오직 `sources/resumes/`로부터만 갱신된다.

## 규칙
- 절대 수정하지 않는다 (원본 보존)
- 파일명: `{YYYY-MM}-{회사}-{직무}.md` (예: `2026-04-kakao-ml-engineer.md`)
- 공고 본문 외에 회사 소개, 자소서 문항, 인재상 등 가능한 한 풍부하게 담는다
  (generate 품질이 여기서 결정됨)

## 메타 예시
```
<!--
date: 2026-04
company: Kakao
role: ML Engineer
url: https://...
deadline: 2026-05-15
-->
```
