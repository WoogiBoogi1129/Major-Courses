# 9.1장 Background
1. MMU(Memory Management Unit)
- 개요 : Logical Address, Physical Address 간의 변화를 공부한다.
2. Contiguous Memory Allocation
- 프로세스가 생성되기 위해서는 운영체제가 프로세스의 주소 영역을 지정해주어야한다. --> 연속적인 메모리 주소에 프로세스를 할당하는 방식이다.
3. Internal and External Fragmantation
- 메모리에 프로세스가 할당되고 그 사이 사용할 수 없는 공간을 나타낸다.
4. Paging System
- 운영체제에서 가장 많이 사용하는 메모리 관리 기법이다.
- Page의 주소를 찾기 위한 방법
    - Translation look-aside buffer (TLB)
- Page를 구분하기 위한 방법
    - Hirerachical Paging
    - Hashed Paging
    - Inverted page table
- IA-32, x86-64, ARMv8의 주소변환 구조

# 9.1 Background
- Basic Hardware
    + CPU는 Main memory와 Register에만 접근할 수 있다.
    + Main Memory는 메모리에 접근하기 위해 많은 Cycle을 소모한다. 이로 인해 Processor Stall이 발생하는데 해당 문제를 해결하기 위해 Cache를 사용한다.
    + User Process 별로 자신의 영역을 보호 받아야한다.
        + 이런 문제를 해결하기 위해 각 프로세스별로 분리된 메모리 공간을 할당한다.
        + 가장 많이 사용하는 방법 : Base register, Limit register를 사용한다.
        ![image](https://github.com/WoogiBoogi1129/Major-Courses/assets/110087545/e9fe2f75-16a0-4625-b9b9-f6da527f28c6)
        ![image](https://github.com/WoogiBoogi1129/Major-Courses/assets/110087545/b73c40ea-67c1-4389-84bf-1aa71b05ff6f)
- Address Binding
    + 실행 코드에서 사용되는 주소와 물리적인 주소를 어떻게 연결할 것인가?
    + 일반적인 실행 프로그램은 실행 파일을 만들고 -> 프로세서를 실행하는 방식이다.
    ![image](https://github.com/WoogiBoogi1129/Major-Courses/assets/110087545/d67a9e3a-801a-460b-9893-f1d844426325)
    + Compile Time : 소스코드에서 오브젝트 파일을 만들기까지의 시간
        + 주소가 바뀌면 다시 컴파일 해주어야한다.
    + Load Time : Linker를 통해 실행 파일을 만들고 Loader를 준비하기까지의 시간
        + 이 때 물리적인 주소가 결정된다.
    + Execution Time : 프로그램이 실행되는 시간