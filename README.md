# MAKJI 브레드마켓 — 1주차 제안 발표 덱

금융시장 지수 등락을 **브레드 인덱스 3종 업다운 예측**으로 옮기고, 주간 예측 랭킹 상위 참여자에게
**7일 프리미엄 브레드 구독권**을 제공하는 참여형 이벤트 서비스의 RFP 제안 발표 자료.

## 열기

**웹:** https://wild-mental.github.io/sesac-4th-corp-rfp/

**로컬:**
```bash
open index.html
```

빌드 과정이 없습니다. 파일을 열면 바로 실행됩니다.

| 키 | 동작 |
|---|---|
| `←` `→` `Space` | 이전 / 다음 장 |
| `Home` `End` | 첫 장 / 마지막 장 |
| `1`~`9` | 해당 번호 장으로 |
| `O` | 전체 개요 |
| `F` | 전체 화면 |
| `?` | 단축키 도움말 |
| `⌘P` | PDF 내보내기 (배경 그래픽 켜기) |

주소창의 `#5` 같은 해시로 특정 장에 바로 접근할 수 있습니다.

## 구성

```
.
├── index.html          발표 덱 본체 — GitHub Pages 진입점이라 루트에 둔다
├── assets/             덱이 직접 불러오는 리소스
│   ├── brand/          로고
│   ├── mockups/        정규화된 앱 화면 목업
│   ├── images/         기타 이미지 조각
│   └── diagrams/       덱에 들어가는 도식
├── docs/               문서
│   └── guides/         별도 배포용 단독 HTML 가이드
├── references/         원본·기획 레퍼런스 (덱에서 직접 쓰지 않음)
│   ├── ux-intro-slides/
│   ├── mobile-screens/
│   └── planning/
└── prototype/          (.gitignore 제외) 별도 저장소 클론 위치
    └── wild-bread-market/
```

| 경로 | 내용 |
|------|------|
| `index.html` | 발표 덱 본체 (12장, 단일 파일) |
| `assets/brand/makji-logo.png` | MAKJI 로고 (표지·3장·클로징) |
| `assets/mockups/ux-*.png` | 정규화된 앱 화면 목업 6종 (380×690 캔버스) |
| `assets/images/reward-box.png` | 리워드 박스 이미지 |
| `assets/diagrams/app-architecture-flow.png` | 애플리케이션 아키텍처 도식 (7장에 사용) |
| `docs/DESIGN.md` | 디자인 정책 SSOT — 컬러 토큰, 타이포, 컴포넌트, 앱 목업 규칙 |
| `docs/presentation-deck-structure.md` | 발표 스토리텔링 구조 (10장 목차 원안) |
| `docs/guides/MAKJI_Cafe24_MVP_교육가이드.html` | 카페24 자사몰 연동 AI-Native 실습 가이드 (63쪽, 단일 파일) — 예제 몰 wildmental · 상품 16 기준. 코드는 🤖 에이전트 프롬프트 24개와 재료 문서로 만들고, 계정·대시보드·비밀값만 사람이 직접. 초반에 진행 방법 자가 점검(HUMAN_TODO만으로 되는 조건)과 비밀값 관리 요약 포함 |
| `docs/cafe24-lab-work-plan.md` | 프로토타입 → AI-Native 가이드 작업 계획 |
| `docs/goal01-prototyping.md` · `docs/goal02-guide-doc-renewal.md` | 두 단계 작업의 /goal 프롬프트 |
| `docs/example-mall-info.md` | 예제 쇼핑몰 wildmental · 상품 16 식별값 |
| `prototype/wild-bread-market/` | **별도 공개 저장소** [wild-mental/wild-bread-market](https://github.com/wild-mental/wild-bread-market) — UP/DOWN 예측 체험 목업 + 카페24 연동 프로토타입. 이 저장소에서는 `.gitignore`로 제외 |
| `references/ux-intro-slides/` | UX 소개 슬라이드 원본 4장 (테마·목업 기준 레퍼런스) |
| `references/mobile-screens/` | 모바일 화면 15종 원본 (사용자 U01~U10 · 관리자 A01~A05) |
| `references/planning/` | 확장형 CJM, 5단계 스토리텔링 구조도 |

## 덱 흐름

5단계로 연결하는 10장 — **컨셉 → 경험 → 구현 → 효과·검증 → 의사결정**

1. 소비자 방문 유인동기 · 2. 기획 배경과 의도
3. 앱 사용 전·중·후 · 4. 핵심 콘텐츠 · 5. 주요 UX
6. 외부데이터 · 7. 구현 구조 (Next.js + Supabase)
8. 기대효과 · 9. 구현·검증 범위
10. 함께 확정할 사항과 다음 단계

## 수정 시

`docs/DESIGN.md`의 체크리스트를 따르고, 반드시 실제 렌더링을 눈으로 확인할 것.
새 리소스는 덱에서 쓰면 `assets/` 아래 용도별 폴더에, 원본·참고용이면 `references/`에 둔다.

```bash
# 저장소 루트에서 실행 (shots/ 는 .gitignore 대상)
CH="/Applications/Google Chrome.app/Contents/MacOS/Google Chrome"
for n in $(seq 1 12); do
  "$CH" --headless=new --disable-gpu --hide-scrollbars --force-device-scale-factor=1 \
    --window-size=1440,810 --virtual-time-budget=2500 \
    --screenshot="shots/s$(printf '%02d' $n).png" "file://$PWD/index.html#$n"
done
```

## 주의

2장의 지수 차트는 각 시장의 변동 특성을 반영해 **생성한 예시 시계열**이며 실제 시장 데이터가 아닙니다.
카드에 "수치는 기획 예시"로 표기되어 있습니다. 실데이터로 교체할 경우 3·4·5·6장 앱 목업 속 수치도
함께 맞춰야 정합성이 유지됩니다.
