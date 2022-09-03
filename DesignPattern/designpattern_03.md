# Design Pattern_03

## Singleton Pattern

![Singleton pattern](https://refactoring.guru/images/patterns/content/singleton/singleton.png)

> Singleton is a creational design pattern,
> 
> which ensures that only one objects of its kind exists
> 
> and provides a single point of access to it for any other code.

<br />

싱글톤 패턴은 다음 **두 가지 문제를 동시에 해결한다.**

1. 클래스가 단 하나의 인스턴스를 갖는 것을 보장
   
   - 왜 클래스가 갖는 인스턴스의 수를 컨트롤하고 싶어할까?
     
     - 가장 흔한 이유는, **공유된 자원(데이터베이스, 파일)에 대한 접근을 컨트롤하기** 위함.
   
   - 싱글톤은 새로운 객체를 생성하는 대신, 이전에 생성해둔 객체를 획득하는 방법으로 동작한다.
     
     - 일반적인 생성자를 이용하는 경우, 이러한 동작을 구현할 수 없다는 것을 기억하자.
     
     - 이유는, 일반적인 생성자를 호출하면 **반드시** 새로운 객체를 리턴하도록 디자인되었기 때문.

2. 인스턴스에 대해 **global access point를** 제공한다.
   
   - 전역 변수를 사용하면 편리하겠지만 다른 코드가 해당 변수의 내용을 덮어쓸 수 있고, 이렇게 되면 앱이 충돌할 가능성이 생기기 마련이다.
   
   - Singleton 패턴은 프로그램의 어느 곳에서나 일부 개체에 접근할 수 있지만 다른 코드가 해당 인스턴스를 덮어쓰지 못하게 보호한다.
   
   - 또한, 1번 문제를 해결하는 코드가 프로그램 전체에 흩어지지 않고 하나의 클래스 안에 응집되어 있도록 설계할 수 있다.

<br />

- 싱글톤 패턴의 구현은 보통 두 단계로 이루어진다.
  
  - **default constructor를 private으로 설정한다.**
  - 생성자로서 역할하는 static creation method를 만들고, 인스턴스 호출시 캐싱된 객체를 리턴하도록 한다.

<br />

### Naïve Singleton

```python
class SingletonMeta(type):
    """
    싱글톤 패턴은 base class, decorator, metaclass를 포함한 방법 등 다양하게 구현될 수 있지만,
    본 예제에서는 metaclass를 이용한 방법을 설명합니다.        
    """
    _instances = {}

    def __call__(cls, *args, **kwargs):
        """
        `__init__` 생성자의 argument 변경은 반환된 인스턴스에 영향을 미치지 못합니다.
        """
        if cls not in cls._instances:
            instance = super().__call__(*args, **kwargs)
            cls._instances[cls] = instance
        return cls._instances[cls]


class Singleton(metaclass=SingletonMeta):
    def some_business_logic(self):
        pass


if __name__ == '__main__':
    s1 = Singleton()
    s2 = Singleton()

    if id(s1) == id(s2):
        print('두 변수는 동일한 인스턴스를 담고 있네요.')
    else:
        print('두 변수가 담고있는 인스턴스가 달라요.')
```

<br />

- 단점
  - 두 가지 문제를 동시에 해결한다는 것은 *단일 책임 원칙을* 위반했다는 이야기.
  
  - multithreaded 환경(e.g., 채팅)에서는 특별한 관리가 필요하다.
    
    - multiple thread는 creation method를 동시에 호출할 가능성이 있으므로 객체가 하나뿐이라는 것을 보장하지 못하는 것.
  
  - unit test가 어렵다.
    
    - 많은 test framework가 mock objects의 생성 시 상속에 의존하기 때문.
    
    - the singleton을 mock 하기 위한 creative way를 생각해야 한다.
    
    - 결국, 테스트를 작성하지 않거나 싱글톤을 쓰지 않아야 하는 상황이 찾아온다.

<br />

### Thread-safe Singleton

```python
from threading import Lock, Thread


class SingletonMeta(type):
    """
    thread-safe Singleton 구현입니다.     
    """
    _instances = {}
    _lock: Lock = Lock()

    def __call__(cls, *args, **kwargs):
        """
        `__init__` 생성자의 argument 변경은 반환된 인스턴스에 영향을 미치지 못합니다.
        """
        # Now, imagine that the program has just been launched. Since there's no
        # Singleton instance yet, multiple threads can simultaneously pass the
        # previous conditional and reach this point almost at the same time. The
        # first of them will acquire lock and will proceed further, while the
        # rest will wait here.
        with cls._lock:
            # The first thread to acquire the lock, reaches this conditional,
            # goes inside and creates the Singleton instance. Once it leaves the
            # lock block, a thread that might have been waiting for the lock
            # release may then enter this section. But since the Singleton field
            # is already initialized, the thread won't create a new object.
            if cls not in cls._instances:
                instance = super().__call__(*args, **kwargs)
                cls._instances[cls] = instance
        return cls._instances[cls]


class Singleton(metaclass=SingletonMeta):
    value: str = ''

    def __init__(self, value: str) -> None:
        self.value = value

    def some_business_logic(self):
        pass


def test_singleton(value: str) -> None:
    singleton = Singleton(value)
    print(singleton.value)


if __name__ == '__main__':
    print("If you see the same value, then singleton was reused (yay!)\n"
          "If you see different values, "
          "then 2 singletons were created (booo!!)\n\n"
          "RESULT:\n")

    process1 = Thread(target=test_singleton, args=("FOO",))
    process2 = Thread(target=test_singleton, args=("BAR",))
    process1.start()
    process2.start()
```

<br />

### Reference

[Singleton](https://refactoring.guru/design-patterns/singleton)

<br />

## Memory Management

### Memory life cycle

Regardless of the programming language, the memory life cycle is pretty much always the same:

1. Allocate the memory you need
2. Use the allocated memory (read, write)
3. Release the allocated memory when it is not needed anymore

The second part is explicit in all languages. The first and last parts are explicit in low-level languages but are mostly implicit in high-level languages like JavaScript.

<br />

### GC(Garbage Collection)

[Garbage Collection in JS](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_Management)

- Low-level languages like C, have manual memory management primitives such as malloc() and free().
- In contrast, JavaScript automatically allocates memory when objects are created and frees it when they are not used anymore (*garbage collection*)
- Release when the memory is not needed anymore
  - The majority of memory management issues occur at this phase.
  - The most difficult aspect of this stage is determining when the allocated memory is no longer needed.
  - Low-level languages require the developer to manually determine at which point in the program the allocated memory is no longer needed and to release it.

<br />

#### References

The main concept that garbage collection algorithms rely on is the concept of *reference*. Within the context of memory management, an object is said to reference another object if the former has access to the latter (either implicitly or explicitly). For instance, a JavaScript object has a reference to its [prototype](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain) (implicit reference) and to its properties values (explicit reference).

In this context, the notion of an "object" is extended to something broader than regular JavaScript objects and also contain function scopes (or the global lexical scope).

<br />

#### Reference-counting garbage collection

This is the most naive garbage collection algorithm. This algorithm reduces the problem from determining whether or not an object is still needed to determining if an object still has any other objects referencing it. An object is said to be "garbage", or collectible if there are zero references pointing to it.

<br />

### ARC(Automatic Reference Counting)

[ARC in Swift](https://docs.swift.org/swift-book/LanguageGuide/AutomaticReferenceCounting.html)

- Reference cycle이 형성되어 메모리 누수가 생기는 것을 방지하기 위해 `Strong reference`와 `Weak reference`를 사용한다.
  - Strong reference로만 이루어진 경우, 실제로 더이상 참조되지 않는 메모리임에도 reference count가 0이 되지 않을 수 있다.
- GC는 runtime 내내 CPU와 Memory를 점유하며 더이상 참조되지 않는 메모리를 tracing하지만, ARC는 컴파일되는 시점에 이를 파악하므로 성능 면에서 유리하다.
- 이때문에, 데스크탑보다 하드웨어 자원이 제한적인 모바일 환경에서 퍼포먼스 향상을 위해 Apple은 GC를 사용하지 않고 ARC를 사용한다.
