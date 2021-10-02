# <strong>3. 자바 연산자
### __GOAL__: 자바가 제공하는 다양한 연산자를 학습하세요.
---

## <strong>3.1 산술 연산자
```
+ 더하기연산자
- 빼기연산자
* 곱하기연산자
/ 나누기연산자
% 나머지연산자
++ 증가연산자
-- 감소연산자
INF 무한 또는 가장 크거나 작은(-INF) 숫자
NAN 숫자가 아님(0으로 나누거나 0을 나머지로 계산할 때)
```

<br><br><br>

## <strong>3.2 비트 연산자
```
&(AND)
|(OR)
^(XOR)
~(NOT)
<<(Left Shift)
>>(right Shift) (빈자리를 MSB 값으로 채움)
>>>(right Shift) (빈자리를 0으로 채움)
```

<br><br><br>

## <strong>3.3 관계 연산자
```
> 크다
< 작다
>= 크거나 같다
<= 작거나 같다
== 같다
!= 같지않다
```

<br><br><br>

## <strong>3.4 논리 연산자
```
& (AND)
| (OR)
^ (XOR)
! (NOT)
&& (AND) (첫번째 조건에서 만족 못하면 pass)
|| (OR) (첫번째 조건에서 만족하면 pass)
```

<br><br><br>

## <strong>3.5 instanceof
```
특정 Class 구조로 메모리에 할당된 실체를 해당 Class의 인스턴스라고 부르는데, 이를 통해 instanceof 가 어떤 연산자인가에 대해 유추해 볼 수 있다.
instanceof를 통해 특정 변수가 특정 class 타입인지를 확인할 수 있다.

해당 연산자를 통해 상속관계를 확인해, 타입캐스팅이 가능한지에 대해 판단할 수 있다.
```
```java
class A {}

class B extends A {}

public static void main(String[] args) {

    A a = new A();
    B b = new B();
    
    System.out.println(a instanceof A); // true
    System.out.println(b instanceof A); // true
    System.out.println(a instanceof B); // false
    System.out.println(b instanceof B); // true

}
```

<br><br><br>

## <strong>3.6 assignment(=) operator
```
변수 += 값 -> 변수의 기존 값에 + 값을 더한 결과를 변수에 대입
변수 -= 값
변수 *= 값
변수 /= 값
변수 *= 값
변수 &= 값
+ 비트연산자까지
```
단, 레퍼런스 타입의 경우 주소값이 변수에 할당되므로, 주소를 이어주는 `얕은 복사`이다.

기본형 타입의 경우 리터럴 자체가 새로운 변수에 복사되므로 `깊은 복사`이다.

<br><br><br>

## <strong>3.7 화살표(->) 연산자
```
( parameters ) -> expression body // 인자가 여러개 이고 하나의 문장으로 구성
( parameters ) -> { expression body } // 인자가 여러개 이고 여러 문장으로 구성
() -> { expression body } // 인자가 없고 여러 문장으로 구성
() -> expression body // 인자가 없고 하나의 문장으로 구성
```
함수형 프로그래밍을 위해 자바 8 부터 도입된 람다식에서 사용되는 -> 연산자는 왼쪽 피연산자에 매개변수가 들어가고 오른쪽 피연산자에 구현부가 들어간다

<br><br><br>

## <strong>3.8 삼항연산자
```
조건식 ? 반환값1 : 반환값2
```
삼항 연산자는 자바에서 유일하게 피연산자를 세 개나 가지는 조건 연산자입니다.

삼항 연산자의 문법은 다음과 같습니다. 물음표(?) 앞의 조건식에 따라 결괏값이 참(true)이면 반환값1을 반환하고, 결괏값이 거짓(false)이면 반환값2를 반환합니다.

<br><br><br>

## <strong>3.9 연산자 우선순위
[링크 참고](https://medium.com/@katekim720/%EC%97%B0%EC%82%B0%EC%9E%90%EB%B6%80%ED%84%B0-%EC%A1%B0%EA%B1%B4-%EB%B0%98%EB%B3%B5%EB%AC%B8%EA%B9%8C%EC%A7%80-3d5cec6513d4)


<br><br><br>

## Reference

https://velog.io/@jaden_94/3%EC%A3%BC%EC%B0%A8-%ED%95%AD%ED%95%B4%EC%9D%BC%EC%A7%80

https://kils-log-of-develop.tistory.com/336