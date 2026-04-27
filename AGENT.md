# Resume Wiki Agent — 워크플로우

Karpathy의 LLM wiki 아이디어를 자소서 작성에 적용한 에이전트.
원본(`sources/`) → 증류된 지식(`wiki/`) → 생성물(`output/`) 의 단방향 흐름.

## 디렉토리
```
sources/        # 원본, 절대 수정 금지
  resumes/      # 과거 자소서 (.md / .txt)
  jobs/         # 채용 공고 (.md / .txt)
wiki/           # sources에서 증류한 "나"의 지식 베이스
output/         # 공고에 맞춰 생성된 자소서
```

## 두 가지 작업 모드

### 1) Ingest — 새 자소서 원본을 wiki에 반영
입력: `sources/resumes/`의 새 파일 (**공고는 ingest 대상이 아님**)
절차:
1. 새 파일을 읽고 어떤 wiki 페이지가 영향을 받는지 식별
2. 기존 wiki 페이지를 **먼저 갱신** (덮어쓰기가 아니라 병합)
3. 새로 발견된 강점·일화·표현이 있으면 해당 페이지에 추가
4. 모든 추가 항목 끝에 출처(`sources/resumes/파일명`)를 단다
5. 중복은 합치고, 모순되면 사용자에게 확인
6. 말투 패턴(어미/접속사/원문 샘플)은 `wiki/writing_style.md`에 누적

### 2) Generate — 공고에 맞춘 자소서 작성
입력: `sources/jobs/` 의 한 공고 파일 (+ 필요 시 문항 명세)
공고는 wiki에 반영하지 않고 이 단계에서만 일회성으로 사용한다.
절차:
1. 공고를 읽고 핵심 요구 역량·인재상·문항 추출
2. `wiki/`를 훑으며 매칭되는 강점·경험·일화 선정
3. `wiki/writing_style.md`를 in-context로 로드 — 원문 샘플 단락을 그대로
   참조하여 같은 어미·호흡·단락 구성으로 작성
4. `output/{날짜}-{회사}-{직무}.md` (정본, 메타 포함) 저장
5. 같은 stem으로 `.txt` (평문)와 `.pdf` (pandoc 변환) 동시 생성
   — 자세한 변환 규칙은 [output/README.md](./output/README.md)
6. 정본 메타에 어떤 wiki 항목을 사용했는지 주석으로 남김

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
