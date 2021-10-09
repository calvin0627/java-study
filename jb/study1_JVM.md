## 1. JVM은 무엇이며 자바 코드는 어떻게 실행하는 것인가

### 목표

자바 소스 파일(.java)을 JVM으로 실행하는 과정 이해

#### 자바

썬 마이크로시스템즈에서 개발한 프로그래밍 언어

**특징**

- os 독립적
  - 일종의 에뮬레이터인 JVM을 통해서 os 독립적으로 실행 가능
  - 자바 애플리케이션은 JVm와 통신하고 JVM이 자바 애플리케이션에서 받은 명령을 os에 맞게 변환
  - JVM이 os에 의존적
- 객체지향 언어
  - 객체지향개념의 특징인 상속, 캡슐화, 다형성이 잘 적용
- Garbage Collection
  - 가비지 컬렉터가 자동으로 메모리 관리

#### Java 실행을 위한 세 가지 구성 요소

![img](http://wikidocs.net/images/page/257/jdk.jpg) 

- **JDK(Java Development Kit)**: Java 애플리케이션 개발을 위한 도구들의 포함
  -  JRE 와 개발에 필요한 도구(javac, jdb)등을 포함한다
- **JRE(Java Runtime Environment)**:  JVM이 자바 프로그램을 동작시킬때 필요한 라이브러리 파일(Java Class Libraries) 등 원할한 실행에 필요한 리소스들을 제공
  - Java Class Libraries: Java실행에 필수적 라이브러리
    - java.io, java.util, java.thread ...

- **JVM(Java Virtual Machine)**: Java  바이트코드를 실행

JRE는 JDK를 사용하여 작성된 Java코드를 JVM에서 실행에 필요한 라이브러리와 결합한 후 결과 프로그램을 실행하는 JVM 인스턴스 작성.

### JVM

가상 머신이란 프로그램을 실행하기 위한 물리적 머신을 소프트웨어로 구현한 것이다. 자바의 목표였던 WORA(write once run anywhere)를 구현하기 위해 물리적 머신과 별도로 자바 바이트코드를 JVM위에서 실행하게 함. JVM은 플랫폼에 의존한다.

이와 반대로 c/c++은 컴파일 플랫폼과 타겟 플랫폼(운영체제 + cpu 아키텍처)이 다를 경우, 프로그램이 동작하지 않는다.

##### 컴파일러와의 비교

1. 복잡한 최적화 과정은 바이트코드 컴파일러가 대신 해주므로 고려하지 않아도 된다.
2. 바이트코드는 빠른 기계어 변환을 목적으로 설계되었기 때문에 일반적인 컴파일러보다 제작 과정이 수월하다.

##### 특징

- 플랫폼 독립성 보장
  - 기본 자료형 크기 플랫폼에 따라 바뀌지 않는다
- **스택 기반**의 가상 머신: 보통의 컴퓨터 아키텍처들은 레지스터 기반으로 동작하지만 JVM은 스택기반으로 동작한다
- primitive data type(int, double, ...)을 제외한 모든 타임(클래스, 인터페이스)를 명시적인 메모리 주소 기반의 레퍼런스가 아니라 심볼릭 레퍼런스 통해 참조
  - 심볼릭 레퍼런스는 런타임 시점에 메모리 상의 실제 주소로 대체, 
- 플랫폼 의존적



**구조**

<img src="https://blog.kakaocdn.net/dn/GhnRV/btqB9Y0Ax1V/snKN7iK04XGZXySMpujhWK/img.png" alt="img" style="zoom:80%;" />

- **클래스 로더 시스템**: 컴파일 결과로 만들어진 .class 바이트코드 파일을 읽어 메모리에 배치 런타임에 클래스 참조시 로드와 링크를 수행하여 메모리에 적재

  **동작**

  - 로드: 클래스를 파일에서 가져와 JVM 메모리에 로드
  - 링킹
    - 검증: 클래스가 자바 언어 명세, JVM 명세에 맞는지 확인
    - 준비: 클래스가 필요로하는 메모리 할당, 데이터 구조 준비
    - 분석: 클래스 상수 풀 속의 심볼릭 레퍼런스를 다이렉트 레퍼런스로 변경
  - 초기화: 클래스 변수들 값 초기화. ex) static

- **JVM 메모리**: JVM이 Java Bytecode를 실행하기 위해 사용하는 메모리 공간

  모든 쓰레드에서 공유

  - Method Area: 클래스 정보를 저장하는 공간
    - 클래스, 인터페이스에 대한 런타임 상수 풀, 필드와 메서드 정보, static 변수, 메서드의 바이트 코드 등을 보관
    - 런타인 상수 풀: 각 클래스와 인터페이스의 상수, 메서드와 필드에 대한 레퍼런스들 담고 있다. 메서드나 필드를 참조할 때 런타임 상수 풀을 통해 해당 메소드나 필드의 실제 메모리 주소를 찾아서 참조한다
  - Heap: <u>객체</u>의 인스턴스나 동적할당 된 데이터를 저장하는 공간

  쓰레드 마다 존재

  - Program Counter register: 각 스레드의 명령어 주소

  - JVM Stack 

    JVM에서 메서드가 수행될떄마다 하나의 스택 프레임 생성되어 해당 스레드의 JVM 스택에 추가되고, 종료되면 제거된다.

    - Stack Frame: 메소드의 상태 정보를 저장하는 구조체
      - 구조
        - Local variables array: 메소드의 지역변수
        - Operand stack: 계산을 위해 사용하는 스택, 레지스터의 역할
        - Frame Data: Constant Pool ref,  메소드의 결과 반환값, 지역변수 저장

  - Native Method Stack: 자바 바이트 코드가 아닌 다른 언어로 작성된 메소드 사용시 쓰이는 스택

- **실행 엔진**

  - 인터 프리터
    - 바이트코드를 한줄씩 읽어 네이티브 코드로 변환
  - JIT(Just In Time) 컴파일러
    - 인터프리터 방식으로 실행하다가 적절한 시점에 바이트코드 전체를 컴파일하여 네이티브 코드로 변경하고, 해당 메서드는 인터프리팅 없이 실행
    - 변환된 네이티브 코드는 캐시에 보관하고 이후 변경된 부분만 컴파일하고 나머지는 바로 실행
  - GC(Garbage Collector)
    - 참조되지 않는 객체를 메모리에서 정리한다

### 컴파일 및 실행

- java 파일 작성

- 컴파일 단계: `javac example.java`  를 통해 .java 파일을 .class 파일(바이트 코드)로 컴파일
- 실행 단계: `java example.class` 명령어를 사용하여 .class 파일을 실행
- JVM을 통해 프로세스가 되어 실행

![JVMInternal1](https://d2.naver.com/content/images/2015/06/helloworld-1230-1.png)  

- javac 옵션

|      옵션      |               설명               |               예시                |
| :------------: | :------------------------------: | :-------------------------------: |
| -classpath, cp |     실행할 클래스 위치 지정      | javac -cp "Users/home/Test.java"  |
|       -d       |    클래스 파일 생성위치 지정     |         javac -d "/path"          |
|   -encoding    | 소스 파일에 사용된 인코딩을 지정 | javac -encodign "utf-8" Test.java |
|       -g       |      모든 디버깅 정보 출력       |          javac -g "path"          |
|    -verbose    |   컴파일러 진행 작업 모두 출력   |       javac -verbose "path"       |
|  -sourcepath   |        소스파일 위치 지정        |      javac -sourpath "path"       |
|    -source     |     소스파일 자바 버전 지정      |     javac -source 1.7 "path"      |
|    -target     |     타겟파일 자바 버전 지정      |       javac -target 1.8 ""        |



#### JIT(Just In Time) Compiler

런타임 시 바이트 코드로 컴파일하여 Java 애플리케이션의 성능을 향상시키는 런타임 환경의 컴포넌트

-  인터프리터 방식: 클래스 파일을 로드하고 각 개별 바이트 코드의 시맨틱을 판별하며 계산 수행. 해석 중 추가 프로세서 및 메모리 사용이 들게 되므로 오버헤드 생긴다. 
-  JIT: 런타임 시 바이트 코드를 원시 시스템 코드로 컴파일하여 Java 프로그램의 성능을 향상시킨다.
   - 컴파일 시 오버헤드가 발생하게 되므로, 모든 메소드를 컴파일하는 것은 비효율적
   - -메소드 처음 호출 시에는 컴파일되지 않고, JVM은 각 메소드에 대해 사전에 정의된 컴파일 임계값에서 시작하여, 해당 메소드가 호출될 때마다 감소되는 호출 개수를 관리하여 자주 사용되는 메소드는 JVM 시작 직후(soon after) 컴파일 되고, 빈도가 낮은 메소드는 나중에 컴파일 되거나, 컴파일 되지 않는다.
     - 임계값을 조절하며 다양한 수준의 최적화 설정할 수 있다
       - cold warm hot very hot

https://www.ibm.com/docs/en/ztpf/2021?topic=reference-jit-compiler

---

#### 참조

https://d2.naver.com/helloworld/1230

http://www.tcpschool.com/java/java_intro_programming



#### 문제

- 바이트 코드 이름의 의미
- stack을 통해 변수 저장하는 이유

#### 질문

- 함수, 메소드의 차이: os 객체랑 연관되어 객체안에서 실행되면 method.
- 클래스로더 jre, jvm?

#### Q/A

