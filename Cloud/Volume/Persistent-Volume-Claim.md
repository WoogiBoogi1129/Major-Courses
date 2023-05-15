# Persistent Volume Claim
# 실습
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
![image](https://github.com/WoogiBoogi1129/Major-Courses/assets/110087545/a2139f54-38e4-4df9-bb17-97dfa4d01b42)