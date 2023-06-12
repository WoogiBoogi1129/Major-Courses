# Auto Scaling
## 이론
- Autoscaling
    - Workload autoscaling
        - Horizontal
            - 노드를 늘리고 줄이는 동작
            - Pod Autoscaling (HPA)
                - Deployment, ReplicaSet처럼 HorizontalPodAutoscaler를 만들어서 설정한다.
                - HPA를 구성하기 위해서는 Collect metrics를 끌어올 수 있는 프로그램을 사용해야한다.
        - Vertical
            - 해당되는 Pod의 용량을 늘리고 줄이는 동작
## 실습
- deployment-HPA.yaml 생성
```
apiVersion: apps/v1
kind: Deployment
metadata: 
    name: test-deploy
spec: 
    replicas: 1
    selector: 
        matchLabels: 
            run: php-apache
    template: 
        metadata: 
            labels: 
                run: php-apache
        spec: 
            containers: 
            - name: php-apache
              image: k8s.gcr.io/hpa-example
              ports: 
              - containerPort: 80
              resources: 
                limits: 
                    cpu: 500m
                requests: 
                    cpu: 200m
```
- autoScaling.yaml 파일 생성
```
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata: 
    name: php-apache
spec: 
    scaleTargetRef: 
        apiVersion: apps/v1
        kind: Deployment
        name: php-apache
    minReplicas: 1
    maxReplicas: 10
    targetCPUUtilizationPercentage: 50
```