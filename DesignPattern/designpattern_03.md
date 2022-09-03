# Design Pattern_03

## Singleton Pattern

> Singleton is a creational design pattern, which ensures that only one objects of its kind exists and provides a single point of access to it for any other code.

<br />

- The singleton design pattern solves problems by allowing it to:
  
  - Ensure that a class only has one instance
  - Easily access the sole instance of a class
  - Control its instantiation
  - Restrict the number of instances
  - Access a global variable

<br />

- The singleton design pattern describes how to solve such problems:
  
  - **Hide the constructors of the class**
  - Define a public static operation `getInstance()` that returns the sole instance of the class.

<br />

- e.g., Dark mode는 다른 페이지로 이동하더라도 다크모드가 유지되어야 함

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

- multithread의 경우 오류가 발생할 여지가 있다. 
  - multiple thread는 creation method를 동시에 호출할 가능성이 있으므로 객체가 하나뿐이라는 것을 보장하지 못하는 것.
- 채팅의 경우 web socket을 이용하는데, 이러한 경우도 오류 발생 가능
- 모바일 프레임워크의 경우 Main class가 거의 singleton pattern
- 장점
  - view 간에 값이 변할 수 있는 데이터를 사용할 때, (계산기의 경우 하나하나의 버튼이 subview, subview에서 superview의 데이터를 확인할 방법이 없다.) -> Singleton을 쓰게 되면 delegation pattern을 써서 이를 우회하거나 할 필요가 없다. (생산성 향상)
- 단점
  - test가 어렵다.
    - e.g., 다크모드 토글의 경우 `테스트 코드 A`에서 라이트 -> 다크, `B`는 다크 -> 라이트인 경우 동시에 시행되어 문제 발생 가능

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
