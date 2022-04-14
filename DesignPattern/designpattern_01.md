# DesignPattern_01

![image-20220407221021381](designpattern_01.assets/image-20220407221021381.png)

## Class Diagram

-   클래스 다이어그램은 구조 다이어그램으로 **클래스 내부 구성요소 및 클래스 간의 관계를 도식화하여 시스템의 특정 모듈이나 일부 및 전체를 구조화한다.**
-   시스템 내 클래스 간의 **의존성 파악과 팀원들 간 의사소통이 편리하다.**



### Elements

#### Class

![img](https://blog.kakaocdn.net/dn/dpDiUW/btrb8T3sAU9/USvKxTjpTNuNzT4drkr3ek/img.png)

-   클래스는 **이름, 속성(변수), 메서드 순으로 나열**

-   `+`: public, `#`: internal, `-`: private
    
    - 자세한 내용은 하단에 Access Level 참고
    
    ```typescript
    class Base {
        public defaultAge = 30;
        protected birthYear = 1989;
        private identifier = '890101-1XXXXXX';
    }
    
    class Member extends Base {
        age = 1;
        
        public getAge() {
            return this.age + this.defaultAge;
        }
        
        protected getBirthYear() {
            return this.birthYear
        }
        
        private getID() {
            return this.identifier;  // Access denied
        }
    }
    
    let member = new Member();
    console.log(member.getAge());  // 31
    console.log(member.getBirthYear());  // Access denied
    console.log(member.getID());  // Access denied
    ```
    
    
    
-   `[*]`나 `[0...1]`은 리스트와 같은 변수에 지정된 사이즈를 뜻한다.

-   속성(변수)
    -   `{Access level} {field name}: {type}`

**Access Level (Swift 기준)**

| Access Level | Symbol | Description |
| --- | --- | --- |
| open | `++` | public + Overridable |
| public | `+` | 모듈을 import 한 모든 파일에서 접근 가능 |
| internal | `~` or `#` | 모듈 안에 정의된 모든 파일에서 접근 가능 |
| fileprivate | `-~` | 정의된 파일 내에서만 접근 가능 |
| private | `-` | 선언부 및 같은 파일내의 extension 에서 접근 가능 |


#### Stereo Type

![img](https://blog.kakaocdn.net/dn/UBY4t/btrb7uC3HQ4/gec0YzkdtvJ8urNOh8Y4vk/img.png)

-   인터페이스나 추상 클래스와 같은 요소를 표기하기 위해 `<<...>>`와 같은 문법을 사용하는데 이를 **길러멧(guillemet)이라 한다.**
-   길러멧은 보통 인터페이스, enum, 추상 클래스 등에서 사용되지만 확장 클래스를 의미하는 데에도 사용될 수 있다.
    - `static` 프로퍼티 혹은 함수를 나타낼 때 사용하기도 한다. e.g., `+ <<static>> shared: SingletonClass`


#### Abstract Class

![img](https://blog.kakaocdn.net/dn/tpAPO/btrcc4QUCCx/OmY8m5pqy9s1dPMLBpq4Mk/img.png)

-   추상 클래스를 나타내는 방법은 총 3가지로 *Italic으로* 표시하거나 클래스 명에 {abstract}, 혹은 길러멧으로 표시할 수 있다.



### Class Relationship

![img](https://blog.kakaocdn.net/dn/Tt5OY/btrci8ZA5yJ/BclKSGo2XXxRHGIcqZIZdk/img.png)

**Easy-read**

| Shape | Meaning | How to say | 
| --- | --- | --- |
| plain arrowhead | Association | `has a` |
| open arrowhead | Inheritance | `is a` |
| open arrowhead with a dashed line | Implementation | `implements` or `conforms to` |
| plain arrowhead with a dashed line| Dependency | `uses` |

**Use cases of a plain arrowhead with a dashed line**

- A weak property.
- An object that’s passed into a method as a parameter, but not held as a property.
- A loose coupling or callback.


### Additional

**How to draw the NESTED CLASS?**

<details>
Hint: using a dot
</details>
 
