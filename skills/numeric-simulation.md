> 📊 **이 파일의 코드는 밸런싱 시뮬레이션 도구다 — 게임 구현 코드가 아님.**
> 개발자가 수치를 검증하기 위해 외부에서 실행하는 코드. 그대로 복사해서 사용 가능.
> 게임 내 가챠/강화 구현 코드가 필요하면 엔진 스킬(unity.skill / godot.skill)에서 생성.

# S-02: 수치 시뮬레이션

## 목적
밸런싱 수치를 엑셀 수식 또는 Python 코드로 즉시 시뮬레이션할 수 있게 제공한다.

---

## 핵심 공식 라이브러리

### 1. TTK (Time To Kill)
```
TTK = HP / DPS
DPS = (기본공격 × 공격횟수/초) + (스킬데미지 / 스킬쿨타임)
실효 HP = HP × (1 / (1 - 방어율))    # 방어율은 0~1
```

**엑셀 수식:**
```excel
=B2/(C2*(1-D2))   # B2=HP, C2=DPS, D2=방어율
```

---

### 2. 성장 곡선

| 유형 | 공식 | 특징 |
|------|------|------|
| 선형 | `Stat = Base + k × Level` | 예측 가능, 단조로움 |
| 지수 | `Stat = Base × r^Level` | 후반부 폭발적 성장 |
| S커브 | `Stat = Max / (1 + e^(-k(Level-mid)))` | 초반·후반 완만, 중반 가파름 |
| 로그 | `Stat = Base + k × ln(Level)` | 초반 빠른 성장 후 수렴 |

**Python — 4가지 성장 곡선 비교:**
```python
import numpy as np
import matplotlib.pyplot as plt

levels = np.arange(1, 101)
base, k, r, max_stat, mid = 100, 10, 1.05, 2000, 50

linear  = base + k * levels
exponential = base * (r ** levels)
sigmoid = max_stat / (1 + np.exp(-0.1 * (levels - mid)))
logarithmic = base + k * 50 * np.log(levels)

plt.figure(figsize=(10, 6))
for label, data in zip(
    ['Linear', 'Exponential', 'Sigmoid', 'Logarithmic'],
    [linear, exponential, sigmoid, logarithmic]
):
    plt.plot(levels, data, label=label)
plt.xlabel('Level'), plt.ylabel('Stat'), plt.legend(), plt.grid(True)
plt.title('Growth Curve Comparison')
plt.tight_layout()
plt.savefig('growth_curves.png', dpi=150)
```

---

### 3. 가챠 (Gacha) 확률 설계

**기댓값 계산:**
```
E[뽑기 수] = 1 / p
E[비용] = E[뽑기 수] × 1회당 비용
```

**Pity System (천장 보정):**
```python
def simulate_gacha(base_rate, pity_start, pity_cap, trials=100_000):
    """
    base_rate: 기본 확률 (예: 0.006 = 0.6%)
    pity_start: 확률 증가 시작 카운트
    pity_cap: 보장 뽑기 수 (천장)
    """
    results = []
    for _ in range(trials):
        count, rate = 0, base_rate
        while True:
            count += 1
            if count > pity_start:
                # 선형 증가: 천장까지 확률 점진적 상승
                rate = base_rate + (1.0 - base_rate) * (count - pity_start) / (pity_cap - pity_start)
            if np.random.random() < rate or count >= pity_cap:
                results.append(count)
                break
    return {
        'mean': np.mean(results),
        'median': np.median(results),
        'p90': np.percentile(results, 90),
        'p99': np.percentile(results, 99),
    }

# 사용 예 (원신 스타일)
result = simulate_gacha(base_rate=0.006, pity_start=74, pity_cap=90)
print(result)
```

---

### 4. 강화 시스템 성공률 기댓값

```python
def enhancement_expected_cost(rates: list[float], cost_per_try: int) -> dict:
    """
    rates: 각 단계 성공 확률 리스트 [+1:80%, +2:60%, ...]
    cost_per_try: 1회 강화 재화 비용
    반환: 각 단계 도달 기댓값 비용
    """
    expected = []
    cumulative = 0
    for i, p in enumerate(rates, 1):
        tries = 1 / p  # 이 단계 성공까지 기댓값 시도 횟수
        cost = tries * cost_per_try
        cumulative += cost
        expected.append({
            'stage': f'+{i}',
            'success_rate': f'{p*100:.1f}%',
            'avg_tries': round(tries, 1),
            'stage_cost': round(cost),
            'total_cost': round(cumulative),
        })
    return expected

# 사용 예
rates = [0.8, 0.6, 0.4, 0.2, 0.1, 0.05]
for row in enhancement_expected_cost(rates, cost_per_try=100):
    print(row)
```

---

### 5. PvP 밸런싱 — 가위바위보 삼각관계

```python
# 세 유형 상성 행렬 (행=공격자, 열=수비자)
# 1.0 = 균형, >1.0 = 공격자 유리
import numpy as np

counter_matrix = np.array([
    [1.0, 1.3, 0.7],  # 탱커 vs [탱, 딜, 힐]
    [0.7, 1.0, 1.3],  # 딜러 vs [탱, 딜, 힐]
    [1.3, 0.7, 1.0],  # 힐러 vs [탱, 딜, 힐]
])

def simulate_match(type_a, type_b, stat_a, stat_b):
    multiplier = counter_matrix[type_a][type_b]
    effective_dmg = stat_a * multiplier - stat_b * 0.5
    return max(effective_dmg, 0)
```

---

## 출력 원칙

- 공식 제시 → 엑셀 수식 → Python 코드 순서로 제공
- 수치는 항상 "왜 이 값인가" 근거(게임 디자인 의도) 포함
- 시뮬레이션 결과는 표 또는 그래프로 시각화