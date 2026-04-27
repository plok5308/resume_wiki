# output

생성된 자기소개서 결과물 보관소.

## 파일 세트
한 번 생성할 때 같은 이름으로 **세 개** 파일을 만든다:
- `{stem}.md` — 정본 (메타 주석 포함, 추후 wiki 역반영의 근거)
- `{stem}.txt` — 평문 (지원 시스템 붙여넣기용, 메타·마크다운 제거)
- `{stem}.pdf` — PDF (제출용 또는 백업용)

## 파일명 규칙
`{YYYY-MM-DD}-{회사}-{직무}` 를 stem으로 사용
예: `2026-04-27-kakao-ml-engineer.md` / `.txt` / `.pdf`

## .md 구조
```
<!--
generated_at: 2026-04-27
job_source: sources/jobs/2026-04-kakao-ml-engineer.md
wiki_used:
  - wiki/strengths.md#문제정의력
  - wiki/experiences.md#카카오톡-알림-개선-프로젝트
  - wiki/writing_style.md (샘플 1, 2)
-->

# {회사} {직무} 자기소개서

## 문항 1. ...
...
```

## .txt 변환 규칙
- 메타 주석(`<!-- -->`) 제거
- 마크다운 헤더(`#`)는 제거하고 줄바꿈 두 번으로 분리
- 굵게/이탤릭(`**`, `*`) 마크업 제거, 텍스트만 남김
- 인코딩: UTF-8

## .pdf 변환
기본 도구: `pandoc` + `weasyprint` (한글 폰트 자동 적용)
```
pandoc {stem}.md -o {stem}.pdf \
  --pdf-engine=weasyprint \
  -V mainfont="Apple SD Gothic Neo"
```
도구가 없으면 사용자에게 설치 여부 확인 후 진행.

## 역반영
사람이 다듬은 결과 중 재사용 가치 있는 표현·일화는 wiki에 반영.
특히 새로 등장하거나 수정된 문구는 `wiki/writing_style.md`의 원문 샘플에 추가.
