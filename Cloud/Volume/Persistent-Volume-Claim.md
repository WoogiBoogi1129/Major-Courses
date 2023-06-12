# Persistent Volume Claim
## 이론
- PersistentVolumeClaim
  - 사용자 입장에서 PV를 사용하기 위한 Volume의 종류 중 하나이다.
## 실습
- pvc.yaml 파일 생성
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata: 
  name: student40-pvc
spec:
  storageClassName: student40-sc
  volumeName: student40-pv
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 2Gi
```
- pvc.yaml 배포
```
$ kubectl apply -f pvc.yaml
```
- pvc가 배포되었는지 확인
```
$ kubectl get pvc
```
- pvc를 호출할 Pod.yaml 생성
```
apiVersion: v1
kind: Pod
metadata:
  name: test-pod40
spec:
  containers:
  - name: container40
    image: kubetm/init
    volumeMounts:
    - name: student40-pvc
      mountPath: /home/student40
  volumes:
  - name: student40-pvc
    persistentVolumeClaim:
      claimName: student40-pvc
```
- pvc의 이름을 포함한 Pod 정보를 yaml 파일로 배포 (test.yaml 참고)
```
$ kubectl apply -f test.yaml
$ kubectl get po
```
- pvc를 이용하여 생성한 pod에 접근하여 testfile 생성해보기
```
$ kubectl exec -it test-pod40 -- bash
$ cd home/student40
$ touch wow.txt
$ touch wowwow.txt
$ exit
```
- pod를 삭제 후 다시 배포
```
$ kubectl delete po test-pod40
$ kubectl apply -f test.yaml
$ kubectl exec -it test-pod40 -- bash
$ cd /home/student40
$ ls
```
- 이전 Pod에서 생성했던 파일이 살아있는 것을 확인할 수 있다.