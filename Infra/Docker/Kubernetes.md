# Kubernetes

## 개요
컨테이너 시스템 전체를 통괄하고, 여러 개의 컨테이너를 관리하는 시스템, 여러 대의 컨테이너가 여러 대의 물리 서버에 걸쳐 실행되는 것을 전제로 한다. 소규모라도 관리의 자동화를 원한다면 고려해볼만하다.
<br/>

쿠버네티스는 YAML 파일에 컨테이너나 볼륨의 개수 등 환경을 설정해두고 자동으로 이 상태를 유지한다. 쿠버네티스가 관리하는 요소를 강제로 변경하는 것은 권장하지 않는다.

## 설치
1. swap 비활성화

```sh
sudo swapoff -a
sudo sed -i '/swap/s/^/#/' /etc/fstab
```

2. 방화벽 설정

|노드|포트|프로토콜|설명|
|---|---|---|---|
|Master|6443|TCP|API 서버, 외부통신 시 사용|
||2379-2380|TCP|etcd 서버|
||10250|kubelet API, 워커노드와 통신에 사용|
||10259|TCP|scheduler|
||10257|TCP|controller-manager|
|Worker|10250|TCP|kubelet API, 마스터노드와 통신에 사용|
||30000-32767|TCP|NodePort에 사용, 외부 클라이언트 접근에 사용|

3. 네트워크 설정

```sh
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

sudo modprobe overlay
sudo modprobe br_netfilter

# 필요한 sysctl 파라미터를 설정하면, 재부팅 후에도 값이 유지된다.
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF

# 재부팅하지 않고 sysctl 파라미터 적용하기
sudo sysctl --system
```

4. docker 설치
5. docker cri 설정

```sh
cat <<EOF | sudo tee /etc/docker/daemon.json
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF

sudo systemctl restart docker
```
6. cgroup 설정

```sh
sudo mkdir -p /etc/containerd
sudo containerd config default | sudo tee /etc/containerd/config.toml
vi /etc/containerd/config.toml
# 파일 내에서 SystemdCgroup = true로 설정

sudo systemctl restart containerd
```

7. kubelet kubeadm kubectl 설치
8. 마스터 노드 초기화

```sh
sudo kubeadm init
```

9. 일반 사용자 kubectl 활성화

```sh
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
10. Pod 네트워크 설정

```sh
# Flannel
kubectl apply -f https://raw.githubusercontent.com/flannel-io/flannel/master/Documentation/kube-flannel.yml
```
11. 워커 노드 등록

```sh
kubeadm join <마스터노드 IP>:<마스터노드 포트(6443)> --token <토큰> --discovery-token-ca-cert-hash sha256:<해시값>
```


## 클러스터
- 마스터 노드 : 컨테이너의 전체적인 관리를 담당
+ kube-apiserver : 외부와 통신을 담당, kubectl로부터 명령을 받아 실행
+ kube-controller-manager : 컨트롤러를 통합, 관리
+ kube-scheduler : 파드를 워커 노드에 할당
+ cloud-controlelr-manager : 클라우드 서비스와 연동
+ etcd : 클러스터 관련 정보를 관리하는 데이터베이스
- 워커 노드 : 실제 동작을 담당, 도커 등의 컨테이너 엔진이 필요
+ kube-let : kube-scheduler와 연동해 워커 노드에 파드를 배치하고 실행, 파드의 상태를 모니터링해 마스터로 전송
+ kube-proxy : 네트워크 통신 라우팅
- 클러스터 : 마스터 노드 + 워커 노드

## 쿠버네티스 리소스
- 파드 : 컨테이너 + 볼륨
- 서비스 : 파드의 집합을 관리, 주 역할은 로드 밸런싱
- 레플리카 세트 : 파드의 수를 관리, 레플리카 세트가 관리하는 동일한 구성의 파드를 레플리카라고 부른다.
- 디플로이먼트 : 배포 담당, 파드가 사용하는 이미지, 파드의 정보를 담고 있음
- 파드 템플릿 : 배포 시 파드의 형틀
- 레플리케이션 컨트롤러 : 레플리케이션 제어
- 리소스쿼터 : 쿠버네티스 리소스 사용량 제한 설정
- 비밀값 : 키 정보 관리
- 서비스어카운트 : 리소스 사용자 관리
- 데몬 세트 : 워커 노드마다 하나의 파드 생성
- 스테이트풀 세트 : 파드의 배포 상태를 유지 및 관리
- 크론잡 : 지정된 스케줄대로 파드를 실행
- 잡 : 파드를 한 번 실행

## 매니페스트 파일
매니페스트 파일은 리소스 단위로 작성한다. 하나의 파일로 작성하고 싶다면 리소스 간 ---로 구분한다. 파드는 대체로 디플로이면트에 포함되는 형태로 기재된다.

### 주 항목
- apiversion : API 그룹 및 버전
- kind : 리소스 유형
- metadata : 메타데이터
+ name : 리소스 이름
+ namespace : 리소스를 세분화한 DNS 호환 레이블
+ uid : 유일 식별자
+ resourceVersion : 리소스 버전
+ generation : 생성 순서
+ creationTimestamp : 생성 일시
+ deletionTimestamp : 삭제 일시
+ labels : 임의의 레이블, 키-값 쌍
+ anotation : 리소스에 설정할 값
- spec : 리소스 내용

### 파드 작성
```yaml
apiVersion: v1
kind: Pod
metadata: # 파드 정보
  name: some_pod
  labels: # 라벨, 객체에 연결할 수 있는 키값쌍
    app: some_label
spec:
  containers: # 컨테이너 정보
    - name: some_container
      image: some_image
      resources:
        requests: # 최소 요구 자원
          cpu: "500m" # CPU 자원의 50%, 1m당 1000분의 1
          memory: "128Mi" # 요구 메모리
        limits: # 최대 가용 자원
          cpu: "1000m"
          memory: "256Mi"
      livenessProbe: # 활성 상태 검사, readinessProbe로 준비 프로프도 가능
        httpGet: # GET 요청을 통한 검사, /healthy 경로 GET 요청 수행
          path: /healthy
          port: 8080
        initialDelaySeconds: 5 # 5초 후 검사 수행
        timeoutSeconds: 1 # 1초 이상 걸리면 실패
        periodSeconds: 10 # 10초 마다 검사
        failureThreshold: 3 # 3번 연속 실패시 중지 및 재시작
      ports:
      - containerPort: 80
      volumeMounts:
        - mountPath: "/data" # 컨테이너 내부 경로
          name: "data"
  volumes: # 볼륨 정의
    - name: "data"
      hostPath:
        path: "/var/data" # 호스트 경로
    - name: "remote-data"
      nfs:
        server: some-url
        path: "/remote"

```

### 디플로이먼트 작성
```yaml
apiVersion: /v1
kind: Deployment
metadata:
  name: some_deployment
spec:
  selector: # 특정 레이블이 부여된 파드를 관리
    matchLabels:
      app: some_label
  replicas: 5 # 파드의 개수
  template: # 생성할 파드의 정보
    metadata:
      labels:
        app: some_label
    spec:
      containers:
      - name: some_pod
        image: some_image
        ports:
        - containerPort: 80
```

### 서비스 작성
```yaml
apiVersion: v1
kind: Service
metadata:
  name: some_deployment
spec:
  type: NodePort # 서비스 유형
  # ClusterIP : 클러스터 IP를 이용해 서비스에 접근(외부에서 접근 불가)
  # NodePort : 워커 노드의 IP를 이용해 서비스에 접근
  # LoadBalancer : 로드밸러서의 IP를 이용해 서비스에 접근
  # ExternalName : 파드에서 서비스를 통해 외부로 나가기 위한 설정
  ports:
  - port: 8099 # 서비스의 포트, 클러스터 내부에 노출할 포트
    targetPort: 80 # 컨테이너 포트
    protocol: TCP # 통신 프로토콜
    nodePort: 30080 # 워커 노드의 포트 30000~32767 사이의 값, 클러스터 외부에 노출할 포트
  selector:
    app: some_label # matchLabels를 사용해선 안된다
```

## 쿠버네티스 명령어
kubectl 커맨드 옵션 형식을 사용한다.

- create : 리소스 생성
- edit : 리소스 편집
- delete : 리소스 삭제
- get : 리소스 상태 출력
- set : 리소스 값 설정
- apply : 리소스 변경 반영
- describe : 상세 정보
- diff : 이상적인 상태와 현재 상태 비교
- expose : 파드의 부하를 분산하는 서비스 생성
- scale : 레플리카 수 변경
- autoscale : 자동 스케일링 적용
- rollout : 롤아웃
- exec : 컨테이너에서 명령 수행
- run : 컨테이너에서 명령을 한 번 수행
- attach : 컨테이너에 접속
- cp : 컨테이너에 파일 복사
- logs : 컨테이너 로그 출력
- cluster-info : 클러스터 상세 정보
- top : 시스템 자원 출력
- port-forward : 포트 포워딩
- label : 객체 라벨링

## 인그레스
k8s의 HTTP 기반 로드밸런싱 시스템   
[Document](https://kubernetes.io/ko/docs/concepts/services-networking/ingress/)

### 예시
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: minimal-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx-example
  rules:
  - http:
      paths:
      - path: /testpath
        pathType: Prefix
        backend:
          service:
            name: test
            port:
              number: 80
```

## Docker desktop에서 k8s 대시보드 활성화(local)
[출처](https://github.com/kubernetes/dashboard/blob/master/docs/user/access-control/creating-sample-user.md)

1. Docker desktop 설정에서 k8s 활성화
2. kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.4.0/aio/deploy/recommended.yaml
3. kubectl proxy
4. 서비스 아카운트 생성

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
```

5. 서비스 아카운트에 역할 부여

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef: # 부여할 역할
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin # 관리자 역할
subjects: # 부여 대상
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
```

6. kubectl -n kubernetes-dashboard create token admin-user
7. http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/ 접속
8. 6번의 토큰을 붙여넣고 사용
9. 재부팅 후 접속 시 3번, 6번, 7번, 8번만 실행