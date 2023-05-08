1. Volume
- 기초용어
    - Drive : Storage Device
    - Volume : 논리적으로 구분된 Storage
    - MountVolume : 특정 위치에 Storage를 연결시키는 것
    - 목적 : Container에서 MountVolume하는 방법
- 쿠버네티스를 이용하여 사용하던 리소스를 유지시키려면 어떻게 해야할까?
    - Persistent Volume을 사용한다.   
    ![image](https://user-images.githubusercontent.com/110087545/236846614-6b57fb68-d8b6-4d9d-9242-db02d951f4f3.png)
    - Persistent Volume : 
    - Persistent Volume Claim : 
    - Storage Class : 
    - 일반적인 상황에서의 Pod 서비스   
    ![image](https://user-images.githubusercontent.com/110087545/236847245-ef61556b-1cde-4d4c-88ff-9ac7ae1646a7.png)
        - 이 상황에서 my-app에서 돌던 서비스에서 사용하던 리소스가 mysql Database에 리소스를 업로드하다가 Pod가 종료되면?
        - 새로 생성된 Pod는 mysql Database 리소스를 유지할 수 없다.
- emptyDir
    - Pod가 죽으면 같이 사라진다. (RAM 형태의 디렉토리라고도 한다.)
    - Pod 내부에 Container가 두개 이상이라면?
        - 같은 Pod 내의 emptyDir은 리소스를 공유한다.
    - 기본 포맷
    ```
    apiVersion: v1
    kind: Pod
    metadata:
        name: test-pd
    spec:
        containers:
        - image: registry.k8s.io/test-webserver
            name: test-container
            volumeMounts:
            - mountPath: /cache                 // volume을 생성할 path
              name: cache-volume
        volumes:
        - name: cache-volume
          emptyDir: {}                        // 기본적인 Volume 형태
    ```
- Persitent Volume
    - 데이터가 사라지지 않는다.
- 외부 사업용 Cloud 서비스
    - 외부에서 제공해주는 Cloud 서비스를 사용할 수 있다.
    - ex) Azure, AWS
- Cephfs
    - 하드웨어적인 스토리지를 관리하는 체계 (오픈소스 프로젝트)
- Cinder
    - Openstack의 스토리지 관리자
- hostPath
    - 하드웨어적인 디렉토리를 사용한다.
    - Pod가 사라져도 리소스가 삭제되지 않는다.
    - Pod가 사라졌다가 다시 생성될 때 Worker Node가 같아야 hostPath를 재사용할 수 있다.
    - Persistent Volume과는 다르다.
    - 기본 포맷
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: test-pd
    spec:
      containers:
      - image: registry.k8s.io/test-webserver
        name: test-container
        volumeMounts:
        - mountPath: /test-pd
          name: test-volume
    volumes:
    - name: test-volume
      hostPath:
        path: /data                             // 호스트의 디렉터리 위치
        type: Directory                         // 이 필드는 선택 사항이다
    ```
- configMap
    - Container를 생성하고 관리할 때 사전에 설정해두는 Configuration 리소스를 관리할 때 사용하는 스토리지
- Secret
    - 보안과 관련된 리소스를 관리할 때 사용하는 스토리지
- downward API