# [2강] System Structure & Program Execution 1

## 컴퓨터 시스템 구조
![](image/컴퓨터 시스템 구조.png)

## I/O 디바이스 (키보드, 프린트, 모니터 등) 구조

- Disk를 I/O 디바이스로 볼 수도 있음 (파일 시스템에 입력하고 출력하기 때문)
- CPU에게 작업 공간인 Memory가 있듯이, I/O 디바이스들도 작업 공간이 있음(초록색 상자) → local buffer
- device driver는 CPU 역할(하드웨어), device driver는 소프트웨어임

## CPU의 구조와 역할

- CPU는 메모리에서 기계어로 된 명령어(instruction) 하나씩 읽는 역할
    - instruction을 하나 실행하면, interrupt line을 체크함(**interrupt 들어온게 있나 체크**)
    - interrupt가 있으면 하던 일 멈추고 CPU 소유권이 **사용자 프로그램으로부터 운영체제**한테 넘어감
- CPU 안에는 메모리보다 빠르고, 정보를 저장할 수 있는 **레지스터**가 있음
- 만약 사용자 프로그램 A에서 Disk에서 파일을 읽어오라는 명령이 있다면 어떻게 처리할까?
    - CPU가 직접 Disk에 접근하지 않고, Disk controller에 일을 시킴(instruction이 있음) → Disk는 읽어서 자신의 local buffer에 넣음
    - Disk가 읽는 동안, CPU는 다시 메모리 접근을 함
        - 근데 일반적으로 Disk에서 값을 읽어온 것으로 그 뒤에 일을 하지않나? → Disk controller한테 작업 끝나면 알려달라함 → 다른 일 함(다른 프로그램(프로그램 B)으로 넘어갈 수도 있음)

### Mode bit

- CPU가 실행하는 것이 **운영체제인지, 사용자의 프로그램인지 구분**해줌
    - 1: 사용자 모드 - 사용자 프로그램 수행
    - 0: 커널 모드 - OS 코드 수행

### Timer

- 만약 프로그램 A가 무한루프를 돈다면? → 이때 timer에 세팅된 시간이 되면 interrupt 를 통해 시간 끝났다고 알려줌
- 매 클럭 틱 때마다 1씩 감소
- 타이머 값이 0이 되면 타이머 인터럽트 발생
- CPU 독점 보호

## DMA Controller (Direct Memory Access Controller)

그렇다면 키보드 입력마다 interrupt가 발생하면, CPU는 매번 멈췄다 일했다 하는데, 오버헤드가 많이 생기지 않을까?

- DMA 뜻만 봐도, 메모리에 직접 접근할 수 있는 controller임
- 원래는 메모리에는 CPU만 접근할 수 있는데, DMA도 접근할 수 있게 함
    - 그래서 memory controller는 CPU와 DMA 동시성 문제 해결하는 용도

<aside>
💡 CPU가 device controller로부터 받은 데이터를 memory에 copy하는 일이 오버헤드가 너무 크기 때문에 **local buffer에 들어온 작업이 끝나면 메모리에 복사하는 역할**을 DMA Controller가 함

</aside>

## 인터럽트 (Interrupt)

### 하드웨어 인터럽트 (Interrupt)

- 일반적으로 말하는 인터럽트
- device controller들이 거는 인터럽트

### 소프트웨어 인터럽트 (Trap)

- 사용자 프로그램이 거는 인터럽트
- 프로그램이 오류(Exception)를 범하거나, 프로그램이 커널 함수를 호출(System call)하는 경우

### 인터럽트 벡터

- 해당 인터럽트의 처리 루틴 주소를 가짐

### 인터럽트 서비스 루틴 (Interrupt Service Routine, 인터럽트 핸들러)

- 인터럽트의 종류가 여러가지라 종류마다 호출하는 함수가 다름 (timer, device controller 등등)
- 해당 인터럽트를 처리하는 커널 함수

### I/O 요청을 할 때는 소프트웨어 인터럽트? 하드웨어 인터럽트?

1. 사용자 프로그램이 Interrupt line을 세팅함(소프트웨어 인터럽트)
2. CPU는 매번 Interrupt line을 확인하기 때문에 Interrupt가 있는 것을 확인함
3. mode bit 변경 후 OS에게 CPU 제어권이 넘어감
4. OS가 device controller에게 일 시킴
5. 일이 끝나면 CPU에게 일 다 했다고 알려줌(하드웨어 인터럽트)

⇒ 즉, 두 인터럽트 모두 사용됨