# hostPath 정리
# 이론
- 하드웨어적인 디렉토리를 사용한다.
- Pod가 사라져도 리소스가 삭제되지 않는다.
- Pod가 사라졌다가 다시 생성될 때 Worker Node가 같아야 hostPath를 재사용할 수 있다.
- Persistent Volume과는 다르다.
# 실습
- hostPath.yaml 생성
```
$ vi hostPath.yaml // 생성된 파일 내에 yaml 파일 입력
```
- nodeSelector 확인하기
    + nodeSelector가 중요한 이유 : yaml 파일 내에 nodeSelector는 아래 명령어를 통해 사용가능한 label의 이름을 알아야함. (실습 그대로만 따라하면 안될 수도 있음)
```
$ kubectl get node --show-labels
```
- hostPath.yaml 배포
```
$ kubectl apply -f hostPath.yaml
```
- hostPath 진입해서 wowwow.txt 생성
```
$ kubectl exec -it test-pod -- bash
$ ls // test1 디렉토리 확인
$ cd test1
$ touch wowowow.txt
$ exit
```
- 새로운 Pod를 생성하고 hostPath만 같게 만들어도 리소스를 공유할 수 있을까?
    + 아니다, hostPath가 같은 리소스를 공유하기 위해서는 Worker-node에 위치해 있어야한다. hostPath2.yaml을 확인해보면 Pod를 생성하는 nodeSelector가 다른 것을 확인할 수 있다. 따라서, 생성한 test-pod와 test-pod2는 같은 리소스를 공유할 수 없다.