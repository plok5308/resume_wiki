# web

면접에서 띄워 두고 자기소개에 사용하는 **회사·직무별 단일 페이지 웹사이트** 보관소.
참고 예시: https://promm.dev/about/

`output/`이 자소서 정본이라면 `web/`은 **같은 지원 건의 시각 자기소개**다.

## 폴더·파일 구성
한 지원 건당 하나의 폴더:
```
web/{YYYY-MM-DD}-{회사}-{직무}/
  index.html       # 본문 (단일 페이지)
  style.css        # 별도 스타일 (선택, 단일 파일이면 <style> 인라인 OK)
  assets/          # 이미지·아이콘 등 정적 자산 (선택)
```
빌드 단계는 두지 않는다. 폴더만 열면 페이지가 떠야 한다.

## 파일명 규칙
- 폴더 stem은 자소서와 **완전히 동일한** `{YYYY-MM-DD}-{회사}-{직무}`.
  같은 지원 건의 자소서 `output/{stem}.md`와 1:1 매칭.
- 예: `web/2026-04-28-voiceon-ai_engineer/index.html`

## 표준 섹션 순서
1. **헤더** — 이름, 한 줄 정체성(공고 키워드와 자연스럽게 닿도록), 연락처 링크 (Email / Mobile / Google Scholar / GitHub 등).
   회사명을 페이지 타이틀이나 헤더에 노출 (예: "박진석 — Voiceon 면접 자기소개")
2. **경력 (Work Experience)** — `wiki/profile.md` 경력 요약 기반. 회사·기간·직함 + 핵심 책임 한두 줄
3. **학력 (Education)** — KAIST 박사·석사, 인하대 학사 (수석 졸업) 등
4. **대표 프로젝트 (Selected Projects)** — `wiki/experiences.md`의 카드 중 **공고 우대사항과 매칭되는 3~5개**.
   매칭 우선순위가 높은 순서로 정렬. 카드별 한 줄 요약 + 결과(수치·수상·학회 anchor)
5. **논문·수상 (Publications & Awards)** — `wiki/skills.md` 기반. 1저자 논문과 주요 수상 분리
6. **외부 활동 (Side Experiences)** — Reviewer, 발표(Naver Techtalk 등), 강의
7. **마무리** — 회사·직무에서 하고 싶은 것(공고 키워드 매칭) + 감사 인사
8. **푸터** — 작성 시점(`YYYY-MM-DD`)

## 메타 주석
HTML 상단(`<head>` 위 또는 바로 아래)에 어떤 wiki 항목과 공고를 사용했는지 명시:
```html
<!--
generated_at: 2026-04-28
job_source: sources/jobs/2026-04-voiceon_tts.md
content_source:
  - wiki/profile.md
  - wiki/experiences.md#7 (LLM 기반 NaturalTTS)
  - wiki/experiences.md#6 (차량용 음성 인식)
  - wiki/experiences.md#8 (차량용 VLM)
  - wiki/skills.md
-->
```

## 디자인 원칙
- **단일 페이지 세로 스크롤** (멀티 페이지 X). 앵커 메뉴는 선택
- **무채색 베이스** (흰 배경, 검정·진회색 텍스트), 강조는 1색만 (파랑 계열 권장)
- **텍스트 중심**. 시각 요소는 타임라인·카드·태그 정도로 절제. 화려한 일러스트·이모지 X
- **본문 max-width 720~880px**, 좌우 여백 충분히. 면접 화면에서 1280px 이상 모니터에 띄울 것을 가정
- **시스템 폰트 우선** (한글: Pretendard·Apple SD Gothic Neo·Noto Sans KR fallback). 외부 폰트 로딩은 최소화
- **링크는 강조색** + 밑줄 hover. 외부 링크는 새 탭(`target="_blank"`)
- **다크 모드는 선택**. 강제하지 않음

## 본문 글쓰기 규칙
- **모든 사실은 `wiki/`에서만**. 창작·추측 금지
- 공고 매칭은 **카드 선정·강조 우선순위·헤더 카피**로만 한다. 사실을 만들어 내지 않는다
- **한국어 글 규칙** 준수: 가운뎃점(`·`) 금지, 본문 굵게(`**...**`) 금지. 쉼표·및·와과로 대체
- 문장은 짧게. 카드·리스트 위주
- 영문 직함·논문 제목은 영문 그대로 (예: "Team Lead, Gleo Multimodal Team at 42dot")

## 작성 흐름
1. 공고를 읽고 핵심 요구 역량·우대사항 추출 (자소서 generate와 동일)
2. `wiki/`에서 매칭되는 강점·경험·일화 선정 → 사용자 확인
3. 폴더 `web/{stem}/` 생성, `index.html` 작성
4. `open web/{stem}/index.html`로 미리 보기

## 미리보기
```
open /Users/jspark/Projects/resume_wiki/web/{stem}/index.html
```
면접 환경에서 곧장 띄울 거면 폴더째 USB·iCloud·Dropbox에 복사.

## 배포 (선택)
- 정적 호스팅이면 어디든 가능 (GitHub Pages, Vercel, Netlify, 개인 서버)
- 회사별로 다른 URL이 되도록 폴더 단위 배포가 자연스러움 (예: `username.github.io/web/{stem}/`)
- **개인 정보 노출 검토 필수**: 휴대폰 번호 등은 공개 배포 시 일반적으로 제거. 비공개 정본만 별도 보관

## 참고
- 같은 지원 건의 자소서 `output/{stem}.md`와 stem은 항상 일치시킨다
- 사람이 다듬은 결과 중 재사용 가치가 있는 표현은 wiki에 역반영
