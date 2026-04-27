# Preset: 텍스트 RPG / 비주얼노벨 (Visual Novel / Text-Based RPG)

## 레퍼런스
Disco Elysium, 오리와, 페이트 시리즈, Doki Doki Literature Club, 80 Days,
AI: The Somnium Files, Ace Attorney, Undertale (텍스트+경량 액션)

## 장르 정의
그래픽보다 텍스트와 선택이 핵심 경험인 장르.
**1인 개발의 가장 현실적인 "퀄리티 높은 게임" 경로**.
그래픽 부담을 스토리와 캐릭터로 완전히 대체할 수 있다.

---

## 장르 하위 분류

```
비주얼노벨 (VN):
  이미지 + 텍스트 + 선택지
  예) 페이트, DDLC, AI: 소름니움 파일즈
  
텍스트 어드벤처:
  텍스트만으로 세계를 묘사, 조사/이동/대화
  예) 80 Days, 파슬리
  
텍스트 RPG (하이브리드):
  텍스트 기반 + 경량 전투/스탯 시스템
  예) Disco Elysium, Undertale, Zork
  
대화 RPG:
  대화가 곧 전투, 스킬 체크가 핵심
  예) Disco Elysium (기술 판정), Planescape: Torment
```

---

## 핵심 루프

### 비주얼노벨
```
[장면 읽기] → [선택지] → [결과/감정 변화] → [다음 장면]
                              ↓
                    [관계 수치 변화] → [분기 엔딩 영향]
```

### 텍스트 RPG (Disco Elysium 모델)
```
[대화/조사] → [스킬 체크] → [성공/실패 결과]
                    ↓
          [캐릭터 성장 반영] → [새 선택지 해금]
```

---

## 필수 시스템

### 텍스트 연출 기법

```
속도감:
  문장 단위 출력 속도로 긴장감 조절
  - 빠름: 일상 대화, 행동
  - 느림: 감정적 순간, 반전

공백의 활용:
  아무것도 없는 화면 = 캐릭터의 침묵, 충격
  예) DDLC의 삭제 연출 → 텍스트 없음 = 강력한 연출

텍스트 스타일링:
  굵게 = 강조/충격  |  기울임 = 내면의 목소리  |  [색상] = 캐릭터 색상 코딩

사운드 트리거:
  텍스트 출력 중 SFX → 몰입도 극적으로 상승
  예) 총성 묘사 + 총소리 SFX 동시 출력
```

### 스킬/스탯 시스템 (텍스트 RPG)

```python
# Disco Elysium 스타일 스킬 체크
import random

SKILLS = {
    'intellect':  ['logic', 'encyclopedia', 'rhetoric', 'drama', 'conceptualization'],
    'psyche':     ['volition', 'inland_empire', 'empathy', 'authority', 'suggestion'],
    'physique':   ['endurance', 'pain_threshold', 'physical_instrument', 'electrochemistry'],
    'motorics':   ['hand_eye_coordination', 'perception', 'reaction_speed', 'composure'],
}

def skill_check(skill_level: int, difficulty: int) -> dict:
    """
    2d6 + 스킬 vs 난이도
    스킬 판정이 단순 성공/실패가 아니라 정도(degree)가 있음
    """
    roll = random.randint(1, 6) + random.randint(1, 6)
    total = roll + skill_level
    margin = total - difficulty
    
    return {
        'roll': roll,
        'total': total,
        'difficulty': difficulty,
        'result': 'success' if margin >= 0 else 'failure',
        'margin': margin,
        # 실패도 재밌는 결과로 — 단순 "안 됨"이 아님
        'flavor': '압도적 성공' if margin >= 4 else
                  '성공' if margin >= 0 else
                  '아슬아슬 실패' if margin >= -3 else '완전한 실패',
    }
```

### 선택-결과 추적 시스템

```python
# 선택의 장기 영향 추적
class NarrativeFlags:
    """
    선택지의 결과를 플래그로 관리
    단기 결과 + 장기 결과가 분리됨 (위쳐 스타일)
    """
    def __init__(self):
        self.flags: dict[str, any] = {}
        self.history: list[dict] = []
    
    def set(self, key: str, value: any, context: str = ""):
        self.flags[key] = value
        self.history.append({'flag': key, 'value': value, 'context': context})
    
    def check(self, condition: str) -> bool:
        # 조건 예시: "helped_merchant AND NOT betrayed_guild"
        # 간단 구현: 실제론 파서 필요
        return eval(condition.replace('AND', 'and').replace('NOT', 'not'),
                   {k: bool(v) for k, v in self.flags.items()})
    
    def get_ending_conditions(self) -> list[str]:
        """현재 플래그 상태로 도달 가능한 엔딩 목록"""
        return [key for key, condition in ENDING_CONDITIONS.items()
                if self.check(condition)]
```

### 대화 분기 설계 (복잡도 관리)

```
분기 복잡도 제어 원칙:

나비 효과 방지:
  선택 → 즉각 결과 + 장기 영향을 분리
  즉각 결과: 대사/장면 변화 (저비용)
  장기 영향: 플래그 설정 (나중에 반영)

수렴형 분기 (권장):
  여러 선택 → 같은 사건 도달 (방식만 다름)
  → 개발 비용 크게 절감

발산형 분기 (선택적):
  선택 → 완전히 다른 챕터
  → 볼륨이 N배 필요, 신중하게 사용

엔딩만 분기 (효율적):
  메인 스토리는 같되 결말부터 다름
  → 1인 개발에 최적
```

---

## 수치 템플릿

### 볼륨 가이드

```python
VOLUME_GUIDE = {
    'short_vn': {          # 단편 VN
        'words': '10,000~30,000자',
        'cg_scenes': '5~15장',
        'endings': '2~3개',
        'dev_time_solo': '1~3개월',
        'price': '$2.99~$7.99',
    },
    'standard_vn': {       # 표준 VN
        'words': '100,000~300,000자',
        'cg_scenes': '30~100장',
        'endings': '4~8개',
        'dev_time_solo': '6~18개월',
        'price': '$9.99~$19.99',
    },
    'text_rpg': {          # 텍스트 RPG (Disco Elysium 급)
        'words': '500,000~1,000,000자',
        'dialogue_nodes': '10,000~30,000',
        'dev_time': '팀 필요 (2~5년)',
        'price': '$19.99~$39.99',
    },
    '1인_현실적': {
        'words': '30,000~80,000자',
        'cg_or_sprite': '10~30장 (AI 생성 활용)',
        'endings': '3~5개',
        'dev_time_solo': '3~8개월',
        'price': '$4.99~$12.99',
    },
}
```

---

## 아트 전략 (1인 개발 특화)

```
그래픽 없이도 퀄리티를 내는 방법:

옵션 A: 순수 텍스트 어드벤처
  → 그래픽 0 필요, 텍스트 연출로 모든 감정 전달
  → Twine, Ink, Ren'Py 등 도구 활용
  → Disco Elysium 초기 프로토타입이 이 방식

옵션 B: 미니멀 비주얼노벨
  → 캐릭터 스프라이트 (AI 생성 + 포즈 변형)
  → 배경은 단색 + 분위기 CG 5~10장만
  → Ren'Py (Python 기반, 무료)

옵션 C: 픽셀아트 텍스트 RPG (Undertale 모델)
  → 단순한 픽셀아트 + 텍스트 중심 스토리
  → 전투는 미니게임 형태 (부담 없음)
  → Godot 4 추천

AI 생성 이미지 활용 시:
  → 스타일 일관성이 핵심 (같은 프롬프트 베이스 유지)
  → 상업적 사용 가능한 라이센스 확인 (Midjourney 정책 변동 있음)
  → 배경/CG에 활용, 캐릭터는 직접 or 커뮤니션 권장
```

---

## Steam BM (텍스트 RPG / VN)

```
가격 전략:
  단편 (3~5시간):    $4.99~$7.99
  중편 (8~15시간):   $9.99~$14.99
  장편 (20시간+):    $14.99~$24.99

플랫폼 적합도:
  Steam: ★★★★☆ (인디 VN 커뮤니티 활발)
  모바일: ★★★★☆ (터치 조작 자연스러움)
  Switch: ★★★★☆ (포터블 + 스토리 게임 시장)

  → 크로스 플랫폼 가능성 높음

DLC 전략:
  사이드 스토리 DLC: 주인공이 아닌 캐릭터 시점
  OST + 아트북 번들
  시즌패스 (에피소드 분할 배포)

성공 사례:
  DDLC (Doki Doki Literature Club): 무료 출시 → DLC로 수익
  Ace Attorney 시리즈: 에피소드 분할로 초기 부담 낮춤
```

---

## 엔진 선택 (텍스트/VN 특화)

```
비주얼노벨 전용:
  Ren'Py   — 업계 표준, Python 기반, 무료
             Steam/모바일 모두 지원

범용 (텍스트 RPG):
  Godot 4  — 자유도 높음, 커스텀 UI 쉬움
  RPG Maker — 텍스트 RPG 전용 친화적 (but 엔진 느낌 강함)

텍스트 어드벤처 전용:
  Twine    — 완전 브라우저 기반, 가장 빠른 프로토타이핑
  Ink      — Unity/Godot 연동 가능한 내러티브 스크립팅 언어

권장:
  "순수 VN" → Ren'Py
  "텍스트 RPG with 전투" → Godot 4
  "프로토타입 빠르게" → Twine or Ink
```

---

## Edge Case (장르 특수)

1. **텍스트 피로**: 읽기가 지루해짐 → 장면 전환 빈도, CG 포인트, SFX로 리듬 조절
2. **선택지 무의미**: 어떤 선택도 결과 같음 → 즉각적인 차이 + 장기 플래그 두 가지 다 설계
3. **스킵 러셔**: 2회차 플레이어가 이미 읽은 내용 스킵 → Skip Seen / Auto 기능 필수
4. **엔딩 수렴 허무함**: "결국 같은 결말" → 과정의 차이가 충분히 의미있어야 함
5. **AI 이미지 저작권**: 상업 출시 시 라이센스 확인 필수 → Stable Diffusion 로컬 or CC0 에셋 고려
