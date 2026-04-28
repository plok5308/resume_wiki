# Resume Wiki

`sources/` (원본) → `wiki/` (증류) → `output/` 또는 `web/` (생성) 단방향 흐름의 자소서·자기소개 에이전트.
전체 워크플로우 상세는 [AGENT.md](./AGENT.md) 참조.

## 디렉토리
- `sources/resumes/` — 과거 자소서 원본
- `sources/publications/` — 본인 논문 메타 (Scholar 등에서 수집)
- `sources/cv/` — 영문 CV 등 정형 이력서 (PDF 등)
- `sources/jobs/` — 채용 공고 원본 (**ingest 대상 아님**, generate 입력 전용)
- `wiki/` — sources에서 증류한 "나"의 지식 베이스 ([wiki/index.md](./wiki/index.md))
- `output/` — 공고에 맞춰 생성한 자소서 (per-job)
- `web/` — 면접용 자기소개 웹 페이지 (**per-job**, 회사·직무별 폴더)

`sources/` 하위에 새 카테고리(예: `portfolios/`, `recommendations/`)를 추가해도 같은 ingest 규칙이 적용된다.

## 절대 규칙
- `sources/`는 read-only. 어떤 경우에도 수정·삭제하지 않는다.
- `wiki/`, `output/`, `web/`의 모든 사실은 `sources/` 또는 사용자가 대화에서 명시한 정보에서만 나온다. **창작·추측 금지.**
- 사용자가 새로 알려준 사실은 먼저 wiki에 기록한 뒤 활용한다.

## Ingest (sources → wiki)
**ingest 대상은 `sources/jobs/`를 제외한 모든 sources 하위 폴더.**
공고는 wiki에 반영하지 않는다 (generate의 일회성 입력일 뿐, "나"의 영속적 지식이 아님).

새 원본이 들어오면:
1. 영향받는 wiki 페이지를 식별하고 **병합** (덮어쓰기 X)
2. 추가하는 모든 항목 끝에 출처 표기 (`출처: sources/{카테고리}/{파일}`)
3. 중복은 합치고, 모순되면 사용자에게 확인
4. 새로 발견된 강점·일화·표현은 해당 페이지에 추가

### 카테고리별 반영 가이드
- **resumes** — profile / experiences / strengths / stories / values / writing_style 모두 영향.
  특히 `wiki/writing_style.md`를 반드시 갱신한다. 추상적 형용사("간결하다")
  대신 원문 문장을 직접 인용하고, 어미/접속사/단락 구성 패턴을 구체적으로 기록한다.
  원문 샘플 단락을 3~5개는 항상 유지한다.
- **publications** — 새 카드를 만들기보다 **기존 experiences/skills 카드의 근거(anchor)로**
  추가한다. 같은 도메인의 경험 카드 안에 1저자/공동저자, 학회·연도, 피인용 수,
  자소서 본문 라인과의 직결 여부를 묶어둔다. `skills.md`의 논문 섹션도 1저자/공동저자
  기준으로 정리.
- **cv** — 정형 이력서(영문 CV 등). 본문이 사실의 정확성과 시점을 가장 잘 정리하고
  있는 1차 출처라 wiki/profile.md, experiences.md, skills.md 모두에 영향.
  특히 **경력 시점·소속·직함·학사 정보·수상 명칭의 정확성**을 CV 기준으로 맞춘다.
  영문 CV의 경우 한국어 wiki에 반영하되, 자소서에 그대로 쓰일 수 있는 영문 표현은
  해당 항목에 영문 원문을 보존(예: "Team Lead, Gleo Multimodal Team at 42dot").
- (향후) **portfolios / recommendations 등** — 카테고리에 맞는 wiki 페이지에 같은
  원칙(병합·출처·중복 합치기)으로 반영.

## Generate (wiki + 공고 → output)
1. `sources/jobs/`의 공고에서 핵심 요구 역량·인재상·문항 추출
2. `wiki/`에서 매칭되는 강점·경험·일화 선정 (참조한 wiki 항목을 메타 주석으로 기록)
3. `wiki/writing_style.md`를 **반드시 컨텍스트에 그대로 로드**.
   원문 샘플 단락을 in-context 예시로 활용해 같은 어미·호흡·단락 구성으로 작성.
   생성 후 본인 말투에서 벗어난 표현(피하는 클리셰 등)이 있는지 자기 검토.
4. **기본 흐름은 지원동기 → 경력 → 포부(= 마무리) 3단**.
   공고에 별도 문항이 명시되어 있지 않으면 **소제목 없이** 단락 분리만으로 이 순서를 표현한다 (메일형).
   `## 지원동기` 같은 소제목은 본문에 넣지 않으며 `# 제목` 한 줄만 둔다.
   공고에 입력 문항이 분리돼 있으면 문항을 그대로 헤더로 쓰되 같은 3단 흐름을 문항 안에서 재현한다.
   상세 규칙은 `output/README.md`와 `wiki/writing_style.md` 6번 항목 참조.
5. **두 가지 포맷 생성** (`output/README.md` 규칙 참조):
   - `output/{YYYY-MM-DD}-{회사}-{직무}.md` — 정본 (메타 주석 포함)
   - `.txt` — 평문 (마크다운·메타 제거, 지원 시스템 붙여넣기용)
6. 사람이 다듬은 결과 중 재사용 가치가 있으면 wiki에 역반영
   (특히 수정된 문구는 `writing_style.md`의 원문 샘플에 추가)

## Generate Web (wiki + 공고 → web/{stem}/)
면접에서 띄워 두고 자기소개에 사용하는 **회사·직무별** 단일 페이지 웹사이트.
참고 예시: https://promm.dev/about/

1. 입력
   - `wiki/profile.md`, `experiences.md`, `skills.md`(필요 시 strengths.md, values.md)
   - `sources/jobs/`의 해당 공고 (대표 프로젝트 선정·헤더 카피·마무리 멘트의 매칭 기준)
2. 출력
   - 폴더: `web/{YYYY-MM-DD}-{회사}-{직무}/` (자소서 stem과 동일한 명명)
   - 그 안에 `index.html` + 필요 시 `style.css`, `assets/`
   - 빌드 단계 없이 `open web/{stem}/index.html`로 바로 열림
3. 본문 사실은 `wiki/`에서만 가져온다. **창작·추측 금지.**
   회사·공고 매칭은 **카드 선정과 강조 우선순위**로 하지, 사실을 만들어 내지 않는다.
4. 디자인 원칙
   - **단일 페이지 세로 스크롤** (멀티 페이지 X), 앵커 메뉴는 선택.
   - 무채색 베이스 (흰/검/회), 강조는 1색만.
   - 텍스트 중심, 시각 요소는 타임라인·카드·태그 정도로 절제.
   - 면접 화면에서 1280px 이상 모니터에 띄울 것을 가정한 레이아웃 (max-width 720~880px 본문).
   - 한국어 글 규칙(가운뎃점 금지, 본문 굵게 금지) 준수.
5. 표준 섹션 순서
   1. 헤더 — 이름, 한 줄 정체성(공고 키워드와 자연스럽게 닿도록), 연락처 링크
      (회사명을 헤더 또는 페이지 타이틀에 노출, 예: "박진석 — Voiceon 면접 자기소개")
   2. 경력 (Work Experience) — wiki/profile.md 경력 요약 기반
   3. 학력 (Education)
   4. 대표 프로젝트 (Selected Projects) — `wiki/experiences.md`의 카드 중 **공고 우대사항과 매칭되는 3~5개**.
      매칭 우선순위가 높은 순서로 정렬
   5. 논문·수상 (Publications & Awards) — `wiki/skills.md` 기반
   6. 외부 활동 (Side Experiences) — Reviewer, 발표 등
   7. 마무리 — 회사·직무에서 하고 싶은 것 (공고 키워드와 매칭) + 감사 인사
   8. 푸터 — 작성 시점(`YYYY-MM-DD`)
6. 메타 주석 (`<!-- ... -->`)
   - HTML 상단에 어떤 wiki 항목과 어떤 공고를 사용했는지 메타 주석으로 남긴다 (output/ 자소서와 같은 추적 원칙).
7. 상세 규칙은 [web/README.md](./web/README.md) 참조.

## 언어
- 모든 wiki·output·대화는 한국어 기본.

## 작업 시 행동
- 자소서를 새로 쓸 때는 먼저 어떤 wiki 항목을 쓸지 사용자에게 보여주고 확인받은 뒤 본문을 작성한다.
- wiki에 근거가 부족한 주장은 쓰지 말고, 사용자에게 질문해서 채운다.
