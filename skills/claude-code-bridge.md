> 🔧 **Claude Code 전용 파일**
> 이 파일의 코드 구조는 Claude Code가 프로젝트를 시작할 때 **베이스 템플릿**으로 사용한다.
> **복붙 금지. 반드시 아래 원칙에 따라 수정 후 사용:**
> - 게임 장르와 아키텍처에 맞게 구조 재설계
> - 디렉토리 구조는 실제 프로젝트에 맞게 변경
> - 스켈레톤 코드는 엔진과 팀 컨벤션에 맞게 재작성
> - 이 파일의 예시를 그대로 따라가지 말 것 — 설계는 개발자가 결정

# S-06: Claude Code 브릿지

## 목적
기획 문서(GDD)를 Claude Code가 즉시 사용할 수 있는 개발 문서로 변환한다.
CLAUDE.md, 프로젝트 디렉토리 구조, 코딩 컨벤션, 태스크 분해까지 생성.

---

## Claude Code가 필요한 문서 계층

```
프로젝트 루트/
├── CLAUDE.md                   ← Claude Code 핵심 컨텍스트 (항상 로드)
├── docs/
│   ├── GDD.md                  ← 게임 기획서 (상세)
│   ├── SYSTEMS.md              ← 시스템별 상세 스펙
│   ├── ARCHITECTURE.md         ← 코드 아키텍처 가이드
│   ├── BALANCE.md              ← 수치 밸런싱 시트
│   └── NARRATIVE.md            ← 스토리/대화 시스템 스펙
├── tasks/
│   ├── BACKLOG.md              ← 전체 태스크 목록
│   ├── SPRINT_CURRENT.md       ← 현재 진행 중인 작업
│   └── DONE.md                 ← 완료된 작업 기록
└── [엔진 프로젝트 폴더]/
```

---

## CLAUDE.md 마스터 템플릿

```markdown
# [게임 제목] — CLAUDE.md

## 프로젝트 개요
- **장르**: [장르]
- **엔진**: Unity [버전] / Godot [버전]
- **언어**: C# / GDScript
- **타겟 플랫폼**: Steam PC / Nintendo Switch / PlayStation
- **현재 단계**: [프로토타입 / 알파 / 베타 / RC]

## 핵심 게임플레이 한 줄 요약
> [게임을 한 문장으로. 예: "탑다운 시점의 로그라이트 액션 RPG, 죽음이 곧 성장인 게임"]

## 코드 아키텍처 원칙
1. **패턴**: [MVC / ECS / ServiceLocator / Observer 등]
2. **씬 구조**: [씬 분리 방식]
3. **데이터 관리**: [ScriptableObject / JSON / CSV 등]
4. **의존성 규칙**: [의존성 방향 정의]

## 디렉토리 구조
```
Assets/ (Unity) 또는 res/ (Godot)
├── Scripts/ (또는 src/)
│   ├── Core/          # 게임 매니저, 씬 매니저
│   ├── Player/        # 플레이어 컨트롤러, 스탯
│   ├── Combat/        # 전투 시스템, 데미지 계산
│   ├── Enemy/         # 적 AI, 행동 트리
│   ├── UI/            # HUD, 메뉴, 대화창
│   ├── Systems/       # 퀘스트, 인벤토리, 저장
│   ├── Data/          # ScriptableObject 정의
│   └── Utilities/     # 공통 헬퍼, 익스텐션
├── Data/ (런타임 데이터)
│   ├── Enemies/       # 적 스탯 데이터
│   ├── Items/         # 아이템 데이터
│   ├── Quests/        # 퀘스트 데이터
│   └── Dialogues/     # 대화 데이터 (JSON)
└── Art/ / Audio/
```

## 코딩 컨벤션
- 클래스명: PascalCase (PlayerController, EnemyAI)
- 메서드명: PascalCase (TakeDamage, OnPlayerDeath)
- 변수명: camelCase (maxHealth, currentSpeed)
- 상수: UPPER_SNAKE_CASE (MAX_PLAYER_LEVEL)
- 인터페이스: I 접두사 (IDamageable, IInteractable)
- 이벤트: On 접두사 (OnDeath, OnLevelUp)

## 핵심 시스템 우선순위
1순위 (MVP 필수):
  - [ ] PlayerController (이동, 카메라)
  - [ ] CombatSystem (공격, 피격, 사망)
  - [ ] EnemyAI (기본 행동 트리)
  - [ ] SceneManager (씬 전환, 저장)

2순위 (알파):
  - [ ] QuestSystem
  - [ ] DialogueSystem
  - [ ] InventorySystem
  - [ ] UIFramework

3순위 (베타):
  - [ ] AudioManager
  - [ ] ParticleSystem
  - [ ] SaveSystem (클라우드)
  - [ ] AchievementSystem

## 현재 작업 컨텍스트
→ tasks/SPRINT_CURRENT.md 참조

## 알려진 이슈 & 주의사항
- [이슈 1]
- [이슈 2]

## 빌드 방법
```bash
# Unity
# File → Build Settings → [플랫폼] → Build

# Godot
# Project → Export → [플랫폼] → Export Project
```
```

---

## 태스크 분해 시스템 (BACKLOG.md 생성 규칙)

### 태스크 크기 기준

| 크기 | 시간 | 예시 |
|------|------|------|
| XS | < 2h | 상수 값 변경, 버그 수정 |
| S | 2~4h | 단순 UI 컴포넌트, 데이터 클래스 |
| M | 4~8h | 시스템 1개 (인벤토리 기본) |
| L | 1~2d | 복합 시스템 (전투 시스템 전체) |
| XL | 2~5d | 아키텍처 변경, 씬 전체 구축 |

### 태스크 작성 템플릿

```markdown
## TASK-[번호]: [제목]

**크기**: [XS/S/M/L/XL]  
**우선순위**: [P0 크리티컬 / P1 높음 / P2 보통 / P3 낮음]  
**시스템**: [Combat / Player / UI / AI / System]  
**의존성**: [선행 TASK 번호]

### 목표
[이 태스크가 완료되면 무엇이 가능한가]

### 구현 스펙
- [구체적 요구사항 1]
- [구체적 요구사항 2]

### 수용 기준 (Acceptance Criteria)
- [ ] [테스트 가능한 조건 1]
- [ ] [테스트 가능한 조건 2]

### 참고 문서
- docs/SYSTEMS.md#[섹션]
- docs/BALANCE.md#[섹션]
```

---

## 엔진별 초기 CLAUDE.md 자동 생성 규칙

스킬 실행 시 엔진과 장르를 입력받아 위 템플릿을 즉시 채워서 출력.

입력 파라미터:
- `engine`: "unity" | "godot"
- `genre`: preset 장르명
- `team_size`: "solo" | "small(2~5)" | "medium(6~15)"
- `target_platform`: "steam" | "console" | "mobile" | "multi"

팀 규모별 아키텍처 조정:
```
solo:   최단 경로 우선. ScriptableObject 기반 데이터 관리.
        패턴: Simple Component + ScriptableObject
        피할 것: 과도한 추상화, DI 프레임워크

small:  기본 패턴 적용. 이벤트 시스템 도입.
        패턴: Observer + ServiceLocator
        피할 것: 엔터프라이즈급 아키텍처

medium: 레이어 분리 명확화. 의존성 주입 고려.
        패턴: MVC/MVP + ECS 부분 적용
```

---

## 문서 동기화 규칙

Claude Code 작업 시 문서와 코드 동기화를 위한 주석 규약:

```csharp
// Unity C# 예시
/// <summary>
/// SYSTEMS.md#combat-system 참조
/// 데미지 공식: BALANCE.md#damage-formula
/// </summary>
public class CombatSystem : MonoBehaviour
{
    // TASK-015 구현
    // 마지막 수정: [날짜] - [변경 내용 한 줄]
}
```

```gdscript
# Godot GDScript 예시
## SYSTEMS.md#player-controller 참조
## 마지막 수정: [날짜]
class_name PlayerController
extends CharacterBody2D
```