# 5주차 강의 요약
2020732071 전기공학과 오준서

## 목차
1. State란 무엇인가
2. Equation의 계산 : Human vs Computer
3. State Space Equation
4. SSE(State Space Equation)의 예시
5. 결론

### 1. State란 무엇인가
State(상태)는 동적 시스템의 현재 상황을 완전히 기술하는 최소한의 변수 집합이다. 제어 시스템에서 상태 변수(state variable)는 시스템의 내부 상태를 나타내며, 이전 상태와 현재 입력이 주어졌을 때 미래의 시스템 동작을 예측할 수 있도록 하는 중요한 개념이다.
![State space representation of a spring-mass-damper system with differential equations and matrix](https://img.youtube.com/vi/Z3zmuSelRKo/maxresdefault.jpg)

#### 상태의 정의 특성

**최소성과 완전성**: 시스템을 완전히 기술하는데 필요한 최소한의 변수 개수로, 현재 상태와 입력을 알면 미래 상태를 예측할 수 있다. 예를 들어, 전기 회로에서 커패시터의 전압과 인덕터의 전류가 상태 변수가 되고, 기계 시스템에서는 위치와 속도가 상태 변수가 된다.

**독립성**: 각 상태 변수는 서로 독립적이며 시스템의 내부 동역학을 나타내는 물리적 의미를 갖는다. 이는 시스템의 과거 이력에 대한 압축적 표현(compact representation)을 제공하여 시스템 분석을 효율적으로 만든다.

### 2. Equation의 계산 : Human vs Computer
컴퓨터는 1차 미분방정식의 행렬 형태를 통해 수치적으로 효율성을 극대화하는 반면, 인간은 고차 미분방정식의 해석적 해법에서 직관성과 창의성을 발휘한다.

#### 인간의 계산 방식의 특징
인간은 미분방정식을 풀 때 직관적 접근과 패턴 인식을 통한 창의적 해결을 추구한다. 문제의 의도를 파악하고 복잡한 문제를 단순한 부분으로 분해하며, 이전 경험을 바탕으로 유사한 문제 해결 방법을 적용한다. 그러나 고차 미분방정식의 해석해 도출 시 복잡한 계산 과정과 연산 실수 발생 가능성, 시간 소모적인 특성이 한계로 작용한다.

#### 컴퓨터의 계산 방식의 우위
컴퓨터는 수치적 근사와 행렬 연산을 통한 체계적 처리에 최적화되어 있다. 유한 차분법, 유한 요소법 등을 활용한 수치적 근사와, 1차 미분방정식 형태로 변환하여 행렬 계산으로 처리하는 방식을 통해 반복적이고 정확한 수치해를 도출한다. 특히 최근 AI 기반 방법론은 기존 슈퍼컴퓨터보다 빠른 속도로 복잡한 편미분방정식을 개인용 컴퓨터에서도 해결할 수 있게 되었다.

### 3. State Space Equation
State Space Equation은 동적 시스템을 1차 미분방정식들의 집합으로 표현하여 현대 제어 이론의 통합적 프레임워크를 제공한다. 이는 기존 전달함수 방법의 한계를 극복하고 MIMO 시스템, 비선형 시스템, 시변 시스템에 대한 통합적 접근을 가능하게 한다.
![State space equations illustrating system dynamics with matrices and vectors](https://img.youtube.com/vi/Ufcv_WLuKo4/maxresdefault.jpg "State space equations illustrating system dynamics with matrices and vectors")

#### State Space 표현의 일반 형태
예를 들어, 연속시간 선형시불변(LTI) 시스템의 상태공간 표현은 다음과 같다.

상태 방정식 (State Equation):
$$\dot{x}(t)=Ax(t)+Bu(t) $$

출력 방정식 (Output Equation):
$$y(t)=Cx(t)+Du(t)$$

여기서 A는 n×n 시스템 행렬, B는 n×m 입력 행렬, C는 p×n 출력 행렬, D는 p×m 직접 전달 행렬이다.

![Block diagram and state space equations with state variables and input-output relationships clearly shown ](https://img.youtube.com/vi/yW-N6Jx2-v0/maxresdefault.jpg "Block diagram and state space equations with state variables and input-output relationships clearly shown ")

#### State Space의 핵심 장점
**MIMO 시스템 처리**: 다입력-다출력 시스템을 자연스럽게 표현하여 복잡한 제어 시스템 설계가 가능하다. 비선형 시스템 확장: 비선형 시스템에도 적용 가능하여 실제 공학 시스템 모델링에 유리하다. 현대 제어 기법 지원: 상태 피드백, 관측기 설계, 최적 제어 등 현대 제어 이론의 핵심 기법들을 지원한다.

**컴퓨터 계산 최적화**: 행렬 연산을 통한 효율적 계산으로 대규모 시스템 분석이 가능하며, 특히 희소 행렬(sparse matrix) 표현을 통해 대규모 모델의 계산 효율성을 크게 향상시킬 수 있다.

#### MATLAB 구현코드
MATLAB에서 상태공간 모델을 생성하고 분석하는 기본 방법은 다음과 같다.

```
% 시스템 행렬 정의
A = [0 1; -2 -3];
B = [0; 1];
C = [1 0];
D = 0;

% 상태공간 모델 생성
sys = ss(A, B, C, D);

% 시스템 응답 분석
step(sys);        % 스텝 응답
impulse(sys);     % 임펄스 응답
bode(sys);        % 주파수 응답
```

### 4. SSE(State Space Equation)의 예시
![State Space Equation Example : Spring-Damper System](https://d2u1z1lopyfwlx.cloudfront.net/thumbnails/ff841091-e929-583e-8d4d-cc5e09e77728/fab81a65-2c50-5a4f-a0f6-a4279f631876.jpg "State Space Equation Example : Spring-Damper System")

교재 Example 3.1은 Spring-Mass-Damper System으로 구성된 복잡한 미분방정식 문제를 보여주고 있다.
보다 쉽게 설명하기 위해, 하나의 Mass만을 가지고 있는 System으로 예시를 들어보겠다.

#### 시스템 모델링과 상태 변수 선택
질량-스프링-댐퍼 시스템의 2차 운동방정식은 다음과 같다.
$$m\ddot{x} + c\dot{x}+kx=F(t)$$

#### state variables 설정
$$ x_{1}(t)=x \\
\\ x_{2}(t)=\dot{x} $$

1차 미분 방정식으로 변환하면,

$$ \dot{x_{1}}=x_{2}\\
\\\dot{x_{2}}=-\frac{k}{m}x_1-\frac{c}{m}x_2+\frac{1}{m}F(t) $$

이를 행렬로 변환하면,

$$
\begin{bmatrix}
\dot{x}_1 \\
\dot{x}_2
\end{bmatrix}
=
\begin{bmatrix}
0 & 1 \\
-\frac{k}{m} & -\frac{c}{m}
\end{bmatrix}
\begin{bmatrix}
x_1 \\
x_2
\end{bmatrix}
+
\begin{bmatrix}
0 \\
\frac{1}{m}
\end{bmatrix}
F(t)
$$

#### 출력 방정식 (위치를 출력으로 가정)

$$
y=\begin{bmatrix}
1&0
\end{bmatrix}\begin{bmatrix}
x_1 \\
x_2
\end{bmatrix}
$$

#### MATLAB을 활용한 실제 구현
```
% 시스템 매개변수 정의
m = 0.1;    % 질량 (kg)
k = 5;      % 스프링 상수 (N/m)  
c = 0.01;   % 댐핑 계수 (N⋅s/m)

% 상태공간 행렬 정의
A = [0 1; -k/m -c/m];
B = [0; 1/m];
C = [1 0];
D = 0;

% 상태공간 모델 생성 및 분석
sys = ss(A, B, C, D);
step(sys);        % 스텝 응답
pole(sys);        % 극점 계산
```

### 5. 결론
State를 활용하면, 인간 중심의 계산에서 벗어나 풀이과정에 대한 직관적인 이해와 함께 더 효율적인 계산이 가능하다. 일차 미분방정식 형태로 모든 방정식을 치환하면, 컴퓨터가 이를 Matrix를 이용해 풀 수 있기 때문이다. 평소 우리의 계산 습관으로 접근한다면, 미분방정식의 차수에 관계없이 일단 변수의 개수만큼의 방정식을 직접 설정하고 위에서 아래로 내려가며 계산하기 때문에, 각 변수가 가지는 근본적인 의미에 대해 탐색해볼 여유가 없다.

앞으로는, State를 활용하여 최대한 직관적인 일차 미분방정식을 만들고, 이를 MATLAB에 입력하여 푸는 과정을 연습해야할 것이다. 이는 우리에게 system에 대한 근본적인 이해를 도와줄 것이다.