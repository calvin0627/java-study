# <strong>6. 상속
### __GOAL__: 자바의 상속에 대해 학습하세요.
---

## <strong>6.1 자바 상속의 특징
상속은 기존의 클래스를 재사용하여 새로운 클래스를 작성하는 것이다. 상속을 통해서 클래스를 작성하면 적은 양의 코드로 새로운 클래스를 작성할 수 있고 코드를 공통적으로 관리할 수 있기 때문에 코드의 추가 및 변경이 매우 용이하다.

자바에서 상속의 구현은 다음과 같다. 새로 작성하고자 하는 클래스의 이름 뒤에 상속받고자 하는 클래스의 이름을 키워드 ‘extends’와 함께 써 주기만 하면 된다.

```java
 class Child extends Parent {
        // ...
    }
```

새로 작성하려는 클래스는 Child이고 기존 클래스는 Parent이다. 두 클래스는 서로 상속 관계에 있다고 하며, 상속해주는 클래스(Parent)를 ‘조상 클래스’라 하고 상속 받는 클래스(Child)를 ‘자손 클래스’라고 한다.

* 조상 클래스 : 부모(parent)클래스, 상위(super)클래스, 기반(base)클래스
* 자손 클래스 : 자식(child)클래스, 하위(sub)클래스, 파생된(derived)클래스

```java
 class Parent { }
 class Child extends Parent { }
```
클래스는 전부 또는 일부를 그 클래스의 특성으로부터 상속받는 서브클래스를 가질 수 있으면, 클래스는 각 서브 클래스에 대해 수퍼 클래스가 된다. 서브클래스는 자신만의 메서드와 변수를 정의할 수도 있다.

이러한 클래스와 그 서브클래스 간의 구조를 "클래스 계층(class hierarchy)"이라 한다.

자손 클래스는 조상 클래스의 모든 멤버를 상속받기 때문에 조상 클래스에 멤버변수가 추가되면 자손 클래스에 자동적으로 멤버변수가 추가된 것과 같은 효과를 얻는다. 반대로 자손 클래스에 새로운 무언가가 추가되어도 조상 클래스에는 아무런 영향을 주지 않는다.
<br>

상속을 받는다는 것은 조상 클래스를 확장(extends)한다는 의미로 해석할 수도 있으며 상속에 사용되는 키워드가 ‘extends’인 이유이기도 하다.

* 생성자와 초기화 블럭은 상속되지 않는다. 멤버만 상속된다.
* 자손 클래스의 멤버 개수는 항상 조상 클래스보다 같거나 많다.

<br>

### 자바의 상속의 특징을 크게 세 가지로 정리하면 다음과 같다.

1. `Single`: 자바는 다중상속을 지원하지 않는다. 즉, 자식 클래스는 하나의 부모만 가질 수 있다. 단, 자바에서 모든 클래스는 Object를 암묵적으로 상속받고 있다.
   ```java
    public class A {
	    public static void main(String args[]){	
    	    System.out.println(A instanceof Object)  // --> true 
        }
    }
   ```
2. `Multilevel`: 자바에서 상속은 여러단계로 이루어질 수 있다. 즉 A 클래스를 B 클래스가 상속받고 B 클래스를 상속받은 C클래스를 받을 수 있다.
3. `Hierarchical`: 자바에서는 한 클래스를 여러 클래스가 상속 받을 수 있다. A 클래스를 상속하는 B 클래스와 C 클래스를 만들 수 있다.

<br>

### 클래스 간의 포함(Composite) 관계
상속을 맺지 않더라도 포함 관계를 사용하면 클래스를 재사용할 수 있다.
```java
    class Circle {
        int x;      // 원점 x 좌표
        int y;      // 원점 y 좌표
        int r;      // 반지름
    }

    class Point {
        int x;      // x 좌표
        int y;      // y 좌표
    }
```

```java
    class Circle {
        Point c = new Point(); // 이처럼 한 클래스를 작성하는데 다른 클래스를 멤버변수로 선언하여 포함시킬 수 있다.
        int r;
    }
```
상속관계를 맺을지, 포함관계를 맺을지는 개념적인 문제이다. 상속은 is-a, 포함은 has-a

(ex. 상속: SportsCar is a Car / 포함: Circle has a Point)


<br><br><br>

## <strong>6.2 super 키워드

* super는 자손 클래스에서 조상 클래스로부터 상속받은 멤버를 참조하는데 사용되는 참조변수이다. 멤버변수와 지역변수의 이름이 같을 때 this를 붙여서 구별했듯이 상속받은 멤버와 자신의 클래스에 정의된 멤버의 이름이 같을 때는 super를 붙여서 구별할 수 있다.

* 조상 클래스로부터 상속받은 멤버도 자손 클래스 자신의 멤버이므로 this를 사용할 수 있다. 조상의 멤버와 자신의 멤버를 구별하는데 사용된다는 점을 제외하고 super와 this는 근본적으로 같다. 모든 인스턴스메서드에는 자신이 속한 인스턴스의 주소가 지역변수로 저장되는데, 이것이 참조변수인 this와 super의 값이 된다.

* static 메서드(클래스 메서드)는 인스턴스와 관련이 없기 때문에 this와 마찬가지로 super 역시 static 메서드에서는 사용할 수 없고 인스턴스 메서드에서만 사용할 수 있다.

* 조상 클래스에 선언된 멤버변수와 같은 이름의 멤버변수를 자손 클래스에서 중복해서 정의하는 것이 가능하며 참조변수 super를 이용해서 서로 구별할 수 있다.

```java
    class App {
        public static void main(String[] args) {
            Child c = new Child();
            c.method;
        }
    }

    class Parent {
        int x = 10;
    }

    class Child extends Parent {
        int x = 20;

        void method() {
            System.out.println("x = " + x); // 20
            System.out.println("this.x = " + this.x); // 20
            System.out.println("super.x = " + super.x); // 10
        }
    }
```
`this()`와 비슷하게, `super()`는 조상 클래스의 생성자를 호출하는데 사용된다.
* 자손 클래스의 인스턴스를 생성하면, 자손의 멤버와 조상의 멤버가 모두 합쳐진 하나의 인스턴스가 생성된다.
* 이 때 조상 클래스 멤버의 초기화 작업이 수행되어야 하기 때문에 자손 클래스의 생성자에서 조상 클래스의 생성자가 호출되어야 한다.
*  이러한 조상 클래스 생성자의 호출은 클래스의 상속관계를 거슬러 올라가서 모든 클래스의 최고 조상인 Object 클래스의 생성자인 Object()까지 가서 끝난다. 그래서 Object 클래스를 제외한 모든 클래스의 생성자는 첫 줄에 반드시 자신의 다른 생성자 또는 조상의 생성자를 호출해야 한다. 그렇지 않다면 컴파일러는 생성자의 첫 줄에 super();를 자동적으로 추가한다.
```java
    class App {
        public static void main(String[] args) {
            Point3D p3 = new Point3D();
            System.out.println("p3.x = " + p3.x);
            System.out.println("p3.y = " + p3.y);
            System.out.println("p3.z = " + p3.z);
        }
    }

    class Point {
        int x = 10;
        int y = 20;

        Point(int x, int y) {
            this.x = x;
            this.y = y;
        }
    }

    class Point3D extends Point {
        int z = 30;

        Point3D() {
            this(100, 200, 300);
        }

        Point3D(int x, int y, int z) {
            super(x, y);
            this.z = z;
        }
    }
```
<br><br><br>

## <strong>6.3 메소드 오버라이딩

### 오버라이딩(Overriding) vs 오버로딩(Overloading)

<br>

### 오버라이딩(Overriding)
조상클래스로부터 상속받은 메서드의 내용을 변경하는 것을 오버라이딩이라고 한다. 상속받은 메서드를 그대로 사용하기도 하지만, 자손 클래스 자신에 맞게 변경해야하는 경우가 많다. 이럴 때 조상의 메서드를 오버라이딩한다.
* 부모 클래스의 private 멤버를 제외한 모든 메소드를 상속받을 수 있고, 상속받은 메소드는 오버라이딩이 가능하다.
* 자손 클래스에서 오버라이딩하는 메서드는 조상 클래스의 메서드와
    * 이름이 같아야 한다.
    * 매개변수가 같아야 한다.
    * 반환타입이 같아야 한다.
* 접근 제어자는 조상 클래스의 메서드보다 좁은 범위로 변경할 수 없다.
  * 만약 조상 클래스에 정의된 메서드의 접근 제어자가 protected라면, 이를 오버라이딩하는 자손 클래스의 메서드는 접근 제어자가 protected나 public이어야 한다.
* 조상 클래스의 메서드보다 많은 수의 예외를 선언할 수 없다.
  * 아래의 코드는 자손 클래스의 메서드에 선언된 예외의 개수가 조상 클래스의 메소드에 선언된 예외의 개수보다 적으므로 바르게 오버라이딩 되었다.
```java
    class Parent {
        void parentMethod() throws IOException, SQLException {
            ...
        }
    }

    class Child extends Parent {
        void parentMethod() throws IOException {
            ...
        }
    }
```
* 인스턴스메서드를 클래스메서드로 변경할 수 없다. 그 반대도 불가.

<br>

### 오버로딩(Overloading)
한 클래스 내의 같은 이름을 가진 메서드이며, 매개변수의 개수 또는 타입이 다르다. (반환타입은 고려X)
> 즉, `오버라이딩`은 조상으로부터 상속받은 메서드의 내용을 변경하는 것, <br> `오버로딩`은 같은 이름의 새로운 메서드를 추가하는 것이다.

<br><br><br>

## <strong>6.1 다이나믹 메소드 디스패치 (Dynamic Method Dispatch)

### 다형성(polymorphism)
객체지향개념에서 다형성이란 여러가지 형태를 가질 수 있는 능력을 의미하며, 자바에서는 한 타입의 참조변수로 여러 타입의 객체를 참조할 수 있도록 함으로써 다형성을 프로그램적으로 구현하였다. 예시는 아래와 같다.

```java
    class Parent {
        String strP;
        int intP;

        void methodP() { }
    }

    class Child extends Parent {
        String strC;

        void methodC() { }        
    }

    /////////////////////////////
    Parent p = new Child();
/*
이와 같이, 조상클래스 타입의 참조변수로 자손클래스의 인스턴스를 참조할 수 있도록 한 것. 이때, Parent타입의 참조변수로는 Child인스턴스 중에서 Parent클래스의 멤버들(상속받은 멤버 포함)만 사용할 수 있다.
*/
```

### 메소드 디스패치
메소드 디스패치(method dispatch)는 어떤 메소드를 호출할지 결정하여 실행시키는 과정을 말한다. 이 과정은 static(정적)과 dynamic(동적)이 있다.

* Static Dispatch
  * 컴파일 시점에서, 컴파일러가 특정 메소드를 호출할 것이라고 명확하게 알고있는 경우이다.
  * 다형성이 사용되지 않았다.

```java
    class Dispatch {
        static class Service {
            void run() {
                System.out.println("run");
            }

            void run(String msg) {
                System.out.println(msg);
            }
        }

        public static void main(String[] args) {
            new Service().run();
        }
    }
```

* Dynamic Dispatch
  * 정적 디스패치와 반대로 컴파일러가 어떤 메소드를 호출하는지 모르는 경우이다. 동적 디스패치는 호출할 메서드를 런타임 시점에서 결정한다.
  * 다형성이 사용되었다.

```java
    class Dispatch {
        static abstract class Service {
            abstract void run();
        }

        static class MyService1 extends Service {
            @Override
            void run() {
                System.out.println("1");
            }
        }

        static class MyService2 extends Service {
            @Override
            void run() {
                System.out.println("2");
            }
        }

        public static void main(String[] args) {
            Service srv = new MyService1();
            srv.run(); // 1이 출력된다
        }
    }
```
<br><br><br>

## <strong>6.1 추상 클래스
>클래스가 설계도라면, 추상 클래스는 미완성 설계도라고 할 수 있다. 클래스가 미완성이라는 뜻은 멤버의 개수에 관계된 것이 아니라, 단지 미완성메서드(추상메서드)를 포함하고 있다는 의미이다.

현대자동차, 기아자동차, 르노자동차, 벤츠자동차, 아우디자동차 와 같은 자동차들이 있다고 생각해보자. 이러한 자동차들은 모두 시동을 걸고, 달리고, 정지하는 것과 같은 자동차가 가지는 기능과 특성들을 가지고 있을 것이다.

그렇다면 우리는 위에서 설명한 다형성을 생각해봤을 때, 이 모든 자동차들의 상위에 자동차라는 클래스를 만들어 모든 자동차를 자동차라는 타입으로 관리할 수 있을 거라는 생각이 든다.

그런데 이 때 자동차라는 클래스는 추상적인 개념을 표현하기 위한 클래스이다.

바로 이럴 때 사용하는 클래스가 추상 클래스(Abstraction Class)이다.
```java
abstract class 클래스이름 {  //이와 같이 class 앞에 abstract 키워드를 붙여서 선언한다.
    ...
} //상속을 통해 구현해야 한다는 것을 의미
```

```java
public abstract class Car {

    public abstract void start();

    public abstract void go();

    public abstract void stop();

}
public class HyndaiCar extends Car{

    public void start() {
        System.out.println("Hyndai car start");
    }

    public void go() {
        System.out.println("Hyndai car go");
    }

    public void stop() {
        System.out.println("Hyndai car stop");
    }
}
public class KiaCar extends Car{
    
    public void start() {
        System.out.println("KiaCar car start");
    }

    public void go() {
        System.out.println("KiaCar car go");
    }

    public void stop() {
        System.out.println("KiaCar car stop");
    }
}
public class Main {
    public static void main(String[] args) {

        Car hCar = new HyndaiCar();
        Car kCar = new KiaCar();

        hCar.start();   // -> Hyndai car start
        hCar.go();      // -> Hyndai car go
        hCar.stop();    // -> Hyndai car stop

        kCar.start();   // -> KiaCar car start
        kCar.go();      // -> KiaCar car go
        kCar.stop();    // -> KiaCar car stop

    }
}
```


<br><br><br>

## <strong>6.1 final 키워드
`접근 제어자`

* `public`: 모든 패키지에서 해당 클래스로 접근가능
* `protected`: 상속받은 클래스에서 접근가능(동일 클래스 + 동일 패키지 + 자손 클래스)
* `(default)`: 자신을 포함한 패키지에서만 해당 클래스로 접근 가능(동일 클래스 + 동일 패키지)
* `private`: 자신을 포함한 클래스에서만 접근 가능

`그 외`
* `static` - 변수, 메서드는 객체가 아닌 클래스에 속한다.
* `final`
  * 클래스 앞에 붙으면 해당 클래스는 상속될 수 없다.
  * 변수 또는 메서드 앞에 붙으면 수정되거나 오버라이딩 될 수 없다.
* `abstract`
  * 클래스 앞에 붙으면 추상 클래스가 되어 객체 생성이 불가하고, 접근을 위해선 상속받아야 한다.
  * 변수 앞에 지정할 수 없다. 메서드 앞에 붙는 경우는 오직 추상 클래스 내에서의 메서드밖에 없으며 해당 메서드는 선언부만 존재하고 구현부는 상속한 클래스 내 메서드에 의해 구현되어야 한다.
* `transient` - 변수 또는 메서드가 포함된 객체를 직렬화할 때 해당 내용은 무시된다.
* `synchronized` - 메서드는 한 번에 하나의 쓰레드에 의해서만 접근 가능하다.
* `volatile` - 해당 변수의 조작에 CPU 캐시가 쓰이지 않고 항상 메인 메모리로부터 읽힌다.

### __final 키워드__
대상 | 의미
:---:|:---:
클래스 | 변경될 수 없는 클래스, 확장될 수 없는 클래스가 된다. 그래서 final로 지정된 클래스는 다른 클래스의 조상이 될 수 없다.
메서드 | 변경될 수 없는 메서드, final로 지정된 메서드는 오버라이딩을 통해 재정의 될 수 없다.
멤버변수 및 지역변수 | 변수 앞에 final이 붙으면 값을 변경할 수 없는 상수가 된다.

### __제어자 조합의 주의사항__
1. 메서드에 static과 abstract를 함께 사용할 수 없다.
   * static은 몸통이 있는 메서드에만 사용 가능하다.
2. 클래스에 abstract와 final을 동시에 사용할 수 없다.
   * final은 클래스를 확장할 수 없다는 의미, abstract는 상속을 통해 완성되어야 한다는 의미
3. abstract메서드의 접근 제어자가 private일 수 없다.
   * private일 경우 자손 클래스에서 접근할 수 없으므로
4. 메서드에 private과 final을 같이 사용할 필요는 없다.
   * 비슷한 의미로 사용됨


<br><br><br>

## <strong>6.1 Object 클래스 - 모든 클래스의 조상
모든 클래스의 클래스 계층 최상위에 있는 조상클래스. 다른 클래스로 부터 상속받지 않는 클래스를 정의하면 코드를 컴파일 할 때 컴파일러에서 자동적으로 extends Object를 추가하여 상속받도록 한다. 

`toString()`, `equals()`와 같은 메서드를 따로 정의하지 않고 사용할 수 있었던 이유는 이 메서드들이 Object클래스에 정의된 것들이기 때문이다. Object클래스에는 모든 인스턴스가 가져야 할 기본적인 11개의 메서드가 정의되어 있으며 그 목록은 다음과 같다.

Object클래스의 메서드 | 설 명
:---:|:---:
protected Object clone(   )	| 객체 자신의 복사본을 반환한다.
public boolean equals(Object obj) | 객체 자신과 객체 obj가 같은 객체인지 알려준다.(같으면 true)
protected void finalize() | 객체가 소멸될 때 가비지 컬렉터에 의해 자동적으로 호출된다. 이 때 수행되어야하는 코드가 있을 때 오버라이딩한다. (거의 사용되지 않음)
public Class getClass() } | 객체 자신의 클래스 정보를 담고 있는 Class인스턴스를 반환한다.
public int hashCode() } | 객체 자신의 해시코드를 반환한다.
public String toString() | 객체 자신의 정보를 문자열로 반환한다.
public void notify() | 객체 자신을 사용하려고 기다리는 쓰레드를 하나만 깨운다.
public void notifyAll()} | 객체 자신을 사용하려고 기다리는 모든 쓰레드를 깨운다.
public void wait() / public void wait(long timeout) / public void wait(long timeout, int nanos) | 다른 쓰레드가 notify()나 notifyAll()을 호출할 때까지 현재 쓰레드를 무한히 또는 지정된 시간(timeout, nanos)동안 기다리게 한다. (timeout은 천 분의 1초, nanos는 109분의 1초)



<br><br><br>

## Reference

Book: Java의 정석 3rd Edition

https://yadon079.github.io/2020/java%20study%20halle/week-06

https://velog.io/@jaden_94/6%EC%A3%BC%EC%B0%A8-%ED%95%AD%ED%95%B4%EC%9D%BC%EC%A7%80