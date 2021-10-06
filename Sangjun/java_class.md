# Class

우선 class에 대해 말하기 전에, 다루지 않았던 함수에 대해 이야기하고자 한다.

### 함수 선언의 6요소

1. Access modifier
2. return type
3. method name
4. parameter
5. exception list
6. method body

사실 5번을 제외하고는 모두 당연하게 받아들이는 사실이다. 5번에 대해서는 밑에서 설명한다.



### 함수 Overriding

Overriding이란, 만약 내가 어떤 새로운 함수를 선언한다고 하자. 그런데 우연의 일치로 같은 scope에 선언된 어떤 함수와 이름이 같았다고 하자.

이러한 경우에 어떻게 될까?

1. parameter의 타입과 개수가 모두 같은 경우

   이러한 경우 Compiler가 두 개의 함수를 구분하지 못하기 때문에 에러가 발생한다.

2. 나머지 경우

   함수의 parameter의 개수가 같아도 type이 다르면 다른 함수로 취급한다. 또는, 개수가 다르면 당연히 다른 함수로 취급한다.

함수 Overriding이 편리한 점은 같은 기능을 하는 함수인데 여러가지 다른 타입의 변수에 작용을 해야할 때이다.

```
public class Data{
	public void simple_calculation(int a, int b){...}
	public void complex_calculation(float a, float b){...}
}
public class Data{
	public void calculation(int a, int b){...}
	public void calculation(float a, float b){...}
}
```

좋은 예시인지는 모르겠으나, 함수 명명을 쉽고 직관적으로 할 수있다.

\* 주의할 점: 함수의 return type이 다르다고 compiler는 다른 함수라고 인식하지 않는다. 따라서 return type에 따른 overriding은 없다.



### 함수의 return 값

<img src='https://docs.oracle.com/javase/tutorial/figures/java/classes-hierarchy.gif'>

위의 그림을 보자. 만약 함수가 

```
public Number returnNumber(){
	return x;
}
```

라고 한다면, x의 타입은 반드시 Number | ImaginaryNumber이어야 한다. 



### Class constructor

#### - 선언

class constructor는 필요성에 따라 argument가 없이 정의해도 되고, argument를 넣어서 class 객체 내의 값을 초기화하도록 정의해도 된다. 또한 그냥 constructor를 정의하지 않아도 된다.

constructor를 정의하지 않으면 compiler는 superclass(부모)로 가서 superclass의 no-argument constructor를 가져온다. 만약 superclass에도 없었다면, 그 superclass의 superclass로 간다. 그렇게 계속 부모 class를 방문했는데 없으면 최종적으로 Object 클래스에 도달하게 된다. Object 클래스는 root Class이고 미리 정의된 no-argument constructor를 가진다. 따라서 constructor가 없어서 일어나는 에러는 절대로 발생하지 않으나, 자신이 의도한대로 프로그램이 흘러가도록 하기 위해서는 가급적 constructor를 정의하는것이 건강에 좋다.

\* 'constructor가 없어서 일어나는 에러' 라는 말은 말 그대로 받아들이면 된다. 부모의 no-argument constructor에서 해당 class가 접근 불가한 변수를 초기화하는 등의 일을 하는데, 해당 class에 아무 constructor도 정의하지 않으면 당연히 에러가 발생한다. 이것과는 다르게 내가 의도한 것은 constructor가 없는 사실 자체로 일어나는 에러가 없다는 것이다. 



#### - 함수와 constructor에서의 variable arguments (varargs) 

```
public void toy(int ... items){...}
```

다음과 같이 argument를 적어줄 수 있다. 만약에 items에 넣을 argument 개수를 잘 모르곘을때 해주면 좋다. items는 함수의 body에서 배열로 취급된다. 또한, argument도 따로 여러 개 전달해 줄 수도 있지만 배열을 사용해 함수에 넘겨도 상관없다.

```
toy(item1, itme2, item3);
toy(items); // items is an array
```

개인적으로 좀 꿀인 것 같다.



#### - type of arguments

이 부분은 그냥 넘어간다. 모두들 잘 알고 있으리라 믿는다. 혹시 모르면 나한테 물어보도록.

근데 혹시몰라서 해두는 말은, primiitve나 reference 모두 call by value로 함수나 constructor에 전달된다는 점이다. reference type의 경우 주소가 call by value로 전달된다. 따라서 reference type의 객체를 참조하여 값을 바꾸면 함수가 종료되고도 그것이 반영이 되지만, 함수 내에서 새로운 객체를 만들어서 할당하면 효과가 없다.

```
public void toy(string s){
	s = new String("xlxlxlxl");
}
```

예를 들어 위의 함수를 실행한다고 s의 내용이 "xlxlxl"로 바뀌지 않고 전의 값 그대로를 유지한다는 뜻이다.



### 객체

사실 이제와서 객체에 대해서 말하라면, 말할 게 크게 없다. 그냥 클래스 생성자를 호출하면 객체가 만들어지기 때문에 그것을 class variable에 넣으면 된다. 생성자 앞에는 new가 반드시 와야한다. 객체 내의 멤버들은 .을 통해 접근 가능하다. 밑의 예시 코드를 보면 이해가 갈 것이다.

```
Toy a1 = new Toy(1,2);
String s = a1.name;
```

이렇게 객체 생성을 남발할 경우 메모리 관리는 어떻게 하냐? -> Java Garbage Collector가 대신 자동적으로 객체들을 관리해준다. 잘 안쓰는 것들은 메모리 해제를 시키고 나머지는 남기는 방식이다. 이것의 기준은 어떠한 객체가 아직 참조가 되고 있느냐이다. 이를 활용하여, 어떤 object reference variable에 null 값을 넣으면 해당 변수가 참조하고 있던 객체는 아무도 참조하고 있지 않은 상태가 된다. 그러면 garbage collector가 일을 수행할때 이것을 가져가서 메모리에서 해제시킨다.(해당 객체를 여러 변수가 참조하고 있는 상황이면 GC가 수행될 때 해제되지 않는다.)



#### - this

```
public class toy {
	public int x = 0;
	public int y = 0;
	
	public toy(int x, int y)
	{
		this.x = x;
		this.y =y;
	}
}  
```

위의 경우 toy의 멤버인 x,y가 constructor안에서는 constructor의 arguments에 의해 shadowed된다. 따라서 toy class의 멤버를 지정하기 위해 this 키워드를 사용한다.

this는 해당 객체에 대한 참조이다. 따라서 this를 객체 그 자체로써 생각해도 된다. 그러므로 객체내의 함수나, constructor도 this를 통해 접근 가능하다.

예를 들어서,

```
public class toy {
	private int x,y;
	
	public toy(){
		this(0,1);
	}
	public toy(int x, int y){
		this(0,0, 1 ,1);
	}
	public toy(int x, int y, int w, int z)
	{
		...
	}
}
```

과 같이 constructor를 정의할 수 있다. 각각 constructor안에서 다른 constructor를 호출하고 있다.



#### - Class access modifier

4 가지 경우가 존재한다.

1. public
2. private
3. protected
4. (Non-modifer) -> 한 마디로 modifier가 붙여지지 않았다는 말



시작하기 전에 '범위'에 대한 이해가 필요하다.

class의 범위를 나누자면,

1. 하나의 class 내
2. 하나의 package 내
3. class의 subclass들(자식들)

4. 전체(world)



class와 subclass간의 경계를 포함관계로 설명할 수는 없지만, class <= package <= world 라고 할 수 있다.

class의 집합체가 package이고 package의 집합체가 world라고 쉽게 생각하면 된다.



* public

어떠한 class/함수/변수를 public으로 선언한 경우 접근에 아무런 제약이 없다. 만약 내가 어떠한 class 안에 public 변수를 정의한다고 치자.

```
public class toy {
	public int a;
}
```

그렇다면 어떠한 toy 객체에서나 변수 a에 접근할 수 있다.

만약에 private int a; 였다고 가정하자. 이 경우에는 내가 toy 객체를 만들어서 a에 접근하려고 하면 에러가 발생한다. 



* private

private modifier가 앞에 붙었다면, 그 변수는 그것이 정의된 class 딱 그 안에서만 사용이 가능하다. 객체를 통한 접근을 포함한 모든 외부로부터의 접근이 불가능하다는 말이다. private 변수를 활용하고자 한다면, 그 class안의 함수나 nested class만이 사용할 수 있다.



* protected

public과 유사하지만, protected의 경우 다른 패키지에서 접근이 불가하다. 그 말은 즉슨, world 범위에서의 접근이 불가하다는 것이다.



* No modifier

protected와 유사하지만, 해당 class의 subclass들에서 접근이 불가하다.



다음 표를 참고하면 이해가 빠를 것이다.

![image-20211006183452415](/Users/june/Library/Application Support/typora-user-images/image-20211006183452415.png)

(출처: https://docs.oracle.com/javase/tutorial/java/javaOO/accesscontrol.html)



간단한 예시 하나를 들어보자.

<img src='https://docs.oracle.com/javase/tutorial/figures/java/classes-access.gif'>

다음과 같은 구조의 자바 프로젝트가 있다고 가정하자.

alpha라는 클래스에 선언되어 있는 어떤 변수가 있다고 가정하자.

그 변수가 public이면 alpha, beta, alphasub, gamma 모두에서 접근이 가능하다. (alpha 객체를 만들었다고 했을 때)

그 변수가 private이면 alpha에서만 접근이 가능하다.

그 변수가 protected이면 alpha, beta와 alphasub에서만 접근이 가능하다. alphasub는 alpha의 subclass이기 때문이다.

그 변수가 modifier를 가지지 않으면 alpha와 beta에서만 접근이 가능하다.



#### static keyword

만약에 class의 어떠한 객체에서도 접근해서 그 값이 같고, 다른 객체에서 값을 바꾸면 또다른 객체에서 그 바뀐 값을 보고 싶다면, static keyword를 사용하여 변수를 선언하자. static modifier가 붙은 변수는 객체가 생성될때 같이 생성되는 것이 아니라, class 자체에 딱 붙어있다고 보면 된다. 따라서 딱히 객체를 통하지 않고도 static 변수는 접근이 가능하다. 그래서 이들을 구분하기 위해 static 변수와 static이 아닌 변수를  각각 class 변수와 객체 변수라고 부르자.

static modifier는 함수에서도 사용이 가능하다.

Class 변수들은 compile-time에 할당되고, 객체 변수들은 run-time으로 dynamic하게 할당되기 때문에 제한사항이 발생한다.

만약 class 함수의 arguments에 객체 변수를 사용하고자 하면 compile-time에서 오류가 나게 된다. 이유는 아직 객체가 만들어지지 않았기 때문이다. 바로 위에 언급했던 변수들의 할당시기에 대해 알고 있으면 헷갈릴 일이 없으니 잘 알아 두자. 



#### class filed(member) initialization

```
public class toy {
	public int a = 10;
	public staic int b = 11;
}
```

사실 field값의 초기화는 위와 같이도 가능하다. 그런데, 초기화하는데 어떤 복잡한 logic이 있어야할 경우 다음과 같은 방법을 사용할 수 있다.

1. using block

   block이란 { } 를 뜻한다. 여기서 또 static 변수와 아닌 것의 사용법이 나뉜다.

   * static 변수

     static {	} 과 같이 block을 선언하여 사용한다. class body 어디서나 사용해도 되고, 명시해둔 순서대로 (위에서 아애로) 실행된다.

     그러니깐  stastic block을 3개 정도 class body에 만들어 뒀다면 위에 만들어둔 block부터 가장 밑의 block까지 순서대로 실행되어 초기화된다는 말이다.

   * 일반 변수

     { 	}과 같이 block을 선언하여 사용한다. 이 경우 자바 컴파일러가 모든 constructor에 이 block의 내용을 복사한다. 그래서 constructor들이 공유하는 내용을 block에 적어서 둔다면 효율적일 것이다.

     

2. using function

   여기서도 사용법이 약간 다르다.

   * static 변수

     static 함수에 선언하고자 하는 로직을 명시하고, 초기화하고자 하는 변수에 호출하여 returnr 값을 할당한다.

   * 일반 변수

     final modifier를 사용해 함수를 선언한다. 그 이후의 사용법은 위와 같다.



#### Inner class

함수나 변수와 같이 class도 class의 member가 될 수 있다. 

다만, static인 경우 compile-time때 값이 정해지므로 outer class의 일반 변수들은 활용하지 못한다. 



# 상속

## 정의

class를 정의하는데, 어떠한 class의 member들을 그대로 가져야 한다고 생각해 보자. 그러한 경우 상속을 하면 효율적 일 수 있다.

```
publi class toy extends item {

}
```

extends 와 함께 상속받고자 하는 class 이름을 적어주면 된다. 그러면 그 class의 public, protected 멤버들을 그대로 상속받게 된다. 만약에 상속받고자 하는 class와 지금 정의하는 class가 같은 package에 있으면, non-modifier 멤버들도 상속되게 된다.

subclass에서 superclass와 같은 이름의 함수를 적게 되면, override가 되고, static 함수를 적게되면 superclass의 static 함수 위에 덧씌워지게 된다.(overriding이 아니고)



## super

super 이라는 keyword는 superclass를 지칭하고 있으며, this와 비슷한 맥락으로 보면 된다.



## override and hiding methods

어떤 class를 상속을 하면, superclass의 함수들에 대해서 같은 이름, 같은 파라미터, 같은 리턴 type을 가지는 함수로 overriding을 할 수가 있다. 리턴 값의 subtype이여도 된다. 만약 override가 static methods에 대해서 일어났다면 그것을 hiding이라고 부른다. Overriding을 할때에는 @Override annotation을 사용하여 superclass에 있는 함수를 override할 의도였다는 것을 컴파일러에게 알려주어야 한다.



## polymorphism

이름만 먹고 겁먹지 말자. subclass의 객체는 superclass의 reference 변수에 할당할 수 있다. 만약 이렇게 된다면 superclass의 함수가 쓰이지 않고 subclass에서 override된 함수가 쓰이게 된다. 



## Object Class

Object class는 모든 class의 가장 윗 부분에 있는 superclass이다. 그러므로 Object가 가지고 있는 함수들은 모든 Class가 상속받게 된다 (public, protected modifier를 가진). 상황에 따라 이러한 함수들을 override하여 사용할 필요가 있다.

다음은 자주 사용되는 Object class 함수들 중의 일부분이다.

- `protected Object clone() throws CloneNotSupportedException`
     해당 객체를 복사하고, 반환한다.
- `public boolean equals(Object obj)`
     obj객체와 해당 객체가 같은지 판별한다.
- `protected void finalize() throws Throwable`
   garbage collector가 이 객체를 없애려 할때 호출되는 함수다.
- `public final Class getClass()`
   해당 객체의 Class 이름을 반환한다. 이건 Override가 불가하다.
- `public int hashCode()`
   해당 객체의 해쉬코드 값을 반환한다.
- `public String toString()`
  해당 객체의 string representation을 반환한다.



## Final keyword

final keyword가 앞에 붙은 함수는 overriding이 불가하다. 또한, class 앞에도 붙을 수 있는데 이러한 경우 class 전체가 overriding이 불가하다.