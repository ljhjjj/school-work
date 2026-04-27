
# [부록] 5-1-2: 매개변수 변환법 공식 유도 (Derivation of Variation of Parameters)

본 유도 과정은 상수 계수가 아닌 일반적인 2계 선형 비제차 미분방정식 $y'' + p(t)y' + q(t)y = r(t)$ 의 특수해 $Y_p$를 구하는 매개변수 변환법의 공식 도출 과정입니다.

## 1. 해의 형태 가정

제차(Homogeneous) 미분방정식 $y'' + p(t)y' + q(t)y = 0$ 의 일반해가 $y_h = C_1 y_1 + C_2 y_2$ 라고 할 때, 상수를 $t$에 대한 함수 $u_1(t), u_2(t)$로 변환하여 특수해 $Y_p$를 다음과 같이 가정합니다.

$$Y_p = u_1 y_1 + u_2 y_2$$

## 2. 도함수 도출 및 제약 조건 (Constraint) 부여

위 식을 미분하여 $Y_p'$를 구합니다.

$$Y_p' = u_1' y_1 + u_1 y_1' + u_2' y_2 + u_2 y_2'$$

이 도함수를 다시 한 번 미분해서 $Y_p''$를 구하면 수식이 너무 길고 복잡해집니다. 이를 방지하고 미지수 2개($u_1, u_2$)를 풀기 위한 조건 2개를 맞추기 위해, 임의의 **제약 조건(Constraint**)을 하나 부여합니다.

$$\mathbf{u_1' y_1 + u_2' y_2 = 0} \quad \cdots (A)$$

이 제약 조건에 의해 1계 도함수는 아주 간단해집니다.

$$Y_p' = u_1 y_1' + u_2 y_2'$$

이제 이를 한 번 더 미분하여 2계 도함수를 구합니다.

$$Y_p'' = u_1' y_1' + u_1 y_1'' + u_2' y_2' + u_2 y_2''$$

## 3. 원래 미분방정식에 대입 및 식 정리

구해진 $Y_p, Y_p', Y_p''$를 원래의 비제차 미분방정식 $Y_p'' + p(t)Y_p' + q(t)Y_p = r(t)$ 에 대입합니다.

$$[u_1' y_1' + u_1 y_1'' + u_2' y_2' + u_2 y_2''] + p(t)[u_1 y_1' + u_2 y_2'] + q(t)[u_1 y_1 + u_2 y_2] = r(t)$$

이 식을 $u_1$과 $u_2$를 기준으로 묶어서 정리합니다.

$$u_1 [y_1'' + p(t)y_1' + q(t)y_1] + u_2 [y_2'' + p(t)y_2' + q(t)y_2] + u_1' y_1' + u_2' y_2' = r(t)$$

여기서 대괄호 안의 식들은 제차 미분방정식의 해 $y_1, y_2$에 대한 식이므로 각각 $0$이 됩니다. 따라서 남은 식은 다음과 같습니다.

$$\mathbf{u_1' y_1' + u_2' y_2' = r(t)} \quad \cdots (B)$$

## 4. 행렬 방정식 구성 및 론스키안(Wronskian) 적용

과정 2에서 세운 제약 조건 $(A)$와 방금 구한 식 $(B)$를 연립방정식으로 묶어 행렬 형태로 표현합니다.

$$\begin{bmatrix} y_1 & y_2 \\ y_1' & y_2' \end{bmatrix} \begin{bmatrix} u_1' \\ u_2' \end{bmatrix} = \begin{bmatrix} 0 \\ r(t) \end{bmatrix}$$

미지수 $u_1', u_2'$를 구하기 위해 역행렬을 곱해줍니다. 이때 행렬식은 두 함수 $y_1, y_2$의 론스키안 $W$와 같습니다. ($W = y_1 y_2' - y_2 y_1'$)

$$\begin{bmatrix} u_1' \\ u_2' \end{bmatrix} = \frac{1}{W} \begin{bmatrix} y_2' & -y_2 \\ -y_1' & y_1 \end{bmatrix} \begin{bmatrix} 0 \\ r(t) \end{bmatrix} = \frac{1}{W} \begin{bmatrix} -r(t) y_2 \\ r(t) y_1 \end{bmatrix}$$

따라서 각각의 도함수는 다음과 같습니다.

- $u_1' = -\frac{r(t) y_2}{W}$
    
- $u_2' = \frac{r(t) y_1}{W}$
    

## 5. 적분 및 최종 특수해 도출

위에서 구한 $u_1', u_2'$를 각각 $t$에 대해 적분하여 $u_1(t), u_2(t)$를 구합니다.

- $u_1(t) = -\int \frac{r(t) y_2}{W} dt + C_3$
    
- $u_2(t) = \int \frac{r(t) y_1}{W} dt + C_4$
    

이를 처음 가정했던 특수해 식 $Y_p = u_1 y_1 + u_2 y_2$에 대입합니다.

$$Y_p = y_1 \left( -\int \frac{r(t) y_2}{W} dt + C_3 \right) + y_2 \left( \int \frac{r(t) y_1}{W} dt + C_4 \right)$$

$$Y_p = -y_1 \int \frac{r(t) y_2}{W} dt + y_2 \int \frac{r(t) y_1}{W} dt + C_3 y_1 + C_4 y_2$$

최종적으로 일반해 $Y = C_1 y_1 + C_2 y_2 + Y_p$를 구성할 때, 꼬리에 붙어있는 $C_3 y_1 + C_4 y_2$는 제차 일반해 부분($C_1 y_1 + C_2 y_2$)과 완전히 겹치므로 흡수되어 사라집니다. 따라서 적분 상수를 무시한 최종 특수해 공식은 다음과 같습니다.

$$\mathbf{Y_p = -y_1 \int \frac{r(t) y_2}{W} dt + y_2 \int \frac{r(t) y_1}{W} dt}$$
