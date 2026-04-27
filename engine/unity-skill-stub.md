<!-- DATA_FRESHNESS -->
> 🔴 **데이터 갱신 필요 항목 포함 — 분기 확인**
> 갱신 대상: Unity Personal Plan 조건, 수익 상한선
> 확인 방법: unity.com/pricing
> 전체 갱신 현황: `ref/data-freshness.md` 참조

# unity.skill 설치 가이드

## 이 파일의 역할
`game-designer` 스킬에서 Unity 관련 코드 작업은 여기로 위임된다.
향후 `unity.skill` 전용 스킬 생성 시 기준 구조 명세.

---

## unity.skill이 포함해야 할 내용

```
unity.skill
├── SKILL.md
│   ├── 트리거: "Unity", "C#", "유니티", "씬 구조"
│   ├── Unity 프로젝트 초기 세팅 절차 (URP/HDRP 선택)
│   ├── 디렉토리 구조 컨벤션
│   └── Claude Code + Unity 협업 시 주의사항
│
├── patterns/
│   ├── PlayerController.cs          — CharacterController 기반 (3D)
│   ├── PlayerController2D.cs        — Rigidbody2D 기반 (2D)
│   ├── CombatSystem.cs              — static 유틸, IDamageable
│   ├── EnemyAI.cs                   — NavMeshAgent + 유틸리티 AI
│   ├── DialogueManager.cs           — JSON 기반 대화 시스템
│   ├── SaveSystem.cs                — JSON 직렬화 + Steam Cloud
│   └── EventBus.cs                  — static 이벤트 버스
│
├── architecture/
│   ├── scene-structure.md           — 씬 분리 패턴
│   ├── scriptable-objects.md        — SO 기반 데이터 관리
│   └── performance-guide.md         — 드로우콜, 메모리 관리
│
└── recipes/
    ├── roguelite-setup.md
    ├── dialogue-tree-setup.md
    └── steam-integration.md         — Steamworks.NET 설정
```

---

## 빠른 시작 (unity.skill 없을 때 임시 가이드)

### 프로젝트 디렉토리 구조 (Unity)
```
Assets/
├── _Project/                 # 프로젝트 전용 (에셋 스토어와 분리)
│   ├── Scripts/
│   │   ├── Core/             # GameManager, SceneManager (싱글턴)
│   │   ├── Player/           # PlayerController, PlayerStats
│   │   ├── Combat/           # CombatSystem, Projectile
│   │   ├── Enemy/            # EnemyAI, EnemyData
│   │   ├── UI/               # HUDController, DialogueUI
│   │   ├── Systems/          # Quest, Inventory, Save, Dialogue
│   │   ├── Data/             # ScriptableObject 정의
│   │   └── Utilities/        # Extensions, Helpers, EventBus
│   ├── ScriptableObjects/    # SO 인스턴스
│   │   ├── Enemies/
│   │   ├── Items/
│   │   └── Quests/
│   ├── Scenes/
│   │   ├── _Persistent/      # 항상 로드되는 씬 (싱글턴 씬)
│   │   ├── MainMenu/
│   │   └── Levels/
│   └── Prefabs/
├── ThirdParty/               # 에셋 스토어, 플러그인 (수정 ✗)
└── StreamingAssets/          # 런타임 로드 파일 (JSON 등)
```

### Unity C# 코딩 컨벤션
```csharp
// 클래스명: PascalCase
public class PlayerController : MonoBehaviour

// 메서드명: PascalCase
public void TakeDamage(float damage) { }

// 변수명: camelCase (private), PascalCase (public property)
private float _currentHealth;
public float CurrentHealth => _currentHealth;

// 상수: SCREAMING_SNAKE_CASE
private const float MAX_HEALTH = 100f;

// 이벤트: On 접두사
public static event Action<float> OnHealthChanged;
public static event Action OnPlayerDied;

// SerializeField: 인스펙터 노출, private 유지
[SerializeField] private float moveSpeed = 5f;

// Header: 인스펙터 그룹화
[Header("Movement")]
[SerializeField] private float moveSpeed;

[Header("Combat")]
[SerializeField] private float attackDamage;
```

### 싱글턴 패턴 (GameManager)
```csharp
public class GameManager : MonoBehaviour
{
    public static GameManager Instance { get; private set; }

    private void Awake()
    {
        if (Instance != null && Instance != this) { Destroy(gameObject); return; }
        Instance = this;
        DontDestroyOnLoad(gameObject);
    }
}
```

---

## Unity 라이센스 주의사항 (2024 기준)

> ⚠️ Unity 라이센스 정책이 변경됐음. 개발 시작 전 최신 정책 확인 필수.
> web_search("Unity pricing 2024") 로 최신 내용 확인.

현재 알려진 내용:
- Personal Plan: 연 수익 $100K 미만 무료 (웹 상 광고 제거)
- Plus/Pro: 유료
- 런타임 요금: 2023년 발표 후 철회, 정책 변동 있었음

---

## 언제 unity.skill을 만들어야 하나

- Unity로 개발 확정됐을 때
- 3D 프로젝트이거나 콘솔 포팅 계획이 있을 때
- Claude Code에게 Unity C# 작업 위임 시작할 때
