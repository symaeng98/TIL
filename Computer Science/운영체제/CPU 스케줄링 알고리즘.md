# CPU 스케줄링 알고리즘

## 비선점형 방식

프로세스가 스스로 CPU 소유권을 포기하는 방식

### FCFS (First Come, First Served)

- 길게 수행되는 프로세스 있으면 ready queue에서 오래 기다림 - **convoy effect**

### SJF (Shortest Job First)

- 긴 시간이 걸리는 프로세스가 실행되지 않을 수 있음 - **starvation**

### 우선순위

- 오래된 작업일수록 우선순위를 높여서 SJF 보완 - **aging**

## 선점형 방식

프로세스를 중단시켜버리고 다른 프로세스에 CPU 할당

### 라운드 로빈 (RR)

- 동일한 할당 시간을 주고 못 끝내면 다시 ready queue로 가는 알고리즘

### SRF (Smallest Remain Time First)

- 남은 시간이 가장 짧은 작업부터 수행
    - 중간에 더 짧은 작업이 들어오면 그 작업을 수행

### 다단계 큐

- 우선순위에 따라 ready queue를 여러 개 사용해서 큐마다 다른 알고리즘 적용