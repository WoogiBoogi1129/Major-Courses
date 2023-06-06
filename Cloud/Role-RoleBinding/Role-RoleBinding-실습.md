# Role & RoleBinding 실습
1. pod 생성
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