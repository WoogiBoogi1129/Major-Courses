# CronJob
## 이론
- 주기적으로 한번씩 Pod가 실행되고 종료되기를 원할 때 사용하는 기능이다.
- yaml파일 내부 schedule을 설정하여 주기를 설정하고 자동으로 동작하게할 수 있다.
- Job과 기능은 비슷하다.
## 실습
- cronjob.yaml 생성
```
apiVersion: batch/v1
kind: CronJob
metadata:
  name: cronjob-1
spec:
  schedule: "*/1 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: hello
            image: busybox:1.28
            imagePullPolicy: IfNotPressent
            command:
            - /bin/sh
            - -c
            - date; echo Hello from the Kubernetes cluster
          restartPolicy: OnFailure
```