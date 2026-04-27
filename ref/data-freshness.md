# ref/data-freshness.md — 데이터 갱신 현황

이 파일은 스킬 내 시간에 민감한 데이터의 위치와 갱신 주기를 추적한다.
스킬을 사용하기 전, 또는 주기적으로 이 파일을 확인하고 해당 항목을 갱신할 것.

마지막 전체 검토일: 2025년 (v7 기준)

---

## 🔴 빠르게 변함 — 분기~연 1회 확인 필요

### Unity 라이센스 정책
```
위치: engine/engine-selection.md, engine/unity-skill-stub.md, ref/dev-tools.md
내용: Personal Plan 조건, 수익 상한선, 런타임 요금 여부
변동 이력: 2023년 런타임 요금 발표 후 커뮤니티 반발로 철회, 이후 계속 변경
갱신 방법: web_search("Unity pricing 2025") 또는 unity.com/pricing 직접 확인
```

### AI 생성 도구 가격·라이센스
```
위치: ref/dev-tools.md (AI 이미지 생성 섹션), skills/audio-design.md
내용: Midjourney 가격, Suno·Udio 상업 라이센스 조건, Stable Diffusion 정책
변동 이유: AI 도구 시장 빠르게 재편 중. 상업 허용 여부가 갑자기 바뀜
갱신 방법:
  Midjourney: midjourney.com/showcase (가격), 약관 페이지
  Suno: suno.com/pricing
  Adobe Firefly: adobe.com/products/firefly
주의: 상업 출시 전 반드시 최신 약관 직접 확인. 이 스킬 믿지 말 것.
```

### Steam Next Fest 일정
```
위치: skills/indie-marketing.md
내용: Next Fest 개최 시기 (연 2회, 대략 2월·6월)
갱신 방법: store.steampowered.com/sale/nextfest 에서 공식 일정 확인
주의: Valve가 일정을 변경한 사례 있음
```

---

## 🟠 연 1~2회 확인 필요

### Steam 위시리스트·전환율 데이터
```
위치: ref/steam-market.md (위시리스트 목표치 섹션)
현재 수치: 첫 달 전환율 중앙값 ~27%, 첫 주 0.15x (25,000+ 위시리스트 기준)
출처: GameDiscoverCo 2024-2025 연구
갱신 방법: web_search("Steam wishlist conversion rate 2025 GameDiscoverCo")
```

### Steam 수수료 구조
```
위치: skills/post-launch-ops.md
현재 수치: 30% (매출 $10M 이하), $10M~$50M 25%, $50M+ 20%
변동 가능성: 낮음 (Valve는 수수료 구조를 자주 바꾸지 않음)
갱신 방법: partner.steamgames.com/doc/finance/payments_salesreports
```

### 오디오 구독 서비스 가격
```
위치: skills/audio-design.md
현재 수치: Epidemic Sound $15/월, Artlist $199/년
갱신 방법:
  epidemicsound.com/pricing
  artlist.io/pricing
```

### FMOD·Wwise 무료 티어 조건
```
위치: ref/dev-tools.md, skills/audio-design.md
현재: FMOD 수익 $200K 이하 무료, Wwise 인디 티어 무료
갱신 방법:
  fmod.com/licensing
  audiokinetic.com/pricing
```

### 모바일 게임 시장 규모·ARPU
```
위치: ref/market-analysis.md
현재 수치: 글로벌 ~$100B, 한국 ~$6B, ARPU $1.5~$25/월
변동 주기: 연 1회 (시장조사 기관 연간 보고서 기준)
갱신 방법: web_search("mobile game market size 2025") 또는
           Newzoo, Sensor Tower 보고서
```

### 게임 가격대 벤치마크
```
위치: ref/steam-market.md, 각 presets/*.md (Steam BM 섹션)
내용: 인디 게임 권장 가격대, DLC 가격 기준
변동 주기: 인플레이션에 따라 서서히 변화
갱신 방법: web_search("indie game steam price 2025") +
           SteamDB에서 유사 장르 게임 가격 직접 확인
```

---

## 🟢 거의 변하지 않음 — 장기 유효

아래 내용은 이론적 기반이므로 수년 단위로도 유효하다.

```
설계 이론:
  ref/design-frameworks.md — MDA, Tetrad, Flow, DDE는 수십 년된 이론
  skills/system-mechanics.md — MDA 프레임워크 적용법

밸런싱 수식:
  ref/game-formulas.md — TTK 공식, 가챠 기댓값, 성장 곡선은 수학적으로 불변
  skills/numeric-simulation.md — 시뮬레이션 로직 자체

장르 설계 원칙:
  presets/*.md 내 핵심 루프, 시스템 설계, Edge Case
  skills/narrative-design.md — 스토리텔링 원칙

심리 기반 내용:
  skills/persona-test.md — 유저 심리는 느리게 변함
  skills/game-juice-launch.md — 히트스톱, 카메라 흔들림, Coyote Time 수치

오픈소스 도구:
  ref/dev-tools.md 내 Godot, Blender, Audacity, BFXR, Aseprite 등
  → 무료 오픈소스는 가격 변동 없음. 기능 변화만 주의.
```

---

## 갱신 체크리스트 (분기 1회)

```
□ Unity 라이센스 정책 확인
□ Midjourney·Suno 상업 라이센스 조건 확인
□ Steam Next Fest 다음 일정 확인
□ 오디오 구독 서비스 가격 확인

갱신 체크리스트 (연 1회)
□ Steam 전환율 최신 데이터 확인 (GameDiscoverCo)
□ 모바일 시장 규모·ARPU 업데이트
□ 게임 가격대 벤치마크 재확인
□ FMOD·Wwise 무료 조건 확인
□ Steam 수수료 구조 변경 여부 확인
```
