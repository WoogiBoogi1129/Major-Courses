# ConfigMap
## 이론
- Key-Value 형태로 구성되어 있어 다양한 환경 변수를 설정해놓고 가져올 수 있다.
- Pod의 환경 변수들을 전체 or 부분 적용이 가능하다.
## 실습
- configMap.yaml 생성
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: config-test
data:
  MYSQL_ROOT_PASSWORD: test
  MYSQL_DATABASE: HTU
```
- configMap-Deploy 생성
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: test-deploy
spec:
  replicas: 1
  selector:
    matchLabels:
      app: test-deploy
  template:
    metadata:
      name: test-deploy
      labels:
        app: test-deploy
    spec:
      containers:
      - name: test-deploy
        image: mariadb
        envFrom:
        - configMapRef:
            name: config-test
```
- configMap, configDeploy 배포
```
$ kubectl apply -f CM.yaml
$ kubectl apply -f deployCM.yaml
```
- 생성된 Pod에 접근
```
$ kubectl exec -it pod/test-deploy-6f86c9b9c5-9xcxk -- bash
```
- Pod에서 configMap으로 설정한 환경변수값 확인
```
$ echo $MYSQL_ROOT_PASSWORD
$ echo $MYSQL_DATABASE
```
### 일부분만 Pod에 할당하기
- configMap.yaml의 일부분만 가져오는 deployment.yaml 파일 생성
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: test-deploy
spec:
  replicas: 1
  selector:
    matchLabels:
      app: test-deploy
  template:
    metadata:
      name: test-deploy
      labels:
        app: test-deploy
    spec:
      containers:
      - name: test-deploy
        image: mariadb
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            configMapKeyRef:
              name: config-test
              key: MYSQL_ROOT_PASSWORD
        - name: MYSQL_DATABASE
          valueFrom:
            configMapKeyRef:
              name: config-test
              key: MYSQL_DATABASE
```
- 배포 후, 이전 실습과 같이 echo 명령어를 사용하여 환경 변수 값 확인
### volume에 configMap을 통해 생성한 환경변수 할당
- volume에 configMap 환경변수를 담는 yaml 파일 생성
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: test-deploy
spec:
  replicas: 1
  selector:
    matchLabels:
      app: test-deploy
  template:
    metadata:
      name: test-deploy
      labels:
        app: test-deploy
    spec:
      containers:
      - name: test-deploy
        image: mariadb
        envFrom:
        - configMapRef:
            name: config-test
        volumeMounts:
        - mountPath: /home/taeuk
          name: config-test
      volumes:
      - name: config-test
        configMap:
          name: config-test
```
- 이전 실습과 확인방법 동일