## Deadlock 발생의 4가지 조건

1. Mutual exclusion (상호 배제)
    - 매 순간 하나의 프로세스만 자원을 사용할 수 있다.
2. No preemption (비선점)
    - 프로세스는 자원을 강제로 빼앗기지 않는다.
3. Hold and wait (보유대기)
    - 자원을 가진 프로세스는 다른 자원을 기다릴 때 보유 자원을 놓지 않고 가지고 있는다.
4. Circular wait (순환대기)
    - 자원을 기다리는 프로세스 간에 사이클이 형성되어야 한다.

## Deadlock의 처리 방법

1. Deadlock Prevention
    - 자원 할당 시 Deadlock의 4가지 필요 조건 중 어느 하나가 만족되지 않게 하는 것
2. Deadlock Avoidance
    - Deadlock의 가능성이 없을 때만 자원을 할당함
    - 은행원 알고리즘
        - 프로세스가 필요한 자원의 양보다 Available한 자원이 더 많은 경우에만 자원을 할당해줌
3. Deadlock Detection and recovery
    - Deadlock 발생은 허용하고, 그에 따른 detection 루틴을 두어서 deadlock 발견시 recover
4. **Deadlock Ignorance** (대부분의 OS가 사용하는 방법)
    - Deadlock을 시스템이 책임지지 않음
    - deadlock은 빈번히 발생하는 현상이 아니기 때문에 deadlock 처리 방법의 overhead를 처리하는 cost가 더 높기 때문

## Deadlock Prevention

- Mutual Exclusion
    - 공유해서는 안되는 자원이라면 성립해야됨
- Hold and wait
    - 자원을 요청할 때는 자신이 자원을 가지고 있지 않아야 함
- No Preemption
    - 어떤 자원을 기다려야 할 때는 이미 보유한 자원을 뺏김
- Circular wait
    - 순서를 정해서 순서대로만 자원 할당

⇒ 하지만 이런 방법들은 효율적이지 않고, starvation이 발생할 수 있다.