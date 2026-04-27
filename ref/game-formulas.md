> 📐 **이 파일의 공식과 코드는 설계 참고용 수치 계산 도구다 — 게임 구현 코드가 아님.**
> 공식은 그대로 사용 가능. 코드는 수치 검증 목적으로만 사용.
> 실제 게임 내 전투/경제 시스템 구현은 엔진 스킬에서 생성.

# Ref: 게임 수식 & 공식 라이브러리

게임 시스템 설계에서 반복적으로 사용되는 수학적 공식 모음.

---

## 1. 전투 공식

### 기본 데미지 공식 변형
```
# 1. 단순 감산형 (단순, 극단적 방어력 문제 있음)
DMG = ATK - DEF

# 2. 비율형 (방어력이 퍼센트로 작동)
DMG = ATK × (1 - DEF_RATE)   # DEF_RATE: 0~0.75

# 3. 분모형 (WoW 스타일, 극단값 완화)
DMG = ATK × (ATK / (ATK + DEF))

# 4. 로그형 (매우 안정적, 후반 방어력 인플레 방지)
import math
DMG = ATK × (1 - math.log(1 + DEF) / math.log(1 + DEF + 1000))
```

### 치명타 시스템
```python
def crit_damage(base_dmg, crit_rate, crit_multi=1.5):
    """
    crit_rate: 0~1 (최대 75% 권장)
    crit_multi: 치명타 배율 (기본 150%)
    """
    avg_multiplier = 1 + crit_rate * (crit_multi - 1)
    return base_dmg * avg_multiplier

# 예: 치명률 40%, 배율 200%
print(crit_damage(1000, 0.4, 2.0))  # → 1400
```

---

## 2. 경험치 & 레벨 공식

### 누적 경험치 곡선
```python
# 각 레벨에 필요한 경험치
def exp_required(level, base=100, exponent=1.5):
    """
    base: 1→2레벨 필요 경험치
    exponent: 성장 가속도 (1.3~1.8 권장)
    """
    return int(base * (level ** exponent))

# 레벨별 필요 경험치 출력
for lv in range(1, 11):
    print(f"Lv {lv}→{lv+1}: {exp_required(lv):,} EXP")
```

---

## 3. 확률 & 기댓값

### 기본 확률 공식
```python
import math

def expected_tries(probability):
    """특정 확률의 이벤트가 1회 발생하기까지 기댓값"""
    return 1 / probability

def prob_at_least_once(probability, tries):
    """n번 시도 시 1번 이상 성공 확률"""
    return 1 - (1 - probability) ** tries

def tries_for_confidence(probability, confidence=0.9):
    """confidence% 확률로 1번 이상 성공하기 위한 시도 수"""
    return math.ceil(math.log(1 - confidence) / math.log(1 - probability))

# 예: 확률 1% 아이템을 90% 확률로 얻기 위한 시도 수
print(tries_for_confidence(0.01, 0.9))  # → 230회
```

### 강화 총 비용 기댓값 (실패 시 소모 포함)
```python
def enhancement_cost(success_rate, material_cost, fail_penalty=0.5):
    """
    success_rate: 강화 성공 확률
    material_cost: 1회 강화 재료 비용
    fail_penalty: 실패 시 재료 소모 비율 (0=소모 없음, 1=전량 소모)
    """
    # 기댓값 시도 횟수
    expected_n = 1 / success_rate
    # 성공 1회 + 실패 (expected_n - 1)회의 비용
    total_cost = material_cost + (expected_n - 1) * material_cost * fail_penalty
    return {
        'expected_tries': round(expected_n, 1),
        'total_cost': round(total_cost),
        'success_cost': material_cost,
        'failure_cost': round((expected_n - 1) * material_cost * fail_penalty),
    }
```

---

## 4. 경제 시스템

### 인플레이션 감지 지표
```python
def inflation_check(gold_in_per_day, gold_out_per_day, total_gold):
    """
    gold_in > gold_out → 인플레이션 위험
    인플레이션율 = (순유입 / 총통화량) × 100
    """
    net_flow = gold_in_per_day - gold_out_per_day
    inflation_rate = (net_flow / total_gold) * 100
    return {
        'net_daily_flow': net_flow,
        'inflation_rate_%': round(inflation_rate, 2),
        'status': '위험' if inflation_rate > 1.0 else '정상'
    }
```

### Faucet-Sink 균형 계산
```
목표: Sink/Faucet 비율 = 0.9 ~ 1.1 (±10%)

Faucet = 일일 총 재화 생산량
Sink   = 일일 총 재화 소비량

비율 < 0.8: 디플레이션 (재화 부족 → 유저 이탈)
비율 > 1.2: 인플레이션 (재화 과잉 → 과금 의미 감소)
```

---

## 5. 리텐션 수학

### Day N 리텐션 예측 모델
```python
def retention_curve(day1, decay_rate=0.85):
    """
    day1: Day 1 리텐션 (예: 0.40)
    decay_rate: 일별 유지율 (0.80~0.90 권장)
    """
    curve = {}
    for day in [1, 3, 7, 14, 30, 60, 90]:
        curve[f'D{day}'] = round(day1 * (decay_rate ** (day - 1)) * 100, 1)
    return curve

# 예: D1=40%, 감소율 85%
print(retention_curve(0.40, 0.85))
```

### LTV (생애 가치) 계산
```
LTV = ARPU_daily × (1 / churn_rate_daily)
churn_rate_daily = 1 - (D30_retention)^(1/29)

예: ARPU=100원/일, D30=5%
→ daily_churn ≈ 0.10
→ LTV = 100 / 0.10 = 1,000원
```

---

## 6. NPC AI 수식

### 유틸리티 기반 AI 점수 계산
```python
def utility_ai(agent, targets):
    """
    유틸리티 AI: 여러 행동 중 점수 최고 행동 선택
    """
    def score_attack(target):
        distance_score = 1 / (1 + target['distance'])
        health_score = 1 - target['hp_ratio']  # 체력 낮을수록 우선
        return distance_score * 0.4 + health_score * 0.6

    def score_flee(agent):
        return 1 - agent['hp_ratio']  # 내 체력 낮으면 도주

    scores = {
        'attack': max(score_attack(t) for t in targets) if targets else 0,
        'flee': score_flee(agent),
        'idle': 0.1,
    }
    return max(scores, key=scores.get), scores
```