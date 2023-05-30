# Synchronization Examples
## Synchroniation problems
### bounded-buffer, readers-writers, and dining-philosophers
- The Bounded-Buffer Problem
    - 다수의 생산자 및 소비자가 Buffer를 공유하며 발생하는 동기화 문제이다
    - Buffer가 가득차 생산자 프로세스가 데이터를 추가할 수 없을 때 발생한다.
    - Buffer가 비어 있어 소비자 프로세스가 데이터를 제거할 수 없을 때 발생한다.
    - 이를 해결하기 위해 Mutex, Semaphore 등의 Critical Section의 동시 접근을 제어하는 방법들이 등장했다.
    
- The Readers-Writers Problem
    - 다수의 프로세스가 공유되는 데이터에 접근하는 방법을 동기화하는 문제이다.
    - 데이터의 접근하는 방식은 아래와 같이 나누어진다.
        - 읽기 프로세스 : 공유 데이터를 읽는 것만 가능하다.
        - 쓰기 프로세스 : 공유 데이터를 쓰거나 업데이트만 가능하다.
    - 읽기 프로세스가 다수 동시에 진행될 때 모든 프로세스들은 같은 데이터의 내용을 읽을 수 있어야한다.
    - 쓰기 프로세스가 다수 동시에 진행될 때 하나의 쓰기 프로세스가 동작할 때, 나머지 쓰기 프로세스들은 대기해야한다.
    - 해당 문제를 해결하는 방법은 두가지로 나누어진다.
        - 읽기/쓰기 모두 Critical Section으로 취급한다.
            - 동시성의 대한 제약이 발생한다.
        - 상호배제를 위해 세마포어 등의 동기화 기법을 사용한다.
            - 동기화 기법 선택에 따라 수행 속도가 차이날 수 있다.


- The Dining-Philosophers Problem
    - 철학자가 한 원탁에 앉아 밥을 먹는 상황에서 발생하는 대표적인 동기화 문제이다.
    - 공유되는 자원을 동시에 사용하려고할 때, 발생하는 무한 Lock 상태를 방지하기 위함이 목표이다.
    - 해당 문제를 해결하는 방법에는 Mutex, Semaphore 등의 기법이 존재한다.

## Specific synchronization tools uesd by Linux and Windows
### POSIX
## Design and develop solutions to process synchronization problems using POSIX APIs