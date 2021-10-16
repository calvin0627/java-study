# <strong>10. 멀티쓰레드 프로그래밍
### __GOAL__: 자바의 멀티쓰레드 프로그래밍에 대해 학습하세요.
---

## <strong>8.1 Thread 클래스와 Runnable 인터페이스
쓰레드를 구현하는 방법은 `Thread 클래스`를 `상속`받는 방법과 `Runnable 인터페이스`를 `구현`하는 방법, 모두 두 가지가 있다. 어느 쪽을 선택해도 쓰레드를 통해 작업하고자 하는 내용으로 run()의 몸통{ }을 채우는 것일 뿐이다. 

Runnable과 Thread모두 java.lang 패키지에 포함되어있다

### 둘 중 무엇을 써야 하나
`extends Thread`, 즉 Thread를 상속받아 사용할 때 `run()` 외에도 다른 것들을 Override를 해야할 필요가 있다면 Thread를 상속해서 만든다.
`run()`만 사용해도 되는 경우에는 Runnable을 사용하면 된다. 또는 Thread를 상속받을 클래스가 다른 클래스도 상속받아야 된다면 Runnable을 사용한다.

### Thread 클래스를 상속
```java
class MyThread extends Thread {

    @Override
    public void run() { ... } // Thread 클래스의 run()을 오버라이딩
}
```
Thread 클래스의 메서드는 너무 많으므로 굳이 전부 언급할 필요는 없는것 같다.

### Runnable 인터페이스를 구현
```java
class MyThread implements Runnable {
    public void run() { ... } // Runnable 인터페이스의 run()을 구현
}
```
Runnable 인터페이스는 오로지 run()만 정의되어 있는 간단한 인터페이스이다. Runnable 인터페이스를 구현하기 위해서 해야 할 일은 추상메서드인 run()의 몸통{ }을 만들어 주는 것 뿐이다.
```java
public interface Runnable {
        public abstract void run();
    }
```

```java
class App {
    public static void main(String[] args) {
        ThreadOne t1 = new ThreadOne(); // Thread의 자손 클래스의 인스턴스를 생성

        Runnable r = new ThreadTwo();   // Runnable을 구현한 클래스의 인스턴스를 생성
        Thread t2 = new Thread(r);      // 생성자 Thread(Runnable Target)

/*
Runnable 인터페이스를 구현한 경우, Runnable 인터페이스를 구현한 클래스의 인스턴스를 생성한 다음, 이 인스턴스를 Thread 클래스의 생성자의 매개변수로 제공해야 한다.
*/

        t1.start();
        t2.start();
    }
}

class ThreadOne extends Thread {

    @Override
    public void run() {
        for(int i = 0; i < 3; i++) {
            System.out.println(getName()); // 조상인 Thread의 getName()을 호출
            //Thread.getName() : 쓰레드의 이름을 반환한다.
        }
    }
}

class ThreadTwo implements Runnable {
    public void run() {
        for(int i = 0; i < 3; i++) {
            // Thread.currentThread() : 현재 실행 중인 Thread의 참조를 반환
            System.out.println(Thread.currentThread().getName());
        }
    }
}
/*
Thread 클래스를 상속받으면, 자손 클래스에서 조상인 Thread 클래스의 메서드를 직접 호출할 수 있지만, Runnable을 구현하면 Thread 클래스의 static 메서드인 currentThread()를 호출하여 쓰레드에 대한 참조를 얻어 와야만 호출이 가능하다.
*/
```

### 쓰레드의 이름
쓰레드의 이름은 다음과 같은 생성자나 메서드를 통해서 지정 또는 변경할 수 있다. 쓰레드의 이름을 지정하지 않으면 ‘Thread-번호’의 형식으로 이름이 정해진다.
```java
Thread(Runnable target, String name)
Thread(String name)
// 위는 생성자, 아래는 메서드 사용 예
void setName(String name)
```

### 쓰레드의 실행
* 쓰레드를 생성했다고 해서 자동으로 실행되는 것은 아니다. start()를 호출해야만 쓰레드가 실행된다.
* start()가 호출되면 실행대기 상태에 있다가 자신의 차례가 되어야 실행된다. 물론 실행대기 중인 쓰레드가 하나도 없으면 바로 실행상태가 된다.
* 한 번 실행이 종료된 쓰레드는 다시 실행할 수 없다. 즉, 하나의 쓰레드에 대해 start()가 한 번만 호출될 수 있다는 뜻이다.
* 따라서 쓰레드의 작업을 한 번 더 수행해야 한다면 새로운 쓰레드를 생성한 다음 start()를 호출해야 한다. 만일 하나의 쓰레드에 대해 start()를 두 번 이상 호출하면 실행 시에 `IllegalThreadStateException`이 발생한다.
    ```java
    class App {
        public static void main(String[] args) {
            ThreadOne t1 = new ThreadOne();
            t1.start();
            t1 = new ThreadOne(); // 다시 생성
            t1.start();
        }
    }

    class ThreadOne extends Thread {

        @Override
        public void run() {
            for (int i = 0; i < 3; i++) {
                System.out.println(getName());
            }
        }
    }
    ```
<br><br><br>

## <strong>8.2 쓰레드의 상태
![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fp7LhJ%2FbtqTKMhEfW1%2FgahJKBIMZUFgxUTDsTWObK%2Fimg.png)
쓰레드의 상태는 다음 중 하나이다. 쓰레드의 상태는 JDK1.5부터 추가된 Thread의 getState() 메서드를 호출해서 확인할 수 있다.
### NEW
아직 시작하지 않은 쓰레드의 상태
### RUNNABLE
실행 가능한 쓰레드의 상태입니다.

JVM에서 실행 중인 쓰레드지만 프로세서와 같은 다른 운영체제의 리소스를 기다리는 상태일 수 있습니다.
### BLOCKED
monitor lock을 기다리는 동안 차단된 쓰레드의 상태입니다.

synchronized block/method를 만난 후 Object.wait을 통해 다시 재진입을 기다리는 상태입니다.
### WAITING
대기중인 쓰레드의 상태입니다.

다음과 같은 메소드를 호출해서 기다리는 상태가 될 수 있습니다.

* Object.wait with no timeout
* Thread.join with no timeout
* LockSupport.park
  
대기 상태의 쓰레드는 다른 쓰레드가 특정 작업을 수행하기를 기다리고 있습니다.

예를 들어 개체에서 Object.wait()를 호출한 쓰레드는 해당 개체의 Object.notify() 또는 Object.notifyAll()을 호출할 다른 쓰레드를 기다리고 있습니다. 쓰레드.join()이라고 하는 쓰레드가 지정된 쓰레드가 종료될 때까지 대기하고 있습니다.
### TIMED_WAITING
대기 시간이 지정된 대기 쓰레드의 쓰레드 상태입니다.

지정한 양의 대기 시간으로 다음 메서드 중 하나를 호출하여 쓰레드가 시간 대기 상태에 있습니다.

* Thread.sleep
* Object.wait with timeout
* Thread.join with timeout
* LockSupport.parkNanos
* LockSupport.parkUntil
### TERMINATED
종료된 쓰레드의 쓰레드 상태입니다. 쓰레드가 실행을 완료했습니다.

### 전체적인 흐름
1. 쓰레드를 생성하고 start()를 호출하면 바로 실행되는 것이 아니라 실행대기열에 저장되어 차례를 기다린다. 실행대기열은 Queue와 같은 구조로 먼저 실행대기열에 들어온 쓰레드가 먼저 실행된다.
2. 실행대기상태에 있다가 자신의 차례가 되면 실행상태가 된다.
3. 주어진 실행시간이 다되거나 yield()를 만나면 다시 실행대기상태가 되고 다음 차례의 쓰레드가 실행상태가 된다.
4. 실행 중에 `suspend()`, `sleep()`, `wait()`, `join()`, `I/O block`에 의해 일시정지상태가 될 수 있다. `I/O block`은 입출력작업에서 발생하는 지연상태를 말한다.
5. 지정된 일시정지시간이 다되거나(time-out), `notify()`, `resume()`, `interrupt()`가 호출되면 일시정지상태를 벗어나 다시 실행대기열에 저장되어 차례를 기다린다.
6. 실행을 모두 마치거나 `stop()`이 호출되면 쓰레드는 소멸된다.
   
단, 무조건 번호 순서대로 쓰레드가 수행되는 것은 아니다.

<br><br><br>


## <strong>8.3 쓰레드의 우선순위
Java Virtual Machine을 사용하면 응용 프로그램에 여러 실행 쓰레드가 동시에 실행될 수 있습니다.

쓰레드는 우선순위(priority)라는 속성(멤버변수)를 가지고 있는데, 이 우선순위의 값에 따라 쓰레드가 얻는 실행시간이 달라진다. 쓰레드가 수행하는 작업의 중요도에 따라 쓰레드의 우선순위를 서로 다르게 지정하여 특정 쓰레드가 더 많은 작업시간을 갖도록 할 수 있다.

쓰레드에 허용되는 우선 순위 값은 1에서 10 사이의 범위입니다. 우선 순위에 대한 쓰레드 클래스에 정의된 static variable는 3개입니다.
* public static int MIN_PRIORITY
    * 제일 낮은 우선순위로 값은 1입니다.
* public static int NORM_PRIORITY
    * 기본 쓰레드 우선순위로 값은 5입니다.
* public static int MAX_PRIORITY
    * 제일 높은 쓰레드 우선순위로 값은 10입니다.
  
`public final int getPriority()`
java.lang.Thread.getPriority() 메소드로 인해 쓰레드 우선순위 값을 가지고 올 수 있습니다.
`public final void setPriority(int newPriority)`
java.lang.Thread.setPriority() 메소드로 인해 쓰레드 우선순위 값으 변경할 수 있습니다.

두 쓰레드의 우선 순위가 같으면 어떤 쓰레드가 먼저 실행될지 예상할 수 없습니다. 쓰레드 스케줄러의 알고리즘(Round-Robin, First Come First Server 등)에 따라 다릅니다.
<br><br><br>

## <strong>8.4 Main 쓰레드
Java는 실행 환경인 JVM(Java Virtual Machine) 에서 돌아가게 된다. 이것이 하나의 프로세스이고 Java를 실행하기 위해 우리가 실행하는 main() 메소드가 메인 쓰레드이다.
```java
public class MainMethod {
    public static void main(String[] args) { // 메인 쓰레드의 시작을 선언

    }
}
```

* Main 쓰레드로부터 하위 쓰레드가 생겨날 수 있다.
* 다양한 종료 작업을 수행하므로 Main 쓰레드가 실행을 마치는 마지막 쓰레드여야 하는 경우가 많다.
* Main 쓰레드의 참조를 얻기 위해서는 Main 메소드에서 Thread Class에 있는 currentThread 메소드를 통해 얻을 수 있다.

* 따로 쓰레드를 실행하지 않고 main() 메소드만 실행하는 것을 싱글쓰레드 애플리케이션 이라고 한다
*  메인 쓰레드에서 쓰레드를 생성하여 실행하는 것을 멀티 쓰레드 애플리케이션이라고 한다
![img2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FysuVu%2FbtqTYRIn4UC%2FiWkbGnDlH6SPmk10XkSkK1%2Fimg.png)

### Daemon Thread
* Main 쓰레드의 작업을 돕는 보조적인 역할을 하는 쓰레드이다
* Main 쓰레드가 종료되면 데몬 쓰레드는 강제적으로 자동 종료가 된다.
* 모니터링 쓰레드의 구현 등에 사용한다.
  * 모니터링하는 쓰레드를 별도로 띄워 모니터링을 하다가, Main 쓰레드가 종료되면 같이 종료될 수 있도록
* Main 쓰레드가 Daemon 이 될 쓰레드의 setDaemon(true)를 호출해주면 Daemon 쓰레드가 된다
* 자세한건 더 공부해야할것 같다.

<br><br><br>

## <strong>8.5 동기화
* 멀티쓰레드 프로세스의 경우 여러 쓰레드가 같은 프로세스 내의 자원을 공유하기 때문에 서로의 작업에 영향을 줄 수 있다.
* 여러 개의 쓰레드가 한 개의 리소스를 사용하려고 할 때 사용 하려는 쓰레드를 제외한 나머지들을 접근하지 못하게 막는 것이다.
* 이것을 쓰레드에 안전하다고 하다 (Thread-safe)
* 자바에서 동기화 하는 방법은 3가지로 분류된다
    * Synchronized 키워드
    * Atomic 클래스
    * Volatile 키워드
이와 관련해서는 우선 개념만 소개를 하겠다. 자세한 사용법에 대한 내용은 양이 방대해서.. 더 공부하고 직접 사용해보며 익히는게 맞는것 같다.

### `Synchronized` 키워드
두 가지 방법으로 동기화를 적용할 수 있다
* 메소드 자체를 synchronized로 선언하는 방법(synchronized methods)
* 다른 하나는 메소드 내의 특정 문장만 synchronized로 감싸는 방법(synchronized statements)

### `Atomic` 클래스
* Atomicity(원자성)의 개념은 '쪼갤 수 없는 가장 작은 단위'를 뜻한다
* 자바의 Atomic Type은 Wrapping 클래스의 일종으로, 참조 타입과 원시 타입 두 종류의 변수에 모두 적용이 가능하다. * 사용시 내부적으로 CAS(Compare-And-Swap) 알고리즘을 사용해 lock 없이 동기화 처리를 할 수 있다.
* Atomic Type경우 volatile과 synchronized 와 달리 java.util.concurrent.atomic 패키지에 정의된 클래스이다
* CAS는 특정 메모리 위치와 주어진 위치의 value를 비교하여 다르면 대체하지 않는다.
* 사용법은 변수를 선언할때 타입을 Atomic Type으로 선언해주면된다
  * ex) AtomicLong

### `Volatile` 키워드
* volatile keyword 는 Java 변수를 Main Memory에 저장하겠다라는 것을 명시하는것이다
* 매번 변수의 값을 Read할 때마다 CPU cache에 저장된 값이 아닌 Main Memory에서 읽는 것입니다.
* 또한 변수의 값을 Write할 때마다 Main Memory 에 까지 작성하는 것입니다.
<br><br><br>

## <strong>8.6 데드락
교착상태(데드락, deadlock)은 두 개 이상의 작업이 서로 상대방의 작업이 끝나기를 기다리고 있어서 아무것도 완료되지 못하는 상태를 말한다.

### 데드락의 조건 4가지
* 상호배제(Mutual exclusion) : 프로세스들이 필요로 하는 자원에 대해 배타적인 통제권을 요구한다.
* 점유대기(Hold and wait) : 프로세스가 할당된 자원을 가진 상태에서 다른 자원을 기다린다.
* 비선점(No preemption) : 프로세스가 어떤 자원의 사용을 끝낼 때까지 그 자원을 뺏을 수 없다.
* 순환대기(Circular wait) : 각 프로세스는 순환적으로 다음 프로세스가 요구하는 자원을 가지고 있다.

점유대기와 비선점이 존재해야 순환대기가 생길 수 있다(모든 조건이 독립이 아님)

위 4가지 중 하나만 불만족하면 데드락은 생기지 않는다.

더 이상의 자세한 이야기는 OS/DB 내용이므로 추가적으로 공부하도록 하자. 
<br><br><br>




## Reference

https://yadon079.github.io/2021/java%20study%20halle/week-10

https://velog.io/@youngerjesus/%EC%9E%90%EB%B0%94-%EB%A9%80%ED%8B%B0%EC%93%B0%EB%A0%88%EB%93%9C

https://sujl95.tistory.com/63