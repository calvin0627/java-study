# <strong>7. 패키지
### __GOAL__: 자바의 패키지에 대해 학습하세요.
---

## <strong>7.1 패키지(package)

### 패키지의 정의
패키지란 클래스의 묶음으로, 클래스 또는 인터페이스를 포함시킬 수 있으며 효율적인 관리를 위해 사용된다.

### 패키지의 특징
* 클래스의 실제 이름은 패키지명을 포함한 것이다.
  * String 클래스의 경우 실제 이름은 java.lang.String
* 클래스가 물리적으로 하나의 클래스파일(.class)인 것과 같이 패키지는 물리적으로 클래스 파일을 포함하는 하나의 디렉토리이다.
* 디렉토리가 하위 디렉토리를 가질 수 있는 것처럼, 패키지도 다른 패키지를 포함할 수 있으며, '.'으로 구분한다.

### 패키지 명명 규칙
* 패키지 이름은 모두 소문자여야한다.
* 자바의 예약어를 사용하면 안된다. (예, int, static)
* 개발 패키지 표준은 정하는 것에 따라 지정하면 된다.

### 패키지의 선언
```java
package 패키지명;
```
* 패키지 선언문은 소스파일에서 주석과 공백을 제외한 첫 번째 문장이어야 한다.
* 하나의 소스파일에 단 한번만 선언될 수 있다.


### 빌트-인 패키지(Built-in Package)
자바는 개발자들이 사용할 수 있도록 여러 많은 패키지 및 클래스를 제공한다.

가장 자주 쓰이는 패키지로는 java.lang과 java.util이 있다.

java.lang은 자주 사용하는 패키지이지만 한번도 import하여 사용한적이 없다.

즉, 자바에서 java.lang 패키지는 아주 기본적인 것들이기 때문에 import로 불러오지 않아도 자바가 알아서 java.lang의 클래스를 불러온다.

예) String, System

### 이름 없는 패키지
자바의 모든 클래스는 하나의 패키지에 포함되어야 한다. 만일 소스파일을 작성할 때 패키지를 지정하지 않으면, 자바에서 기본적으로 제공하는 이름없는 패키지에 포함된다. 따라서 패키지를 지정하지 않은 모든 클래스들은 같은 패키지에 속하게 된다.
<br><br><br>

## <strong>7.2 import 키워드
소스코드에서 다른 패키지의 클래스를 사용하려면 패키지명이 포함된 클래스 이름을 사용해야 한다. import문으로 사용하고자 하는 클래스의 패키지를 미리 명시해주면 이 패키지명을 생략할 수 있다.

```java
import 패키지명.클래스명;
//or
import 패키지명.*; // 지정된 패키지에 속하는 모든 클래스를 패키지명 없이 사용가능
```
* 위 두 선언방식에서 실행 시 성능상의 차이는 전혀 없다.
* 단, '*'은 하위 패키지의 클래스까지 포함하지 않는다.
```java
import java.util.*;
import java.text.*;

// Date today = new Data();

import java.*;

// util.Date today = new Data();
```

### static import문
static import문을 사용하면 static멤버를 호출할 때 클래스 이름을 생략할 수 있다.
```java
import static java.lang.Integer.*;    // Integer클래스의 모든 static메서드
import static java.lang.Math.random;  // Math.random()만.
import static java.lang.System.out;   // System.out을 out만으로 참조가능.

//////////////////////////////////////////////////////////////////////////

System.out.println(Math.random());
// import static을 사용하면 아래와 같이 선언이 가능하다.
out.println(random());
```

### import하지 않아도 되는 package
* java.lang 패키지 ( 예:: System.out.println("--static case--");, String )
* 같은 패키지

<br><br><br>

## <strong>7.3 클래스패스
클래스패스란 클래스를 찾기위한 경로이다. 즉, JVM이 프로그램을 실행할 때, 클래스 파일(.class)을 찾는데 기준이 되는 파일 경로를 말하는 것이다.
* JVM은 CLASSPATH의 경로를 확인하여 라이브러리 클래스들의위치를 참조하게 된다
* J2JDK 버전부터는 \jre\lib\ext 폴더에 필요한 클래스 라이브러리들을 복사해 놓으면 사용가능하여 특별한 경우가 아니면 설정을 하지 않는다.
  
![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc9SxVe%2FbtqRt6IVNEY%2FqzfqkA4a6Da4Z2tVyD9d4k%2Fimg.png)

java runtime(java 또는 jre)으로 이 .class 파일에 포함된 명령을 실행하려면, 이 파일을 찾을 수 있어야 한다. .class 파일을 찾을 때, classpath에 지정된 경로를 사용한다.

classpath는 .class 파일이 포함된 디렉토리와 파일을 콜론(;)으로 구분한 목록이다.

이 classpath 를 지정하기 위한 두 가지 방법이 있다.

CLASSPATH 환경변수 사용
java runtime 에 -classpath 옵션 사용

<br><br><br>

## <strong>7.4 CLASSPATH 환경변수
```
CLASSPATH=.;C:\Program Files\Java\jdk-10.0.1\lib\tools.jar
```

JVM이 시작될 때 JVM의 클래스 로더는 CLASSPATH 환경변수를 호출해서 환경 변수에 설정되어 있는 디렉토리의 클래스들을 먼저 JVM에 로드한다.

그러므로 CLASSPATH 환경 변수에는 필스 클래스들이 위치한 디렉토리를 등록하도록 한다.

컴퓨터 시스템 변수 설정을 통해 지정할 수 있다.

<br>

### 환경변수는 세 가지를 설정할 수 있다

|      옵션      |               설명               |   
| :------------: | ------------------------------ |
| Path | OS에서 명령어를 실행할 때 명령어를 찾아야 하는 폴더의 순위를 설정하는 환경 변수
| CLASSPATH | JVM이 시작될 때 JVM의 클래스 로더는 이 환경 변수를 호출한다. 그래서 환경 변수에 설정되어 있는 디렉토리가 호출되면 그 디렉토리에 있는 클래스들을 먼저 JVM에 로드한다. 그러므로 CLASSPATH 환경 변수에는 필스 클래스들이 위치한 디렉토리를 등록하도록 한다.
| JAVA_HOME | JDK가 설치된 홈 디렉토리를 설정하기 위한 환경 변수다. 반드시 필요한 환경 변수는 아니지만 Path와 CALLPATH 환경 변수에 값을 설정할 때 JAVA_HOME 환경 변수를 포함하여 설정한다.
|


<br><br><br>

## <strong>7.5 -classpath 옵션
컴파일러가 컴파일 하기 위해서 필요로 하는 참조할 클래스 파일들을 찾기 위해서 컴파일시 파일 경로를 지정해주는 옵션이다
```
java <option> <classfile> <argument>
```
* 옵션
* 호출될 클래스 파일 이름
* main 함수에 파라미터로 보내질 문자열

Hello.java파일이 C:\Java 디렉터리에 존재하고,

필요한 클래스 파일들이 C:\Java\Engclasses에 위치한다면,
```
javac -classpath C:\Java\Engclasses C:\Java\Hello.java 
```
로 해주면 된다.

만약 참조할 클래스 파일들이 그 외의 다른 디렉터리, 그리고 현 디렉토리에도 존재한다면,
```
javac -classpath .;C:\Java\Engclasses;C;\Java\Korclasses C:\Java\Hello.java
```
과 같이 ; 으로 구분해줄 수 있다. ( . 은 현 디렉토리, .. 은 현 디렉토리에서 상위 디렉토리를 의미)

또한 classpath 대신 단축어인 cp를 사용해도 된다.
```
javac -cp .;C:\Java\Engclasses;C;\Java\Korclasses C:\Java\Hello.java
```
<br><br><br>


## Reference

Book: Java의 정석 3rd Edition

https://kils-log-of-develop.tistory.com/430

https://yadon079.github.io/2020/java%20study%20halle/week-07

https://blog.opid.kr/62