/goal

## 1) 작업 핵심 목표 및 범위
- 목표: `prototype/wild-bread-market/`에 B안 프로토타입을 만들고, 이 폴더에서 `npm test`·`npm run build`·`npm run lint`가 모두 exit 0이며 `node scripts/smoke.mjs`가 전 항목 PASS를 출력하는 상태로 만든다.
- 시작 지점: 저장소 `sesac-4th-corp-rfp` 루트(main, 스테이징된 미커밋 변경 있음). `prototype/wild-bread-market/`은 별도 git 저장소로 만든다(`git init`).
- 작업 대상:
  - (연동 레퍼런스) `docs/guides/MAKJI_Cafe24_MVP_교육가이드.html` 코드 박스에 있는 40개 파일을 출발점으로 삼는다. 라우트 경로·응답 JSON 모양·오류 코드·결과 코드(connected, state_mismatch 등)·테이블 이름은 가이드와 같게 유지한다.
  - (MVP 체험 목업) `/` 랜딩(3종 지수 카드와 이번 주 챌린지 안내), `/predict` 일일 UP/DOWN 예측(3종 지수, 마감 카운트다운, 제출·수정), `/result` 결과 확인(지수별 정답·오답, 주간 정확도, 월~금 진행 표시). 데이터는 목업이며 DB·외부 시세 API는 쓰지 않는다.
  - 화면 기준 자료: `references/mobile-screens/U01~U06`, `references/ux-intro-slides/01~02`, `index.html` 3~6장. 지수 이름·값은 발표자료 값을 따른다(통밀 102.4 ▲1.2% · 크루아상 98.7 ▼0.5% · 골든 105.1 ▲0.8%).
- 작업 자율성: 코드·테스트 작성, 고정 버전 패키지 설치, 저장소 내부 커밋, 로컬 서버·헤드리스 Chrome 점검은 사용자 확인 없이 진행한다. 계정 가입·대시보드 설정, 원격 push, 배포, 실계정 카페24 API 호출은 하지 않는다.

## 2) 작업 세부 규칙
- 사이클: 기능 단위로 테스트 작성 → 구현 → 테스트 실행 → 실패 수정 → 저장소 커밋(`feat: …` / `test: …`).
- 버전 고정: Next.js 16.3.5, react 19.2.8, @supabase/supabase-js 2.116.0, @supabase/ssr 0.12.7, server-only 0.0.1, @types/node 24.13.4, vitest 5.0.0, @electric-sql/pglite 0.5.8, Node.js 24. 코드를 쓰기 전에 `node_modules/next/dist/docs/`의 관련 문서를 확인한다.
- 목업 로직(예측 마감 판정, 정답 판정, 주간 정확도 = 정답 수 ÷ 유효 예측 수, 무효 회차 제외)은 순수 함수로 만들고 Vitest 테스트를 붙인다.
- `scripts/smoke.mjs`를 만든다. 프로덕션 빌드를 로컬에서 실행하고 가짜 Supabase 서버를 붙여 아래를 확인한 뒤 항목마다 PASS/FAIL, 마지막 줄에 `SMOKE: n/n PASS`를 출력한다.
  - 관리 API의 401·403·BAD_ORIGIN·JSON_REQUIRED·SCENARIO_INVALID
  - OAuth 시작 주소·state 쿠키, 콜백의 state_mismatch·denied_* 결과
  - 공개 위젯 API의 CORS·404
  - `/admin/cafe24` UUID 안내, `/`·`/predict`·`/result` 응답 200
  - 실제 상품 16 공개 페이지 사본(`scripts/fixtures/`)에서 위젯이 상품번호 16을 찾아 표시하는지(헤드리스 Chrome)
  - `--project <경로>` 인자로 다른 프로젝트 폴더도 점검할 수 있게 한다.
- 의사결정 기록: `prototype/wild-bread-market/docs/DECISION_LOG.md`에 가이드·발표자료에 없는 결정을 CORE(아키텍처·보안·데이터 구조·가이드 계약 변경) 또는 MINOR(화면 문구·레이아웃·목업 값 세부)로 기록하고, grep 가능한 `CORE: N`, `MINOR: M` 줄을 따로 유지한다.
- 사람이 해야 할 웹 작업(카페24 개발자센터·쇼핑몰 관리자, Supabase·Vercel·GitHub 대시보드, 비밀값 입력)은 수행하지 않고 `docs/HUMAN_TODO.md`에 순서대로 적는다.
- 상위 저장소 `.gitignore`에 `prototype/wild-bread-market/` 한 줄을 추가해 중첩 저장소가 상위 git에 잡히지 않게 한다.

## 3) 종료 조건 및 종료 방법
- goal 평가자: `aztks-agent`(MODE: EVALUATE). 메인 에이전트는 스스로 완료를 선언하지 않는다. 진행·종료 판단은 매 turn 대화에 남긴 `AZTKS VERDICT` 줄을 근거로 한다(호출 규칙은 6) 참고).
- turn 정의: 메인 에이전트가 작업 한 묶음을 마친 뒤 `aztks-agent`를 1회 호출하고 그 판정을 대화에 남기는 사이클.
- 종료 조건 (아래 중 하나라도 충족되는 순간 즉시 멈춘다):
  - test·build·lint가 exit 0이고 `SMOKE: n/n PASS`이며, `aztks-agent`가 검증 명령을 직접 다시 실행한 최종 판정에서 `AZTKS VERDICT: GO` → STOP REASON: PROTOTYPE_VERIFIED
  - CORE 카운터 3 도달 → STOP REASON: CORE_BUDGET
  - MINOR 카운터 12 도달 → STOP REASON: MINOR_BUDGET
  - 같은 smoke 항목이 서로 다른 수정 시도 3회 연속 FAIL → STOP REASON: SMOKE_STUCK
  - `aztks-agent`가 같은 `NEXT FIX` 항목으로 NO-GO를 3회 연속 반환 → STOP REASON: EVAL_STUCK
  - 누적 40 turns(= `aztks-agent` 평가 40회) 도달 → STOP REASON: TURN_CAP (= or stop after 40 turns)
- 종료 방법:
  1) `cd prototype/wild-bread-market && npm test && npm run build && npm run lint && node scripts/smoke.mjs` 출력(각 exit 0, `SMOKE: n/n PASS`)을 대화에 남긴다.
  2) `aztks-agent` 최종 평가를 호출하고, 반환된 5차원 등급표·`AZTKS VERDICT`·`NEXT FIX`를 대화에 그대로 남긴다.
  3) `docs/DECISION_LOG.md` 마지막에 `STOP REASON: <코드>`와 `AZTKS VERDICT: <GO|NO-GO>` 두 줄을 덧붙이고 커밋한다.
  4) `grep -E '^(CORE|MINOR): |STOP REASON|AZTKS VERDICT' prototype/wild-bread-market/docs/DECISION_LOG.md`, `git -C prototype/wild-bread-market log --oneline | head -20`, 상위 저장소 `git status --porcelain` 출력을 대화에 남긴다. 상위 변경은 `.gitignore`만 늘어나야 한다.

## 4) 기타 제약조건
- 상위 저장소에 커밋·push하지 않는다. 기존 스테이징 상태를 바꾸지 않는다(`.gitignore` 한 줄 추가만 예외).
- 수정 금지: `index.html`, `assets/`, `references/`, `docs/DESIGN.md`, `docs/presentation-deck-structure.md`, `docs/example-mall-info.md`, `docs/guides/`.
- 변경은 `prototype/wild-bread-market/`과 상위 `.gitignore` 한 줄로 한정한다.
- 비밀값을 만들거나 요청하거나 출력하지 않는다. 테스트 전용 가짜 값만 쓴다.
- `wildmental.cafe24api.com`에 인증 요청을 보내지 않는다. 공개 상품 페이지 GET(픽스처 저장용)만 허용한다.
- Vercel 배포, `vercel`·`gh` 명령으로 원격 상태를 바꾸지 않는다.

## 5) 사전 조건
- Node.js 24, npm, Google Chrome이 설치되어 있다.
- 이전 작업이 남긴 `.guide-build/`는 이 goal에서 쓰지 않는다.

## 6) goal 평가자 운용 규칙 (aztks-agent)
- 호출 시점: 매 turn 끝에 1회. 완료를 주장하기 직전에는 최종 판정으로 1회 더 호출한다.
- 호출 입력(서브에이전트 프롬프트에 포함):
  - `MODE: EVALUATE`
  - 대상: `prototype/wild-bread-market/` 전체와 상위 저장소 `git status --porcelain`
  - 기준: 이 /goal의 1)~5) 섹션 원문
  - 증거: 이번 turn에 실행한 명령과 출력 원문(명령당 마지막 30줄 이내), `DECISION_LOG.md`의 `CORE:`·`MINOR:` 줄
- 평가 방식:
  - 읽기 전용: 파일 수정·커밋·패키지 설치를 하지 않는다. `cat`·`grep`·`ls`·`git status`·`git log`와 1) 목표의 검증 명령(`npm test`, `npm run build`, `npm run lint`, `node scripts/smoke.mjs`) 재실행만 허용한다.
  - 최종 판정에서는 검증 명령을 직접 다시 실행해 메인 에이전트가 보고한 결과와 대조한다.
  - AZTKS 5차원을 이 goal 기준으로 채점한다.
    - A 알아서: 범위 누락 여부 — 연동 레퍼런스 계약, 목업 3화면, smoke 점검 항목, `HUMAN_TODO.md`
    - Z 잘: 검증 명령 통과와 목업 로직 테스트 존재
    - T 딱: 4) 제약 준수, 변경 범위가 `prototype/wild-bread-market/`과 상위 `.gitignore` 한 줄 안인지
    - K 깔끔: 결정 로그·`HUMAN_TODO.md`가 grep 가능하고 중복·모순이 없는지
    - S 센스: 사람 작업 경계와 비밀값 취급, 발표자료 화면 흐름과의 일치
- 판정 규칙(결정적): 아래를 모두 만족할 때만 `AZTKS VERDICT: GO`, 하나라도 아니면 `AZTKS VERDICT: NO-GO`.
  1) 1) 목표의 네 명령 결과가 exit 0이고 `SMOKE: n/n PASS`
  2) 4) 제약 위반 0건
  3) 5차원 중 ✕ 등급 0개
- 출력 형식: 5차원 등급표(◎/○/△/✕ + 한 줄 근거), `AZTKS VERDICT: GO|NO-GO`, NO-GO일 때 `NEXT FIX: <가장 영향이 큰 수정 1개>`. 메인 에이전트는 이를 대화에 그대로 옮기고 다음 turn에 `NEXT FIX`부터 처리한다.
- 평가는 산출물과 정합성에 대한 것이며, 관찰 → 영향 → 개선 순서로 적고 비난·페널티 표현을 쓰지 않는다.
