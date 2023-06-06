# StatefulSets
- Deployment로 Pod를 여러개 생성했을 때 Pod의 Name 뒤에 고유번호는 새로 생성될 때마다 매번 바뀜
- Volume과 Pod를 연결해놔도 Pod가 다시 실행될 때 Volume과 연결될 지는 보장되지 않음
- 기존 Pod 서비스들은 Stateless 기법이 적합하다. 하지만 과거 내역들이 유지되어야한다면 새로운 Deployment 기법(StatefulSets)이 필요하다.
    - 이전에 쓰던 Volume을 계속 이어 쓰겠다. 라는 의미를 가지고 있다.
- Deployment와 StatefulSet 비교
    + StatefulSet으로 Pod를 배포하면 Pod에 번호를 붙일 수 있다. Deployment로 배포한 Pod는 번호를 원하는대로 붙일 수 없다.
    + 새롭게 생성된 Pod도 이전에 있던 번호를 일치시켜서 PV를 계속 사용할 수 있다.
    + 고유 번호를 보장하기 때문에 Stateful 형식의 APP을 유지할 수 있다.
- Storage를 특정 Workload에 유지시켜주고 싶을 때 사용한다.
- yaml 파일에서 kind: StatefulSet 으로 바꾸는 것 이외에는 형식이 Deployment와 같다.
- Scale Up/Down도 가능하다.
    + Pod 운용 도중에 replicas를 변경하면 고유 번호가 큰 순서대로 사라진다.
        + 하지만 Storage는 남아있다. 이말을 즉, Scale Up을 다시하면 남아있는 Storage를 다시 사용할 수 있다.
- Service 측면에서의 StatefulSet
    + Service를 배포할 때 clusterIP를 None으로 설정한다.
    + 이름으로 찾아간다 DNS서버와 같이 (Headless Service라고한다.)
- Stateful Service일 경우 Pod에게 각자 고유의 이름이 부여된다.
    + 전체 Service로 묶은 이름과 개별 이름도 부여된다.
- Stateful Service는 Haedless Service로 배포한다. 각각의 Pod별로 DNS entry로 생성되어 Name을 통해 접근한다.
- 그렇다면 DNS와 연결된 주소는 누가 관리하나? --> CoreDNS라는 K8S에서 관리하는 프로젝트에서 해당 기능을 제공한다.
- Deployment를 삭제하면 Storage는 그대로 남아있는다. StatefulSet도 같다.
- 한계점
    + 관리자가 Storage 정보를 미리 준비해놔야한다.
    + Storage 정보는 Volume 자체는 삭제되지 않기 때문에 잘 삭제해야한다.
    + Headless Service를 사용해야한다.
    + Stateful을 삭제하고 싶으면 scale down을 0으로 해야한다. 불편하다.