# Role & Role Binding
## 이론
- Role & Role Binding
    - Role : 하나의 Namespace 별 또는 클러스터 전체에 대해 권한을 정해주는 수단
        - Pod별로 권한을 부여하는 것이라 생각하면 된다.
    - Role Binding : Pod가 어떤 권한을 부여할 지 연결시켜주는 과정
    - API Server
        - User, Pod 들은 모두 API Server를 통해 정보를 얻어온다.
        - API Server 내부에서는 인증, Authorization (권한 부여), Admission Control 세 작업이 동작한다.
            - Authentication
                - 개발자의 설계에 따라 여러가지 플러그인을 추가할 수 있다. (K8S에서 인증에 관련된 모듈을 제공하지는 않는다.)
                - http request 메시지를 확인하여 Authentication을 진행한다.
                - 별도의 시스템이 필요하다.
                - API Server로부터 온 요청을 수행한다.
            - Authorization
                - Role Base Control을 사용하여 기능을 제공한다.
                    - Role : get, create, update, patch, delete의 기능을 가진 명령어들
                    - Role Binding : Role 명령어를 부여하는 도구
                - Namespace : 동일한 함수 및 변수들을 독립적으로 사용할 수 있도록 영역을 나누는 기술이다.
                - 실습을 실제로 해야하는 부분이다. (Role과 Role Binding)
            - Admission Control
                - 사전에 정의해둔 여러가지 기능을 사용할 수 있다.
                - Resource Quota : 
                - AlwaysPullImage : 이미지가 Pod가 생성될 때마다 새롭게 다운받는다. (보통 기본적으로 써놓는다.)
                    - 인증이 된 이미지 파일만 가져오게 하는 것이다.
                    - 이 외 다른 이미지를 가져왔다면 Pod는 해당 명령을 무시한다.
                    - Service Account
                        - Service Account란 Pod가 인증과 권한을 관리하기 위한 수단 (Pod의 ID/PW)
                        - 그럼 Namespace는 어떤 단위로 주어지는건가? Pod? WorkerNode
                        - Namespace 마다 Default 값으로 Service Account가 생성된다. 이후 Pod가 생성되었을 때 Serive Account 정보로 Pod가 생성된다.
                        - 여러 Pod가 Namespace 단위로 생성된 Service Account를 사용할 수 있고, 새롭게 만든 Service Account는 Namespace 한정으로 Pod와 연결시킬 수 있다.
                    - 독자적인 Control 기능들을 추가할 수 있다.
        - 이 중간 단계에서 Role & Role Binding 개념이 사용된다.
    - Role & Role Binding 은 보안과 밀접한 관련이 있다.
- 숙제에서 어떻게 개념을 잘 보여주는 실습을 할 것인가를 잘 생각해야함.
## 실습
- pod 생성
    - pod.yaml을 생성하여 pod를 생성한다.
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: pod-40
      namespace: student40-role
    spec:
      serviceAccountName: sa-40
      containers:
      - name: my-container
        image: nginx
    ```
    - pod를 생성할 때 namespace는 student40-role이라는 미리 생성된 namespace 로 설정한다.

2. role 생성
    - User의 접근 권한을 지정해줄 role.yaml을 설정한다.
    ```
    apiVersion: rbac.authorization.k8s.io/v1
    kind: Role
    metadata:
      name: role-40
      namespace: student40-role
    rules:
    - apiGroups: [""]
      resources: ["pods"]
      verbs: ["get", "list"]
    ```
3. role-binding 생성
    - role.yaml을 통해 설정한 권한을 User 권한에 부여해주기 위해 role-binding.yaml 을 설정한다.
    ```
    apiVersion: rbac.authorization.k8s.io/v1
    kind: RoleBinding
    metadata:
      name: role-binding-40
      namespace: student40-role
    subjects:
    - kind: User
      name: student-role40
    roleRef:
      kind: Role
      name: role-40
      apiGroup: rbac.authorization.k8s.io
    ```
4. 접근 확인
    - admin 권한에서 일반 User 권한으로 이동한다. 현재 권한 목록을 살펴보는 명령어는 아래와 같다.
    ```
    $ kubectl config get-contexts
    ```
    - 권한을 변경하는 명령어는 아래와 같다.
    ```
    $ kubectl config use-context [User이름]
    ```
    - 권한 이동 후 role에 부여한 권한 명령어를 실행한다.
    ```
    $ kubectl get po
    ```
    - Admin 권한으로 다시 돌아가 role.yaml에 Verb에 지정해둔 권한을 지운 후 다시 apply 해본다.
    - 다시 User 권한으로 돌아가 해당 명령어를 실행했을 때, 권한 금지 메시지를 출력하는 것을 확인할 수 있다.