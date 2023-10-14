# [5장] CPU Scheduling

### CPU Scheduling, 왜 필요할까?

- I/O bound job, 즉 I/O 요청이 빈번하게 일어나는 경우가 있다면, CPU를 우선적으로 주는 것이 효율적임 (사람과의 interection이 많은 경우니까)
- CPU와 I/O 장치 등 시스템 자원을 골고루 효율적으로 사용하기 위해

## CPU Scheduler & Dispatcher

CPU Scheduler

- 운영체제 안에서 스케줄링하는 **코드**임
- Ready 상태의 프로세스 중에 누구에게 줄 지 결정

Dispatcher

- 마찬가지로 운영체제의 코드
- CPU를 실제로 주는 일을 함 (context switch)

### CPU 스케줄링은 언제 필요한가?

프로세스에게 아래와 같은 **상태 변화**가 있는 경우 필요하다

1. Running → Blocked (ex. I/O 요청하는 시스템 콜) - **nonpreemptive**
2. Running → Ready (ex. timer 인터럽트) - **preemptive**
3. Blocked → Ready (ex. I/O 완료 후 인터럽트) - **preemptive**
4. Terminate - **nonpreemptive**

## Scheduling Criteria

### 시스템 입장에서의 성능 척도

최대한 일 많이 시키기

- CPU utilization (이용률)
    - 가능한 CPU를 계속 일하게 하는 것
- Throughput (처리량)
    - 주어진 시간 동안 처리한 일의 양

### 프로그램(사용자) 입장에서의 성능 척도

최대한 빠르게 처리하기

- Turnaround time (소요 시간)
    - 쓰기 시작 ~ 다 쓰고 나간 시간, 기다린 시간 포함
- Waiting time (대기 시간)
    - ready queue에서 기다린 시간
- Response time (응답 시간)
    - 처음 CPU를 얻기까지 걸린 시간

## CPU Scheduling Algorithm

- FCFS (First-Come First-Served)
    - **Convoy effect**: short process들이 기다리게 됨
- SJF (Shorted-Job-First)
    - CPU burst가 제일 짧은 애한테 주자
    - Nonpreemptive
        - CPU 를 잡으면 CPU burst가 끝날 때까지 preemption당하지 않음
    - Preemptive
        - 현재 수행 중인 프로세스보다 짧은 프로세스가 오면 CPU 뺏김
        - Shortest-Remaining-Time-First(SRTF)라고도 불림
    - 문제점
        - Long process의 **starvation** 문제
        - 실제로는 CPU burst time을 예측할 수 없음
            - 과거를 통해 예측함
- Priority Scheduling
    - SJF도 사실상 Priority Scheduling이라고 할 수 있음
        - Priority의 기준이 CPU burst time인 경우임
    - starvation 문제 해결
        - aging: 시간이 지날수록 우선순위 높임
- **Round Robin (RR)**
    - 할당 시간(time quantum)을 가짐
    - **응답 시간(Response time), 즉 처음으로 CPU얻는 시간이 빨라짐**
    - 어떤 프로세스도 특정 시간 이상 기다리지 않음
        - 할당 시간 때문에
    - 극단적인 상황이라면
        - q가 길면 → FCFS
        - q가 짧으면 → **context switch 오버헤드가 커짐**
- Multilevel Queue
    - 이전까진 줄이 한 줄이었는데 우선순위 별로 줄을 여러 개 두고, 제일 높은 우선순위부터 순차적으로 진행됨
    - 각 큐는 독립적인 스케줄링 알고리즘을 가짐
- Multilevel Feedback Queue
    - 우선순위별로 queue를 두지만, 우선순위가 바뀌면서 할당이 됨
    - 그렇다면 기준이 무엇일까?
        - 처음 들어오는 프로세스는 우선순위 높게 줌, 하지만 time quantum을 짧게 줌
        - 우선순위가 낮아질수록 time qunatum이 길어짐

## Multiple-Processor Scheduling

- Homogeneous processor인 경우
    - queue에 한 줄로 세워서 각 프로세서가 알아서 처리
- Load sharing
    - 특정 프로세서에 job이 몰리지 않도록 부하를 적절히 두어야 함
- Symmetric Multiprocessing
    - 각 프로세서가 알아서 스케줄링 결정
- Asymmetric multiprocessing
    - 하나의 프로세서가 책임지고 다른 프로세서는 따라감

## Thread Scheduling

- Local Scheduling
    - 사용자 프로그램의 프로세스가 알아서 스레드 스케줄링 (어떤 스레드에게 CPU 줄 지 결정)
- Global Scheduling
    - 커널이 스레드 스케줄링