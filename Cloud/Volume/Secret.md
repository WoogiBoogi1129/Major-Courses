# Secret
## 이론
- 일반적인 리소스가 아닌 보안성이 필요한 리소스의 경우 Secret을 통해 따로 관리한다.
- 외부 Storage에 volumeMount를 할 수도 있다.
- Secret 정보를 바꾸기 위해서는 Pod를 종료했다가 다시 실행해야한다.
    - Pod를 수동으로 종료하고 재실행 시키는 방법이 아닌 자동화된 방법을 사용하고 싶다면?
        - Pod의 변동 사항을 감지하는 프로그램이 필요하다. (해당 프로그램을 통해 Secret을 관리하는 방식을 사이드카 방식이라고 한다.)