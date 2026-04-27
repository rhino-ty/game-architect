# godot.skill 설치 가이드

## 이 파일의 역할
`game-designer` 스킬에서 Godot 4 관련 코드 작업은 여기로 위임된다.
이 파일은 향후 `godot.skill` 전용 스킬 생성 시 기준이 되는 구조 명세다.

---

## godot.skill이 포함해야 할 내용

```
godot.skill
├── SKILL.md
│   ├── 트리거: "Godot", "GDScript", "씬 구조", "고도 엔진"
│   ├── Godot 4 프로젝트 초기 세팅 절차
│   ├── 디렉토리 구조 컨벤션
│   └── Claude Code + Godot 협업 시 주의사항
│
├── patterns/
│   ├── player_controller_2d.gd     — CharacterBody2D 기반
│   ├── player_controller_3d.gd     — CharacterBody3D 기반
│   ├── combat_system.gd            — 데미지 계산, IDamageable
│   ├── enemy_ai_state_machine.gd   — StateMachine + State 패턴
│   ├── dialogue_manager.gd         — JSON 기반 대화 시스템
│   ├── save_system.gd              — JSON 직렬화 세이브
│   └── event_bus.gd                — AutoLoad 전역 이벤트
│
├── architecture/
│   ├── scene-structure.md          — 씬 분리 패턴 (Main/UI/Game)
│   ├── data-management.md          — Resource vs JSON 선택 기준
│   └── performance-guide.md        — 2D/3D 최적화 팁
│
└── recipes/
    ├── roguelite-setup.md          — 런 생성, Boon 시스템
    ├── dialogue-tree-setup.md      — Dialogic 플러그인 vs 자체 구현
    └── steam-integration.md        — GodotSteam 플러그인 설정
```

---

## 빠른 시작 (godot.skill 없을 때 임시 가이드)

### 프로젝트 초기 구조 (Godot 4)
```
res://
├── scenes/
│   ├── main/          # 메인 씬, 게임 매니저
│   ├── player/        # 플레이어 씬 + 스크립트
│   ├── enemies/       # 적 씬들
│   ├── ui/            # HUD, 메뉴, 대화창
│   └── levels/        # 레벨/맵 씬
├── scripts/
│   ├── core/          # GameManager, SceneManager (AutoLoad)
│   ├── systems/       # Combat, Dialogue, Quest, Save
│   └── utils/         # 헬퍼, 공통 함수
├── data/
│   ├── enemies/       # .tres Resource 파일 또는 .json
│   ├── items/
│   └── dialogues/     # .json 대화 데이터
└── assets/
    ├── sprites/
    ├── audio/
    └── fonts/
```

### AutoLoad 설정 (필수)
```
Project Settings → AutoLoad:
  GameManager     → res://scripts/core/game_manager.gd
  SceneManager    → res://scripts/core/scene_manager.gd
  EventBus        → res://scripts/core/event_bus.gd
  AudioManager    → res://scripts/core/audio_manager.gd
  SaveManager     → res://scripts/core/save_manager.gd
```

### GDScript 코딩 컨벤션
```gdscript
# 클래스명: PascalCase
class_name PlayerController

# 변수명: snake_case
var current_health: float = 100.0

# 상수: SCREAMING_SNAKE_CASE
const MAX_HEALTH: float = 100.0

# 시그널: snake_case (동사 과거형)
signal health_changed(new_value: float)
signal player_died

# 프라이빗: _ 접두사
var _internal_state: int = 0

# 익스포트 변수 그룹화
@export_group("Movement")
@export var move_speed: float = 200.0

@export_group("Combat")
@export var max_health: float = 100.0
```

---

## 언제 godot.skill을 만들어야 하나

다음 상황에서 전용 스킬 생성 권장:
- Godot로 개발 시작이 확정됐을 때
- 씬 구조나 코드 패턴에 대한 질문이 반복될 때
- Claude Code에게 Godot 작업을 위임하기 시작할 때

생성 방법:
- `skill-creator` 스킬 실행
- 위 구조 명세를 기반으로 SKILL.md 작성
- 주요 패턴 코드 파일 채우기
