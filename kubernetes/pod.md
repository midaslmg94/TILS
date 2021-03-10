# Pod



> Container의 특징

Pods는 1개 이상의 컨테이너로 구성되며 각 컨테이너는 포트를 가지고 있다. 포트는 중복될 수 없다.

같은 pod내에 있는 컨테이너들은 같은 호스트를 갖고 있는것이므로 localhost:포트번호 로 접근 가능하다

파드가 생성되면 ip가 생성되며 쿠버네티스 내부에서만 이 ip를 가지고 접근 가능하다.

파드가 재생성되는 경우 ip는 변경된다

> Label의 특징

라벨은 pod뿐만 아니라 모든 오브젝트에 붙일 수 있다. 라벨의 목적은 오브젝트를 분류하고, 같은 라벨이 붙은 오브젝트를 따로 관리하기 위해서이다.

* 1개의 파드에 여러개의 라벨을 달 수 있다.
* Key:value의 형태로 달 수 있다.

예를 들어 type이 web으로 된 파드들만 묶어서 Service에 달면 외부로 노출 시킬 수 있다

* Service를 만들 때 Selector에 key와 value를 넣으면 된다

해시태그로 검색하듯이 원하는 파드들을 검색해서 사용 가능하다

> NodeSchedule

파드는 결국 여러 노드들 중 1개의 노드에 올라가야 한다.

* node를 직접 선택하는 경우
  * nodeSelector에 node의 라벨과 매칭되는 key:value를 입력해준다
* node를 스케줄러가 판단해서 선택하는 경우
  * 쿠버네티스가 Node별로 메모리, CPU등 자원량을 판단하여 알맞는 Node에 배포해준다.
  * resources에 request를 작성한다

> 실습

ReplicationController

레플리케이션 컨트롤러는 항상 지정된 수의 Pod Replica가 실행중임을 보장한다. 만약 파드가 죽으면, 같은 갯수만큼의 파드를 자동으로 새로 생성한다.

* ReplicaSet을 구성하는 Deployment가 현재 권장하는 레플리케이션 설정 방법이다
* rc로 축약하여 사용한다.

```bash
$ ktl apply -f replicaController.yaml
replicationcontroller/replication-1 created

$ ktl get pods
NAME                  READY   STATUS              RESTARTS   AGE
replication-1-xncxk   0/1     ContainerCreating   0          5s

$ ktl get pods
NAME                  READY   STATUS    RESTARTS   AGE
replication-1-xncxk   1/1     Running   0          25s

$ ktl delete pods/replication-1-xncxk
pod "replication-1-xncxk" deleted

$ ktl get pods
NAME                  READY   STATUS    RESTARTS   AGE
replication-1-jjwqq   1/1     Running   0          37s
```

파드를 삭제할 때의 대시보드 모습이다. 동일한 파드가 뒤에 이름만 다르게 해서 새로 뜬 것을 확인할 수 있다

