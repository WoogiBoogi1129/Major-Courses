# StorageClass
# 실습
- SC.yaml 파일 생성
```
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: student40-sc
provisioner: kubernetes.io/no-provisioner
volumeBindingMode: WaitForFirstConsumer
```

- SC.yaml 파일 배포
```
$ kubectl apply -f SC.yaml
```

- 생성된 SC 확인
```
$ kubectl get sc
```
![image](https://github.com/WoogiBoogi1129/Major-Courses/assets/110087545/57e64774-4f26-4f52-8301-af9b3a1c955f)