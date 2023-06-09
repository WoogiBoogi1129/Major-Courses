# Job
## 이론
- Pod는 한번 생성해두면 죽을 때까지 계속 살아있는 특징을 가지고 있다.
    - Job을 통해 Pod를 동작시키면 동작을 단 한번 하고 Pod를 종료시킬 수 있다.
    - 정상적으로 Pod가 동작을 하고 멈출 때까지 Pod를 관리하는 기능
    - Job 실행 중 동작을 멈추게 할 수 있다. 서스펜딩이라고 함
        - 기존 Pod를 지우고 재실행 시, 다시 Pod를 생성함
    - Pod를 지우고 만드는거랑 다른점
        - 만약 Job을 이용해서 Pod를 만들고 동작 중 강제로 멈춰도 재실행 시, Pod가 다시 동작한다.
        - Job을 통해 Pod를 만들면 시간이 지나면 사라진다.
        - 일반적인 Pod는 프로그램의 끝이 없다.
        - Job의 경우는 프로그램의 끝이 있을 때 사용한다.
    - 여러개의 Pod를 만들어서 Job으로 관리할 수 있다.
        - 예시로는 거대한 규모의 작업을 Job을 통해 자동으로 수행시킬 수 있다.
    - restartPolicy는 Never와 Onfailure 가 있다.
    - 문제가 발생했을 때, 재동작을 몇번 동작해볼지도 설정할 수 있다.
    - Job이 끝나도 남아있는 오브젝트들을 언제까지 유지할 지 설정할 수 있다 : ttl을 사용하여 설정한다.
    - 독립적으로 Pod들이 병렬처리를 하지만 Job으로 묶인 Pod들은 모두 같은 작업들을 하는 것이다.
## 실습
- job.yaml 만들기
```
apiVersion: batch/v1
kind: Job
metadata:
  name: job-1
spec:
  template:
    spec:
      containers:
      - name: container1
        image: busybox
        command: ["echo", "hellworld"]
      restartPolicy: Never
  backoffLimit: 4
```
- job.yaml 배포하기
```
$ kubectl apply -f job.yaml
```
- 실행 후 Job 로그 확인하기
```
$ kubectl logs -l job-name=kob-1
```
### Completion, parallelism 선언한 Job
- job-cp.yaml 생성
```
apiVersion: batch/v1
kind: Job
metadata:
  name: job-2
spec:
  completions: 5
  parallelism: 2
  template:
    spec:
      containers:
      - name: container1
        image: perl
        command: ["perl", "-Mbiguum=bpi", "-wle", "print bpi(100)"]
      restartPolicy: Never
  backoffLimit: 4
```