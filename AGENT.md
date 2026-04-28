# Resume Wiki Agent — 워크플로우

Karpathy의 LLM wiki 아이디어를 자소서·자기소개 자료 작성에 적용한 에이전트.
원본(`sources/`) → 증류된 지식(`wiki/`) → 생성물(`output/`, `web/`) 의 단방향 흐름.

## 디렉토리
```
sources/          # 원본, 절대 수정 금지
  resumes/        # 과거 자소서 (.md / .txt)
  publications/   # 본인 논문 메타 (Scholar 등에서 수집)
  cv/             # 영문 CV 등 정형 이력서 (PDF 등)
  jobs/           # 채용 공고 — ingest 대상 아님, generate 입력 전용
wiki/             # sources에서 증류한 "나"의 지식 베이스
output/           # 공고에 맞춰 생성된 자소서 (.md / .txt, per-job)
web/              # 면접용 자기소개 웹 페이지 (per-job, 회사·직무별 폴더)
  {stem}/         # {YYYY-MM-DD}-{회사}-{직무}/index.html + 선택적 정적 자산
                  # 빌드 단계 없음
```
`sources/` 하위에 새 카테고리(`portfolios/`, `recommendations/` 등)를 추가해도
같은 ingest 규칙이 적용된다.

## 세 가지 작업 모드

### 1) Ingest — 새 원본을 wiki에 반영
입력: `sources/jobs/`를 **제외한** 모든 sources 하위의 새 파일
(jobs는 generate 입력 전용)
절차:
1. 새 파일을 읽고 어떤 wiki 페이지가 영향을 받는지 식별
2. 기존 wiki 페이지를 **먼저 갱신** (덮어쓰기가 아니라 병합)
3. 새로 발견된 강점·일화·표현이 있으면 해당 페이지에 추가
4. 모든 추가 항목 끝에 출처(`sources/{카테고리}/{파일명}`)를 단다
5. 중복은 합치고, 모순되면 사용자에게 확인
6. 카테고리별 가이드는 [CLAUDE.md](./CLAUDE.md)의 ingest 섹션 참조
   - resumes → 모든 wiki 페이지, 특히 writing_style.md
   - publications → 새 카드 생성보다 기존 experiences/skills의 anchor로 추가
   - cv → profile/experiences/skills의 사실(시점·소속·학사·수상 명칭) 정확화 기준

### 2) Generate — 공고에 맞춘 자소서 작성
입력: `sources/jobs/` 의 한 공고 파일 (+ 필요 시 문항 명세)
공고는 wiki에 반영하지 않고 이 단계에서만 일회성으로 사용한다.
절차:
1. 공고를 읽고 핵심 요구 역량·인재상·문항 추출
2. `wiki/`를 훑으며 매칭되는 강점·경험·일화 선정
3. `wiki/writing_style.md`를 in-context로 로드 — 원문 샘플 단락을 그대로
   참조하여 같은 어미·호흡·단락 구성으로 작성
4. `output/{날짜}-{회사}-{직무}.md` (정본, 메타 포함) 저장
5. 같은 stem으로 `.txt` (평문) 생성
   — 자세한 변환 규칙은 [output/README.md](./output/README.md)
6. 정본 메타에 어떤 wiki 항목을 사용했는지 주석으로 남김

### 3) Generate Web — 면접용 자기소개 웹 페이지 (per-job)
입력: `wiki/` + `sources/jobs/`의 해당 공고
출력: `web/{YYYY-MM-DD}-{회사}-{직무}/index.html` + 선택적 정적 자산.
용도: 면접에서 띄워 두고 자기소개에 사용. 모니터에서 세로로 스크롤하며 보여 줌.
공고는 **카드 선정과 강조 우선순위**로 반영하고 사실을 창작하지 않는다.
참고 예시: https://promm.dev/about/
절차:
1. 공고를 읽고 핵심 요구 역량·우대사항 추출 (자소서 generate와 동일한 단계)
2. `wiki/profile.md`, `experiences.md`, `skills.md`를 컨텍스트에 로드해 매칭 카드 선정
3. 표준 섹션 순서: 헤더 → 경력 → 학력 → 대표 프로젝트(매칭 3~5개) → 논문·수상 → 외부 활동 → 마무리 → 푸터
4. `web/{stem}/index.html` 단일 파일로 생성. `open web/{stem}/index.html`로 바로 열림
5. 디자인 원칙: 단일 페이지 세로 스크롤, 무채색 베이스 + 강조 1색, 텍스트 중심, 본문 max-width 720~880px, 한국어 글 규칙 준수
6. HTML 상단에 사용한 wiki 항목과 공고를 메타 주석으로 남김 (자소서 정본과 동일한 추적 원칙)
7. 자세한 규칙은 [web/README.md](./web/README.md)

## 원칙
- `sources/`는 read-only
- 모든 wiki 항목은 출처 추적 가능
- 한 사실은 wiki의 한 곳에만 기록 (중복 금지)
- 생성한 자소서를 사람이 다듬은 결과 중 재사용 가치가 있으면 wiki에 역반영

## 다음 단계
- [ ] `sources/resumes/`에 과거 자소서 채워 넣기
- [ ] `sources/jobs/`에 참고용 공고 한두 개 넣기
- [ ] 첫 ingest 실행 → wiki 초기 채우기
- [ ] 첫 generate로 결과물 품질 확인 후 프롬프트/구조 조정
