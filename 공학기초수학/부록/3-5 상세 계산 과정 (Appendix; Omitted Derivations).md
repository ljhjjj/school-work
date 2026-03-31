# [부록] 3-5 상세 계산 과정 (Appendix: Omitted Derivations)

## 1. 특성방정식의 대수적 도출

$y = e^{\lambda t}$를 해로 가정했을 때, 왜 이차방정식이 나오는지에 대한 상세 과정입니다.

1. **가정 및 미분:**
    
    - $y = e^{\lambda t}$
        
    - $y' = \lambda e^{\lambda t}$
        
    - $y'' = \lambda^2 e^{\lambda t}$
        
2. **원래 식($y'' + ay' + by = 0$)에 대입:**
    
    $$\lambda^2 e^{\lambda t} + a(\lambda e^{\lambda t}) + b(e^{\lambda t}) = 0$$
    
3. **공통인수 $e^{\lambda t}$로 묶기:**
    
    $$(\lambda^2 + a\lambda + b) e^{\lambda t} = 0$$
    
4. **특성방정식 도출:**
    
    지수함수 $e^{\lambda t}$는 결코 $0$이 될 수 없으므로, 반드시 괄호 안의 식이 $0$이어야 합니다.
    
    $$\lambda^2 + a\lambda + b = 0$$
    

---

## 2. 계수 감소법(Reduction of Order) 상세 전개

중근($D=0$)일 때 두 번째 해 $y_2 = u e^{-\frac{a}{2}t}$를 찾아가는 과정입니다.

### 2.1 미분 과정 (곱의 미분법 적용)

$y_1 = e^{-\frac{a}{2}t}$라 할 때, $y_2 = u \cdot y_1$의 미분은 다음과 같습니다.

- **1계 미분 ($y_2'$):**
    
    $$y_2' = u' e^{-\frac{a}{2}t} - \frac{a}{2} u e^{-\frac{a}{2}t}$$
    
- **2계 미분 ($y_2''$):**
    
    $$y_2'' = \underbrace{u'' e^{-\frac{a}{2}t} - \frac{a}{2} u' e^{-\frac{a}{2}t}}_{u' e^{-\frac{a}{2}t} \text{ 미분}} - \underbrace{\left( \frac{a}{2} u' e^{-\frac{a}{2}t} - \frac{a^2}{4} u e^{-\frac{a}{2}t} \right)}_{\frac{a}{2} u e^{-\frac{a}{2}t} \text{ 미분}}$$
    
    $$y_2'' = u'' e^{-\frac{a}{2}t} - a u' e^{-\frac{a}{2}t} + \frac{a^2}{4} u e^{-\frac{a}{2}t}$$
    

### 2.2 방정식 대입 및 정리

$y_2'', a y_2', b y_2$를 각각 원래의 미분방정식에 대입합니다.

1. **대입 식:**
    
    $$\left( u'' e^{-\frac{a}{2}t} - a u' e^{-\frac{a}{2}t} + \frac{a^2}{4} u e^{-\frac{a}{2}t} \right) + a \left( u' e^{-\frac{a}{2}t} - \frac{a}{2} u e^{-\frac{a}{2}t} \right) + b \left( u e^{-\frac{a}{2}t} \right) = 0$$
    
2. **공통인수 $e^{-\frac{a}{2}t}$ 추출:**
    
    $$e^{-\frac{a}{2}t} \left[ u'' - \cancel{a u'} + \frac{a^2}{4} u + \cancel{a u'} - \frac{a^2}{2} u + b u \right] = 0$$
    
3. **상수항 정리:**
    
    $$e^{-\frac{a}{2}t} \left[ u'' + \left( b - \frac{a^2}{4} \right) u \right] = 0$$
    
4. **중근 조건($a^2 = 4b$) 적용:**
    
    $b - \frac{a^2}{4} = \frac{4b - a^2}{4}$이며, 판별식 $D=0$ 조건에 의해 이 값은 $0$이 됩니다.
    
    $$u'' = 0$$
    
5. **적분을 통한 $u$ 구하기:**
    
    $u'' = 0$을 두 번 적분하면 $u(t) = k_1 t + k_2$가 됩니다.
    
    가장 단순한 독립 해를 얻기 위해 $k_1 = 1, k_2 = 0$으로 두면 **$u = t$** 입니다.
    

**최종 결론:** 따라서 중근일 때의 두 번째 해는 **$y_2 = t e^{-\frac{a}{2}t}$** 가 됩니다.

---
