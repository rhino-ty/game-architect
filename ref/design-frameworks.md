# Ref: 게임 설계 프레임워크 (Design Frameworks)

게임 설계에 사용되는 핵심 이론 프레임워크 모음.
각 프레임워크는 "게임을 보는 다른 렌즈"다 — 상황에 따라 다른 것을 쓴다.

---

## Framework 1: MDA (Mechanics · Dynamics · Aesthetics)
*Hunicke, LeBlanc, Zubek — GDC 2004*

```
[디자이너 시점]  Mechanics → Dynamics → Aesthetics  [플레이어 시점]
[플레이어 시점]  Aesthetics → Dynamics → Mechanics  [역방향 분석]
```

**정의:**
- **Mechanics**: 게임의 규칙, 데이터 구조, 알고리즘. 설계자가 직접 제어하는 것
- **Dynamics**: 플레이어 입력 + 메커닉이 만나 실시간으로 발생하는 행동 패턴
- **Aesthetics**: 플레이어가 경험하는 감정 반응. 설계자가 간접적으로만 영향을 줄 수 있음

**언제 쓰는가:**
- 시스템 설계 초반: "이 메커닉이 어떤 다이나믹을 만들고, 어떤 감정을 유발하는가?"
- 버그처럼 보이는 밸런스 문제 분석: "메커닉은 의도대로인데 다이나믹이 이상하다"

**MDA의 한계:**
내러티브 설계를 수용하지 못함. "스토리는 메커닉인가, 다이나믹인가, 에스테틱인가?"에 명확한 답이 없음.
→ 내러티브 중심 게임이라면 DDE 프레임워크 병행

---

## Framework 2: 8 Kinds of Fun (LeBlanc의 미적 분류)
*Marc LeBlanc — MDA 논문에서 정의*

재미의 종류를 어휘화하는 도구. "어떤 재미를 목표로 하는가"를 팀 내에서 공유할 때 사용.

```
1. Sensation   — 감각적 쾌락. 아름다운 그래픽, 타격감 있는 SFX, 만족스러운 UI
2. Fantasy     — 다른 세계에서 다른 존재가 되는 것. RPG, 오픈월드의 핵심
3. Narrative   — 드라마로서의 게임. 이야기와 캐릭터에 감정 투자
4. Challenge   — 숙달과 극복. 소울류, 퍼즐, 스피드런의 핵심
5. Fellowship  — 사회적 연결. 협동 플레이, 길드, 커뮤니티
6. Discovery   — 탐험과 발견. 오픈월드, Metroidvania, 숨겨진 요소
7. Expression  — 자기 표현. 캐릭터 커스터마이징, 빌드 설계, 창작
8. Submission  — 일상 탈출. 방치형 게임, 편안한 루프, 힐링 게임
```

**실전 활용:**
게임 설계 초반에 "우리 게임의 주요 재미는 1번 Challenge + 6번 Discovery"처럼 명시.
→ 설계 결정이 이 목표와 일치하는지 기준점이 됨
→ 다음 질문: "이 시스템이 Challenge와 Discovery를 강화하는가, 약화하는가?"

---

## Framework 3: Elemental Tetrad
*Jesse Schell — "The Art of Game Design" (2008)*

```
         [Mechanics]
        /            \
[Story]              [Technology]
        \            /
         [Aesthetics]
```

**4개 요소:**
- **Mechanics**: 규칙과 절차. 플레이어가 할 수 있는 것과 할 수 없는 것
- **Story**: 서사와 맥락. 왜 이 행동이 의미있는가
- **Aesthetics**: 보이고 들리고 느껴지는 것 (시각·청각·감촉)
- **Technology**: 구현 도구와 제약. 어떤 플랫폼, 어떤 엔진, 어떤 하드웨어

**핵심 원칙: 4개 요소는 서로 지탱하고 강화한다**

실제 사례:
```
Silent Hill의 안개:
  Technology: 초기 PS1의 렌더링 거리 한계로 안개를 넣음
  Mechanics:  시야 제한 → 공포 서스펜스 강화
  Aesthetics: 음산하고 불안한 분위기
  Story:      현실과 꿈 사이 경계의 모호함
  → 기술적 제약이 오히려 최고의 공포 연출이 됨

홀로우나이트의 미니멀 UI:
  Technology: UI 요소 최소화 (화면에 HUD 거의 없음)
  Aesthetics: 몰입 극대화, 세계관이 가득 찬 느낌
  Story:      플레이어가 이 세계의 주민이 된 것 같은 느낌
  Mechanics:  중요 정보는 게임 내 오브젝트로 전달
```

**MDA와의 차이:**
MDA = "어떻게 작동하는가" (프로세스 관점)
Tetrad = "무엇으로 만들어졌는가" (구성 요소 관점)

**언제 쓰는가:**
- 전체적인 게임 개요를 잡을 때
- "왜 이 게임이 재미없는가" 진단할 때 — 4개 중 어느 요소가 약한지 찾기
- 레퍼런스 게임 분석할 때 — 4개 축으로 분해

---

## Framework 4: Flow Theory (몰입 이론)
*Mihaly Csikszentmihalyi — "Flow: The Psychology of Optimal Experience" (1990)*

```
      높음
       │    [불안/좌절]         Flow Channel
       │         ╲              ╱╲
  도전  │          ╲           ╱  ╲
  난이도 │           ╲         ╱    ← 이 영역에 머물게 설계
       │            ╲       ╱
       │  [무관심]   ╲     ╱  [지루함]
       │              ╲   ╱
       └────────────────────────────
      낮음       플레이어 실력       높음
```

**Flow Channel 핵심:**
- 도전 > 실력: 불안, 좌절, 이탈
- 도전 < 실력: 지루함, 이탈
- 도전 ≈ 실력: 몰입(Flow) 상태 — 시간 가는 줄 모름

**게임 설계 적용:**

*난이도 곡선 설계:*
```
초보자: 낮은 도전 → 실력 빠르게 성장 → 도전 함께 상승
중급자: 도전이 실력 바로 앞에서 당기는 느낌
고급자: 마스터리 달성의 만족감
→ 항상 "다음에 도전할 것"이 보여야 함
```

*Flow를 깨는 것들:*
```
  ✗ 갑작스러운 난이도 스파이크 (배운 것이 통하지 않는 구간)
  ✗ 과도한 반복 (학습 없는 반복 = 지루함)
  ✗ 모호한 목표 (무엇을 해야 하는지 모름)
  ✗ 즉각적 피드백 부재 (내가 잘하고 있는지 모름)
  ✗ 불필요한 중단 (로딩, 컷신 강제 시청)
```

*Flow를 지원하는 설계:*
```
  ✓ 명확한 단기 목표 (다음 체크포인트, 다음 방)
  ✓ 즉각적인 피드백 (타격감, SFX, 수치 팝업)
  ✓ 실력 곡선에 맞춘 점진적 난이도
  ✓ 실패해도 빠른 재시도 (Dead Cells의 빠른 리스폰)
  ✓ 도전 완수 순간의 보상 (레벨업 연출, 보스 처치 연출)
```

**Csikszentmihalyi의 Flow 8가지 조건:**
```
1. 명확한 목표
2. 즉각적인 피드백
3. 도전-실력 균형
4. 행동과 인식의 합일 (생각 없이 몸이 움직이는 순간)
5. 집중 방해 요소 배제
6. 실패에 대한 걱정 감소
7. 자의식 소멸 (자신을 잊고 게임에 빠짐)
8. 시간 감각 왜곡 (시간이 빠르게 느껴짐)
```

---

## Framework 5: DDE (Design · Dynamics · Experience)
*Walk, Görlich, Barrett — Springer 2017. MDA의 공식 발전형.*

MDA가 내러티브 설계를 수용 못하는 문제를 해결한 프레임워크.
내러티브 RPG, 스토리 중심 게임에서 MDA보다 유용.

```
Design               Dynamics             Experience
  ├─ Blueprint         ├─ 게임 시스템         ├─ Player-Subject
  ├─ Mechanics         ├─ 내러티브 전개       └─ Antagonist
  └─ Interface         └─ 플레이어 행동
```

**핵심 개념:**

*Player-Subject:*
플레이어는 단순히 인터페이스를 조작하는 것이 아니라, 게임 내에서 "주체(Subject)"가 됨.
현실에서 불가능한 결정을 내리고, 다른 정체성으로 경험함.

*Antagonist (게임 설계 용어):*
단순히 빌런이 아닌, 플레이어의 진행을 막는 모든 도전.
좋은 Antagonist = Player-Subject와 의미 있는 긴장을 만드는 것.

*Story-Gameplay 통합:*
```
실패한 통합: 컷신(스토리) ↔ 전투(게임플레이) 단절
성공한 통합: 전투 방식이 캐릭터 성격을 표현 (갓오브워 크레토스의 분노)
             게임플레이 결과가 서사를 바꿈 (위쳐3 선택의 장기 결과)
```

**언제 쓰는가:**
- 내러티브 RPG, 어드벤처, 선택형 게임 설계
- "스토리와 게임플레이가 따로 논다"는 문제 진단할 때
- 캐릭터 설계를 메커닉과 연결할 때

---

## 프레임워크 선택 가이드

```
"어떤 재미를 만들 것인가" 정의할 때:
  → 8 Kinds of Fun

"시스템이 어떻게 작동하는가" 분석할 때:
  → MDA

"게임이 무엇으로 만들어졌는가" 점검할 때:
  → Elemental Tetrad

"플레이어가 몰입하는가" 진단할 때:
  → Flow Theory

"스토리와 게임플레이를 통합할 때":
  → DDE

실전에서는 하나만 쓰지 않는다.
설계 단계에 따라 다른 렌즈를 쓰는 것이 좋다.
```

---

## 필독 자료 (1인 개발자 추천 우선순위)

```
1. Jesse Schell, "The Art of Game Design: A Book of Lenses" (2008)
   → 100개의 설계 렌즈. 게임 설계 교과서 중 가장 실용적
   → Kindle/PDF로 구할 수 있음

2. Hunicke et al., "MDA: A Formal Approach to Game Design" (2004)
   → 무료 PDF. 10페이지. MDA 원본 논문
   → cs.northwestern.edu/~hunicke/MDA.pdf

3. Csikszentmihalyi, "Flow: The Psychology of Optimal Experience" (1990)
   → 심리학 원서. 게임 이외 분야에도 적용 가능
   → 요약본만 읽어도 충분

4. Walk·Görlich·Barrett, "DDE Framework" (2017)
   → Springer 논문. 내러티브 게임 만들 때 참고
   → ResearchGate에서 무료 접근 가능

5. Derek Yu, "Finishing a Game" (블로그 포스트)
   → 스코프 관리와 완성의 중요성. 1인 개발 필독
   → makegames.tumblr.com
```
