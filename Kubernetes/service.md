### Service - ClusterIP, NodePort, LoadBalancer

외부에서 접근할 수 있도록 함. 사용자가 삭제하지 않는 이상 Service의 IP는 바뀌지 않는다.

 

> ClusterIP


클러스터 내에서 접근 가능한 IP. 외부에서는 접근할 수 없다.

- type: ClusterIP (생략 가능, 기본값이 ClusterIP)
- 항상 연결된 파드에 접근 가능
- 여러개의 Pod에 연결 가능.  Pod에 붙어있는 labels를 selector에서 선택하여 연결

사용 방안

- 외부에서 접속할 수 없기 때문에 인가된 사용자(운영자)
- 내부의 대시보드
- Pod의 서비스 상태 디버깅

> NodePort


서비스에 기본적으로 ClusterIP가 할당된다. 클러스터에 구성된 모든 Node에 동일한 Port가 할당된다.(Pod가 떠있지 않은 Node에도 할당된다)

- type: NodePort
- 할당가능한 포트는 30000~32767 사이(안넣으면 이 범위에서 임의로 할당)
- 예를 들어, 2개의 노드가 있을 경우(Node1, Node2) Node1에 있는 Pod에 온 요청을 Node2에 있는 Pod에 응답할 수 있다.
    - `externalTrafficPolicy: Local` 을 쓰면 요청이 온 Node에 있는 Pod에만 응답을 보낸다.

사용 방안

- 물리적인 IP를 통해 접근 가능하므로 내부망 연결 시 사용하면 좋음
- 데모 or 임시 테스트를 위해 외부 연동이 필요한 경우

```yaml
apiVersion: v1
kind: Service
metadata:
  name: svc
spec:
	type: NodePort
	ports:
	 - targetPort: 80 # pod의 포트
     port: 80 # service의 포트
     nodePort: 30008 # node 밖으로 나가는 포트, 지정안하면 자동으로 지정
	selector: # pod에 정의된 label과 연결
		app: myapp
		type: front-end
```

> LoadBalancer


기본적으로 NodePort의 성격을 가지고 있다. 외부에서 접속가능한 IP가 생겨서 각 노드의 트래픽을 분산시켜준다. SaaS를 사용하지 않고 쿠버네티스를 사용할 경우 LoadBalancer는 기본적으로 제공되지 않으므로 별도의 플러그인 설치가 필요하다

- type: LoadBalancer

사용 방안 

- 실제 서비스를 외부 시스템에 노출할 때 사용