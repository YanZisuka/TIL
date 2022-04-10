# Operating System_06

## Chapter 6. Process Synchronization

-   shared data의 concurrent access는 데이터의 inconsistency를 발생시킬 수 있다.
-   consistency 유지를 위해서는 cooperating process 간의 orderly execution을 정해주는 메커니즘이 필요
-   race condition을 막기 위해서 concurrent process는 synchronize 되어야 한다.

### Process Interaction

#### In a single-processor system,

-   Process executions are interleaved to increase CPU utilization
-   The relative speed of execution of processes cannot be predicted
    -   It depends on the activities of other processes, the way OS handles interrupts, and the scheduling policies
-   The following difficulties arise
    -   Mutual exclusion
    -   Deadlock (모든 프로세스들이 다른 프로세스의 자원을 할당받기 위해 대기하는 상태)
    -   Starvation (자원을 요청한 프로세스가 소외되어 계속 대기하는 상태)
    -   Race condition


#### The same problems exist in a multiprocessor system



### Mutual exclusion

-   Suppose two processes require access to a single printer
-   Printer is a nonsharable resource
    -   Without care, line from competing processes will be interleaved
-   During the course of execution, only one process should be allowed to access the resource at a time.
    -   The portion of program that accesses the resource is called a ***critical section*** of the program.
    -   Only one process at a time allowed in its critical section

### Deadlock

-   Consider two processes P1 and P2, and two resources R1 and R2
-   Each process needs to access both resources to complete its function
-   Suppose the following scenario:
    -   OS assigns R1 to P2 and R2 to P1
    -   Each process is waiting for the other resource
    -   Neither will release the resource until it acquires the other resource
    -   Two processes are deadlocked!

### Starvation (Indefinite postponement)

-   Consider 3 processes P1, P2, and P3.
-   Each process requires access to resource R
-   Suppose the following scenario:
    -   P1 has R and both P2 and P3 wait for R
    -   OS grants access to P3, then P1, then P3, ...
    -   P2 may be indefinitely postponed to access the resource



### Cooperating Processes

#### 독립적 프로세스 (Independent process)

-   프로세스는 각자의 주소 공간을 가지고 수행되므로 원칙적으로 하나의 프로세스는 다른 프로세스의 수행에 영향을 미치지 못함

#### 협력 프로세스 (Cooperating process)

-   프로세스 협력 메커니즘을 통해 하나의 프로세스가 다른 프로세스의 수행에 영향을 미칠 수 있음

#### 프로세스간 협력 메커니즘 (IPC: Interprocess Communication)

-   메시지를 전달하는 방법

    -   Message passing: kernel을 통해 메시지 전달

        -   Message system: 프로세스 사이에 공유 변수(shared variable)를 일체 사용하지 않고 통신하는 시스템

        -   Direct communication: 통신하려는 프로세스의 이름을 명시적으로 표시

        -   Indirect communication: mailbox (or port)를 통해 메시지를 간접 전달

-   주소 공간을 공유하는 방법

    -   Shared memory: 서로 다른 프로세스 간에도 일부 주소 공간을 공유하게 하는 shared memory 메커니즘이 존재

        ![image-20220330011359073](operating_system_06.assets/image-20220330011359073.png)

-   **Thread**

    -   Thread는 사실상 하나의 프로세스이므로 프로세스간 협력으로 보기는 어렵지만 동일한 process를 구성하는 thread들 간에는 주소 공간을 공유하므로 협력 가능

#### Sharing of global data may lead to *race condition*



### Race condition

|       Execution-box        |      Storage-box       |
| :------------------------: | :--------------------: |
|            CPU             |         Memory         |
| 컴퓨터 내부 (CPU & Memory) |  디스크 (I/O Device)   |
|          프로세스          | 그 프로세스의 주소공간 |

-   S-box를 공유하는 E-box가 여럿 있는 경우 **Race Condition의** 가능성이 있음
-   커널모드 수행 중 인터럽트로 커널모드의 다른 루틴 수행
    -   커널 모드 수행 중이면 인터럽트 처리 X
-   Process가 system call을 하여 kernel mode로 수행 중인데 context switching이 일어나는 경우
    -   커널 모드 수행 중이면 Preempted X
-   Multiprocessor에서 shared memory 내의 kernel data
    -   한번에 하나의 CPU만이 커널모드로 진입할 수 있게 하는 방법 (비효율적)
    -   커널 내부에 있는 각 공유 데이터에 접근할 때마다 그 데이터에 대한 lock/unlock을 수행
-   **The problem can be avoided by declaring the entire sequence in each process to be a critical section**



### The Critical-Section Problem

-   n개의 프로세스가 공유 데이터를 동시에 사용하기를 원하는 경우
-   각 프로세스의 code segment에는 공유 데이터를 접근하는 코드인 **critical section이** 존재
-   하나의 프로세스가 critical section에 있을 때 다른 모든 프로세스는 그 공간에 접근할 수 없어야 한다.

```pseudocode
do {
	entry section
	critical section
	exit section
	remainder section
} while (1);
```

#### 프로그램적 해결법의 충족 조건

-   **Mutual Exclusion(상호 배제)**
    -   프로세스 Pi가 ciritical section 부분을 수행 중이면 다른 모든 프로세스들은 그들의 critical section에 들어가서는 안된다.
-   **Progress**
    -   아무도 critical section에 있지 않은 상태에서 critical section에 들어가고자 하는 프로세스가 있다면 들어가게 해주어야 한다.
-   **Bounded Waiting**
    -   프로세스가 critical section에 들어가려고 요청한 이후부터 그 요청이 허용될 때까지 다른 프로세스들이 critical section에 들어가는 횟수에 한계가 있어야 한다.
-   가정
    -   모든 프로세스의 수행 속도는 0보다 크다.
    -   프로세스들 간의 상대적인 수행 속도는 가정하지 않는다.

#### Algorithm 1

-   Synchorinization variable

    -   **int turn;**
    -   initially **turn = 0;** => Pi can enter its ciritical section **if (turn == i)**

-   Process P0

    ```pseudocode
    do {
        while (turn != 0);
        critical section
        turn = 1;
        remainder section
    } while (1);
    ```

-   Progress 조건을 만족하지 못한 algorithm

#### Algorithm 2

-   **boolean flag[2];** initially **flag[all] = false;**

-   Pi ready to enter its ciritical section **if (flag[i] == true)**

-   Process Pi

    ```pseudocode
    do {
    	flag[i] = true;
    	while (flag[j]);
    	critical section
    	flag[i] = false;
    	remainder section
    } while (1);
    ```

-   Satisfies mutual exclusion, but *not progress requirement*

-   둘다 2행까지 수행 후 끊임없이 양보하는 상황 발생 가능

#### Algorithm 3 (Peterson's Algorithm)

-   Combined synchronization variables of algorithms 1 and 2

-   Process Pi

    ```pseudocode
    do {
    	flag[i] = true;
    	turn = j;
    	while (flag[j] && turn == j);
    	critical section
    	flag[i] = false;
    	remainder section
    } while (1);
    ```

-   *Meets all three requirements;* solves the critical section problem for two processes

-   **Busy Waiting(=spin lock)!** (계속 CPU와 memory를 점유하면서 wait)



### Synchronization Hardware

-   하드웨어적으로 Test & modify를 **atomic하게** 수행할 수 있도록 지원하는 경우 앞의 문제는 간단히 해결

-   Mutual Exclusion with Test & Set

    ```pseudocode
    Synchronization variable:
    	boolean lock = false;
    	
    Process Pi
    	do {
    		while (Test_and_Set(lock));
    		critical section
    		lock = false;
    		remainder section
    	}
    ```



### Atomic Operation

#### "Atomic" means

-   Indivisible, uninterruptable
-   Must be performed atomically, which means either "success" or "failure"
    -   Success: successfully change the system state
    -   Failure: no effect on the system state

#### Atomic operation

-   A function or action implemented as a single instruction or as a sequence of instructions that appears to be indivisible
-   Can be implemented by HW or by SW
-   HW-level atomic operations
    -   Test-and-set, fetch-and-add, compare-and-swap, load-link/store-conditional
-   SW-level solutions
    -   Running a group of instructions in a *critical section*



### Semaphore

-   A variable that provides a simple abstraction for controlling access to a common resource in a programming environment
-   앞에서 소개된 방식들을 추상화시킨 것
-   The value of the semaphore variable can be changed by only 2 operations
    -   V operation (also known as "signal")
        -   Increment the semaphore
    -   P operation (also known as "wait")
        -   Decrement the semaphore
    -   The value of the semaphore **S** is usually the number of units of the resource that are currently available.

#### Type of Semaphore

-   Binary semaphore
    -   Have a value of 0 or 1
        -   0 (locked, unavailable)
        -   1 (unlocked, available)
-   Counting semaphore
    -   Can have an arbitrary resource count