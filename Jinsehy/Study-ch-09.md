# <strong>9. 예외 처리
### __GOAL__: 자바의 예외 처리에 대해 학습하세요.
---

## <strong>8.1 자바가 제공하는 예외 계층 구조
자바에서는 실행 시 발생할 수 있는 오류(Exception과 Error)를 클래스로 정의하였다. Exception과 Error 또한 클래스이므로 Object 클래스의 자손이다.

![img](https://media.vlpt.us/images/youngerjesus/post/e881e26d-31e1-4bec-b852-5092df2840c9/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-01-13%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.46.09.png)

java.lang.Throwable class가 Java Exception의 root Class 입니다. hierarchy은 크게 두개로 나뉘는데 Exception과 Error로 나뉩니다.

이런 Exception도 Checked Exception과 Unchecked Exception으로 나뉩니다.

이 예외 계층을 아는 것은 catching exceptions 할 때 유용합니다.

예를들면 catch(IOException e)를 한다면 자식 클래스인 FileNotFoundException도 catch 할 것입니다.
<br><br><br>

## <strong>8.3 Exception과 Error의 차이는?
자바에서는 실행 시(runtime) 발생할 수 있는 프로그램 오류를 에러(Error)와 예외(Exception), 두 가지로 구분한다.

### 에러 Error
에러는 메모리 부족(OutOfMemoryError)이나 스택오버플로우(StackOverflowError)와 같은 발생하게 되면 복구할 수 없는 심각한 수준의 오류를 뜻한다. 시스템에 비정상적인 상황이 생겼을 때 발생하므로 System level의 문제이다.

### 예외 Exception
예외는 개발자가 작성한 로직 내에서 발생한 오류를 뜻한다. 따라서 발생하더라도 개발자가 이에 대한 적절한 코드를 미리 작성해 놓음으로써 프로그램의 비정상적인 종료와 같은 오류를 방지할 수 있다.

* 에러(error): 프로그램 코드에 의해서 수습될 수 없는 심각한 오류
  * 주로 확인되지 않은 유형에 속하며 런타임 시점 때 확인되고 컴파일 시점에는 알 수 없음
* 예외(exception): 프로그램 코드에 의해서 수습될 수 있는 상대적으로 미약한 오류
  * 컴파일 시점과 런타임 시점 모두에서 발생할 수 있음
  
Exception과 Error는 System Level의 문제와 Application Level의 문제의 차이이다.
<br><br><br>

## <strong>8.2 RuntimeException과 RE가 아닌 것의 차이는?
>오라클에서는 `Exception`을 `Event`라고 합니다. 즉, 프로그램의 정상적인 흐름을 방해시키는 이벤트 입니다.
>
>이런 이벤트가 발생하면 메소드에서 에러 정보가 담긴 Exception Object를 만들고 런타임 시스템에 전달합니다.
>
>그러면 런타임 시스템은 이 Exception Object를 처리할 수 있는 Exception Handler를 찾으려고 합니다.
>
>이 Exception Handler를 찾으려는 집합은 메소드를 호출한 집합인 Method Call Stack 입니다. 먼저 에러가 발생한 메소드부터 찾고 거기서 찾지 못하면 이 메소드를 호출한 메소드에서 찾는식으로 진행합니다.
>
>이렇게해서 찾으면 런타임 시스템은 Exception Object를 Exception Handler에게 전달합니다.
>
>Exception Handler는 Exception Object를 보고 자신이 처리할 수 있는 타입인지 판단하고 처리합니다.
>
>이 Exception Handler가 Catch Code Block 입니다.
>
>만약 Exception Handler를 찾지 못한다면 런타임 시스템은 종료됩니다.


예외 클래스는 두 그룹으로 나눌 수 있다.

* Exception클래스와 그 자손들
* RuntimeException클래스와 그 자손들
  
자바에서 RuntimeException은 Unchecked Exception으로 
RuntimeException이 아닌것은 Checked Exception으로 분류됩니다.

### Unchecked Exception
`Unchecked Exception`(RuntimeException클래스와 그 자손들)은 주로 프로그래머의 실수에 의해서 발생될 수 있는 예외들로 자바의 프로그래밍 요소들과 관계가 깊다.

예를 들어
* 배열의 범위를 벗어난다던 (ArrayIndexOutOfBoundsException)
* 값이 null인 참조변수의 멤버를 호출하려 했다던가 NullPointerException)
* 클래스 간의 형변환을 잘못했다던가 (ClassCastException)
* 정수를 0으로 나누려고 (ArithmeticException)
  
하는 경우에 발생한다.

명시적인 처리를 강제하지 않고, 실행되는 시점(Runtime)에 확인한다.

### Checked Exception
`Checked Exception`(RE클래스들을 제외한 나머지 클래스들)은 주로 외부의 영향으로 발생하는 것들로, 프로그램 사용자들의 동작에 의해서 발생하는 경우가 많다.

예를 들어
* 존재하지 않는 파일의 이름을 입력했다던  (FileNotFoundException)
* 실수로 클래스의 이름을 잘못 적었다던가 (ClassNotFoundException)
* 입력한 데이터 형식이 잘못된(DataFormatException)
  
경우에 발생한다.

이름에서 알 수 있듯이 반드시 예외를 처리해야하고, 컴파일되는 시점에 확인한다.

### 어떻게 구분 할 수 있을까?
두 가지의 가장 명확한 구분 기준은 꼭 처리를 해야 하는냐이다. `Checked Exception`이 발생할 가능성이 있는 메소드라면 반드시 `try - catch`로 감싸거나 `throw`로 던져서 처리해야 한다.

반면에 `Unchecked Exception`은 명시적인 예외처리를 하지 않아도 된다. 대부분의 예외가 부주의로 발생하고, 예측하지 못했던 상황의 예외가 아니기 때문에 굳이 로직으로 처리 할 필요가 없도록 만들어져 있다.

예외를 확인할 수 있는 시점으로도 구분할 수 있다. 일반적으로 컴파일 단계에서 명확하게 Exception 체크가 가능한 것을 Checked Exception이라 하며, 실행과정 중 어떠한 특정 논리에 의해 발견되는 Exception을 Unchecked Exception이라 한다.
<br><br><br>




## <strong>8.4 자바에서 예외 처리 방법 (try, catch, throw, throws, finally)

프로그램이 실행 중 어떤 원인에 의해서 오작동을 하거나 비정상적으로 종료되는 경우가 있다. 이러한 결과를 초래하는 원인을 프로그램 에러 또는 오류라고 한다.

이를 Java의 클래스 계층 관점이 아닌, 발생시점에 따라 분류한다면 `컴파일 에러(compile-time error)`와 `런타임 에러(runtime error)`로 나눌 수 있다.
컴파일 에러는 컴파일 할 때 발생하는 에러를 말하고 런타임 에러는 프로그램의 실행도중에 발생하는 에러를 말한다. 이 외에 논리적 에러(logical error)가 있는데, 컴파일도 잘되고 실행도 잘되지만 의도한 것과 다르게 동작하는 것을 말한다.

* `컴파일 에러`: 컴파일 시에 발생하는 에러
* `런타임 에러`: 실행 시에 발생하는 에러
* `논리적 에러`: 실행은 되지만, 의도와 다르게 동작하는 것
  
기본적으로 소스코드를 컴파일 하면 컴파일러가 소스코드(*.java)에 대해 오타나 잘못된 구문, 자료형 체크등의 기본적인 검사를 수행하여 오류가 있는지 알려준다. 컴파일러가 알려준 에러들을 모두 수정해서 컴파일을 성공적으로 마치고 나면, 클래스 파일(*.class)이 생성되고, 생성된 클래스 파일을 실행할 수 있게 되는 것이다.

하지만 컴파일이 에러없이 성공적으로 마쳤다고 해서 실행 시 에러가 발생하지 않는다는 보장은 없다. 컴파일러가 소스코드의 기본적인 사항은 컴파일 시에 모두 걸러 줄 수 있지만, 실행 중에 발생할 수 있는 잠재적인 오류까지 검사할 수 없기 때문이다.

### 예외 처리 (Exception Handling)
예외 처리란, 프로그램 실행 시 발생할 수 있는 예기치 못한 예외의 발생에 대비한 코드를 작성하는 것이다. 예외처리의 목적은 예외의 발생으로 인한 실행 중인 프로그램의 비정상 종료를 막고, 정상적인 실행상태를 유지할 수 있도록 하는 것이다.

* 정의: 프로그램 실행 시 발생할 수 있는 예외에 대비한 코드를 작성하는 것
* 목적: 프로그램의 비정상 종료를 막고, 정상적인 실행상태를 유지하는 것
 
발생한 예외를 처리하지 못하면, 프로그램은 비정상적으로 종료되며, 처리되지 못한 예외(uncaught exception)는 JVM의 예외처리기(UncaughtExceptionHandler)가 받아서 예외의 원인을 출력한다.

### The try block
try Block은 예외가 발생할 수 있는 코드를 넣는 곳입니다. 여기서 발생한 예외는 Catch Block을 통해 전달됩니다.

문법은 다음과 같습니다.
```java
try {
    code
}
catch and finally blocks . . .
```

### The catch block
Catch Block은 try 블록에서 발생한 예외 코드나 예외 객체를 Arguement로 전달받아 그 처리를 담당합니다.

문법은 다음과 같습니다.
```java
try {

} catch (ExceptionType name) {
...
} catch (ExceptionType name) {
...
}
```
여기에 있는 ExceptionType은 자바에 있는 Exception과 Error의 superClass인 Throwable을 상속한 클래스 중 하나여야 합니다.

try 블럭에는 여러 개의 catch 블록이 올 수 있습니다.

* 예외가 발생시 `예외에 대한 인스턴스`가 생성된다.
  * 예를 들어 `ArithmeticException`이 발생했다면 `ArithmeticException`인스턴스가 생성된다.
* 이후, try 문을 실행하지 않고, 바로 catch 블록으로 이동한다.
* catch 문들을 차례대로 살펴보면서, instance of연산자를 이용해 발생한 예외의 종류와 일치하는 단 한 개의 catch 블록을 찾는다.
  * 모든 예외 클래스는 `Exception`클래스의 자손이므로, catch블럭의 괄호에 `Exception`클래스 타입의 참조변수를 선언해 놓으면 어떤 종류의 예외가 발생하더라도 이 catch블럭에 의해서 처리된다.
  * catch와 일치하는 블록이 여러개더라도 제일 첫 번째로 일치하는 catch 블록만 실행하고, 나머지는 넘어간다.
* 해당 catch 문을 수행한 뒤, try-catch문을 탈출한다.

Catch Block을 실제적인 예외를 처리하는 부분으로 간단하게 에러 메시지를 출력하는 것 뿐 아니라 에러가 났을때 복구 작업을 하거나 Chained exception을 통해 High level handler에게 에러를 전파할 수 있습니다.

### Multi-catch blcok
Java 7 이후 부터는 하나의 Catch Block에서 하나 이상의 Exception Type을 전파할 수 있습니다.

예시는 다음과 같습니다.
```java
catch (IOException | SQLException ex) {
    logger.log(ex);
    throw ex;
}
```
이걸 통해 코드의 중복을 줄이거나 광범위한 예외를 처리하지 않아도 됩니다.

그리고 하나 이상의 ExceptionType을 처리하는 경우 Catch parameter는 내재적으로 final이 됩니다. 그러므로 ex는 다른 값으로 할당할 수 없습니다.

* 연결할 수 있는 예외 클래스의 개수에는 제한이 없다
* 만일 멀티 catch 블럭의 기호로 연결된 예외 클래스가 조상과 자손의 관계에 있다면 컴파일 에러가 발생
  * 두 예외 클래스가 조상과 자손의 관계라면, 조상클래스만 써주는 것과 똑같기 때문에 불필요한 코드를 제거하라는 의미에서 에러가 발생
  ```java
    try {
        ...
    } catch (ParentException | ChildException e) { // 에러!
        e.printStackTrace();
    }
  ```

### printStackTrace()와 getMessage()
예외가 발생했을 때 생성되는 예외 클래스의 인스턴스에는 발생한 예외에 대한 정보가 담겨져 있으며, `getMessage()`와 `printStackTrace()`를 통해서 이 정보들을 얻을 수 있다.

catch블럭의 괄호에 선언된 참조변수를 통해 이 인스턴스에 접근할 수 있으며, 이 참조변수는 선언된 catch블럭 내에서만 사용 가능하다.

* printStackTrace()
  * 예외 발생 당시의 호출스택(Call Stack)에 있었던 메서드의 정보와 예외 메시지를 화면에 출력한다.
* getMessage()
  * 발생한 예외클래스의 인스턴스에 저장된 메시지를 얻을 수 있다.

### The finally block
finally block은 try block이 끝난 후 또는 예상치 못한 에러가 발생 했을때도 실행을 보장합니다.

일반적인 실행의 흐름은 아래와 같습니다.
* 예외가 발생하면 ‘try → catch → finally’순으로 실행
* 예외가 발생하지 않은 경우에는 ‘try → finally’순으로 실행

하지만 만약에 JVM이 try catch block을 실행 중에 종료된다면 finally block은 실행되지 않습니다.

마찬가지로 Thread가 try catch block을 실행 중에 interrupt 당하거나 kill 당한다면 finally block은 실행되지 않습니다.

finally block은 cleanup code를 넣어서 resource leak을 막을 용도로 사용하는게 좋습니다.

try-catch-finally를 사용한 예제는 아래와 같습니다.
```java
public void writeList() {
    PrintWriter out = null;
    try {
        System.out.println("Entered try statement");
        out = new PrintWriter(new FileWriter("OutFile.txt"));
        // OutFile.txt 파일을 성공적으로 열지 못한다면 IOException이 발생
        for (int i = 0; i < SIZE; i++) {
            out.println("Value at: " + i + " = " + list.get(i));
            // ArrayList에서 get Method를 통해 값을 가지고 오려고 할 때 해당 인덱스 값이 없다면 IndexOutOfBoundsException이 발생
        }
    }catch (IndexOutOfBoundsException e) {
    	System.err.println("IndexOutOfBoundsException: " + e.getMessage());
    }catch (IOException e) {
    	System.err.println("Caught IOException: " + e.getMessage());
    }finally {
    if (out != null) { 
        System.out.println("Closing PrintWriter");
        out.close(); 
    } else { 
        System.out.println("PrintWriter not open");
    } // Cleanup code
} 

}
```
try block에 있는 writeList method에는 파일을 읽기 위해 PrintWriter 라는 리소스를 사용합니다.

그러므로 writeList method를 종료할 땐 PrintWriter 리소스를 반환하기 위해 close 해줘야 합니다.

이 경우 finally block에 cleanup code를 넣어줌으로써 정상적으로 try block이 종료되든 예외가 일어나서 흐름이 바뀌든 상관없이 PrintWriter 리소스를 반환합니다.

### The try-with-resources Statement
try-with-resource는 Java 7에서 들어온 기능으로 try block에서 사용할 resource를 선언하는 식으로 사용됩니다.

finally block 대신에 resource를 자동으로 반환하기 위해 사용합니다.

여기서 말하는 리소스는 java.lang.AutoClosable 이나 java.io.Closable을 구현한 오브젝트를 말하며 반드시 close 되야하는 리소스 입니다.
>  주로 입출력에 사용되는 클래스 중에서는 사용한 후에 꼭 닫아 줘야 하는 것들이 있다. 닫아야만 사용했던 자원(resources)이 반환되기 때문이다.

예시는 다음과 같습니다.
```java
try {
    fis = new FileInputStream("score.dat");
    dis = new DataInputStream(fis);
        ...
} catch (IOException ie) {
    ie.printStackTrace();
} finally {
    dis.close();
}

//위 예제는 파일로부터 데이터를 읽는 코드인데, 데이터를 읽는 도중에 예외가 발생하더라도 DataInputStream이 닫히도록 finally블럭 안에 close()를 넣었다. 문제는 close()가 예외를 발생시킬 수 있기 때문에 아래와 같이 해야 올바른 코드이다.

try {
    fis = new FileInputStream("score.dat");
    dis = new DataInputStream(fis);
        ...
} catch (IOException ie) {
    ie.printStackTrace();
} finally {
    try {
        if(dis != null)
            dis.close();
    } catch (IOException ie) {
        ie.printStackTrace();
    }
}
//이렇게 작성하게되면 코드가 복잡해지고 try블럭과 finally블럭 모두 예외가 발생해버리면 try 블럭의 예외가 무시된다.

try (FileInputStream fis = new FileInputStream("score.dat");
        DataInputStream dis = new DataInputStream(fis)) {
    while(true) {
        score = dis.readInt();
            ...
    }   
} catch (EOFException e) {
    System.out.println(" ... ");
} catch (IOException ie) {
    ie.printStackTrace();
    }
//try-with-resources문의 괄호 안에 객체를 생성하는 문장을 넣으면, 이 객체는 따로 close()를 호출하지 않아도 try블럭을 벗어나는 순간 자동적으로 close()가 호출된다. 이 후 catch블럭 또는 finally블럭이 수행된다.
```

### throws : 메서드에 예외 선언
예외를 처리하는 방법 중에는 예외를 메서드에 선언하는 방법이 있다.

메서드에 예외를 선언하려면 메서드 선언부에 키워드 `throws`를 사용해서 메서드 내에서 발생할 수 있는 예외를 적어주기만 하면 된다. 그리고 여러 개일 경우에는 쉼표로 구분한다.
```java
void method() throws Exception1, Exception2, ... , ExceptionN {
    // 메서드의 내용
}
```
메서드의 선언부에 예외를 선언함으로써 메서드를 사용하려는 사람이 메서드의 선언부를 보았을 때, 이 메서드를 사용하기 위해서는 어떠한 예외들이 처리되어야 하는지 쉽게 알 수 있다.

메서드에 예외를 선언할 때 일반적으로 RuntimeException클래스들은 적지 않는다. throws에 선언한다고 해서 문제가 되지는 않지만, 보통 반드시 처리되어야 하는 예외들만 선언한다.

예외를 메서드의 throws에 명시하는 것은 예외를 처리하는 것이 아니라, 자신을 호출한 메서드에게 예외를 전달하여 예외처리를 떠맡기는 것이다.

예외를 전달받은 메서드는 자신을 호출하는 또다른 메서드에게 전달할 수 있으며, 이런 식으로 계속 호출스택에 있는 메서드들을 따라 전달되다가 마지막에 main메서드에서도 예외가 처리되지 않으면, main메서드가 종료되면서 프로그램 전체가 종료된다.

결국 예외는 처리되는 것이 아닌 단순히 전달만 되는 것이므로 결국 어느 한 곳에서는 try-catch문으로 처리를 해주어야 한다.
```java
class App {
    public static void main(String[] args) {
        try {
            method1();
        } catch (Exception e) {
            System.out.println("main메서드에서 예외가 처리되었습니다.");
            e.printStackTrace();
        }
    }

    static void method1() throws Exception {
        throw new Exception();
    }
}
```
### throw : 인위적으로 예외 발생
키워드 `throw`를 사용해서 예외를 고의로 발생시킬 수 있다.

1. 연산자 new를 이용해 발생시키려는 예외 클래스의 객체를 만든다.
```java
Exception e = new Exception(“예외 발생”);
```
2. 키워드 throw를 이용해서 예외를 발생시킨다.
```java
throw e;
```
위의 예제처럼 한줄로도 가능하다
```java
throw new Exception();
```
Exception인스턴스를 생성할 때, 생성자에 String을 넣어주면 메시지로 저장된다. 이 메시지는 `getMessage()`를 이용해서 얻을 수 있다.
<br><br><br>

### Chained Exception : 연결된 예외
한 예외가 다른 예외를 발생시킬 수도 있다. 예를 들어 예외 A가 예외 B를 발생시켰다면, A는 B의 원인 예외(cause exception)라고 한다.

아래 코드는 프로그램을 설치하는 코드에서 `SpaceException`을 원인 예외로 하는 `InstallException`을 발생시키는 방법이다.
```java
try {
    startInstall();     //SpaceException 발생
    copyFiles();
} catch (SpaceException e) {
    InstallException ie = new InstallException("설치 중 예외발생"); // 예외 생성
    ie.initCause(e);  // InstallException의 원인 예외를 SpaceException으로 지정
    throw ie; // InstallException을 발생시킴
} catch (MemoryException me) {
    ...
}
```
`initCause()`는 Exception클래스의 조상인 Throwable클래스에 정의되어 있기 때문에 모든 예외에서 사용가능하다.

* Throwable initCause(Throwable cause) 지정한 예외를 원인 예외로 등록
* Throwable getCause() 원인 예외를 반환
  
발생한 예외를 그냥 처리하면 될 텐데, 원인 예외로 등록해서 다시 예외를 발생시키는 이유는 뭘까? 그 이유는 `여러가지 예외를 하나의 큰 분류의 예외로 묶어서 다루기 위해서`이다.

그렇다고 아래와 같이 InstallException을 SpaceException과 MemoryException의 조상으로 해서 catch블럭을 작성하면, 발생한 예외가 어떤 것인지 알 수 없다는 문제와 상속관계를 변경해야한다는 문제가 생긴다.
```java
try {
    startInstall();
    copyFiles();
} catch (InstallException e) {
    e.printStackTrace();
}
```
그래서 생각한 것이 예외가 원인 예외를 포함시킬 수 있게 한 것이다. 이렇게 하면, 두 예외는 상속관계가 아니어도 상관없다.

또 다른 이유는 `Checked Exception`을 `Unchecked Exception`으로 바꿀 수 있도록 하기 위해서이다. Checked Exception이 발생해도 예외를 처리할 수 없는 상황이 나타나면서 이를 해결하기 위해 의미없는 try-catch문을 추가하는 것 뿐이였다. 이럴 때 Checked Exception을 Unchecked Exception으로 바꾸면 예외처리가 선택적이 되므로 억지로 예외처리를 하지 않아도 된다.

예를 들어 MemoryException은 Exception의 자손이므로 반드시 예외를 처리해야하는데, 이 예외를 RuntimeException으로 감싸서 Unchecked Exception으로 만들 수 있다. 이러면 더 이상 선언부에 MemoryException을 선언하지 않아도 된다.
```java
static void startInstall() throws SpaceException, MemoryException {
    if(!enoughSpace())
        throw new SpaceException("설치 공간이 부족합니다.");

    if(!enoughMemory())
        throw new MemoryException("메모리가 부족합니다.");
}
```
```java
static void startInstall() throws SpaceException {
    if(!enoughSpace())
        throw new SpaceException("설치 공간이 부족합니다.");

    if(!enoughMemory())
        throw new RuntimeException(new MemoryException("메모리가 부족합니다."));
}
```

## <strong>8.5 커스텀한 예외 만드는 방법
Java platform에서는 자신만의 예외 클래스를 만들 수 있습니다.

다음의 조건에 부합된다면 예외 클래스를 만들어서 사용하고 그렇지 않다면 기존의 자바에서 제공하는 예외를 사용하면 됩니다.

* Do you need an exception type that isn't represented by those in the Java platform?

* if they could differentiate your exceptions from those thrown by classes written by other vendors?

* Does your code throw more than one related exception?

* If you use someone else's exceptions, will users have access to those exceptions?

### How to implement?
보통 Exception클래스로부터 상속받는 클래스를 만들지만, 필요에 따라서 알맞은 예외 클래스를 선택할 수 있다.
```java
class MyException extends Exception {
    private final int ERR_CODE;

    MyException(String msg, int errCode) {    // 생성자
        super(msg);
        ERR_CODE = errCode;
    }

    MyException(String msg) {   // 생성자
        this(msg, 100);         // ERR_CODE를 100(기본값)으로 초기화
    }

    public int getErrCode() {   // 에러 코드를 얻을 수 있는 메서드
        return ERR_CODE;        // getMessage()와 함께 사용될 것
    }
}
```
Exception클래스로부터 상속받아서 만들어진 MyException클래스에 필요에 따라 멤버변수나 메서드를 추가할 수도 있다.

Exception클래스는 생성 시에 String값을 받아서 메시지로 저장할 수 있는데, 이처럼 String을 매개변수로 받는 생성자를 추가하여 메시지를 저장할 수 있도록 만들었다.

에러코드 값을 저장할 수 있도록 ERR_CODE와 getErrCode()를 멤버로 추가했다. 이렇게 함으로써 MyException이 발생했을 때, catch블럭에서 getMessage()와 getErrCode()를 사용해서 에러코드와 메시지를 모두 얻을 수 있다.

기존의 예외 클래스는 주로 Exception을 상속받아서 Checked Exception으로 작성하는 경우가 많았다고 한다. 

요즘은 예외처리를 선택적으로 할 수 있도록 RuntimeException을 상속받아서 작성하는 쪽으로 바뀌어가고 있다고 한다. (Unchecked Exception)

Checked Exception은 반드시 예외처리를 해주어야 하기 때문에 불필요한 경우에도 try-catch문을 넣어 코드가 복잡해지기 떄문이다.


### Custom Checked Exception
```java
try (Scanner file = new Scanner(new File(fileName))) {
    if (file.hasNextLine()) return file.nextLine();
} catch(FileNotFoundException e) {
    // Logging, etc 
}
```
FileNotFoundException 같은 경우는 정확한 예외의 원인을 알 수 없습니다. 파일으 이름이 유효하지 않을 수 있고 파일이 존재하지 않을 수도 있습니다.

이 경우 java.lang.Exception class를 상속받은 커스텀 클래스를 만들어서 해결할 수 있습니다.

```java
public class IncorrectFileNameException extends Exception { 
    public IncorrectFileNameException(String errorMessage) {
        super(errorMessage);
    }
    
    public IncorrectFileNameException(String errorMessage, Throwable err) {
    	super(errorMessage, err);
    }
}
```
에러 메시지를 담을 생성자와, 근본 원인을 담을 생성자를 추가해서 예외를 만들 수 있습니다.

이 커스텀 예외를 사용해 정확한 원인을 알 수 있습니다.
```java
try (Scanner file = new Scanner(new File(fileName))) {
    if (file.hasNextLine()) {
        return file.nextLine();
    }
} catch (FileNotFoundException err) {
    if (!isCorrectFileName(fileName)) {
        throw new IncorrectFileNameException(
          "Incorrect filename : " + fileName , err);
    }
    // ...
}
```
### Custom Unchecked Exception
동일한 예제에서 파일 이름에 확장자가 없는 경우에 대해 생각해보겠습니다.

이 경우 오류는 런타임에서 감지되므로 Unchecked Exception이며 사용자 정의 예외가 필요합니다.

Unchecked Exception 에서 커스텀 예외를 만들기 위해선 java.lang.RuntimeException class를 상속받아야 합니다.
```java
public class IncorrectFileExtensionException 
  extends RuntimeException {
    public IncorrectFileExtensionException(String errorMessage, Throwable err) {
        super(errorMessage, err);
    }
}
```
이 예외를 통해 다음과 같이 해결할 수 있습니다.
```java
try (Scanner file = new Scanner(new File(fileName))) {
    if (file.hasNextLine()) {
        return file.nextLine();
    } else {
        throw new IllegalArgumentException("Non readable file");
    }
} catch (FileNotFoundException err) {
    if (!isCorrectFileName(fileName)) {
        throw new IncorrectFileNameException(
          "Incorrect filename : " + fileName , err);
    }
    
    //...
} catch(IllegalArgumentException err) {
    if(!containsExtension(fileName)) {
        throw new IncorrectFileExtensionException(
          "Filename does not contain extension : " + fileName, err);
    }   
    //...
}
```

<br><br><br>

## Reference

https://yadon079.github.io/2021/java%20study%20halle/week-09

https://velog.io/@youngerjesus/%EC%9E%90%EB%B0%94-%EC%98%88%EC%99%B8-%EC%B2%98%EB%A6%AC