# <strong>2. 자바 데이터 타입, 변수 그리고 배열
### __GOAL__: 자바의 프리미티브 타입, 변수 그리고 배열을 사용하는 방법을 익힙니다.
---


## <strong>2.1 프리미티브 타입? 레퍼런스 타입?

자바의 변수 공부를 들어가면서, 한 가지 중요한 개념을 놓치면 안된다. 바로 자바에서 사용하는 데이터 타입의 종류이다. 

자바에서 데이터를 다룰 때 사용하는 데이터 타입은 기본형 타입(Primitive Type)과 참조형 타입(Reference Type)으로 나뉜다.

* 기본형 타입(Primitive Type)
  *  언어에서 사전에 정의해 놓은 데이터 타입으로 저장공간에 실제 리터럴 형태의 값이 저장된다.
  *  총 8개의 타입[int, char, short, long, float, double, boolean, byte]에 해당
  *  JVM Runtime Data Area 영역 중 Stack 영역에 값이 저장된다.
* 참조형 타입(Reference Type)
  * 실제 값이 아닌 해당 값을 참조하는 참조값(주소값)이 메모리상에 저장된다.
  * 기본형 제외한 모든 타입에 해당
  * JVM Runtime Data Area 영역 중 Heap 영역에 값이 저장되며, 해당 값을 참조하는 주소값이 Stack 영역에 저장된다.

> `Stack 영역`은 컴파일 타임에 크기가 결정되고, 지역변수나 매개변수가 저장되는 공간이다. <br> <br> `Heap 영역`은 런 타임에 크기가 결정되고, 사용자의 동적 할당으로 값이 저장된다.


<br><br><br>

## <strong>2.2 기본형 타입
* 논리형
  * boolean
    * 1 byte
    * true / false
* 문자형
  * char
    * 2 byte
    * '\u0000'~'\uffff' (0~2^16-1)
* 정수형
  * byte
    * 1 byte
    * -2^7 ~ 2^7-1
  * short
    * 2 byte
    * -2^15 ~ 2^15-1
  * `int` (기본)
    * 4 byte
    * -2^31 ~ 2^31-1
  * long
    * 8 byte
    * -2^63 ~ 2^63-1
* 실수형
  * float
    * 4 byte
    * 7자리 정밀도
    * 부호:1 bit / 지수: 8 bit / 가수: 23 bit
  * `double` (기본)
    * 8 byte
    * 15자리 정밀도
    * 부호:1 bit / 지수: 11 bit / 가수: 52 bit

<br><br><br>

## <strong>2.3 리터럴과 상수
`2.1`에서 기본형 타입의 저장공간에는 리터럴이 저장된다고 설명하였다. 그럼 리터럴은 무엇인가? 상수에 대해 먼저 짚고가자. 

### `상수와 final 키워드`
상수는 변수와 동일하게 값을 저장할 수 있는 공간이며, 변수와 달리 한번 값을 지정하면 다른 값으로 변경할 수 없다. 상수는 다음과 같이 `final 키워드`를 사용해서 선언한다. 

(단, 상수는 선언과 초기화가 '한 줄'에 이뤄져야 한다)

`final int MY_AGE = 25`

### `리터럴`

리터럴은 여기서 사용하는 `25`처럼 그 자체로 값을 의미한다. 이는 상수가 저장공간의 의미로 쓰이기 때문에, 이와 구분짓기 위해 위와 같이 따로 정의되었다.

리터럴 역시 타입이 존재한다. 자바에서 사용하는 리터럴 타입은 다음과 같다.
종류 | 리터럴 타입 | 다른 표현
:---:|:---:|:---:
`논리형` | false, true |
`정수형` | 12(int 타입), 12l(or L / long 타입) | 012(8진수), 0x12(16진수), 0b12(2진수)
`실수형` | 3.14(double 타입), 3.14d(or D /double 타입), 3.14f(or F / float 타입) | 1e1(or E / 제곱), p2(or P / 2의제곱)
`문자형` | 'J'(''안에 무조건 하나의 문자가 있어야함)| 기본적으로 UTF-16 사용
`문자열` | "Java", ""(빈 문자열 가능) | 

byte과 short타입의 변수에는 int타입의 리터럴이 저장되며, 나머지도 형변환 규칙을 따라서 저장된다.

>UTF-16은 모든 문자를 2 byte의 고정크기로 표현하고, UTF-8은 하나의 문자를 1~4 byte의 가변크기로 표현한다. 두 인코딩 모두 처음 128문자가 아스키와 동일하다.

<br><br><br>

## <strong>2.4 변수의 선언과 초기화
변수 선언은 타 언어와 비슷하게 다음과 같은 '변수 타입 : 변수 이름'을 지정하는 방식으로 이루어진다.

* 변수 선언
  
    ```java
        int age;
    ```

위의 경우는 변수의 선언은 이루어졌지만, 변수에 값을 저장하는 변수의 초기화는 이루어지지 않았다. 중요한점은, 지역변수로 변수를 선언할 경우 다음과 같이 변수의 초기화를 필수적으로 해주는 것이 좋다.

* 변수 선언과 초기화

    ```java
    int age = 25; // 명시적 초기화 (explicit initialization)
    //or
    int age;
    age = 25;
    ```

> __지역변수 선언시 반드시 초기화를 하자. 메모리는 여러 프로그램이 공유하는 자원이므로 전에 다른 프로그램에 의해 저장된 알 수 없는 '쓰레기 값'이 남아있을 수 있다. (초기화가 안된 지역변수의 사용을 시도하면 컴파일 에러가 나타난다.)<br><br>클래스변수와 인스턴스변수는 기본값(int는 0)으로 초기화가 자동으로 이루어진다.__

<br>

* 변수 선언 잡기술
    ```java
        int i = 1, j = 2;
        // 같은 타입의 변수를 여러개 선언할 시, 콤마를 사용해서 함께 선언 및 초기화 가능
    ```
    
<br><br><br>

## <strong>2.5 변수의 스코프와 라이프타임
Scope는 해당 변수를 사용할 수 있는 영역범위, LifeTime은 해당 변수가 메모리에 얼마나 살아있는지를 의미한다.

<br>

__인스턴스 변수(Instance Variables)__

    * 클래스 안에서 선언되어있고, 어떠한 method나 block안에서 선언되지 않은 변수
  
    * scope - static method를 제외한 클래스 전체
  
    * lifetime - 해당 인스턴스가 생성될 때부터 클래스를 인스턴스화한 객체가 메모리에서 사라질 때 까지
<br>

__클래스 변수(Class Variables) / 정적 변수(Static Variables)__

    * 클래스 안에서 선언되어있고, 어떠한 메서드나 블럭안에서 선언되지 않았으며 static 키워드가 포함되어 선언된 변수

    * 일반적으로 변수나 메소드를 여러 메소드나 클래스에서 공용으로 사용 시 선언

    * 클래스 변수는 인스턴스 변수를 사용하지 못한다. static이 붙을 경우 객체(인스턴스)에 소속된 멤버가 아니라 클래스에 고정된 멤버이다. 따라서 클래스 로더가 클래스를 로딩해서 메소드 메모리 영역에 적재할때 클래스별로 관리가 된다.

    * 클래스 변수는 메모리의 Stack이나 Heap이 아닌 Static 영역에 할당되며, Static 영역에 할당된 메모리는 모든 객체가 공유하여 하나의 멤버를 어디서든지 참조할 수 있는 장점을 가지지만 Garbage Collector의 관리 영역 밖에 존재하기에 Static영역에 있는 멤버들은 프로그램의 종료시까지 메모리가 할당된 채로 존재하게 된다. 그렇기에 Static을 너무 남발하게 되면 성능에 악영향을 줄 수 있다.

    * scope - 클래스 전체

    * lifetime - 클래스가 메모리에 올라갈 때부터 프로그램 종료시 까지
<br>

__지역 변수(Local Variables)__

    * 인스턴스 변수, 클래스 변수가 아닌 모든 변수

    * scope - 변수가 선언된 block 내부

    * lifetime - 변수 선언문이 수행 되었을 때부터 control이 변수가 선언된 block 내부에 있는 동안

<br>

```java
    
    public class ValableScopeExam{

        int globalScope = 10;   // 인스턴스 변수 
        static int staticVal = 7; // 클래스 변수

        public void scopeTest(int value){   
            int localScope = 10;   // 지역 변수
            System.out.println(globalScope);
            System.out.println(localScpe);
            System.out.println(value);
        }  
        public static void main(String[] args) {
            System.out.println(globalScope);    //오류
            System.out.println(localScope);     //오류
            System.out.println(value);          //오류 
            System.out.println(staticVal);      //사용가능 
        }
    }
```

<br><br><br>

## <strong>2.6 타입 변환, 캐스팅 그리고 타입 프로모션

__Type Casting__

타입캐스팅이란 크기가 더 큰 자료형을 크기가 더 작은 자료형에 대입하는 것을 의미한다. 예를들어 int(4byte)타입의 데이터를 byte(1byte) 타입에 대입하는 경우가 있을 수 있겠다. 물론, 데이터 크기가 더 크기 때문에 변환과정에서 데이터의 손실이나 변형이 올 수도 있다.

__Type Promotion__

타입캐스팅과 반대로 크기가 더 작은 자료형을 더 큰 자료형에 대입하는 것을 의미한다. 예를들어 byte(1byte)타입의 데이터를 int(4byte) 타입에 대입하는 경우이다. 그리고 이 경우에는 데이터 손실이나, 변형이 오지 않음으로 캐스팅할 때 처럼 명시적으로 적지 않아도 자동으로 변환이 가능하다.

<br><br><br>

## <strong>2.7 1차 및 2차 배열 선언하기
1차 배열 선언
```java
int[] score = new int[5]; // 5개의 int 값을 저장할 수 있는 배열 생성
//or
int[] score;              // 배열을 선언
score = new int[5];       // 배열을 생성

int tmp = score.length;   // tmp에 배열의 길이인 5가 저장
```

<br>

1차원 배열 초기화
```java
// 하나 하나 초기화하는 방법
//or
int[] score = new int[]{1,2,3,4,5};
```

2차원 배열 선언
```java
int[][] score = new int[5][3]; // 5행 3열의 2차원 배열 생성
//or
int[][] score;                 // 배열을 선언
score = new int[5][3];         // 배열을 생성

int tmp = score.length;        // tmp에 배열의 행인 5가 저장
int tmp2 = score[0].length;    // tmp2에 배열의 열인 3이 저장
```


<br><br><br>

## <strong>2.8 타입 추론, var
타입 추론이란 데이터 타입을 소스코드에 명시하지 않아도, 컴파일 단계에서 컴파일러가 타입을 유추해 정해주는 것을 뜻한다. 1.5버전 부터 추가된 Generic 이나 자바 8 버전에서 추가된 lamda 에서 타입추론이 사용된다. 그리고 자바 10 에서는 이러한 타입추론을 사용하는 var 이라는 Local Variable Type-Inference 가 추가되었다.

이 `var` 키워드는 지역변수이면서 선언과 동시에 초기화가 반드시 되어야한다.

```java
var a = "hello";	// String a = "hello";
var b = 10;		// int b = 10;
```

<br><br><br>

## Reference
Book: Java의 정석 3rd Edition

https://junghyun100.github.io/%ED%9E%99-%EC%8A%A4%ED%83%9D%EC%B0%A8%EC%9D%B4%EC%A0%90/

https://gosasac.tistory.com/42

https://programmers.co.kr/learn/courses/5/lessons/231

https://velog.io/@jaden_94/2%EC%A3%BC%EC%B0%A8-%ED%95%AD%ED%95%B4%EC%9D%BC%EC%A7%80

https://coding-factory.tistory.com/524

https://drinkcoldbrew.tistory.com/2