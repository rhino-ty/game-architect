<!-- DATA_FRESHNESS -->
> 🔴 **데이터 갱신 필요 항목 포함 — 분기 확인**
> 갱신 대상: Unity 라이센스 정책 (가격·조건이 자주 바뀜)
> 확인 방법: web_search('Unity pricing 2025') 또는 unity.com/pricing
> 전체 갱신 현황: `ref/data-freshness.md` 참조

# Engine: 엔진 선택 가이드 & 전용 스킬 브릿지

## 역할
이 파일은 **어떤 엔진을 선택할지** 도와주는 가이드다.
엔진을 결정했다면, 해당 **전용 스킬**을 설치해서 사용한다.

실제 코드/아키텍처 패턴은 이 스킬이 아닌 전용 스킬에서 다룬다.

---

## 엔진 선택 의사결정 트리

```
내 게임은...

2D 위주인가?
├─ YES → Godot 4 강력 추천
│         Unity도 가능하지만 Godot이 2D에서 더 가볍고 직관적
└─ NO (3D)
    ├─ AAA급 그래픽 / 하이엔드 3D가 필요한가?
    │   ├─ YES → Unreal Engine 5
    │   │         (단: 학습 곡선 높음, 1인 개발엔 부담)
    │   └─ NO → 중간급 3D면 충분
    │            ├─ C# 경험이 있는가?
    │            │   ├─ YES → Unity
    │            │   └─ NO  → Godot 4 (GDScript 진입 쉬움)
    │            └─ 콘솔 포팅이 필수인가?
    │                ├─ YES → Unity (PS/Switch 인증 경로 확립됨)
    │                └─ NO  → Godot 4 (Steam 충분)
```

---

## 엔진별 강점/약점 (1인 개발 기준)

### Godot 4
```
강점:
  ✓ 완전 무료 (수익 로열티 없음)
  ✓ 2D 렌더링이 Unity보다 직관적
  ✓ GDScript 학습 곡선 낮음 (Python 유사)
  ✓ 엔진 자체가 가볍고 빠른 이터레이션
  ✓ 오픈소스 (필요하면 엔진 수정 가능)
  ✓ Steam 출시 친화적

약점:
  ✗ 에셋 스토어 생태계가 Unity보다 작음
  ✗ 콘솔 공식 포팅 절차가 Unity보다 복잡
  ✗ 3D는 Unity/Unreal 대비 성숙도 낮음
  ✗ 모바일 최적화 도구 제한적

추천 장르: 2D 액션, Roguelite, 플랫포머, 비주얼노벨, 텍스트 RPG

전용 스킬: godot.skill (별도 설치 필요)
```

### Unity
```
강점:
  ✓ 에셋 스토어 최대 생태계
  ✓ C# 숙련자에게 친숙
  ✓ 콘솔 포팅 파이프라인 잘 확립됨 (PS, Switch, Xbox)
  ✓ 3D/2D 모두 성숙한 도구
  ✓ DOTS/ECS로 고성능 가능
  ✓ 가장 많은 튜토리얼/커뮤니티

약점:
  ✗ 유료 라이센스 (수익 일정 이상 시 로열티)
    → 2024 이후 런타임 요금 정책 변경됨 (최신 확인 필요)
  ✗ 에디터가 무거움 (빌드 시간 김)
  ✗ 2D는 Godot보다 설정이 복잡

추천 장르: 3D 액션, 오픈월드, 모바일 포팅 계획 있는 게임

전용 스킬: unity.skill (별도 설치 필요)
```

### Unreal Engine 5
```
강점:
  ✓ 최고 수준 그래픽 (Nanite, Lumen)
  ✓ 에픽게임즈 퍼블리싱 경로 존재
  ✓ Blueprint (비주얼 스크립팅)으로 진입 가능

약점:
  ✗ 1인 개발엔 과도한 복잡도
  ✗ 빌드 시간 매우 김
  ✗ 2D 지원 약함
  ✗ C++ 모를 경우 심화 개발 한계
  ✗ 하드웨어 요구사항 높음

추천 장르: AA급 3D 액션/FPS, 그래픽이 경쟁력인 게임
1인 개발 권장도: ★★☆☆☆

전용 스킬: unreal.skill (별도 설치 필요, 향후)
```

---

## 1인 개발자 권장 조합

### 조합 A: 내러티브/텍스트 RPG, 2D 어드벤처
```
엔진: Godot 4
언어: GDScript (또는 C#)
그래픽: 픽셀아트 / 2D 벡터 → AI 생성 가능
스코프: 솔로 12~24개월 현실적
전용 스킬: godot.skill 설치
```

### 조합 B: 3D 액션 / 오픈월드 RPG
```
엔진: Unity (LTS 버전)
언어: C#
그래픽: 로우폴리 3D + 아트 스타일로 승부
스코프: 솔로 18~36개월 (스코프 엄격히 제한)
전용 스킬: unity.skill 설치
```

### 조합 C: 1인 최소 비용, Steam 빠른 출시
```
엔진: Godot 4
장르: Roguelite, 덱빌딩, 2D 플랫포머
이유: 가장 빠른 이터레이션, 무료, 커뮤니티 좋음
목표: 6~18개월 내 얼리 액세스
전용 스킬: godot.skill 설치
```

---

## 엔진 결정 후 다음 단계

```
1. 엔진 선택 완료
      ↓
2. 전용 엔진 스킬 설치
   - Godot 4:  godot.skill
   - Unity:    unity.skill
   - Unreal:   unreal.skill (향후)
      ↓
3. S-06 Claude Code 브릿지 실행
   → CLAUDE.md 생성 (엔진 정보 포함)
      ↓
4. 전용 스킬 내 스켈레톤 코드로 개발 시작
```

---

## 엔진 전용 스킬에서 다룰 내용

각 `engine.skill`은 아래를 포함한다:

```
[엔진명].skill
├── SKILL.md
│   ├── 프로젝트 초기 세팅 절차
│   ├── 디렉토리 구조 컨벤션
│   └── Claude Code 작업 시 주의사항
├── patterns/
│   ├── player-controller.[cs/gd]   ← 즉시 사용 가능
│   ├── combat-system.[cs/gd]
│   ├── enemy-ai.[cs/gd]
│   ├── dialogue-system.[cs/gd]
│   ├── save-system.[cs/gd]
│   └── ui-framework.[cs/gd]
├── architecture/
│   ├── scene-structure.md
│   ├── data-management.md       ← ScriptableObject vs JSON 등
│   └── performance-guide.md
└── recipes/
    ├── roguelite-setup.md
    ├── dialogue-tree-setup.md
    └── steam-integration.md
```

---

> ⚠️ 이 파일(`engine/engine-selection.md`)에는 실제 코드가 없다.
> 코드는 전용 엔진 스킬에서만 다룬다.
> 엔진 결정 전에는 이 파일로 선택 지원, 결정 후에는 전용 스킬로 핸드오프.
