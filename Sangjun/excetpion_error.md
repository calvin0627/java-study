## 자바에서의 에러 발생 메커니즘

함수에서 에러가 발생하면, 그 함수는 exception 객체라는 것을 생성하여 runtime system으로 보낸다. 이것을 throwing an exception이라고 한다.

Runtime system에는 이 에러를 처리하기 위한 함수들이 존재하는데, 이것들은 ordered list로 되어 있다. 그리고 에러가 한 번 발생하면 순차적으로 함수를 호출하여 가장 안쪽부터 체크하면서 해당 에러에 맞는 핸들러를 탐색한다.

<img src='https://docs.oracle.com/javase/tutorial/figures/essential/exceptions-callstack.gif'>

<img src='https://docs.oracle.com/javase/tutorial/figures/essential/exceptions-errorOccurs.gif'>

만약 찾지 못하면 runtime system은 종료한다. 탐색의 방법은 해당 핸들러와 전달 받은 exception 객체의 타입, 그러니까 클래스가 동일한지의 여부이다.



## Exception

### 예외 처리 방법

- try catch

try catch를 이용한 예외 처리 방법은 다음과 같다.

```Java
try{

} catch(Exception e) {

}
```

try 블록안에는 우리가 예외처리 하고싶은 statement를 위치시키고, catch의 brace 안에는 우리가 감지하고 싶은 에러의 타입을 기입한다. 예를 들어, ArthmeticExcpetion 타입의 에러를 감지하고 싶으면,

```Java
try{

} catch(ArithmeticException e){

}
```

위와 같이 코드를 작성하면 된다.

에러가 발생할 경우에도 반드시 실행시켜야 하는 statement가 존재한다면 아래와 같이 finally 블록 안에 위치시킨다.

``` Java
try{

} catch(ArithmeticException e){

} finally{
	System.out.println("Hi");
}
```



### 커스텀 예외 발생시키기

예외에는 자바에서 미리 정의된 타입의 예외가 존재한다. 그러나 나만의 예외를 만들고 싶을 때가 있을 것이다. 어떻게 해야 할까?

1. 예외 타입(클래스) 만들기

   RuntimeException이나 Exception이라는 타입(클래스)를 상속받으면 예외 타입이라고 부를 수 있다. RuntimeException의 이름에는 특징이 잘 들어나는데 Exception은 어떤 종류의 예외인지 도통 알 수 없다. 이것은 Compile-time Exception을 의미한다고 한다.

   예를 들어,

   ```java
   public class PhoneException extends Exception {
   ...
   }
   ```

   위와 같이 클래스를 선언하면 에외 타입 하나가 만들어진 것이다.

2. 예외 발생시키기

   * throw
   
   예외를 클래스로 정의했으니 예외 발생 또한 클래스 객체를 만들어 발생시키면 된다.
   
   ```Java
   throw new PhoneException()
   ```
   
   * throws
   
   throw는 해당 함수에서 예외를 발생시킨다. 그런데, 해당 함수를 호출한 곳에서 예외를 다루고 싶다면  함수를 선언할 당시에  `throws PhoneExcpeiton` 를 뒤에 붙여준다.
   
   ```Java
   public class Myfunc throws PhoneException { ... }
   ```
   
   예외 발생을 미루기 때문에 예외를 미룬다고도 표현한다고 한다.



## Error

에러란 Exception과 다르게 시스템 내부에서 일어나는 치명적인 오류이다. Exception이란 문자그대로 예외처리를 하여 프로그램의 오작동을 보겠다는 것인데 여기에는 프로그램이 그래도 잘 수행되고 있을 것이라는 가정이 포함되어 있다. 예외처리를 하는 부분이 프로그램의 수행에 문제를 준다면 따로 예외처리를 하여 처리할 필요가 없을 것이다. 그런 상황에는 바로 프로그램이 멈추기 때문이다. 따라서 에러란 프로그램의 수행에 문제를 주는 치명적인 오류라고 보면 될 것이다.

종류에는 여러가지가 있는데 가장 흔하게 볼 수 있는 에러는

* StackOverflowError
* OutOfMemoryError

이다.

여담으로 StackOverflow는 너무나도 자주 볼 수 있어서 어느 웹사이트의 이름으로도 사용중이다..



## 예외 계층

<img src=https://rollbar.com/wp-content/uploads/2021/07/java-exceptions-hierarchy-example.png>

모든 클래스의 부모인 Object 클래스를 제외하고 Error와 Exception의 부모는 Throwable이다.







