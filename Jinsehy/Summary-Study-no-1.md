## 1. JVM은 무엇이며 자바 코드는 어떻게 실행하는 것인가

## 목표

자바 소스 파일(.java)을 JVM으로 실행하는 과정 이해


## 1.1 Bytecode와 Binarycode
`Bytecode`

* binary data이고, loading info나 JVM의 execution instructions에 대한 정보를 포함하고 있다. 그러므로 binarycode의 specialcase다.

`Binarycode`

* ARM과 같은 processor를 위한 machine instruction들로 이루어진 코드파일을 의미한다.

Bytecode에도 instructions 정보가 있는데 왜 Binarycode라고 안하냐? 라고 물으면 이 둘을 나누는 기준은 Processor-specific 한가, 아니냐 이다.

Bytecode는 JVM instructions 에 대한 코드파일이 포함되어 있기 때문에 JVM만 있으면 어떠한 hardware(processor)에서도 실행가능하다.


## 1.2 자바(JAVA)

썬 마이크로시스템즈에서 개발한 프로그래밍 언어

### 1.2.1 특징

- os 독립적
  - 일종의 에뮬레이터인 JVM을 통해서 os 독립적으로 실행 가능
  - 자바 애플리케이션은 JVm와 통신하고 JVM이 자바 애플리케이션에서 받은 명령을 os에 맞게 변환
  - JVM이 os에 의존적
- 객체지향 언어
  - 객체지향개념의 특징인 상속, 캡슐화, 다형성이 잘 적용
- Garbage Collection
  - 가비지 컬렉터가 자동으로 메모리 관리

### 1.2.2 Java 실행을 위한 세 가지 구성 요소

![img](http://wikidocs.net/images/page/257/jdk.jpg) 

- **JDK(Java Development Kit)**: Java 애플리케이션 개발을 위한 도구들의 포함
  -  JRE 와 개발에 필요한 도구(javac, jdb)등을 포함한다
- **JRE(Java Runtime Environment)**:  JVM이 자바 프로그램을 동작시킬때 필요한 라이브러리 파일(Java Class Libraries) 등 원할한 실행에 필요한 리소스들을 제공
  - Java Class Libraries: Java실행에 필수적 라이브러리
    - java.io, java.util, java.thread ...

- **JVM(Java Virtual Machine)**: Java  바이트코드를 실행

JRE는 JDK를 사용하여 작성된 Java코드를 JVM에서 실행에 필요한 라이브러리와 결합한 후 결과 프로그램을 실행하는 JVM 인스턴스 작성.

## 1.3 JVM

* Java virtual machine의 약자로, VM(가상 머신)이란 프로그램을 실행하기 위한 물리적 머신을 소프트웨어로 구현한 것이다. 자바의 목표였던 WORA(write once run anywhere)를 구현하기 위해 물리적 머신과 별도로 자바 바이트코드를 JVM위에서 실행하게 함. JVM은 플랫폼에 의존한다.

* 이와 반대로 c/c++은 컴파일 플랫폼과 타겟 플랫폼(운영체제 + cpu 아키텍처)이 다를 경우, 프로그램이 동작하지 않는다.

### 1.3.1 컴파일러와의 비교

1. 복잡한 최적화 과정은 바이트코드 컴파일러가 대신 해주므로 고려하지 않아도 된다.
2. 바이트코드는 빠른 기계어 변환을 목적으로 설계되었기 때문에 일반적인 컴파일러보다 제작 과정이 수월하다.

### 1.3.2 특징

- 플랫폼 독립성 보장
  - 기본 자료형 크기 플랫폼에 따라 바뀌지 않는다
- **스택 기반**의 가상 머신: 보통의 컴퓨터 아키텍처들은 레지스터 기반으로 동작하지만 JVM은 스택기반으로 동작한다
- primitive data type(int, double, ...)을 제외한 모든 타임(클래스, 인터페이스)를 명시적인 메모리 주소 기반의 레퍼런스가 아니라 심볼릭 레퍼런스 통해 참조
  - 심볼릭 레퍼런스는 런타임 시점에 메모리 상의 실제 주소로 대체, 
- 플랫폼 의존적



### 1.3.3 구조

<img src="https://blog.kakaocdn.net/dn/GhnRV/btqB9Y0Ax1V/snKN7iK04XGZXySMpujhWK/img.png" alt="img" style="zoom:80%;" />

- **클래스 로더 시스템**: 컴파일 결과로 만들어진 .class 바이트코드 파일을 읽어 메모리에 배치 런타임에 클래스 참조시 로드와 링크를 수행하여 메모리에 적재

  **동작**

  - `로드(Loading)`: 클래스를 파일에서 가져와 JVM 메모리에 로드
  - `링킹(Linking)`
    - 검증: 클래스가 자바 언어 명세, JVM 명세에 맞는지 확인
    - 준비: 클래스가 필요로하는 메모리 할당, 데이터 구조 준비
    - 분석: 클래스 상수 풀 속의 심볼릭 레퍼런스를 다이렉트 레퍼런스로 변경
  - `초기화(Initialization)`: 클래스 변수들 값 초기화. ex) static


>우선은 `Lion.java` 을 우리가 코딩하여 만들었다고 치자.
>
>`javac Lion.java`(Java Compiler로 컴파일) 을 하면 `Lion.class` 가 생성된다. 이 파일은 바이트코드로 이루어져 있다.
>
>여기서부터 JVM의 역할이 시작된다. (여기서 부터 언급되는 것들은 전부 JVM안에서 이루어지고 있는 일들임.)
>
>### 1. JVM-Loading
>
>우선 Lion.class가 Class Loader에 들어간다. 그러면 Class Loader는 binary data를 만들어 Method Area에 저장한다. binary data에는 
>
>* Method Area어 저장된(load된) class의 fully qualified name과 그것의 parent class
>* Lion.class가 Class or Interface or Enum에 관련되어 있는지
>* Lion.class 파일 내 Modifier, Variables, Method 정보들이 저장되어 있다.
>
>이것을 두고 Lion.class 파일을 loading 했다고 한다.
>
>이후에 JVM은 곧바로 이 파일을 표현하는 Class type의 객체를 만들어 heap memory에 저장한다. 이 Class type은 java.lang.package에 정의된 것이다.
>
>### 2. JVM-Linking
>
>Linking에는 3가지 과정이 있다.
>
>1. Verification
>2. Preparation
>3. Resolution
>
>Verification은 ByteCodeVerifier로 이루어진다. Lion.class 파일이 정확히 >formatted되었고, valid compiler로 만들어 졌는지 확인한다.
>
>Preparation은 말 그대로 class 변수들을 위한 메모리 공간을 할당하고, 기본값으로  초기화 시킨다.
>
>Resolution. Verification이 끝나면 Lion.class의 변수나 함수같은 components가 symbol로 변환되어 string으로 run-time constant pool에 저장된다. 이러한 symbolic reference를 .가지고 method area를 searching하여 referenced 된 것으로 교체하는 작업이다. 사실 Resolution에 관한 부분은 명확하지 않다.
>
>### 3. JVM-Initialization
>
>class 안에서와 class hierarchy에 대해서나 top bottom 순서로 static 변수들을 모두 할당하는 작업이다.


- **JVM 메모리**: JVM이 Java Bytecode를 실행하기 위해 사용하는 메모리 공간

  모든 쓰레드에서 공유

  - `Method Area`: 클래스 정보를 저장하는 공간
    - 클래스, 인터페이스에 대한 런타임 상수 풀, 필드와 메서드 정보, static 변수, 메서드의 바이트 코드 등을 보관
    - 런타인 상수 풀: 각 클래스와 인터페이스의 상수, 메서드와 필드에 대한 레퍼런스들 담고 있다. 메서드나 필드를 참조할 때 런타임 상수 풀을 통해 해당 메소드나 필드의 실제 메모리 주소를 찾아서 참조한다
  - `Heap`: <u>객체</u>의 인스턴스나 동적할당 된 데이터를 저장하는 공간

  쓰레드 마다 존재

  - `Program Counter register`: 각 스레드의 명령어 주소

  - `JVM Stack`

    JVM에서 메서드가 수행될떄마다 하나의 스택 프레임 생성되어 해당 스레드의 JVM 스택에 추가되고, 종료되면 제거된다.

    - Stack Frame: 메소드의 상태 정보를 저장하는 구조체
      - 구조
        - Local variables array: 메소드의 지역변수
        - Operand stack: 계산을 위해 사용하는 스택, 레지스터의 역할
        - Frame Data: Constant Pool ref,  메소드의 결과 반환값, 지역변수 저장

  - `Native Method Stack`: 자바 바이트 코드가 아닌 다른 언어로 작성된 메소드 사용시 쓰이는 스택

- **실행 엔진**
  - `인터 프리터`
    - 바이트코드를 한줄씩 읽어 네이티브 코드로 변환
  - `JIT(Just In Time) 컴파일러`
    - 인터프리터 방식으로 실행하다가 적절한 시점에 바이트코드 전체를 컴파일하여 네이티브 코드로 변경하고, 해당 메서드는 인터프리팅 없이 실행
    - 변환된 네이티브 코드는 캐시에 보관하고 이후 변경된 부분만 컴파일하고 나머지는 바로 실행
  - `GC(Garbage Collector)`
    - 참조되지 않는 객체를 메모리에서 정리한다

### 1.3.4 컴파일 및 실행

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



## 1.4 JIT(Just In Time) Compiler

### 인터프리터 방식
 클래스 파일을 로드하고 각 개별 바이트 코드의 시맨틱을 판별하며 계산 수행. 해석 중 추가 프로세서 및 메모리 사용이 들게 되므로 오버헤드 생긴다. 
### JIT Compiler
런타임시 동적으로 바이트 코드를 컴파일하여 애플리케이션의 성능을 향상시키는 런타임 환경의 컴포넌트,
### JIT in JVM
런타임 시 바이트 코드를 원시 시스템 코드로 컴파일하여 Java 프로그램의 성능을 향상시킨다. 메소드 영역에 있는 코드 캐시(Code Cache) 공간에 컴파일된 코드 저장하여 캐싱한다

### 컴파일 정책
- 컴파일 시 오버헤드가 발생하게 되므로, 모든 메소드를 컴파일하는 것은 비효율적
- JVM은 각 메소드에 대해 사전에 정의된 컴파일 임계값에서 시작하여, 해당 메소드가 호출될 때마다 감소되는 호출 개수를 관리하고 임계값에 도달할 시 컴파일 시킨다. 따라서 자주 사용되는 메소드는 JVM 시작 후 곧 컴파일 되고, 빈도가 낮은 메소드는 나중에 컴파일 되거나, 컴파일 되지 않게 된다
 - 카운터 종류
    - invocation counter: 메소드 시작시 마다 증가
    - backedge counter: 큰 값의 인덱스를 가진 바이트코드에서 작은 값을 가진 인덱스로 컨트롤 흐름이 변경될 때 증가(루프)

## 1.5 Deep into Garbage collection
Garbage collection이란, 메모리의 할당을 자동적으로 관리에 주는 algorithm이다. 따라서 c, c++과 같이 메모리 할당과 해제에 관하여 프로그래밍 중에 깊에 고민할 필요가 없다. 그러나 GC에는 여러가지 알고리즘이 있고, 상황에 따라 성능또한 달라지므로 어느 GC 알고리즘을 써야할 지에 대한 고민은 남아있다. 그러므로 이 글에서는 여러가지 GC 알고리즘을 소개하고, 어떠한 상황에서 사용하면 좋은지 소개할 예정이다.

Java에서는 GC를 지원하는데, 그렇다고 명시적으로 메모리 해제를 못하는 것도 아니다. System.gc()로 해제할 수 있으나, 성능에 지대한 영향을 끼치기 때문에 절대로 사용하지 않아야 한다고 한다.(비효율적인 작동을 한다) 이 부분은 구글링하면 나오는 것 같다.

시작하기 전에 이 용어를 꼭 알고 가자.

`stop-the-world`: GC(garbage collection)을 위해 JVM이 application 실행을 멈추는 것이다. 만약 여러개의 스레드가 작업을 하고 있었다면, GC를 실행할 스레드를 제외하고는 전부 멈춘 상태가 된다.

### Garbage Collection 알고리즘 선택의 중요성

Oracle 공식 문서에 따르면 GC는 4가지 작업을 통해 메모리 관리를 한다고 한다.

1. OS로 부터 메모리를 할당하고 해제
2. Application으로의 메모리 할당
3. 메모리의 어느 부분이 Application이 아직 사용 중인지 판단
4. Application이 사용하지 않는 메모리를 회수



### Garbage collector의 성능 향상을 위한 요소

1. generational scavenging

   한글로 번역이 어려워 영어로 적었지만, 어려운 내용이 아니다. GC는 해당 객체가 아직까지 사용되고 있는지를 알고 싶어 하기 때문에, 이를 쉽게 파악하기 위한 구조를 만든 것이다. heap을 young - old 부분으로 나누어서, 객체는 먼저 young 부분에 저장이 된다. young 부분만 따로 GC를 수행하는데, 여기에서 살아남은 객체들이 Old 부분으로 복사되어진다. 그리고 young과는 독립적으로 old에서도 GC가 수행되는데, 이때도 살아남은 객체들은 Permanent 부분으로 복사되어 프로그램의 종료 시 까지 유지된다. 이 Permanent generation 부분이 Method area이다.

   <img src="https://d2.naver.com/content/images/2015/06/helloworld-1329-1.png" >

   

2. multi-thread 사용

   작업을 parallel, concurrent하게 수행하는 것이 가능해져 효율이 상승한다. parallel이란 동시에 여러 스레드가 작업할 수 있으므로 single-thread를 이용하는 것보다 GC를 빨리 끝낼 수 있다. 여기서 concurrent하다는 말은 즉슨, application의 실행과 GC를 context-switching을 하며 수행할 수 있다는 것이다. 그러므로 Multi-thread를 사용해서 Concurrent하게 작업하게 되면 최대 stop-the-world 시간이 작기 때문에 application이 멈추는 시간을 줄일 수 있으므로 latency가 낮다.  

   

   3. 최대한 큰 연속적인 빈 메모리를 유지하기 위한 노력
      compaction을 통해 fragementation이 일어난 부분을 정리하여 최대한 큰 연속적인 빈 메모리를 유지한다. 메모리 한쪽으로 데이터를 몬다고 보면 된다. Serial이나 Parallel의 경우 힙의 가장 앞부터 끝에 있는 객체까지 순차적으로 수행하기 때문에 이러한 노력의 이점이 있다.

   

### Garbage collection 선택의 중요성

<img src="https://docs.oracle.com/en/java/javase/17/gctuning/img/jsgct_dt_005_gph_pc_vs_tp.png">

위의 그림에서, 1% GC의 의미란 application 실행시간중 1%를 garbage collection에 할당한다는 것이다. 눈으로 보면 알 수 있듯이, 프로세서의 수가 늘어날수록 엄청난 성능의 차이를 보인다. 반대로 보면, garbage collector의 조그만한 성능 향상이 앱 실행 시간에 큰 영향을 끼칠 수 있으므로 아주 중요한 이슈이다.

### Garbage Collection 알고리즘의 종류

1. Serial Collector

   single thread를 사용하여 GC를 수행하여 context-switch나 thread간의 통신이 없어도 돼 그 부분에 대한 오버헤드가 없다. 따라서 힙 사이즈 100MB이하인 작은 프로그램을 수행하기에 알맞다.

2. Parallel Collector

   Serial Collector와 같지만 multi-thread로 GC를 수행한다는 점이 다르다. 중형 ~ 대형 힙 크기를 가지는 프로그램을 수행하기에 알맞다.

3. Garbage-First(G1) Collector

   큰 메모리의 GC를 수행하기 위해 고안되었다. 높은 throughput을 제공하면서, pause-time goal을 높은 확률로 맞출 수 있다. 따라서 대부분의 OS나 하드웨어에 대해서 JVM은 G1 collector을 기본으로 하고 있다.

4. The Z Garbage Collector

   application thread의 멈춤없이 GC가 concurrent하게 진행된다. concurrentcy의 장점을 살린 알고리즘이라고 보면 된다. 따라서 가장 낮은 latency를 제공하나,  context-switching에서 발생하는 overhead 때문에 그만큼 throughput에서의 손실이 있다.ㄴ

   

### Generational Garbage Collection

위에서 언급한 성능 향상을 위한 요소중 generational scavenging을 이용한 알고리즘이다. The Z Garbage Collector를 제외한 모든 Collector가 Generational Garbage Collection이다. 

위에서 대강 언급한 내용에 대해 자세히 설명하겠다.



#### Young 영역에서의 GC

힙을 두 영역 Young, Old 영역으로 나눈다. Young 영역은 또다시 Eden 과 두 개의 Survivor 영역으로 나누어진다. 처음 객체가 할당되면 Eden 영역에 들어가고,  Eden 영역에서 GC가 실행되고 남은 객체는 Survivor 영역으로 들어오게 된다. 이때 주의해야하는 것은 반드시 두 개의 Survivor 영역중 한번에 하나만 사용되어야 한다는 것이다. 이 말은 즉슨, 항상 한개의 Survivor영역은 비어있어야한다. 만약 사용중인 Survivor영역이 가득차게 되면 그곳에 GC가 일어나게 되고, 거기서 살아남은 객체들은 반대편 Survivor영역에 복사된다. 그리고 가득찬 Survivor영역은 비어진다. 여기서 의문이 하나 드는 것이 정상이다. GC가 되었는데 왜 또다시 Survivor 영역으로 들어가는거지? Survivor 영역에서 GC가 한 번 되었다고 바로 Old로 가는 것이 아니라, pre-defined된 Survivor에서의 생존 횟수에 따라 그 횟수를 넘으면 Old 영역에 들어간다. 이 값은 커맨드 라인에서 옵션을 통해 정해 줄 수가 있다. 



#### Old 영역에서의 GC

Old 영역에서의 GC는 알고리즘 별로 다르다.

* Serial GC

  Old 영역에서 살아 있는 객체를 마크하고, 힙 부분부터 마크된 객체만을 남긴뒤에, Compaction을 실시한다.

  앞에서 언급했듯이, 100MB 이하의 힙을 사용하는 application을 수행할때 사용하면 좋다.

* Parallel GC

  Serial과 같다. 당연히 스레드가 많으므로 어느정도 크기가 큰 application에 대해서 Serial GC보다 좋은 성능을 가진다.

* G1 GC

  <img src='https://d2.naver.com/content/images/2015/06/helloworld-1329-6.png'>

  위의 그림을 보면 이해가 편하다. 바둑판의 한 영역에 객체를 할당하고 GC를 수행하는데, 해당 영역이 꽉차면 다른 영역으로 가서 객체를 생성하고 GC를 수행한다. 따라서 Young -> Old 처럼 객체를 옮길 필요가 없다.



### JVM GC 디폴트 값

* Garbage-First (G1) Collector
* GC thread의 수는 maximum heap size 와 프로세서 수에 달려있다.
* initial heap size: 1/64 of physical memory
* maximum heap size: 1/4 of physical memory



### Garbage collection 성능의 두 요소

#### 1. physical memory size(Total heap size)

힙의 크기는 명시적으로 커맨드 라인을 통해 정해줄 수가 있다. 그러나 물리 메모리의 크기를 넘으면 안되기 때문에 물리 메모리의 크기는 힙 크기와 상관이 깊다. JVM은 힙의 크기를 자동으로 관리해 주는데, Heap에 비는(empty) 크기가 크면 Heap의 크기를 줄이고, 작으면 Heap의 크기를 늘여준다. 이에 대한 thredhold 값은 명시적으로 커맨드 라인에서 설정할 수도 있다. 힙의 크기가 GC에 중요한 이유는, 힙의 크기에 따라 Young이나 Old 영역에 할당해 줄 수 있는 크기가 달라지기 때문이다.

#### 2. heap size dedicated to the young generation

Young에서 Eden과 survivor의 크기도 명시적으로 정해줄 수 있다. 또한, Young 영역의 크기가 eden + survivor 영역의 크기보다 크게 설정할 수 도 있는데, 남는 부분은 Virtual이라고 부르며 Young 영역의 일부분 이기도 하다. Eden과 survivor 부분의 할당된 크기가 커지면, Virtual 부분이 작아지면서 Eden과 Survivor부분이 커지게 된다.



이렇게 힙 사이즈가 GC에 중요한 이유는, 힙 사이즈가 GC의 빈도와 수행시간에 큰 영향을 끼치기 때문이다. 힙의 절대적인 사이즈가 크다면, Young, Old 부분에 모두 큰 사이즈의 힙을 할당해 줄 수가 있다. 그러면 GC의 빈도가 줄어들 것이다. 그러나 computing power가 그만큼을 커버하지 못한다면, GC의 수행시간은 늘어난 힙 사이즈만큼 늘어날 것이다. 따라서 CPU의 성능과, heap의 크기는 GC와 매우 깊은 관련이 있다고 할 수 있다.



#### 참조

https://d2.naver.com/helloworld/1230

http://www.tcpschool.com/java/java_intro_programming

---

##### JIT compiler 정의

 https://www.ibm.com/docs/en/ztpf/2021?topic=reference-jit-compiler

##### JIT compiler counter 동작

https://goateedev.tistory.com/197

https://sungjk.github.io/2019/04/16/java-performance-tuning-4.html

---

##### Garbage collection

https://docs.oracle.com/en/java/javase/17/gctuning/index.html

https://d2.naver.com/helloworld/1329

