# Operating System_01

-   **A layer** of software between **application programs and the hardware**
-   Three purposes
    -   **Easy of use**: Provide applications with simple and uniform interface to manipulate complicated and often widely different low-level hardware devices
    -   **Sharing**: Share the hardware resource from multiple processes/users and increase the resource utilization
        -   컴퓨터의 뇌: **CPU + Memory**
        -   나머지는 모두 **Input/Output**
    -   **Protection**: Protect the hardware from misuse by runaway applications
-   Commercial operating systems usually provide not only **kernels** but also system
-   프로그램을 실행할 수 있다면 **광의의 컴퓨터**(메모리로부터 값을 읽어들이고, 연산을 한 후 메모리에 결과값을 쓴다.)라고 할 수 있다.
-   An OS is a program that provides convenient services for application programs by managing hardware resources and acts as an interface between applications and computer hardware

## Key Interfaces

-   **Instruction set architecture (ISA)**

    -   Define the interface between SW and HW

        -   A set of machine language instructions
        -   Both application programs and utilities can access the ISA directly

    -   Instruction?

        -   ADD, SUB, MUL, DIV, SQR, AND, OR, NAND, LOAD, STORE, ...

        -   ADD R1 <- R2, R3 (R2와 R3의 값을 더해서 R1에 저장)

-   Application binary interface (ABI)
    -   Define the system call interface to OS
-   Application programming interface (API)
    -   Define the program call interface to system services. System calls are performed through library calls

## Terminology

-   Microprocessor: a single chip processor
-   ISA (Instruction Set Architecture)
    -   Defines machine instructions and visible machine states (registers + memories)
    -   Examples
        -   **x86 family** : Intel Pentium, Pentium Pro, Pentium 4, AMD Athlon
        -   **x86-64 family** : Intel i3, i5, i7, i9
        -   **ARM family** : ARM7, ARM11, ARM Cortex-A53
-   Microarchitecture
    -   Implementation: HW design and implementation according to the ISA
        -   Pipelining, caches, branch prediction, buffers, out-of-order execution
-   CISC (Complex Instruction Set Computer)
    -   Each instruction is complex
    -   A large number of instructions in ISA
    -   Architectures until mid 80's
-   RISC (Reduced Instruction Set Computer)
    -   Each instruction is simple
    -   A small number of instructions in ISA
    -   Load-store architectures
        -   Computations are allowed only on registers
            -   Data must be transferred to registers before computation

-   Word
    -   Default data size for computation
    -   The word size determines if a processor is a 8b, 16b, 32b or 64b processor
-   Address (or pointer)
    -   Points to a location in memory
    -   Each address points to a byte (byte addressable)
-   Caches
    -   Faster but smaller memory close to processor