# Auto Scaling
- Autoscaling
    - Workload autoscaling
        - Horizontal
            - 노드를 늘리고 줄이는 동작
            - Pod Autoscaling (HPA)
                - Deployment, ReplicaSet처럼 HorizontalPodAutoscaler를 만들어서 설정한다.
                - HPA를 구성하기 위해서는 Collect metrics를 끌어올 수 있는 프로그램을 사용해야한다.
        - Vertical
            - 해당되는 Pod의 용량을 늘리고 줄이는 동작