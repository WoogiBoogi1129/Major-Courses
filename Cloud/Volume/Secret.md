# Secret
## 이론
- 일반적인 리소스가 아닌 보안성이 필요한 리소스의 경우 Secret을 통해 따로 관리한다.
- 외부 Storage에 volumeMount를 할 수도 있다.
- Secret 정보를 바꾸기 위해서는 Pod를 종료했다가 다시 실행해야한다.
    - Pod를 수동으로 종료하고 재실행 시키는 방법이 아닌 자동화된 방법을 사용하고 싶다면?
        - Pod의 변동 사항을 감지하는 프로그램이 필요하다. (해당 프로그램을 통해 Secret을 관리하는 방식을 사이드카 방식이라고 한다.)
## 실습
### 기본 Secret 생성
- Secret 생성
```
$ kubectl create secret generic test-secret --from-literal=HELLO=WORLD
```
- Secret.yaml 확인
```
$ kubectl get secret test-secret -o yaml
```
- 생성된 Secret.yaml
```
apiVersion: v1
data:
  HELLO: V09STEQ=
kind: Secret
metadata:
  creationTimestamp: "2023-06-11T15:57:17Z"
  name: test-secret
  namespace: student40
  resourceVersion: "17969707"
  uid: ef32ffba-735a-4860-9358-4c641940995a
type: Opaque
```
- Decode 실행
```
$ echo "V09STEQ=" | base64 --decode
```
### Secret을 통해 환경변수 할당하기
- Secret 생성
```
$ kubectl create secret generic db-secret --from-literal=MYSQL_DATABASE=test --from-literal=MYSQL_ROOT_PASSWORD=TAEUK --dry-run=client -o yaml > db-secret.yaml
```
- Secret.yaml 생성 확인
```
$ vi db-secret.yaml
```
- Secret.yaml 배포
```
$ kubectl apply -f db-secret.yaml
```
- Secret 배포를 위한 db-envfrom.yaml 생성
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-envfrom
spec:
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      name: mysql
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mariadb
        ports:
        - containerPort: 3306
        envFrom:
        - secretRef:
            name: db-secret
```
- db-envfrom.yaml 배포
```
$ kubectl apply -f db-envfrom.yaml
```
- db-envform을 통해 생성된 pod에 접근
```
$ kubectl exec -it mysql-envfrom-67cc5977b4-n72rr -- bash
```
- 환경변수 확인
```
$ printenv | grep MYSQL
```
- MySQL을 통해 환경변수 적용 확인
```
$ mysql -uroot -p
``` 
- 패스워드 입력 후 환경변수 적용 확인
```
$ show databases;
```
### 특정 환경 변수만 전달하기
- env.yaml 이라는 새로운 yaml 파일 생성
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-envfrom
spec:
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      name: mysql
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mariadb
        ports:
        - containerPort: 3306
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: db-secret
              key: MYSQL_ROOT_PASSWORD
```
- 이전 실습과 동일하게 환경 변수 확인
### Secret을 통해 생성한 환경변수를 volume으로 마운팅
- volume을 지정해줄 yaml파일 생성
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-envfrom
spec:
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      name: mysql
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mariadb
        ports:
        - containerPort: 3306
        envFrom:
        - secretRef:                // db 이외에는 필요 없는 내용임
            name: db-secret
        volumeMounts: 
        - mountPath: /tmp/secret
          name: db-secret
      volumes:
      - name: db-secret
        secret: 
          secretName: db-secret

```
- db-env-volume.yaml 배포
```
$ kubectl apply -f db-env-volume.yaml
```
- 이전 실습과 같이 생성된 pod에 접근