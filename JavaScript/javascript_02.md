# JavaScript_02

## 숫자와 문자

### 수의 표현

-   자바스크립트에서는 큰따옴표나 작은따옴표가 붙지 않은 숫자는 숫자로 인식한다.

```javascript
alert(1+1);  // 2
alert(1.2+1.3);  // 2.5
console.log(6/2);  // 3
```

### 수의 연산

```javascript
Math.pow(3, 2);  // 9
Math.round(10.6);  // 11
Math.ceil(10.2);  // 11
Math.floor(10.6);  // 10
Math.sqrt(9);  // 3
Math.random();  // 0부터 1.0 사이의 랜덤한 숫자
```

### 문자

-   큰따옴표("")나 작은따옴표('')로 감싸면 문자열로 인식한다.
-   `\'`, `\"`, `\n` 등의 **이스케이프 문자열을 사용 가능**하다.

```javascript
alert('coding everybody!');
alert('hanndrednine\'s coding everybody!');
```

### 문자의 연산

-   `+`를 통해 문자열끼리 합칠 수 있다.
-   문자열의 길이를 구할 때는 문자열 뒤에 `.length`를 붙인다.

```javascript
alert("coding"+" everybody");  // coding everybody
alert("coding everybody".length);  // 16
"code".indexOf("e")  // 3
```



## 변수

-   JavaScript에서 **변수는 var로 시작한다.**
-   var을 생략할 수도 있지만 이것은 유효범위에 영향을 미친다.
-   그렇기에 var의 의미를 명확하게 이해하기 전까지는 var을 명시하는 것을 추천

```javascript
var a = 1;
alert(a+1);  // 2
```

### 재활용성

-   변수는 코드의 재활용성을 높여준다.

```javascript
//변수를 쓰기 전

alert(100+10);
alert((100+10)/10);
alert(((100+10)/10)-10);
alert((((100+10)/10)-10)*10);

// 변수 사용 후

a = 100;
a = a + 10;
alert(a);
a = a / 10;
alert(a);
a = a - 10;
alert(a);
a = a * 10;      
alert(a);
```



## 비교

### 연산자

-   연산자란 값에 대해서 어떤 작업을 컴퓨터에게 지시하기 위한 기호이다.
-   `=`는 우항의 값을 좌항의 변수에 대입하는 **대입 연산자이다.**

### 비교 연산자

-   프로그래밍에서 비교란 주어진 값들이 같은지, 다른지, 큰지, 작은지를 구분하는 것을 의미한다. 이 때 비교 연산자를 사용하는데 비교 연산자의 결과는 `true`나 `false` 중의 하나다.
-   동등 연산자(equal operator) `==`
    -   좌항과 우항을 비교해서 서로 값이 같다면 `true` 다르다면 `false`가 된다.
-   일치 연산자(**strictly** equal operator) `===`
    -   `===`서로 같은 수를 표현하고 있더라도 데이터 타입까지 같은 경우에만 같다고 판단한다.

```javascript
//== 사용하기

alert(1==2)             //false
alert(1==1)             //true
alert("one"=="two")     //false 
alert("one"=="one")     //true

//===사용하기
alert(1=='1');              //true
alert(1==='1');             //false
alert(null == undefined);       //true
alert(null === undefined);      //false
alert(true == 1);               //true
alert(true === 1);              //false
alert(true == '1');             //true
alert(true === '1');            //false
 
alert(0 === -0);                //true
alert(NaN === NaN);             //false
```

-   [==과 ===의 차이점](http://dorey.github.io/JavaScript-Equality-Table/)
