# Operating System_05

## Chapter 5. CPU Scheduling

### CPU and I/O Bursts in Program Execution

![image-20220330010856249](operating_system_05.assets/image-20220330010856249.png)

### CPU-burst Time의 분포

![image-20220330010917914](operating_system_05.assets/image-20220330010917914.png)

-   여러 종류의 job(=process)이 섞여 있기 때문에 CPU 스케줄링이 필요하다.
-   Interactive job에게 적절한 response 제공 요망
-   CPU와 I/O 장치 등 시스템 자원을 골고루 효율적으로 사용

### 프로세스의 특성

-   프로세스는 그 특성에 따라 다음 두 가지로 나뉨
    -   **I/O-bound process**
        -   CPU를 잡고 계산하는 시간보다 I/O에 많은 시간이 필요한 job
        -   Many short CPU bursts
    -   **CPU-bound process**
        -   계산 위주의 job
        -   few very long CPU bursts

### CPU Scheduler & Dispatcher

-   CPU Scheduler

    -   Ready 상태의 프로세스 중에서 이번에 CPU를 줄 프로세스를 고른다.

-   Dispatcher

    -   CPU의 제어권을 CPU scheduler에 의해 선택된 프로세스에게 넘긴다.
    -   이 과정을 context switching(문맥 교환)이라고 한다.

-   CPU 스케줄링이 필요한 경우는 프로세스에게 다음과 같은 상태 변화가 있는 경우이다.

    1.   Running -> Blocked (ex. I/O 요청하는 시스템 콜)
    2.   Running -> Ready (ex. 할당시간만료로 timer interrupt)
    3.   Blocked -> Ready (ex. I/O 완료 후 인터럽트)
    4.   Terminate

    -   i, iv의 스케줄링은 **nonpreemptive (=자진반납)**
    -   All other scheduling is **preemptive**

### Scheduling Criteria

#### CPU utilization (이용률)

-   Keep the *CPU as busy as possible*

#### Throughput (처리량)

-   *number of processes* that *complete* their execution per time unit

#### Turnaround time (소요시간, 반환시간)

-   amount of time to *execute a particular process*

#### Waiting time (대기 시간)

-   amount of time a process has been *waiting in the ready queue*

#### Response time (응답 시간)

-   amount of time it takes *from when a request was submitted until the first response is produced*, **not** output (for time-sharing environment)



### Scheduling Algorithms

#### FCFS (First-Come First-Served)

![image-20220330010958427](operating_system_05.assets/image-20220330010958427.png)



#### SJF (Shortest-Job-First)

-   각 프로세스의 다음번 CPU burst time을 가지고 스케줄링에 활용
-   CPU burst time이 가장 짧은 프로세스를 제일 먼저 스케줄링
-   Two schemes:
    -   Nonpreemptive
        -   일단 CPU를 점유하면 이번 CPU burst가 완료될 때까지 CPU를 선점당하지 않음
    -   Preemptive
        -   현재 수행중인 프로세스의 남은 burst time보다 더 짧은 CPU burst time을 가지는 새로운 프로세스가 도착하면 CPU를 빼앗김
        -   이 방법을 Shortest-Remaining-Time-First (SRTF)라고도 부른다.
-   SJF is optimal
    -   주어진 프로세스들에 대해 minimum average waiting time을 보장

![image-20220330011022784](operating_system_05.assets/image-20220330011022784.png)