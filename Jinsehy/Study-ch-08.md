# <strong>8. 인터페이스
### __GOAL__: 자바의 인터페이스에 대해 학습하세요.
---

## <strong>8.1 인터페이스의 정의
> 인터페이스(interface)는 서로 다른 두 개의 시스템, 장치 사이에서 정보나 신호를 주고받는 경우의 접점이나 경계면이다. - wiki

인터페이스는 일종의 추상클래스(abstract class)이다. 인터페이스는 추상클래스처럼 추상메서드를 가지지만 추상클래스보다 추상화의 정도가 높아서 몸통을 갖춘 일반 메서드 또는 멤버변수를 구성원으로 가질 수 없다. 오직 추상메서드와 상수만을 멤버로 가질 수 있으며, 그외의 다른 어떠한 요소도 허용하지 않는다. (JDK 1.8 이전)

추상클래스가 미완성 설계도라면, 인터페이스는 밑그림만 그려져 있는 기본 설계도라 할 수 있다.

### 사용하는 이유
설계시 선언해두면, 개발할 때 기능을 구현하는 데에만 집중할 수 있다. 인터페이스는 하위 클래스에 특정 메소드가 반드시 존재하게 강제한다.

### interface vs abstract
1. 사용 의도
   * 추상클래스는 IS - A "~이다".
   * 인터페이스는 HAS - A "~을 할 수 있는".

2. 공통된 기능의 사용 여부
   * 만약 모든 클래스가 인터페이스를 사용해서 기본 틀을 구성한다면 공통으로 필요한 기능들도 모든 클래스에서 오버라이딩 하여 재정의 해야하는 번거로움이 있습니다.
   * 공통된 기능이 필요하다면 추상클래스를 이용해서 일반 메서드를 작성하여 자식 클래스에서 사용할 수 있도록 하면 된다.
   * 만약 각각 다른 추상클래스를 상속하는데 공통된 기능이 필요하다면 해당 기능을 인터페이스로 작성해서 구현한다. (상속을 할 때 하나의 부모만 가질 수 있다)
  
![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbstzKQ%2FbtqBSPHUrzK%2FKpjOscUdnrMyOyJrWilSH1%2Fimg.png)

3. 다중 상속의 차이 (다이아몬드 문제)
   * abtract와 클래스는 다중 상속이 불가능
     * Compile error
   * interface는 다중 상속이 가능
     * 어차피 구현체가 없기에, 상속받은 클래스에서 구현이 이루어지므로 메소드 이름이 중복되어도 상관이 없다.
  
![img](https://user-images.githubusercontent.com/33862991/112723009-d7c2fa80-8f4f-11eb-998b-da43ed8c0427.png)

<br><br><br>

## <strong>8.2 인터페이스의 작성

```java
interface 인터페이스이름 {
    public static final 타입 상수이름 = 값;
    public abstract 메서드이름(매개변수목록);
}
```
* 작성 방법은 클래스를 작성하는 것도 동일하지만 `class` 키워드 대신 `interface` 키워드를 사용한다.
* 접근제어자로 `public` 또는 default를 사용할 수 있다. (public interface)
* protected 및 private 접근제어자는 클래스에 직접 둘러싸인 멤버 인터페이스에만 선언할 수 있다.
  
인터페이스의 멤버들은 다음의 제약을 가진다.
* 모든 멤버변수는 public static final 이어야 하며, 이를 생략할 수 있다.
* 모든 메서드는 public abstract 이어야 하며, 이를 생략할 수 있다. 단, static 메서드와 default 메서드는 예외(JDK 1.8부터)
> 자바8에서부터 Default Method(디펄트 메소드)가 추가되었다. 메소드 선언 시에 default를 명시하게 되면 인터페이스 내부에서도 로직이 포함된 메소드를 선언할 수 있습니다. 자세한 내용은 아래에서 추가로 언급

<br><br><br>

## <strong>8.3 인터페이스의 구현
* implements 라는 예약어를 쓴 후에 인터페이스를 나열하며 된다.

* 클래스는 상속과 달리 여러개의 인터페이스를 implements 할 수 있다. (상속과 구현도 동시에 가능)
```java
class Fighter extends Unit implements Figtable { 
    ... 
}
```

* 인터페이스가 가지고 있는 메소드를 하나라도 구현하지 않으면, 해당 클래스는 추상 클래스가 된다. ( 추상 클래스는 인스턴스를 만들 수 없다. 즉, 메모리에 할당되어 실제 사용될 수 없다.)

* 참조변수의 타입으로 인터페이스를 사용할 수 있다. 이 경우 인터페이스가 가지고 있는 메소드만 사용할 수 있다.

* 만약 TV인터페이스를 구현하는 LcdTV를 만들었다면 위의 코드에서 new LedTV부분만 new LcdTV로 변경해도 똑같이 프로그램이 동작할 것다. 동일한 인터페이스를 구현한다는 것은 클래스 사용법이 같다.

```java
// 인터페이스 선언부
package me.whiteship.tv;

public interface TV {
  public int MAX_VOLUME = 10;
  public int MIN_VOLUME = 10;

  public void turnOn();

  public void turnOff();

  public void changeVolume(int volume);

  public void changeChannel(int channel);
}

// 인터페이스 구현부
public class UHDTV implements TV {

  @Override
  public void turnOn() {
    System.out.println("tv를 켭니다.");
  }

  @Override
  public void turnOff() {
    System.out.println("tv를 끕니다.");
  }

  @Override
  public void changeVolume(int volume) {
    System.out.println(volume + "로 볼륨을 조정합니다.");
  }

  @Override
  public void changeChannel(int channel) {
    System.out.println(channel + "로 채널을 조정합니다.");
  }
}
```

<br><br><br>

## <strong>8.4 인터페이스 레퍼런스를 통해 구현체를 사용하는 방법
> 구현체란 인터페이스를 구현한 클래스라는 뜻이며, 구현 클래스 혹은 실체 클래스 라고도 부른다.

> 다형성에 의해 자손클래스의 인스턴스를 조상타입의 참조변수로 참조하는 것이 가능하다.

인터페이스 역시 해당 인터페이스의 타입의 참조변수로 이를 구현한 클래스의 인스턴스를 참조할 수 있으며, 인터페이스 타입으로의 형변환도 가능하다.

이펙티브 자바에서는 매개변수의 타입으로 클래스가 아닌 인터페이스를 사용하면 프로그램이 유용하게 짜여진다고 설명한다.

적합한 인터페이스만 있다면 매개변수뿐 아니라 반환값, 변수, 필드를 전부 인터페이스 타입으로 선언해야 한다.

다음과 같은 예가 있다 하자.
```java
//좋은 예. 인터페이스를 타입으로 사용했다.
Set<Son> sonSet = new LinkedHashSet<>();

//나쁜 예. 클래스를 타입으로 사용했다.
LinkedHashSet<Son> sonSet = new LinkedHashSet<>();
```
나중에, 구현 클래스를 교체하고자 한다면, 위의 좋은 예의 경우, 그저 새 클래스의 생성자를 호출해주기만 하면 된다. 주변 코드도 옛클래스의 존재를 몰랐기 때문에, 영향을 받는 코드가 없다.
```java
Set<Son> sonSet = new TreeSet<>();
```
![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FqJFq1%2FbtqSpG9K3iH%2Ff5KUNOkYmwegd42Twz2VAk%2Fimg.png)
<br><br><br>

## <strong>8.5 인터페이스 상속
* 인터페이스는 인터페이스로부터만 상속을 받을 수 있다.
  * 추상클래스는 몸통을 가진 메서드를 가질 수 있으므로 인터페이스가 상속받을 수 없다.

* 클래스의 상속과 마찬가지로 자손 인터페이스는 조상 인터페이스에 정의된 멤버를 모두 상속받는다.
  
* `extends` 라는 예약어를 쓴 후에 인터페이스를 쓰면 된다.

* 클래스는 상속과 달리 여러개의 인터페이스를 상속할 수 있다. ( 다중 상속 지원 )

* 인터페이스는 클래스와 달리 Object클래스와 같은 최고 조상이 없다.
```java
package me.whiteship.interfaceEx;

interface A {
  int ex = 10;
}

interface B extends A {
  int ex = 20;
}

class C implements B {
  int ex = 30;
}
```


<br><br><br>

## <strong>8.6 인터페이스의 기본 메소드 (Default Method), 자바 8
디폴트 메서드는 추상 메서드의 기본적인 구현을 제공하는 메서드이다.  
자바 8이 등장하면서 새로 생긴 interface 정의이다. 이 기능은 이전 인터페이스를 사용하여 Java 8의 람다 표현식 기능을 활용할 수 있도록 이전 버전과의 호환성을 위해 추가되었다.



* 인터페이스가 default키워드로 선언되면 메소드가 구현될 수 있다. 또한 이를 구현하는 클래스는 default메소드를 오버라이딩 할 수 있다.

* 인터페이스를 구현한 이후, 수정과정에서 인터페이스 모든 구현체에게 광역으로 함수를 만들어주고 싶을 때 사용가능하다.( 단, 모든 구현체가 원하는 값을 return 하게 보장하기 위해 @implSpec 자바 독 태그를 사용해 문서화 해줘야한다.)

* 구현체에서 오버라이딩, 재정의 가능하다.

* 제약 조건: Object 가 제공하는 기능(equals, hasCode)는 기본 메소드로 제공할 수 없다. ( 구현체가 재정의 해야함)
  
```java
interface MyInterface {
    void method();
    // void newMethod();  추상 메서드
    default void newMethod() { }
}
```
### 디폴트 메서드 충돌 규칙 (다이아몬드 문제)
새로 추가된 디폴트 메서드가 기존의 메서드와 이름이 중복되어 충돌하는 경우 해결하는 규칙은 다음과 같다.

* 여러 인터페이스의 디폴트 메서드 간의 충돌
    * 인터페이스를 구현한 클래스에서 디폴트 메서드를 오버라이딩해야 한다.
* 디폴트 메서드와 조상 클래스의 메서드 간의 충돌
    * 조상 클래스의 메서드가 상속되고, 디폴트 메서드는 무시된다.
  
단순하게 필요한 쪽의 메서드와 같은 내용으로 오버라이딩 해서 해결하는 방법도 있다.


<br><br><br>

## <strong>8.7 인터페이스의 static 메소드, 자바 8
Java 8부터 인터페이스에 static 메서드 추가가 가능해졌다. 클래스에서 작성하는 방법과 동일하게 작성할 수 있고, 접근 제어자는 항상 public이며 역시 생략이 가능하다.

해당 타입 관련 헬퍼 또는 유틸리티 메소드를 제공할 때, 인터페이스에 스태틱 메소드를 사용한다.

static 메서드는 오버라이딩이 불가능하다.

```java
public interface Calculator {
    public int plus(int i, int j);
    public int multiple(int i, int j);
    default int exec(int i, int j){
        return i + j;
    }
    public static void explain(){   //static 메소드 
        System.out.println("interface Caculater. 우리는 plus, multiple, exec 함수를 제공함.");   
        }
```
<br><br><br>

## <strong>8.8 인터페이스의 private 메소드, 자바 9
Java 9부터 사용할 수 있게된 private 메서드는 인터페이스에 default, static 메소드가 생긴 이후, 이러한 메소드들의 로직을 공통화하고, 재사용하기 위해 생긴 메소드이다. 

* 메서드의 몸통 { }이 있고 abstract이 아니다.
  * Static, default Method 같이 구현부를 가져야함
* 구현체에서 구현할 수 없고 자식 인터페이스에서 상속이 불가능하다.
* static 메서드도 private이 가능하다.
* private 메서드는 private, abstract, default 또는 static 메서드를 호출할 수 있다. 
  * private static은 static 및 static private 메서드만 호출 할 수 있다.

```java
default void multiplyAfterAddingNumbers(long num1, long num2) { 
    long val = num1 + num2; 
    val = val * val; 
    System.out.println("result = " + val); 
} 
default void multiplyAfterSubtractingNumbers(long num1, long num2) { 
    long val = num1 - num2; 
    val = val * val; 
    System.out.println("result = " + val); 
}

// private 메서드를 사용하면 아래와 같이 코드의 중복을 줄일 수 있다.

default void multiplyAfterAddingNumbers(long num1, long num2) { 
    long val = multiplyNumbers(num1 + num2); 
    System.out.println("result = " + val); 
} 
default void multiplyAfterSubtractingNumbers(long num1, long num2) { 
    long val = multiplyNumbers(num1 - num2); 
    System.out.println("result = " + val); 
} 

private long multiplyNumbers(long val) { 
    return val * val; 
}
```


<br><br><br>

## Reference

Book: Java의 정석 3rd Edition

https://kils-log-of-develop.tistory.com/437?category=923003

https://yadon079.github.io/2020/java%20study%20halle/week-07

https://myjamong.tistory.com/150

https://siyoon210.tistory.com/95

https://youngjinmo.github.io/2021/03/diamond-problem/