# emptyDir
# 이론
- Pod가 죽으면 같이 사라진다. (RAM 형태의 디렉토리라고도 한다.)
- Pod 내부에 Container가 두개 이상이라면?
    - 같은 Pod 내의 emptyDir은 리소스를 공유한다.
# 실습
- yaml 파일을 작성한다.
```
apiVersion: v1
kind: Pod
metadata:
  name: test-pod
spec:
  containers:
  - name: container1
    image: kubetm/init
    volumeMounts:
    - name: test-dir
      mountPath: /test1
  - name: container2
    image: kubetm/init
    volumeMounts:
    - name: test-dir
      mountPath: /test2
  volumes:
  - name: test-dir
    emptyDir: {}
```
- kubectl 명령어를 사용하여 배포한다.
```
$ kubectl apply -f emptyDir.yaml
```
- Pod 생성 확인은 아래 명령어로 가능하다.
```
$ kubectl get po -w
```
- 이번에는 생성한 Pod의 Container 중 1개에 접근해보자.
```
$ kubectl exec -it test-pod -c container1 -- bash
```
![image](https://github.com/WoogiBoogi1129/Major-Courses/assets/110087545/3a97c7d9-2fe0-4e3e-b52a-13fd9f69489e)   
yaml 파일에서 지정해두었던 volumeMount 아래 mountPath에 설정해두었던 test1이 존재하는 것을 확인할 수 있다.
- test1에 wow.txt 파일을 생성해놓으면 emptyDir의 특성 상 test2에도 wow.txt가 존재해야한다. 아래는 이를 확인할 수 있는 실험 과정이다.
```
// 명령어는 위에서 접근한 container1 bash에 접근한 부분부터 이어진다.
$ ls // test1 디렉토리가 있는지 확인
$ cd test1 // test1 디렉토리에 접근
$ touch wow.txt // test1 디렉토리에 wow.txt 파일 생성
$ exit // container1 bash에서 탈출

$ kubectl exec -it test-pod -c container2 -- bash
$ ls // test2 디렉토리 확인
$ cd test2 // test2로 접근, 내부에 wow.txt 파일이 있는 것을 확인할 수 있다.
```
![image](https://github.com/WoogiBoogi1129/Major-Courses/assets/110087545/f0e2358e-8e0e-4d59-b3ee-cbd0070b3206)
- 실습 후 emptyDir Pod 삭제
```
$ kubectl delete -f emptyDir.yaml
```