# Chapter 9. Main Memory

## Logical vs. Physical Address

-   주소 바인딩: 주소를 결정하는 것
    -   Symbolic Address -> Logical Address -> Physical Address



### Logical address (=Virtual address)

-   프로세스마다 독립적으로 가지는 주소 공간
-   각 프로세스마다 0번지부터 시작
-   CPU가 보는 주소



### Physical address

-   메모리에 실제 올라가는 위치



## Address Binding

### Compile time binding

-   physical address가 컴파일 시 알려짐
-   시작 위치 변경시 재컴파일
-   컴파일러는 **절대 코드(absolute code)** 생성



### Load time binding

-   Loader의 책임하에 물리적 메모리 주소 부여
-   컴파일러가 **재배치가능 코드(relocatable code)를** 생성한 경우 가능



### Execution time binding (=Run time binding)

-   수행이 시작된 이후에도 프로세스의 메모리 상 위치를 옮길 수 있음
-   CPU가 주소를 참조할 때마다 binding을 점검 (address mapping table)
-   하드웨어적 지원이 필요
    -   e.g., base and limit registers, MMU



### Memory-Management Unit (MMU)

-   logical address를 physical address로 매핑해주는 Hardware device
-   MMU scheme
    -   사용자 프로세스가 CPU에서 수행되며 생성해내는 모든 주소값에 대해 base register (=relocation register)의 값을 더한다.
-   User program
    -   logical address만을 다룬다.
    -   실제 physical address를 볼 수 없으며, 알 필요가 없다.

## Dynamic Relocation

![image-20220501104451363](operating_system_08.assets/image-20220501104451363.png)

![image-20220501104642296](operating_system_08.assets/image-20220501104642296.png)

-   Relocation register(=base register)
    -   접근할 수 있는 물리적 메모리 주소의 최소값
-   Limit register
    -   논리적 주소의 범위(=user program이 사용하는 메모리 주소의 범위)



## Dynamic Loading

-   프로세스 전체를 메모리에 미리 다 올리는 것이 아니라 해당 루틴이 불려질 때 로드하는 것
    -   Loading: 메모리로 올리는 것
-   Memory utilization의 향상
-   가끔씩 사용되는 많은 양의 코드인 경우 유용
    -   e.g., 오류 처리 루틴
-   운영체제의 특별한 지원 없이 프로그램 자체에서 구현 가능 (OS는 라이브러리를 통해 지원 가능)



## Overlays

-   메모리에 프로세스의 부분 중 실제 필요한 정보만을 올림
-   프로세스의 크기가 메모리보다 클 때 유용
-   운영체제의 지원없이 사용자에 의해 구현
-   작은 공간의 메모리를 사용하던 초창기 시스템에서 수작업으로 프로그래머가 구현
    -   "Manual Overlay"
    -   프로그래밍이 매우 복잡



## Swapping

-   프로세스를 일시적으로 메모리에서 backing store로 쫓아내는 것
-   **Backing store (=swap area)**
    -   디스크
    -   많은 사용자의 프로세스 이미지를 담을 만큼 충분히 빠르고 큰 저장 공간
-   **Swap in / Swap out**
    -   일반적으로 중기 스케줄러(swapper)에 의해 swap out 시킬 프로세스 선정
    -   Priority-based CPU scheduling algorithm
        -   priority가 낮은 프로세스를 swapped out 시킴
        -   priority가 높은 프로세스를 메모리에 올려놓음
    -   Compile time 혹은 load time binding에서는 원래 메모리 위치로 swap in 해야 함
    -   Execution time binding에서는 추후 빈 메모리 영역 아무 곳에나 올릴 수 있음
    -   Swap time은 대부분 transfer time (swap되는 양에 비례하는 시간)임



## Dynamic Linking

-   Linking을 실행시간(execution time)까지 미루는 기법
-   **Static linking**
    -   라이브러리가 프로그램의 실행 파일 코드에 포함됨
    -   실행 파일의 크기가 커짐
    -   동일한 라이브러리를 각각의 프로세스가 메모리에 올리므로 메모리 낭비 (e.g., `printf` 함수의 라이브러리 코드)
-   **Dynamic linking**
    -   라이브러리가 실행시 연결됨
    -   라이브러리 호출 부분에 라이브러리 루틴의 위치를 찾기 위한 stub이라는 작은 코드를 둠
    -   라이브러리가 이미 메모리에 있으면 그 루틴의 주소로 가고 없으면 디스크에서 읽어옴
    -   운영체제의 도움이 필요



## Allocation of Physical Memory

-   메모리는 일반적으로 두 영역으로 나뉘어 사용
-   **OS 상주 영역**
    -   interrupt vector와 함께 낮은 주소 영역
-   **사용자 프로세스 영역**
    -   높은 주소 영역
-   사용자 프로세스 영역의 할당 방법
    -   **Contiguous allocation**
        -   각각의 프로세스가 메모리의 연속적인 공간에 적재되도록 하는 것
        -   Fixed partition allocation
        -   Variable partition allocation
    -   **Noncontiguous allocation**
        -   하나의 프로세스가 메모리의 여러 영역에 분산되어 올라갈 수 있음
        -   Paging
        -   Segmentation
        -   Paged Segmentation



### Contiguous Allocation

-   Hole

    -   가용 메모리 공간
    -   다양한 크기의 hole들이 메모리 여러 곳에 흩어져 있음
    -   프로세스가 도착하면 수용가능한 hole을 할당
    -   운영체제는 다음의 정보를 유지
        -   할당 공간
        -   가용 공간(hole)

-   **Dynamic Storage-Allocation Problem**

    -   가변 분할 방식에서 size n인 요청을 만족하는 가장 적절한 hole을 찾는 문제

    -   **First-fit**
        -   Size가 n 이상인 것 중 최초의 hole에 할당
    -   **Best-fit**
        -   Size가 n 이상인 가장 작은 hole을 찾아서 할당
        -   Hole들의 리스트가 크기순으로 정렬되지 않은 경우 모든 hole의 리스트를 탐색해야 함
        -   많은 수의 아주 작은 hole들이 생성됨
    -   **Worst-fit**
        -   가장 큰 hole에 할당
        -   모든 리스트를 탐색해야 함
        -   상대적으로 아주 큰 hole들이 생성됨
    -   First-fit과 best-fit이 worst-fit보다 속도와 공간 이용률 측면에서 효과적인 것으로 알려짐 (실험적 결과)

-   **Compaction**

    -   External fragmentation 문제를 해결하는 한 가지 방법
    -   사용 중인 메모리 영역을 한군데로 몰고 hole들을 다른 한 곳으로 몰아 큰 block을 만드는 것
    -   매우 비용이 많이 드는 방법
    -   최소한의 메모리 이동으로 compaction하는 방법 (매우 복잡한 문제)
    -   Compaction은 프로세스의 주소가 실행 시간에 동적으로 재배치 가능한 경우에만 수행될 수 있다.

#### Fixed partition allocation

-   External fragmentation
    -   분할된 곳이 작아 프로그램이 올라가지 못해 남은 잉여 공간
-   Internal fragmentation
    -   분할된 곳에 프로그램이 들어가고 남은 잉여 공간

#### Variable partition allocation

![image-20220501111258181](operating_system_08.assets/image-20220501111258181.png)



### Paging

-   Process의 virtual memory를 동일한 사이즈의 page 단위로 나눔
-   Virtual memory의 내용이 page 단위로 *noncontiguous* 하게 저장됨
-   일부는 backing storage에, 일부는 physical memory에 저장

#### Basic Method

-   physical memory를 동일한 크기의 `frame`으로 나눔
-   logical memory를 동일 크기의 `page`로 나눔 (frame과 같은 크기)
-   모든 가용 frame들을 관리
-   page table을 사용하여 logical address를 physical address로 변환
-   External fragmentation 발생 안함
-   Internal fragmentation 발생 가능







