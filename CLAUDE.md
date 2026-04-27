# Resume Wiki

`sources/` (원본) → `wiki/` (증류) → `output/` (생성) 단방향 흐름의 자소서 작성 에이전트.
전체 워크플로우 상세는 [AGENT.md](./AGENT.md) 참조.

## 디렉토리
- `sources/resumes/` — 과거 자소서 원본
- `sources/publications/` — 본인 논문 메타 (Scholar 등에서 수집)
- `sources/jobs/` — 채용 공고 원본 (**ingest 대상 아님**, generate 입력 전용)
- `wiki/` — sources에서 증류한 "나"의 지식 베이스 ([wiki/index.md](./wiki/index.md))
- `output/` — 공고에 맞춰 생성한 자소서

`sources/` 하위에 새 카테고리(예: `portfolios/`, `recommendations/`)를 추가해도 같은 ingest 규칙이 적용된다.

## 절대 규칙
- `sources/`는 read-only. 어떤 경우에도 수정·삭제하지 않는다.
- `wiki/`와 `output/`의 모든 사실은 `sources/` 또는 사용자가 대화에서 명시한 정보에서만 나온다. **창작·추측 금지.**
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
- (향후) **portfolios / recommendations 등** — 카테고리에 맞는 wiki 페이지에 같은
  원칙(병합·출처·중복 합치기)으로 반영.

## Generate (wiki + 공고 → output)
1. `sources/jobs/`의 공고에서 핵심 요구 역량·인재상·문항 추출
2. `wiki/`에서 매칭되는 강점·경험·일화 선정 (참조한 wiki 항목을 메타 주석으로 기록)
3. `wiki/writing_style.md`를 **반드시 컨텍스트에 그대로 로드**.
   원문 샘플 단락을 in-context 예시로 활용해 같은 어미·호흡·단락 구성으로 작성.
   생성 후 본인 말투에서 벗어난 표현(피하는 클리셰 등)이 있는지 자기 검토.
4. **세 가지 포맷 동시 생성** (`output/README.md` 규칙 참조):
   - `output/{YYYY-MM-DD}-{회사}-{직무}.md` — 정본 (메타 주석 포함)
   - `.txt` — 평문 (마크다운·메타 제거, 지원 시스템 붙여넣기용)
   - `.pdf` — `pandoc` 기반 변환
5. 사람이 다듬은 결과 중 재사용 가치가 있으면 wiki에 역반영
   (특히 수정된 문구는 `writing_style.md`의 원문 샘플에 추가)

## 언어
- 모든 wiki·output·대화는 한국어 기본.

## 작업 시 행동
- 자소서를 새로 쓸 때는 먼저 어떤 wiki 항목을 쓸지 사용자에게 보여주고 확인받은 뒤 본문을 작성한다.
- wiki에 근거가 부족한 주장은 쓰지 말고, 사용자에게 질문해서 채운다.
