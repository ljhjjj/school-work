### MATLAB 환경 (MATLAB Environment)

|**명령어 / 구문**|**설명**|
|---|---|
|`clc`|커맨드 창 지우기|
|`help fun`|`fun`에 대한 인라인 도움말 표시|
|`doc fun`|`fun`에 대한 문서 열기|
|`load("filename","vars")`|`.mat` 파일에서 변수 불러오기|
|`uiimport("filename")`|대화형 데이터 가져오기 도구 열기|
|`save("filename","vars")`|파일에 변수 저장하기|
|`clear item`|작업 공간(Workspace)에서 항목 제거하기|
|`examplescript`|이름이 `examplescript`인 스크립트 파일 실행|
|`format style`|출력 표시 형식 설정|
|`ver`|설치된 툴박스 목록 확인|
|`tic, toc`|타이머 시작 및 중지|
|`Ctrl+C`|현재 진행 중인 계산 중단|

### 연산자 및 특수 문자 (Operators and Special Characters)

|**기호 / 연산자**|**설명**|
|---|---|
|`+, -, *, /`|행렬 수학 연산|
|`.*, ./`|배열 곱셈 및 나눗셈 (요소별 연산)|
|`^, .^`|행렬 및 배열 거듭제곱|
|`\`|왼쪽 나눗셈 또는 선형 최적화|
|`.', '`|일반 전치 및 복소공액 전치|
|`==, ~=, <, >, <=, >=`|관계 연산자|
|`&&, \|, ~, xor`|논리 연산자 (AND, OR, NOT, XOR)|
|`;`|출력 표시 억제|
|`...`|줄 연결 (줄 바꿈 시 코드를 이어줌)|
|`%`|주석 작성|
|`'Hello'`|문자 벡터(Character vector) 정의|
|`"This is a string"`|문자열(String) 정의|
|`str1 + str2`|문자열 연결|

### 배열 변수 정의 및 변경 (Defining and Changing Array Variables)

| **문법**                                             | **설명**                                                            |
| -------------------------------------------------- | ----------------------------------------------------------------- |
| `a = 5`                                            | 값이 5인 변수 `a` 정의                                                   |
| `A = [1 2 3; 4 5 6]`<br><br>`A = [1 2 3 \n 4 5 6]` | `A`를 $2 \times 3$ 행렬로 정의<br><br>(공백은 열을 구분하고, `;` 또는 줄 바꿈은 행을 구분) |
| `[A,B]`                                            | 배열을 수평으로 결합                                                       |
| `[A;B]`                                            | 배열을 수직으로 결합                                                       |
| `x(4) = 7`                                         | `x`의 4번째 요소를 7로 변경                                                |
| `A(1,3) = 5`                                       | 행렬 `A`의 (1,3) 요소를 5로 변경                                           |
| `x(5:10)`                                          | `x`의 5번째부터 10번째 요소 가져오기                                           |
| `x(1:2:end)`                                       | `x`의 첫 번째부터 마지막까지 2칸 간격으로 요소 가져오기                                 |
| `x(x>6)`                                           | 6보다 큰 요소 나열                                                       |
| `x(x==10)=1`                                       | 조건을 만족하는(10과 같은) 요소를 1로 변경                                        |
| `A(4,:)`                                           | `A`의 4번째 행 가져오기                                                   |
| `A(:,3)`                                           | `A`의 3번째 열 가져오기                                                   |
| `A(6, 2:5)`                                        | `A`의 6번째 행에서 2~5번째 요소 가져오기                                        |
| `A(:,[1 7])=A(:,[7 1])`                            | 1번째 열과 7번째 열 교환                                                   |
| `a:b`                                              | $a$부터 $b$ 이하까지 1씩 증가하는 벡터 `[a, a+1, ..., a+n]` 생성                 |
| `a:ds:b`                                           | 간격이 `ds`인 일정한 간격의 벡터 생성                                           |
| `linspace(a,b,n)`                                  | $a$부터 $b$까지 $n$개의 동일한 간격 값을 가진 벡터 생성                              |
| `logspace(a,b,n)`                                  | $a$부터 $b$까지 $n$개의 로그 간격 값을 가진 벡터 생성                               |
| `zeros(m,n)`                                       | $m \times n$ 크기의 영행렬(0으로 채워짐) 생성                                  |
| `ones(m,n)`                                        | $m \times n$ 크기의 1행렬(1로 채워짐) 생성                                   |
| `eye(n)`                                           | $n \times n$ 크기의 단위 행렬(Identity matrix) 생성                        |
| `A=diag(x)`                                        | 벡터 `x`로부터 대각 행렬 생성                                                |
| `x=diag(A)`                                        | 행렬 `A`의 대각 요소 가져오기                                                |
| `meshgrid(x,y)`                                    | 2D 및 3D 그리드 생성                                                    |
| `rand(m,n), randi`                                 | 균일 분포 난수 또는 정수 생성                                                 |
| `randn(m,n)`                                       | 정규 분포 난수 생성                                                       |

### 특수 변수 및 상수 (Special Variables and Constants)

|**변수 / 상수**|**설명**|
|---|---|
|`ans`|가장 최근의 연산 결과 (Answer)|
|`pi`|원주율 $\pi = 3.141592654...$|
|`i, j, 1i, 1j`|허수 단위|
|`NaN, nan`|숫자가 아님 (Not a Number, 예: 0으로 나누기 결과)|
|`Inf, inf`|무한대 (Infinity)|
|`eps`|부동 소수점 상대 정밀도|

### 복소수 (Complex Numbers)

|**함수 / 기호**|**설명**|
|---|---|
|`i, j, 1i, 1j`|허수 단위|
|`real(z)`|복소수의 실수부 반환|
|`imag(z)`|복소수의 허수부 반환|
|`angle(z)`|위상각 반환 (라디안 단위)|
|`conj(z)`|요소별 복소공액 반환|
|`isreal(z)`|배열이 실수로만 이루어져 있는지 확인|

### 기본 수학 함수 (Elementary Functions)

|**함수 / 구문**|**설명**|
|---|---|
|`sin(x), asin`|사인 및 역사인 (라디안 단위)|
|`sind(x), asind`|사인 및 역사인 (도(degree) 단위)|
|`sinh(x), asinh`|쌍곡선 사인 및 역쌍곡선 사인 (라디안 단위)|
|`cos, tan, csc, sec, cot`|다른 삼각함수들에도 위와 동일한 규칙(`d`, `h` 접미사 등)이 적용됨|
|`abs(x)`|`x`의 절댓값, 복소수의 크기(Magnitude)|
|`exp(x)`|`x`의 지수 함수|
|`sqrt(x), nthroot(x,n)`|제곱근, 실수의 실수 `n`제곱근|
|`log(x)`|`x`의 자연 로그|
|`log2(x), log10`|각각 밑이 2와 10인 로그|
|`factorial(n)`|`n`의 팩토리얼|
|`sign(x)`|`x`의 부호|
|`mod(x,d)`|나눗셈 후 나머지 (모듈로 연산)|
|`ceil(x), fix, floor`|각각 `+inf`, `0`, `-inf` 방향으로 내림/올림 (올림, 0을 향해 버림, 내림)|
|`round(x)`|가장 가까운 소수점 또는 정수로 반올림|

### 테이블 (Tables)

| **함수 / 구문**                                    | **설명**                            |
| ---------------------------------------------- | --------------------------------- |
| `table(var1,...,varN)`                         | 변수 `var1, ..., varN`의 데이터로 테이블 생성 |
| `readtable("file")`                            | 파일에서 테이블 생성                       |
| `array2table(A)`                               | 숫자 배열을 테이블로 변환                    |
| `T.var`                                        | 변수 `var`에서 데이터 추출                 |
| `T(rows,columns)`<br>`T(rows,["col1","coln"])` | `T`에서 지정된 행과 열을 사용하여 새 테이블 생성     |
| `T.varname=data`                               | `T`의 (새로운) 열에 데이터 할당              |
| `T.Properties`                                 | `T`의 속성에 액세스                      |
| `categorical(A)`                               | 범주형 배열(Categorical array) 생성      |
| `summary(T), groupsummary`                     | 테이블 요약 정보 출력                      |
| `join(T1, T2)`                                 | 공통 변수가 있는 테이블 결합                  |

### 그래프 그리기 (Plotting)

| **함수 / 구문**                            | **설명**                                                               |
| -------------------------------------- | -------------------------------------------------------------------- |
| `plot(x,y,LineSpec)`                   | `x`에 대한 `y`의 그래프 생성 (`LineSpec`은 선택 사항)                              |
| `-, --, :, -.`                         | **선 스타일 (Line styles):** 실선, 파선, 점선, 1점 쇄선                           |
| `+, o, *, ., x, s, d`                  | **마커 (Markers):** 더하기, 원, 별표, 점, x표, 사각형(square), 다이아몬드              |
| `r, g, b, c, m, y, k, w`               | **색상 (Colors):** 빨강, 초록, 파랑, 청록, 자홍, 노랑, 검정, 흰색                      |
| _(LineSpec 예시)_                        | `LineSpec`은 선 스타일, 마커, 색상을 문자열로 결합한 것입니다. (예: `"-r"` = 마커 없는 빨간색 실선) |
| `title("Title")`                       | 그래프 제목 추가                                                            |
| `legend("1st", "2nd")`                 | 축에 범례 추가                                                             |
| `x/y/zlabel("label")`                  | x/y/z 축 레이블 추가                                                       |
| `x/y/zticks(ticksvec)`                 | x/y/z 축 눈금(ticks) 가져오기 또는 설정                                         |
| `x/y/zticklabels(labels)`              | x/y/z 축 눈금 레이블 가져오기 또는 설정                                            |
| `x/y/ztickangle(angle)`                | x/y/z 축 눈금 레이블 회전                                                    |
| `x/y/zlim`                             | x/y/z 축 표시 범위 가져오기 또는 설정                                             |
| `axis(lim), axis style`                | 축 제한 및 스타일 설정                                                        |
| `text(x,y,"txt")`                      | 텍스트 추가                                                               |
| `grid on/off`                          | 축 눈금선(그리드) 표시/숨기기                                                    |
| `hold on/off`                          | 새 그래프를 추가할 때 현재 그래프를 유지/해제                                           |
| `subplot(m,n,p)`<br>`tiledlayout(m,n)` | 바둑판식(타일형) 위치에 여러 축 생성                                                |
| `yyaxis left/right`                    | 두 번째 y축 생성                                                           |
| `figure`                               | 피규어(Figure) 창 생성                                                     |
| `gcf, gca`                             | 현재 피규어(Get current figure) 가져오기, 현재 축(Get current axis) 가져오기         |
| `clf`                                  | 현재 피규어 내용 지우기                                                        |
| `close all`                            | 열려 있는 모든 피규어 창 닫기                                                    |

### 일반적인 그래프 유형 (Common Plot Types)

다양한 그래프의 유형과 예시는 아래 링크의 Plot Gallery를 참조할 수 있습니다.

- **Plot Gallery:** [mathworks.com/products/matlab/plot-gallery](https://mathworks.com/products/matlab/plot-gallery)
    

### 라이브 편집기 작업 (Tasks - Live Editor)

라이브 편집기 작업(Task)은 특정 일련의 연산을 대화형으로 수행하기 위해 라이브 스크립트에 추가할 수 있는 앱과 같습니다. 이 작업들은 여러 줄의 MATLAB 명령어를 시각적으로 대체하며, 작업 내역이 어떤 코드로 실행되는지 확인하기 위해 생성된 코드를 직접 열어볼 수도 있습니다.

**데스크톱 툴스트립의 '라이브 편집기' 탭에서 사용할 수 있는 주요 작업:**

- `Clean Missing Data`: 결측 데이터 정리
    
- `Clean Outlier`: 이상값(Outlier) 정리
    
- `Find Change Points`: 데이터 변화점 찾기
    
- `Find Local Extrema`: 국소 극점(최대/최소값) 찾기
    
- `Remove Trends`: 데이터 추세 제거
    
- `Smooth Data`: 데이터 평활화(스무딩)


### 프로그래밍 방법 (Programming Methods)

| **분류**                          | **문법 / 코드 예시**                                                                                                                                                                                                                             | **설명**                                                                                      |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------- |
| **함수 (Functions)**              | `function cavg = cumavg(x)`<br><br>`cavg=cumsum(vec)./(1:length(vec));`<br><br>`end`                                                                                                                                                       | 여러 개의 인수(arguments) 사용 가능.<br><br>함수 파일이나 스크립트 파일의 끝에 저장하며, 함수 파일명은 반드시 첫 번째 함수 이름과 동일해야 함. |
| **익명 함수 (Anonymous Functions)** | `fun = @(x) cos(x.^2)./abs(3*x);`                                                                                                                                                                                                          | 함수 핸들(function handle, `@`)을 통해 정의됨.                                                        |
| **if, elseif 조건문**              | `if n<10`<br><br>`disp("n smaller 10")`<br><br>`elseif n<=20`<br><br>`disp("n between 10 and 20")`<br><br>`else`<br><br>`disp("n larger than 20")`                                                                                         | 조건에 따라 다른 코드를 실행함.<br><br>(_모든 제어 구조는 `end`로 끝남_)                                           |
| **Switch Case 문**               | `n = input("Enter an integer: ");`<br><br>`switch n`<br><br>`case -1`<br><br>`disp("negative one")`<br><br>`case {0,1,2,3}`<br><br>`disp("integer between 0 and 3")`<br><br>`otherwise`<br><br>`disp("value outside [-1,3]")`<br><br>`end` | 입력된 값에 따라 분기함. `{0,1,2,3}`처럼 여러 케이스를 한 번에 묶어서 확인 가능. 지정되지 않은 값은 `otherwise`로 처리.            |
| **For 루프**                      | `for i = 1:3`<br><br>`disp("cool");`<br><br>`end`                                                                                                                                                                                          | 특정 횟수만큼 반복하며, 증가하는 인덱스 변수(`i`)로 각 반복을 추적함.                                                  |
| **While 루프**                    | `n = 1; nFactorial = 1;`<br><br>`while nFactorial < 1e100`<br><br>`n = n + 1;`<br><br>`nFactorial = nFactorial * n;`<br><br>`end`                                                                                                          | 조건이 참(True)으로 유지되는 동안 계속해서 반복함.                                                             |

#### 기타 프로그래밍/제어 명령어 (Further programming/control commands)

|**명령어**|**설명**|
|---|---|
|`break`|`for` 또는 `while` 루프의 실행을 즉시 종료함|
|`continue`|루프의 다음 반복(iteration)으로 제어를 넘김|
|`try, catch`|명령문을 실행(`try`)하고, 실행 중 발생하는 오류를 포착(`catch`)함|

---

### 수치 해석 방법 (Numerical Methods)

|**함수 / 구문**|**설명**|
|---|---|
|`fzero(fun,x0)`|비선형 함수의 근(Root) 찾기|
|`fminsearch(fun,x0)`|함수의 최솟값(minimum) 찾기|
|`fminbnd(fun,x1,x2)`|`[x1, x2]` 구간 내에서 함수의 최솟값 찾기|
|`fft(x), ifft(x)`|고속 푸리에 변환(Fast Fourier transform) 및 그 역변환|

---

### 적분 및 미분 (Integration and Differentiation)

|**함수 / 구문**|**설명**|
|---|---|
|`integral(f,a,b)`|수치 적분 (2D 및 3D용 유사 함수도 존재함)|
|`trapz(x,y)`|사다리꼴(Trapezoidal) 수치 적분|
|`diff(X)`|차분(Differences) 및 근사 미분값|
|`gradient(X)`|수치적 기울기(Numerical gradient)|
|`curl(X,Y,Z,U,V,W)`|회전(Curl) 및 각속도|
|`divergence(X,..,W)`|벡터장의 발산(Divergence) 계산|
|`ode45(ode,tspan,y0)`|비경직성(Nonstiff) 상미분 방정식(ODE) 시스템 풀이|
|`ode15s(ode,tspan,y0)`|경직성(Stiff) 상미분 방정식(ODE) 시스템 풀이|
|`deval(sol,x)`|미분 방정식 해(solution)의 값 평가|
|`pdepe(m,pde,ic, bc,xm,ts)`|1차원 편미분 방정식(1D PDE) 풀이|
|`pdeval(m,xmesh, usol,xq)`|수치적 PDE 해 보간(Interpolate)|

---

### 보간법 및 다항식 (Interpolation and Polynomials)

| **함수 / 구문**             | **설명**                                                                |
| ----------------------- | --------------------------------------------------------------------- |
| `interp1(x,v,xq)`       | 1차원 보간 (2D 및 3D용 유사 함수 존재)                                            |
| `pchip(x,v,xq)`         | 조각별 3차 에르미트 다항식 보간 (Piecewise cubic Hermite polynomial interpolation) |
| `spline(x,v,xq)`        | 3차 스플라인 데이터 보간                                                        |
| `ppval(pp,xq)`          | 조각별 다항식 평가                                                            |
| `mkpp(breaks,coeffs)`   | 조각별 다항식 만들기                                                           |
| `unmkpp(pp)`            | 조각별 다항식 세부 정보 추출                                                      |
| `poly(x)`               | 지정된 근(roots) `x`를 갖는 다항식 생성                                           |
| `polyeig(A0,A1,...,Ap)` | 다항 고윳값 문제(Polynomial eigenvalue problem)의 고윳값 계산                      |
| `polyfit(x,y,d)`        | 다항식 곡선 피팅 (Polynomial curve fitting)                                  |
| `residue(b,a)`          | 부분 분수 전개/분해 (Partial fraction expansion/decomposition)                |
| `roots(p)`              | 다항식의 근(Roots) 구하기                                                     |
| `polyval(p,x)`          | 점 `x`에서 다항식 `p`의 값 평가                                                 |
| `polyint(p,k)`          | 다항식 적분                                                                |
| `polyder(p)`            | 다항식 미분                                                                |

### 행렬 및 배열 (Matrices and Arrays)

|**함수 / 구문**|**설명**|
|---|---|
|`length(A)`|배열의 가장 큰 차원의 길이|
|`size(A)`|배열의 차원(크기)|
|`numel(A)`|배열 내 전체 요소(원소)의 개수|
|`sort(A)`|배열 요소 정렬|
|`sortrows(A)`|배열 또는 테이블의 행 기준 정렬|
|`flip(A)`|배열 내 요소의 순서 뒤집기|
|`squeeze(A)`|길이가 1인 차원 제거|
|`reshape(A,sz)`|배열 크기 재구성 (Reshape)|
|`repmat(A,n)`|배열의 복사본을 만들어 반복|
|`any(A), all`|배열에 0이 아닌 요소가 하나라도 있는지(`any`) / 모두 0이 아닌지(`all`) 확인|
|`nnz(A)`|0이 아닌 배열 요소의 개수|
|`find(A)`|0이 아닌 요소의 인덱스 및 값 찾기|

### 선형 대수 (Linear Algebra)

|**함수 / 구문**|**설명**|
|---|---|
|`rank(A)`|행렬의 랭크(계수)|
|`trace(A)`|행렬 대각선 요소들의 합 (트레이스)|
|`det(A)`|행렬식 (Determinant)|
|`poly(A)`|행렬의 특성 다항식 (Characteristic polynomial)|
|`eig(A), eigs`|행렬의 고윳값(Eigenvalues) 및 고유벡터|
|`inv(A), pinv`|행렬의 역행렬 및 의사(Pseudo) 역행렬|
|`norm(x)`|벡터 또는 행렬의 노름(Norm)|
|`expm(A), logm(A)`|행렬 지수(Exponential) 및 행렬 로그|
|`cross(A,B)`|외적 (Cross product)|
|`dot(A,B)`|내적 (Dot product)|
|`kron(A,B)`|크로네커 텐서 곱 (Kronecker tensor product)|
|`null(A)`|행렬의 영공간 (Null space)|
|`orth(A)`|행렬 치역(Range)에 대한 정규 직교 기저|
|`tril(A), triu`|행렬의 하삼각(Lower) 및 상삼각(Upper) 부분|
|`linsolve(A,B)`|$AX=B$ 형태의 선형 시스템 풀이|
|`lsqminnorm(A,B)`|선형 방정식의 최소제곱해 (Least-squares solution)|
|`qr(A), lu, chol`|행렬 분해 (QR, LU, 숄레스키 분해)|
|`svd(A)`|특이값 분해 (Singular value decomposition)|
|`gsvd(A,B)`|일반화된 특이값 분해 (Generalized SVD)|
|`rref(A)`|기약 행 사다리꼴 (Reduced row echelon form)|

### 기술 통계 (Descriptive Statistics)

| **함수 / 구문**                                                                               | **설명**                                                                     |
| ----------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| `sum(A), prod`                                                                            | (열 기준) 합계 또는 곱                                                             |
| `max(A), min, bounds`                                                                     | 가장 큰 요소, 가장 작은 요소, 경계값                                                     |
| `mean(A), median, mode`                                                                   | 통계 연산 (평균, 중앙값, 최빈값)                                                       |
| `std(A), var`                                                                             | 표준편차 및 분산                                                                  |
| `movsum(A,n), movprod,`<br>`movmax, movmin,`<br>`movmean, movmedian,`<br>`movstd, movvar` | 이동 통계 함수 (Moving statistical functions)<br>* `n` = 이동 창(Moving window)의 길이 |
| `cumsum(A), cumprod,`<br>`cummax, cummin`                                                 | 누적 통계 함수 (Cumulative statistical functions)                                |
| `smoothdata(A)`                                                                           | 노이즈가 있는 데이터 평활화(스무딩)                                                       |
| `histcounts(X)`                                                                           | 히스토그램 구간(bin) 빈도수 계산                                                       |
| `corrcoef(A), cov`                                                                        | 상관계수, 공분산                                                                  |
| `xcorr(x,y), xcov`                                                                        | 상호 상관(Cross-correlation), 상호 공분산                                           |
| `normalize(A)`                                                                            | 데이터 정규화                                                                    |
| `detrend(x)`                                                                              | 다항식 추세 제거                                                                  |
| `isoutlier(A)`                                                                            | 데이터 내 이상치(Outlier) 찾기                                                      |

### 기호 수학 (Symbolic Math)*

_*이 기능들은 Symbolic Math Toolbox가 필요할 수 있습니다._

| **구문 / 연산**                                           | **설명**                      |
| ----------------------------------------------------- | --------------------------- |
| `sym x, syms x y z`                                   | 기호 변수(Symbolic variable) 선언 |
| `eqn = y == 2*a + b`                                  | 기호 방정식 정의                   |
| `solve(eqns,vars)`                                    | 특정 변수에 대해 기호 수식 풀이          |
| `subs(expr,var,val)`                                  | 수식 내 변수를 다른 값이나 기호로 치환(대입)  |
| `expand(expr)`                                        | 기호 수식 전개                    |
| `factor(expr)`                                        | 기호 수식 인수분해                  |
| `simplify(expr)`                                      | 기호 수식 단순화                   |
| `assume(var,assumption)`                              | 기호 변수에 대한 가정(조건) 설정         |
| `assumptions(z)`                                      | 기호 객체에 적용된 가정(조건) 확인        |
| `fplot(expr), fcontour,`<br>`fsurf, fmesh, fimplicit` | 기호 수식을 위한 다양한 그래프 그리기 함수    |
| `diff(expr,var,n)`                                    | 기호 수식 미분 (변수에 대해 `n`번 미분)   |
| `dsolve(deqn,cond)`                                   | 기호적으로 미분 방정식 풀이             |
| `int(expr,var,[a, b])`                                | `[a, b]` 구간에 대해 기호 수식 적분    |
| `taylor(fun,var,z0)`                                  | 특정 지점 `z0`에서 함수의 테일러 급수 전개  |