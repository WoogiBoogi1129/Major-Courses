# Persitent Volume
- 데이터가 사라지지 않는다.
- Local Disk : Storage만 모아둔 Cluster
- nfs Disk : Network File System 방식의 Storage
- 외부 사업 Storage를 사용할 수도 있다.
- 어떤 Storage를 사용할 지는 Storage 관리자가 정한다.
- Persistent Volume은 Namespace로 구분되지 않으므로 서로 다른 Namespace에서도 같은 Persistent Volume을 가진다.
    - Persistent Volume Claim은 Namespace 범주에 속해 있으므로 따로 설정 가능하다.
- 중요한 데이터를 유지하고 싶다면 Remote Storage를 사용해야한다. (Storage의 요구사항 참조)
- Pod는 Persistent Volume을 사용하기 위해 Persistent Volume Claim을 사용한다.   
![image](https://user-images.githubusercontent.com/110087545/236852075-e70243cf-6c45-4cf1-95f5-56306cf5a501.png)
- Why? Persistent Volume을 사용할 때에 상세한 내용들을 파악하고 있을 필요는 없기 때문에 필요한 내용만 간단히 사용하기 위해서이다.

- Pod가 Storage를 가지는 과정
    - Persistent Volume의 이름을 Persistent Volume Claim에서 가져오고 Persistent Volume Claim에서 가져온 PV를 Container의 어디에 위치할 지 mountPath에 설정한다.

- PV와 PVC를 통해 관리하지 않는 Storage가 있다. (configMap, Secret)

- 내가 원하는 PV가 없다면? or 너무 많다면?
    - 시간이 매우 많이 소요되고 복잡할 것이다.
    - 그래서 Storage Class가 등장했다.
        - Storage Class를 이용한다면 그때 그때 Stroage 스펙을 생성한다.

# 실습
- PV.yaml 작성
```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: student40-pv
spec: 
  capacity: 
    storage: 2Gi
  accessModes: 
  - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain 
  storageClassName: student40-sc
  local:
    path: /home/student40
  nodeAffinity: 
    required: 
      nodeSelectorTerms:
      - matchExpressions:
        - key: kubernetes.io/hostname
          operator: In
          values:
          - worker-5
```

- PV 배포
```
$ kubectl apply -f PV.yaml
```
- PV 생성 확인
```
$ kubectl get pv
```
![image](https://github.com/WoogiBoogi1129/Major-Courses/assets/110087545/aa1ba81d-9236-44bc-80be-3c2be159dd3f)