# Game Architect

> AI coding agent skill for end-to-end indie game design — system mechanics, balancing, GDD, narrative, audio, marketing, post-launch ops, and Claude Code integration.

## 이 스킬이 하는 일

**The Architect** — 15년 경력 시니어 게임 시스템 디자이너 페르소나로 동작하는 게임 기획 AI 스킬.
1인/소규모 팀이 실제로 만들고 팔 수 있는 범위로 기획을 조율하며, Claude Code와 연동해 개발 문서 구조까지 생성한다.

지원 영역:

1. 게임 시스템·전투·성장·경제·내러티브 구조 설계
2. 수치 밸런싱 (TTK, 확률, 절차적 생성)
3. GDD 작성 및 구조화
4. 플랫폼 적합도 분석 (Steam vs 모바일 vs 하이브리드)
5. 엔진 선택 (Unity / Godot / Unreal) 의사결정
6. Claude Code용 `CLAUDE.md` · 프로젝트 문서 구조 설계
7. 오디오 전략 (BGM 확보, SFX 설계, 미들웨어)
8. 인디 마케팅 (데브로그, 위시리스트, 스트리머 전략)
9. 출시 후 운영 (패치 우선순위, 세일 전략, 로드맵)
10. 절차적 생성 (맵, 아이템 풀, 시드 설계)

## 핵심 스킬 13종

| ID | 명칭 | 파일 |
|----|------|------|
| S-01 | 시스템 메커닉 설계 | `skills/system-mechanics.md` |
| S-02 | 수치 시뮬레이션 | `skills/numeric-simulation.md` |
| S-03 | GDD 구조화 | `skills/gdd-structure.md` |
| S-04 | 페르소나 테스트 | `skills/persona-test.md` |
| S-05 | 내러티브 설계 | `skills/narrative-design.md` |
| S-06 | Claude Code 브릿지 | `skills/claude-code-bridge.md` |
| S-07 | 플랫폼 적합도 분석 | `skills/platform-fit.md` |
| S-08 | Game Juice·접근성·Steam 카피 | `skills/game-juice-launch.md` |
| S-09 | 오디오 설계 | `skills/audio-design.md` |
| S-10 | 인디 마케팅 | `skills/indie-marketing.md` |
| S-11 | 출시 후 운영 | `skills/post-launch-ops.md` |
| S-12 | 절차적 생성 | `skills/procedural-generation.md` |
| S-13 | 솔로 개발 생존 | `skills/solo-dev-survival.md` |

## 장르 Preset (10종)

액션 어드벤처 / 내러티브 RPG / Roguelike / Metroidvania / 텍스트 RPG·비주얼노벨 / 액션 RPG / 시뮬레이션 / 카드게임 / 방치형 RPG / 오픈월드 RPG

→ `presets/*.md`

## Install

```bash
npx skills add rhino-ty/game-architect
```

## 사용 예시

```
"Roguelite 게임 만들고 싶어"
→ 장르 확인 → 플랫폼 적합도 분석 → Preset 로드 → 엔진 추천

"우리 게임 TTK 0.8초인데 너무 빠른가?"
→ 수치 시뮬레이션 + 레퍼런스 비교 + 밸런싱 제안

"Steam 위시리스트 어떻게 모아?"
→ 데브로그·Next Fest·스트리머 전략 통합 플랜

"GDD 어떻게 써?"
→ 아이디어 → GDD 구조 즉시 변환 + Claude Code용 CLAUDE.md 생성
```

## 작업 흐름

```
Step 1. 장르/컨셉 파악 → 핵심 시스템 구조도 시각화
Step 2. 플랫폼 적합도 (S-07) — Steam vs 모바일 vs 하이브리드
Step 3. Preset 로드 → 장르 설계 원칙 참조
Step 4. 엔진 선택 (미결정 시) → engine/engine-selection.md
Step 5. Claude Code 연동 (S-06) → CLAUDE.md + 문서 구조 생성
```

## 파일 구조

```
SKILL.md                         # 메인 스킬 지침 (페르소나·작업 흐름·정책)
skills/                          # 핵심 스킬 13종 (S-01 ~ S-13)
presets/                         # 장르별 설계 Preset 10종
engine/                          # 엔진 선택 의사결정 + 엔진 스킬 stub
ref/                             # 참고 자료
  design-frameworks.md           # MDA · Flow · Tetrad · DDE · 8 Kinds of Fun
  game-formulas.md               # 밸런싱 공식
  steam-market.md                # Steam 시장 데이터
  market-analysis.md             # 모바일/콘솔 시장 분석
  dev-tools.md                   # 개발 툴 추천
  data-freshness.md              # 외부 데이터 갱신 정책
```

## 코드 사용 정책

이 스킬의 코드는 **목적이 다른 세 종류**가 존재한다. 혼동 금지.

| 종류 | 위치 | 용도 |
|------|------|------|
| A. 밸런싱 시뮬 도구 | `skills/numeric-simulation.md`, `ref/game-formulas.md` | 외부 실행용. 게임 코드 아님. 복사 가능 |
| B. 참고용 의사코드 | `skills/procedural-generation.md` 등 | 개념 설명용. 게임에 복사 금지 |
| C. Claude Code 템플릿 | `skills/claude-code-bridge.md` | 베이스로 사용. 게임 특성에 맞게 수정 후 적용 |

**핵심 원칙: 이 스킬은 설계 문서를 생성한다. 구현 결정은 에이전트가 아닌 개발자가 한다.**

## 데이터 갱신 정책

🔴 / 🟠 라벨이 붙은 외부 데이터(Unity 라이센스, AI 도구 상업 라이센스, Steam 통계 등)는 사용 전 검증 필요.
전체 갱신 현황 → `ref/data-freshness.md`

## Trigger keywords

| Language | Keywords |
|----------|----------|
| 한국어 | 게임 만들려고, 시스템 설계, 밸런싱, GDD, 위시리스트, 스코프, 번아웃, 솔로 개발 |
| English | game design, balancing, GDD, narrative, Steam wishlist, indie marketing |

## Language

- 스킬 본문: 한국어
- 응답: 사용자 언어에 맞춰 대응

## License

MIT
