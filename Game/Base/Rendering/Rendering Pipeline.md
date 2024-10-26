# Rendering Pipeline

## Vertex
폴리곤 메시는 정점을 정점 배열과 인덱스 배열로 나누어 저장한다   
정점 배열의 크기 = 정점의 개수 / 인덱스 배열의 크기 = 면의 개수 * 3   
=> 정점 데이터는 정점의 위치뿐만 아니라 다양한 정보가 들어가있다. 면의 개수와 정점의 개수가 늘어날 수록 인덱스 배열로 나누는 것이 유리하다.   
삼각형의 세 정점은 반시계 방향으로 저장되어있다.   

## 변환 행렬 ( 열벡터 기준 )
행렬 연산의 순서 = 크기 => 회전 => 이동   
크기는 회전의 영향을 받고, 회전은 이동의 영향을 받는다   
정점의 행렬은 변환 행렬의 오른쪽에 붙는다
### 2차원

**축소 확대**   

$$

\begin{align}
\begin{pmatrix} s_x & 0 \\ 0 & s_y \end{pmatrix}{x \choose y} = {s_xx \choose s_yy} \\
\begin{pmatrix} s_x & 0 & 0 \\ 0 & s_y & 0 \\ 0 & 0 & 1 \end{pmatrix}\begin{pmatrix} x \\ y \\ 1 \end{pmatrix} = \begin{pmatrix} s_xx \\ s_yy \\ 1 \end{pmatrix}
\end{align}

$$   

**회전**   

$$

\begin{align}
\begin{pmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{pmatrix}{x \choose y} = {x' \choose y'} \\
\begin{pmatrix} \cos\theta & -\sin\theta & 0 \\ \sin\theta & \cos\theta & 0 \\ 0 & 0 & 1 \end{pmatrix}\begin{pmatrix} x \\ y \\ 1 \end{pmatrix} = \begin{pmatrix} x' \\ y' \\ 1 \end{pmatrix}
\end{align}

$$   

**이동**   

$$

\begin{align}
{x \choose y}+{d_x \choose d_y} = {x+d_x \choose y+d_y} \\
\begin{pmatrix} 1 & 0 & d_x \\ 0 & 1 & d_y \\ 0 & 0 & 1 \end{pmatrix}\begin{pmatrix} x \\ y \\ 1 \end{pmatrix} = \begin{pmatrix} x+d_x \\ y+d_y \\ 1 \end{pmatrix}
\end{align}

$$   

### 3차원
**축소 확대**   
$$
\begin{pmatrix} s_x & 0 & 0 & 0 \\ 0 & s_y & 0 & 0 \\ 0 & 0 & s_z & 0 \\ 0 & 0 & 0 & 1 \end{pmatrix}
$$   
**X축 회전**   
$$
\begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & \cos\theta & -\sin\theta & 0 \\ 0 & \sin\theta & \cos\theta & 0 \\ 0 & 0 & 0 & 1 \end{pmatrix}
$$   
**Y축 회전**   
$$
\begin{pmatrix} \cos\theta & 0 & \sin\theta & 0 \\ 0 & 1 & 0 & 0 \\ -\sin\theta & 0 & \cos\theta & 0 \\ 0 & 0 & 0 & 1 \end{pmatrix}
$$   
**Z축 회전**   
$$
\begin{pmatrix} \cos\theta & -\sin\theta & 0 & 0 \\ \sin\theta & \cos\theta & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1\end{pmatrix}
$$   
**이동**   
$$
\begin{pmatrix} 1 & 0 & 0 & d_x \\ 0 & 1 & 0 & d_y \\ 0 & 0 & 1 & d_z \\ 0 & 0 & 0 & 1\end{pmatrix}
$$   
## 노말 벡터
노말 벡터는 이동을 제외한 크기+회전행렬의 역전치행렬을 취해야한다.

## 뷰 변환
카메라의 좌표를 EYE, 월드 좌표의 원점을 O라고 할 때 모든 정점에 대해서 다음 연산을 수행한다.   
EYE가 O로 이동하는 행렬 => EYE의 기저를 O의 기저로 변환하는 행렬   
$$
\begin{align}
T=\begin{pmatrix} 1 & 0 & 0 & -EYE_x \\ 0 & 1 & 0 & -EYE_y \\ 0 & 0 & 1 & -EYE_z \\ 0 & 0 & 0 & 1 \end{pmatrix} \\
R=\begin{pmatrix} u_x & u_y & u_z & 0 \\ v_x & v_y & v_z & 0 \\ n_x & n_y & n_z & 0 \\ 0 & 0 & 0 & 1 \end{pmatrix}
\end{align}
$$

## 투영 행렬(-z기준, +z일 경우 3행의 부호를 반대로한다)
fovy = y축 방향 시야각, aspect = 종횡비(x/y), n = near(시야의 최소거리), f = far(시야의 최대거리)   
결과에 W값을 나누면 실제로 화면에 표시할 좌표를 구할 수 있다.   
$$
M = \begin{pmatrix} {\cot {fovy \over 2} \over aspect} & 0 & 0 & 0 \\ 0 & \cot {fovy \over 2} & 0 & 0 \\ 0 & 0 & {f \over f-n} & {-nf \over f-n} \\ 0 & 0 & -1 & 0 \end{pmatrix}
$$

## 래스터라이저
클리핑 -> 원근 나눗셈 -> 뒷면 제거 -> 뷰포트 변환 -> 스캔 전환   
**클리핑** : 2*2*2로 변환한 뷰 공간의 바깥에 있는 정점을 잘라낸다. 만약 삼각형이 걸쳐 있는 경우 잘라내고 새 정점을 할당한다.   
**원근 나눗셈** : 투영 행렬 적용 후 모든 원소를 w로 나눈다. 투영 행렬을 적용했을 경우 뷰 공간의 z좌표가 정점의 w좌표에 저장된다. 이는 뷰 공간의 xy축 평면에서의 거리가된다.   
따라서 멀리 있는 정점은 w값이 크기 때문에 삼각형이 작아지게된다. 이를 통해 원근을 표현한다.   
**뒷면 제거** : 삼각형의 법선벡터와 삼각형의 정점과 카메라의 원점을 잇는 선을 내적했을 때 음수가 나올 경우 렌더링하지 않는다.   
**뷰포트 변환** : 2*2*2크기의 뷰 볼륨을 뷰포트로 변환한다.   
$$
\begin{align}
\begin{pmatrix} {w \over 2} & 0 & 0 & minX + {w \over 2} \\ 0 & {h \over 2} & 0 & minY + {h \over 2} \\ 0 & 0 & {maxZ-minZ \over 2} & {maxZ + minZ \over 2} \\ 0 & 0 & 0 & 1 \end{pmatrix} \\
minZ = 0, maxZ = 1 (기본값일 경우) \\
\begin{pmatrix} {w \over 2} & 0 & 0 & {w \over 2} \\ 0 & {h \over 2} & 0 & { h \over 2} \\ 0 & 0 & { 1 \over 2 } & {1 \over 2} \\ 0 & 0 & 0 & 1 \end{pmatrix}
\end{align}
$$   
**스캔 전환** : 삼각형 내부를 채우는 프래그먼트를 생성한다.
