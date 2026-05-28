
# 5주차 실습 \- 상태 공간 방정식, Github

2020732071 전기공학과 오준서

# 실습 1

용수철\-질량\-댐퍼 시스템 (Mechanical System)

## Task 1\-1: 상태 공간 방정식 유도하기

![](https://kr.mathworks.com/help/examples/robust/win64/xxmass\_spring\_damper.png)


뉴턴의 제 2법칙을 적용하면 다음과 같다.


**시스템 파라미터**

-  질량 (m): 1 kg 
-  댐핑 계수 (b): 2 Ns/m 
-  용수철 상수 (k): 5 N/m 
-  입력 r(t): Unit Step (1N의 힘) 
-  출력 y(t): 질량의 위치 

**1단계 : 뉴턴의 제2법칙 적용**


시스템에 작용하는 힘들을 분석하면

-  입력 힘: r(t) 
-  용수철 복원력: \-k·y(t) = \-5y(t) 
-  댐핑 저항력: \-b·dy/dt = \-2ẏ(t) 

이제 식으로 변환하면 다음과 같다.

 $$ m\cdot \frac{d^2 y}{{\mathrm{dt}}^2 }=r\left(t\right)-k\cdot y\left(t\right)-b\cdot \frac{\mathrm{dy}}{\mathrm{dt}} $$ 

위에서 작성한 파라미터를 적용하면,

 $$ 1\cdot \ddot{y} +2\cdot \dot{y} +5\cdot y=r\left(t\right) $$ 

**2단계: state variables 정의**


**state variables**를 다음과 같이 정의하겠다.

-  x₁ = y(t): 위치 
-  x₂ = ẏ(t): 속도 

**3단계: 상태방정식 유도**


각 상태변수(state variable)의 미분을 구하면 다음과 같다.

 $$ \begin{array}{l} \dot{x_1 } =\frac{{\mathrm{dx}}_1 }{\mathrm{dt}}=\frac{\mathrm{dy}}{\mathrm{dt}}=\dot{y} =x_2 \newline \dot{x_2 } =\frac{{\mathrm{dx}}_2 }{\mathrm{dt}}=\frac{\dot{\mathrm{dy}} }{\mathrm{dt}}=\ddot{y}  \end{array} $$ 

지금까지의 계산과정을 모두 통합하면, 다음과 같은 결론이 나온다.

-  $\displaystyle \dot{x_1 } =x_2$ 
-  $\displaystyle \dot{x_2 } =-5x_1 -2x_2 +r\left(t\right)$ 

**4단계: 시스템 행렬 도출**


상태공간 표현 ẋ = Ax + Br, y = Cx + Dr을 matrix로 표현하겠다.

 $$ A=\left\lbrack \begin{array}{cc} 0 & 1\newline -5 & -2 \end{array}\right\rbrack ,B=\left\lbrack \begin{array}{c} 0\newline 1 \end{array}\right\rbrack ,C=\left\lbrack \begin{array}{cc} 1 & 0 \end{array}\right\rbrack ,D=\left\lbrack \begin{array}{c} 0 \end{array}\right\rbrack $$ 
## Task 1\-2: MATLAB으로 시뮬레이션 및 분석하기
```matlab
% 시스템 파라미터 정의
M = 1;
b = 2;
k = 5;

% Task 1-1에서 유도한 A, B, C, D 행렬을 여기에 입력하세요.
A = [0 1; -k/M -b/M];
B = [0; 1/M];
C = [1 0]; % 출력은 위치(y)이므로
D = 0;

% 상태 공간 모델 생성
sys_mech1 = ss(A, B, C, D);

% Step 입력에 대한 응답 시뮬레이션 (0초부터 10초까지)
figure(1); % 새로운 그림 창 생성
step(sys_mech1, 10);
title('Step Response of Spring-Mass-Damper System');
xlabel('Time (seconds)');
ylabel('Position (m)');
grid on;
```

![figure_0.png](./practice_media/figure_0.png)

-  시간이 지남에 따라, Mass는 damping하면서 점차 position 0.2m로 수렴하여, 결국 시간이 지나면 정지한다. 
-  즉, 이 그래프의 최종값(position\[m\])은 0.2이며, 이는 mass가 정지하는 위치를 의미한다. 

*`* 댐핑 계수 b의 값을 0.5 (Underdamped), 10 (Overdamped)으로 바꾸어 실행하고, 그래프의 변화를 비교 확인하세요.`*

```matlab
% Underdamped Test
M = 1;
b = 0.5; %underdamped
k = 5;

% Task 1-1에서 유도한 A, B, C, D 행렬을 여기에 입력하세요.
A = [0 1; -k/M -b/M];
B = [0; 1/M];
C = [1 0]; % 출력은 위치(y)이므로
D = 0;

% 상태 공간 모델 생성
sys_mech1 = ss(A, B, C, D);

% Overdamped Test
M = 1;
b = 10; %overdamped
k = 5;

% Task 1-1에서 유도한 A, B, C, D 행렬을 여기에 입력하세요.
A = [0 1; -k/M -b/M];
B = [0; 1/M];
C = [1 0]; % 출력은 위치(y)이므로
D = 0;

% 상태 공간 모델 생성
sys_mech2 = ss(A, B, C, D);

% Step 입력에 대한 응답 시뮬레이션 (0초부터 10초까지)
figure(1); % 새로운 그림 창 생성
step(sys_mech1, 10);
hold on;
step(sys_mech2, 10);
hold off; % 이전 그래프를 지우고 새로운 그래프를 그리기 위해 hold off
legend('Underdamped', 'Overdamped'); % 범례 추가
title('Step Response of Spring-Mass-Damper System');
xlabel('Time (seconds)');
ylabel('Position (m)');
grid on;
```

![figure_1.png](./practice_media/figure_1.png)

[Robust Tuning of Mass\-Spring\-Damper System](https://kr.mathworks.com/help/robust/gs/robust-tuning-of-mass-spring-damper-system.html)


위 Mathworks 공식 예제를 참고하여, 문제 풀이를 진행하였습니다.

# 실습 2

R\-L\-C 회로 시스템 (Electrical System)

## Task 2\-1: 상태 공간 방정식 유도하기

![](https://upload.wikimedia.org/wikipedia/commons/thumb/7/75/RL\_series\_C\_parallel.svg/250px\-RL\_series\_C\_parallel.svg.png)


**회로 파라미터 정의**

-  저항 R = 3 Ω 
-  인덕터 L = 1 H 
-  캐패시터 C = 0.5 F 
-  입력 u(t): Unit Step 전류 소스 (1A) 
-  출력 y(t): 저항 R 양단의 전압 

**state variables settings**

 $$ x_1 \left(t\right)=v_c \left(t\right),x_2 \left(t\right)=i_L \left(t\right) $$ 

전체 에너지를 계산하는 방정식은 다음과 같다.

 $$ E=\frac{1}{2}{\mathrm{Li}}_L^2 \left(t\right)+\frac{1}{2}{\mathrm{Cv}}_C^2 \left(t\right) $$ 

이 때, Kirchhoff's current law에 따르면,


 $i_C \left(t\right)=C\frac{{\mathrm{dv}}_C \left(t\right)}{\mathrm{dt}}=+u\left(t\right)-i_L \left(t\right)$ 이고,


마찬가지로, Kirchhof's voltage law에 따르면,


 $L\frac{{\mathrm{di}}_L \left(t\right)}{\mathrm{dt}}=-{\mathrm{Ri}}_L \left(t\right)+v_c \left(t\right)$ 이다.


마지막으로, 이 시스템 내에서의 $v_o \left(t\right)={\mathrm{Ri}}_L \left(t\right)$ 임을 확인할 수 있다.


**state space equation**


이제, 위에서의 n차 differential equaitons를 state variables를 활용하여 1st order differential equation(state space equation)으로 변환한다.


다음과 같다.

 $$ \begin{array}{l} \frac{{\mathrm{dx}}_1 \left(t\right)}{\mathrm{dt}}=-\frac{1}{C}x_2 \left(t\right)+\frac{1}{C}u\left(t\right)\newline &\newline \frac{{\mathrm{dx}}_2 \left(t\right)}{\mathrm{dt}}=+\frac{1}{L}x_1 \left(t\right)-\frac{R}{L}x_2 \left(t\right)\newline &\newline y_1 \left(t\right)=v_o \left(t\right)={\mathrm{Rx}}_2 \left(t\right) \end{array} $$ 


**more variables**


위에서 얻은 변수 외의 추가 변수를 생성한다. 이는 일차 미분방정식의 집합을 만들기 위한 과정이다. (궁극적으로 MATLAB에 matrix를 만들어 계산할 것이기 때문이다.)

 $$ x_1^* \left(t\right)=v_C \left(t\right)=x_1 \left(t\right)\;&\;x_2^* \left(t\right)=v_L \left(t\right)=v_C \left(t\right)-{\mathrm{Ri}}_L \left(t\right)=x_1 \left(t\right)-{\mathrm{Rx}}_2 \left(t\right) $$ 


**matrix**


이제 총 네 개의 state variables를 활용하여 일차 미분 방정식(Matrix 형태)을 만들면 된다.

 $$ \dot{\mathit{\mathbf{x}}} \left(\mathit{\mathbf{t}}\right)=\mathbf{Ax}\left(\mathit{\mathbf{t}}\right)+\mathbf{Bu}\left(\mathit{\mathbf{t}}\right) $$ 

 $$ \mathit{\mathbf{y}}\left(\mathit{\mathbf{t}}\right)=\mathbf{Cx}\left(\mathit{\mathbf{t}}\right)+\mathbf{Du}\left(\mathit{\mathbf{t}}\right) $$ 

 $ $ \begin{array}{l} \dot{\mathit{\mathbf{x}}} \left(t\right)=\left\lbrack \begin{array}{cc} 0 & -\frac{1}{C}\newline \frac{1}{L} & -\frac{R}{L} \end{array}\right\rbrack \mathit{\mathbf{x}}\left(t\right)+\left\lbrack \begin{array}{c} \frac{1}{C}\newline 0 \end{array}\right\rbrack u\left(t\right)\\
&\\
y\left(t\right)=\left\lbrack \begin{array}{cc} 0 & R \end{array}\right\rbrack \mathit{\mathbf{x}}\left(t\right)
\end{array} $ $ 

## Task 2\-2: MATLAB으로 시뮬레이션 및 분석하기
```matlab
% 시스템 파라미터 정의
R = 3;
L = 1;
C = 0.5;

% Task 2-1에서 유도한 A, B, C, D 행렬을 여기에 입력하세요.
A = [0 -1/C; 1/L -R/L];
B = [1/C; 0];
C = [0 R]; % 출력은 vo(t) = R*iL(t) = R*x2 이므로
D = 0;

% 상태 공간 모델 생성
sys_elec1 = ss(A, B, C, D);

% Step 입력에 대한 응답 시뮬레이션 (0초부터 10초까지)
figure(2); % 두 번째 그림 창 생성
step(sys_elec1, 10);
title('Step Response of RLC Circuit System');
xlabel('Time (seconds)');
ylabel('Output Voltage Vo(t) (Volts)');
grid on;
```

![figure_2.png](./practice_media/figure_2.png)

-  Resistance에 걸리는 전압인 $v_o \left(t\right)$ 는 서서히 Log Scale로 0에서 증가하여 3V로 수렴한다. 
-  전압이 안정되기까지 약 6초의 시간이 소요된다. 


 *`*`*  *저항* *`R`* *의 값을* *`1`* *과* *`10`**으로 바꾸어 실행하고, 응답 속도와 최종 전압 값이 어떻게 변하는지 확인하세요.*

```matlab
% Resistance = 1
R = 1;
L = 1;
C = 0.5;

% Task 2-1에서 유도한 A, B, C, D 행렬을 여기에 입력하세요.
A = [0 -1/C; 1/L -R/L];
B = [1/C; 0];
C = [0 R]; % 출력은 vo(t) = R*iL(t) = R*x2 이므로
D = 0;

% 상태 공간 모델 생성
sys_elec1 = ss(A, B, C, D);

% Resistance = 10
R = 10;
L = 1;
C = 0.5;

% Task 2-1에서 유도한 A, B, C, D 행렬을 여기에 입력하세요.
A = [0 -1/C; 1/L -R/L];
B = [1/C; 0];
C = [0 R]; % 출력은 vo(t) = R*iL(t) = R*x2 이므로
D = 0;

% 상태 공간 모델 생성
sys_elec2 = ss(A, B, C, D);

% Step 입력에 대한 응답 시뮬레이션 (0초부터 10초까지)
figure(2); % 두 번째 그림 창 생성
step(sys_elec1, 10);
hold on;

step(sys_elec2, 10);
hold off;
legend('Resistance = 1', 'Resistance = 10'); % 범례 추가
title('Step Response of RLC Circuit System');
xlabel('Time (seconds)');
ylabel('Output Voltage Vo(t) (Volts)');
grid on;
```

![figure_3.png](./practice_media/figure_3.png)
