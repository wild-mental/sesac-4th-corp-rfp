/goal

## 1) 작업 핵심 목표 및 범위
- 목표: `docs/guides/MAKJI_Cafe24_MVP_교육가이드.html`을 AI-Native형으로 교체하고, 가이드의 🤖 프롬프트만 새 폴더에서 순서대로 실행해 만든 프로젝트가 `npm test`·`npm run build`·`npm run lint` exit 0과 `SMOKE: n/n PASS`를 통과하게 한다.
- 시작 지점: 저장소 루트. 선행 조건으로 `grep 'STOP REASON: PROTOTYPE_VERIFIED' prototype/wild-bread-market/docs/DECISION_LOG.md`가 1줄 이상 나와야 한다. 없으면 즉시 멈춘다.
- 작업 대상:
  - 코드 단계의 구성: 코드 전문을 싣지 않고, 단계마다 [만들어질 파일명 + 담길 내용(재료)] → [에이전트 프롬프트] → [수락 기준] → [막히면] 순서로 쓴다.
  - 공통 재료 문서: `docs/lab-context.md`(몰 값, 지수·시나리오, 문구·가격 규칙, 카페24 API 사실, 환경변수 이름, 고정 버전)와 `AGENTS.md` 실습 규칙 섹션을 에이전트가 만들게 하는 프롬프트를 포함한다.
  - 사람 작업은 계정 가입·온보딩·대시보드 설정(카페24 개발자센터·쇼핑몰 관리자, Supabase, Vercel, GitHub 웹)과 비밀값 입력에만 배정한다. 역할 표시는 🤖 에이전트 / 🧑 직접·웹 설정 / 👀 직접·확인 / 🔑 직접·비밀값.
  - B안 체험 화면(`/`, `/predict`, `/result`)을 만드는 단계를 포함한다.
  - 프로토타입과 같아야 하는 계약(경로·JSON·오류 코드)은 프롬프트와 수락 기준에 명시한다.
- 작업 자율성: 가이드 작성, 시험 실행 폴더에서의 프롬프트 실행, 점검 스크립트 실행은 사용자 확인 없이 진행한다. 커밋·push·배포는 하지 않는다.

## 2) 작업 세부 규칙
- 사이클: 가이드 초안 → 🤖 프롬프트 추출 → 시험 실행 → 점검 → 프롬프트·수락 기준 보완을 반복한다.
- 시험 실행:
  - 폴더: `.guide-build/agent-run/`. 시작 전에 `.guide-build/` 기존 내용을 지우고 `.git/info/exclude`에 등록한다.
  - 실행자: 서브에이전트. 가이드의 🤖 프롬프트 텍스트만 입력으로 받고 `prototype/`과 `docs/guides/`는 읽지 않게 지시한다.
  - git: 생성 프로젝트 안에서 `git init` 후에만 git 명령을 쓰게 한다.
  - 🧑·🔑 단계는 테스트용 가짜 값으로 대신하고, 그 사실을 보고하게 한다.
- 점검 스크립트:
  - `prototype/wild-bread-market/scripts/guide-check.mjs`를 추가한다. 헤드리스 Chrome으로 가이드를 열어 슬라이드 수, JS 오류 수, 390px·1366px 가로 넘침 수, 🤖 프롬프트 박스 수, "새 파일/전체 교체" 코드 전문 박스 수, 내 앱 주소 치환 동작을 출력한다.
  - 마지막 줄 형식: `GUIDE_CHECK: slides=N errors=0 overflow=0 prompts=P fullcode=0 appOrigin=OK`
- 유지할 기존 요소: 단일 HTML 파일, 내장 Noto Sans KR `@font-face` 124개와 OFL 라이선스 주석, 목차·검색·방향키 이동·완료 체크·전체 읽기·인쇄, 내 앱 주소 입력 치환, 예제 몰 사실(wildmental · 16 · P000000Q · 81 · 1)과 공식 문서 확인 내용, 마지막 "검증 범위" 페이지(시험 실행 결과와 미검증 항목을 사실대로 갱신).
- 의사결정 기록: `prototype/wild-bread-market/docs/GUIDE_DECISION_LOG.md`에 CORE(가이드 구조·역할 배정·계약 변경)와 MINOR(문구·페이지 분할)를 기록하고 `CORE: N`, `MINOR: M` 줄을 유지한다.
- 상위 저장소 `README.md`의 가이드 설명 한 줄을 새 방식에 맞게 고친다.

## 3) 종료 조건 및 종료 방법
- goal 평가자: `aztks-agent`(MODE: EVALUATE). 메인 에이전트는 스스로 완료를 선언하지 않는다. 진행·종료 판단은 매 turn 대화에 남긴 `AZTKS VERDICT` 줄을 근거로 한다(호출 규칙은 5) 참고).
- turn 정의: 메인 에이전트가 작업 한 묶음을 마친 뒤 `aztks-agent`를 1회 호출하고 그 판정을 대화에 남기는 사이클. 선행 조건 확인은 평가자 호출 없이 grep 출력으로 판단한다.
- 종료 조건 (아래 중 하나라도 충족되는 순간 즉시 멈춘다):
  - 선행 조건 grep 결과 0줄 → STOP REASON: PREREQ_MISSING
  - 다음을 모두 만족 → STOP REASON: GUIDE_VERIFIED
    - 시험 실행 프로젝트의 test·build·lint exit 0
    - `SMOKE: n/n PASS`
    - `GUIDE_CHECK`의 errors=0 overflow=0 fullcode=0 appOrigin=OK
    - `aztks-agent`가 검증 명령을 직접 다시 실행한 최종 판정에서 `AZTKS VERDICT: GO`
  - 시험 실행 → 보완 라운드가 3회 실패 → STOP REASON: DRYRUN_FAIL_CAP
  - `aztks-agent`가 같은 `NEXT FIX` 항목으로 NO-GO를 3회 연속 반환 → STOP REASON: EVAL_STUCK
  - CORE 카운터 3 도달 → STOP REASON: CORE_BUDGET
  - MINOR 카운터 12 도달 → STOP REASON: MINOR_BUDGET
  - 누적 40 turns(= `aztks-agent` 평가 40회) 도달 → STOP REASON: TURN_CAP (= or stop after 40 turns)
- 종료 방법:
  1) `cd .guide-build/agent-run/makji-cafe24-lab && npm test && npm run build && npm run lint` 출력과 `node prototype/wild-bread-market/scripts/smoke.mjs --project .guide-build/agent-run/makji-cafe24-lab` 출력(`SMOKE: n/n PASS`)을 대화에 남긴다.
  2) `node prototype/wild-bread-market/scripts/guide-check.mjs docs/guides/MAKJI_Cafe24_MVP_교육가이드.html`의 `GUIDE_CHECK:` 줄과 `grep -c '@font-face' docs/guides/MAKJI_Cafe24_MVP_교육가이드.html`(124)을 대화에 남긴다.
  3) `aztks-agent` 최종 평가를 호출하고, 반환된 5차원 등급표·`AZTKS VERDICT`·`NEXT FIX`를 대화에 그대로 남긴다.
  4) `GUIDE_DECISION_LOG.md` 마지막에 `STOP REASON: <코드>`와 `AZTKS VERDICT: <GO|NO-GO>` 두 줄을 덧붙인다.
  5) 상위 저장소 `git status --porcelain` 출력을 대화에 남긴다. 이 goal의 변경은 가이드 HTML과 `README.md`만이어야 한다.
  6) GUIDE_VERIFIED이면 증거를 남긴 뒤 `.guide-build/`와 `.git/info/exclude`의 해당 줄을 삭제한다.

## 4) 기타 제약조건
- 상위 저장소에 커밋·push하지 않는다. 기존 스테이징 상태를 바꾸지 않는다.
- 수정 금지: `index.html`, `assets/`, `references/`, `docs/DESIGN.md`, `docs/presentation-deck-structure.md`, `docs/example-mall-info.md`, 프로토타입의 앱 코드(`prototype/wild-bread-market/src/`, `public/`, `supabase/`, `tests/`). 가이드 검증에 필요한 `scripts/`와 `docs/` 추가·수정은 예외다.
- 변경은 `docs/guides/MAKJI_Cafe24_MVP_교육가이드.html`, `README.md`, `prototype/wild-bread-market/scripts/`·`docs/`, `.guide-build/`로 한정한다.
- 가이드에 앱 소스 코드 전문을 싣지 않는다. 명령어 한두 줄, SQL 확인 쿼리, 응답 예시는 허용한다.
- 비밀값을 가이드·프롬프트·로그에 쓰지 않는다. 프롬프트는 에이전트에게 비밀값을 요청하지 않도록 쓴다.
- 확인하지 않은 카페24·Supabase·Vercel 화면 동작을 사실처럼 쓰지 않는다. 미확인 사항은 "검증 범위" 페이지에 적는다.
- 배포, 원격 상태 변경, 실계정 카페24 API 인증 호출을 하지 않는다.

## 5) goal 평가자 운용 규칙 (aztks-agent)
- 호출 시점: 매 turn 끝에 1회. 완료를 주장하기 직전에는 최종 판정으로 1회 더 호출한다.
- 역할 분리: `aztks-agent`는 평가만 한다. 가이드 프롬프트를 실행하는 시험 실행 서브에이전트로 쓰지 않는다.
- 호출 입력(서브에이전트 프롬프트에 포함):
  - `MODE: EVALUATE`
  - 대상: `docs/guides/MAKJI_Cafe24_MVP_교육가이드.html`, `.guide-build/agent-run/makji-cafe24-lab/`, `prototype/wild-bread-market/scripts/`·`docs/`, 상위 저장소 `git status --porcelain`
  - 기준: 이 /goal의 1)~4) 섹션 원문
  - 증거: 이번 turn에 실행한 명령과 출력 원문(명령당 마지막 30줄 이내), `GUIDE_DECISION_LOG.md`의 `CORE:`·`MINOR:` 줄, 시험 실행 서브에이전트 보고 요약
- 평가 방식:
  - 읽기 전용: 파일 수정·커밋·패키지 설치를 하지 않는다. `cat`·`grep`·`ls`·`git status`·`git log`와 3) 종료 방법 1)·2)의 검증 명령 재실행만 허용한다.
  - 계약이 프로토타입과 같은지 확인할 때는 `prototype/wild-bread-market/`을 읽을 수 있다.
  - 최종 판정에서는 검증 명령을 직접 다시 실행해 메인 에이전트가 보고한 결과와 대조한다.
  - AZTKS 5차원을 이 goal 기준으로 채점한다.
    - A 알아서: 범위 누락 여부 — 재료 문서(`docs/lab-context.md`·`AGENTS.md`) 프롬프트, B안 화면 단계, 프로토타입 계약, 사람 작업 단계, 검증 범위 페이지
    - Z 잘: 시험 실행·smoke·guide-check 통과, 수락 기준이 명령 결과로 확인 가능한지
    - T 딱: 4) 제약 준수, 변경 범위, 앱 코드 전문 박스 0개
    - K 깔끔: 코드 단계가 [파일명·재료 → 프롬프트 → 수락 기준 → 막히면] 순서를 지키는지, 프롬프트 간 중복·모순이 없는지
    - S 센스: 학습자 관점 — 에이전트에게 비밀값을 요청하지 않음, 🤖·🧑·👀·🔑 역할 표시가 맞음, 미확인 사실을 단정하지 않음
- 판정 규칙(결정적): 아래를 모두 만족할 때만 `AZTKS VERDICT: GO`, 하나라도 아니면 `AZTKS VERDICT: NO-GO`.
  1) 3) 종료 조건 GUIDE_VERIFIED의 명령 기준(test·build·lint exit 0, `SMOKE: n/n PASS`, `GUIDE_CHECK` 기준값, `@font-face` 124)을 모두 충족
  2) 4) 제약 위반 0건
  3) 5차원 중 ✕ 등급 0개
- 출력 형식: 5차원 등급표(◎/○/△/✕ + 한 줄 근거), `AZTKS VERDICT: GO|NO-GO`, NO-GO일 때 `NEXT FIX: <가장 영향이 큰 수정 1개>`. 메인 에이전트는 이를 대화에 그대로 옮기고 다음 turn에 `NEXT FIX`부터 처리한다.
- 평가는 산출물과 정합성에 대한 것이며, 관찰 → 영향 → 개선 순서로 적고 비난·페널티 표현을 쓰지 않는다.
