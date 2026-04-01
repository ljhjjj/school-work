# [부록]3-5-2: 계수 감소법(Reduction of Order) 상세 전개

중근($D=0$)일 때 두 번째 해 $y_2 = u e^{-\frac{a}{2}t}$를 찾아가는 과정입니다.

## 1. 미분 과정 (곱의 미분법 적용)

$y_1 = e^{-\frac{a}{2}t}$라 할 때, $y_2 = u \cdot y_1$의 미분은 다음과 같습니다.

- **1계 미분 ($y_2'$):**
    
    $$y_2' = u' e^{-\frac{a}{2}t} - \frac{a}{2} u e^{-\frac{a}{2}t}$$
    
- **2계 미분 ($y_2''$):**
    
    $$y_2'' = \underbrace{u'' e^{-\frac{a}{2}t} - \frac{a}{2} u' e^{-\frac{a}{2}t}}_{u' e^{-\frac{a}{2}t} \text{ 미분}} - \underbrace{\left( \frac{a}{2} u' e^{-\frac{a}{2}t} - \frac{a^2}{4} u e^{-\frac{a}{2}t} \right)}_{\frac{a}{2} u e^{-\frac{a}{2}t} \text{ 미분}}$$
    
    $$y_2'' = u'' e^{-\frac{a}{2}t} - a u' e^{-\frac{a}{2}t} + \frac{a^2}{4} u e^{-\frac{a}{2}t}$$
    

## 2. 방정식 대입 및 정리

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

