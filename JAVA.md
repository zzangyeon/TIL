### java 언어의 장단점
- 장점
  - **운영체제에 독립적이다.**
    - JVM에서 동작하기 때문에, 특정 운영체제에 종속되지 않는다.
  - **객체지향 언어이다.**
    - 객체지향적으로 프로그래밍 하기 위해 여러 언어적 지원을 하고있다. (캡슐화, 상속, 추상화, 다형성 등)
    - 객체지향 패러다임의 특성상 비교적 이해하고 배우기 쉽다.
  - **자동으로 메모리 관리를 해준다.**
    - JVM에서 Garbage Collector라고 불리는 데몬 쓰레드에 의해 GC(Garbage Collection)가 일어난다. GC로 인해 별도의 메모리 관리가 필요 없으며 비지니스 로직에 집중할 수 있다. [(참고)](https://velog.io/@litien/%EA%B0%80%EB%B9%84%EC%A7%80-%EC%BB%AC%EB%A0%89%ED%84%B0GC)
  - **오픈소스이다.**
    - *정확히 말하면 OpenJDK가 오픈소스이다. OracleJDK는 사용 목적에 따라서 유료가 될 수 있다.*
      - OracleJDK의 유료화 이슈는 다음을 참고. [(참고)](https://okky.kr/article/490213)
    - 많은 Java 개발자가 존재하고 생태계가 잘 구축되어있다. 덕분에 오픈소스 라이브러리가 풍부하며 잘 활용한다면 짧은 개발 시간 내에 안정적인 애플리케이션을 쉽게 구현할 수 있다.
  - **멀티스레드를 쉽게 구현할 수 있다.**
    - 자바는 스레드 생성 및 제어와 관련된 라이브러리 API를 제공하고 있기 때문에 실행되는 운영체제에 상관없이 멀티 스레드를 쉽게 구현할 수 있다.
  - **동적 로딩(Dynamic Loading)을 지원한다**
    - 애플리케이션이 실행될 때 모든 객체가 생성되지 않고, 각 객체가 필요한 시점에 클래스를 동적 로딩해서 생성한다. 또한 유지보수 시 해당 클래스만 수정하면 되기 때문에 전체 애플리케이션을 다시 컴파일할 필요가 없다. 따라서 유지보수가 쉽고 빠르다.
- 단점
  - **비교적 속도가 느리다.**
    - 자바는 한 번의 컴파일링으로 실행 가능한 기계어가 만들어지지 않고 JVM에 의해 기계어로 번역되고 실행하는 과정을 거치기 때문에 C나 C++의 컴파일 단계에서 만들어지는 완전한 기계어보다는 속도가 느리다. 그러나 하드웨어의 성능 향상과 바이트 코드를 기계어로 변환해주는 JIT 컴파일러 같은 기술 적용으로 JVM의 기능이 향상되어 속도의 격차가 많이 줄어들었다.
  - **예외처리가 불편하다.**
    - 프로그래머 검사가 필요한 예외가 등장한다면 무조건 프로그래머가 선언을 해줘야 한다.


> - [http://yolojeb.tistory.com/17](http://yolojeb.tistory.com/17)
> - [http://huhghiza.tistory.com/7](http://huhghiza.tistory.com/7)


### 인터페이스와 추상 클래스의 차이
* 추상 메서드(Abstract Method)
  * abstract 키워드와 함께 원형만 선언되고, 코드는 작성되지 않은 메서드
~~~java
public abstract String getName(); // 추상 메서드
public abstract String fail() { return "Fail"; } // 추상 메서드 아님. 컴파일 오류 발생
~~~
* 추상 클래스(Abstract Class)
  * 개념: abstract 키워드로 선언된 클래스
    1. 추상 메서드를 최소 한 개 이상 가지고 abstract로 선언된 클래스
        * 최소 한 개의 추상 메서드를 포함하는 경우 **반드시** 추상 클래스로 선언하여야 한다.
    2. 추상 메서드가 없어도 abstract로 선언한 클래스
        * 그러나 추상 메서드가 하나도 없는 경우라도 추상 클래스로 선언할 수 있다.
  * 추상 클래스의 구현
    * 서브 클래스에서 슈퍼 클래스의 모든 추상 메서드를 오버라이딩하여 실행가능한 코드로 구현한다.
  * 추상 클래스의 목적
    * 객체(인스턴스)를 생성하기 위함이 아니며, 상속을 위한 부모 클래스로 활용하기 위한 것이다.
    * 여러 클래스들의 **공통된 부분을 추상화(추상 메서드)** 하여 상속받는 클래스에게 구현을 강제화하기 위한 것이다. (메서드의 동작을 구현하는 자식 클래스로 책임을 위임)
    * 즉, 추상 클래스의 추상 메서드를 자식 클래스가 구체화하여 그 기능을 확장하는 데 목적이 있다.
~~~java
/* 개념 a의 예시 */
abstract class Shape { // 추상 클래스
  Shape() {...}
  void edit() {...}
  abstract public void draw(); // 추상 메서드
}
~~~
~~~java
/* 개념 b의 예시 */
abstract class Shape { // 추상 클래스
  Shape() {...}
  void edit() {...}
}
~~~
~~~java
/* 추상 클래스의 구현 */
class Circle extends Shape {
  public void draw() { System.out.println("Circle"); } // 추상 메서드 (오버라이딩)
  void show() { System.out.println("동그라미 모양"); }
}
~~~
  <!-- * 추상 클래스의 특징
    1. 추상 클래스의 인스턴스(객체)를 생성할 수 없다.
    2. 상속받은 추상 클래스의 추상 메서드를 구현하지 않으면 자동으로 추상 클래스가 된다.
        * `interface MobilePhone extends Phone { }` -->
* 인터페이스(Interface)
  * 개념: 추상 메서드와 상수만을 포함하며, interface 키워드를 사용하여 선언한다.
  * 인터페이스의 구현
    * 인터페이스를 상속받고, 추상 메서드를 모두 구현한 클래스를 작성한다.
    * implements 키워드를 사용하여 구현한다.
  * 인터페이스의 목적
    * 상속받을 서브 클래스에게 구현할 메서드들의 원형을 모두 알려주어, 클래스가 자신의 목적에 맞게 메서드를 구현하도록 하는 것이다.
    * **구현 객체의 같은 동작을 보장하기 위한 목적이 있다.**
    * 즉, 서로 관련이 없는 클래스에서 공통적으로 사용하는 방식이 필요하지만 기능을 각각 구현할 필요가 있는 경우에 사용한다.
  * 인터페이스의 특징
    1. 인터페이스는 상수 필드와 추상 메서드만으로 구성된다.
    2. 모든 메서드는 추상 메서드로서, abstract public 속성이며 생략 가능하다.
    3. 상수는 public static final 속성이며, 생략하여 선언할 수 있다.
    4. 인터페이스를 상속받아 새로운 인터페이스를 만들 수 있다.
        * `interface MobilePhone extends Phone { }`
~~~java
/* 인터페이스의 개념 */
interface Phone { // 인터페이스
  int BUTTONS = 20; // 상수 필드 (public static final int BUTTONS = 20;과 동일)
  void sendCall(); // 추상 메서드 (abstract public void sendCall();과 동일)
  abstract public void receiveCall(); // 추상 메서드
}
~~~
~~~java
/* 인터페이스의 구현 */
class FeaturePhone implements Phone {
  // Phone의 모든 추상 메서드를 구현한다.
  public void sendCall() {...}
  public void receiveCall() {...}

  // 추가적으로 다른 메서드를 작성할 수 있다.
  public int getButtons() {...}
}
~~~
* 추상 클래스와 인터페이스의 ***공통점***
  * 인스턴스(객체)는 생성할 수 없다.
  * 선언만 있고 구현 내용이 없다.
  * 자식 클래스가 메서드의 구체적인 동작을 구현하도록 책임을 위임한다.
* 추상 클래스와 인터페이스의 ***차이점***
  * 서로 다른 목적을 가지고 있다.
    * 추상 클래스는 추상 메서드를 자식 클래스가 구체화하여 그 기능을 확장하는 데 목적이 있다. (상속을 위한 부모 클래스)
    * 인터페이스는 서로 관련이 없는 클래스에서 공통적으로 사용하는 방식이 필요하지만 기능을 각각 구현할 필요가 있는 경우에 사용한다. (구현 객체의 같은 동작을 보장)
  * 추상 클래스는 클래스이지만 인터페이스는 클래스가 아니다.
  * 추상 클래스는 단일 상속이지만 인터페이스는 다중 상속이 가능하다.
  * 추상 클래스는 “is a kind of” 인터페이스는 “can do this”
    * Ex) 추상 클래스: Appliances(Abstract Class) - TV, Refrigerator
    * Ex) 인터페이스: Flyable(Interface) - Plane, Bird
(https://github.com/WeareSoft/tech-interview#tech-interview)
> - [http://loustler.io/languages/oop_interface_and_abstract_class/](http://loustler.io/languages/oop_interface_and_abstract_class/)
> - [http://alecture.blogspot.com/2011/05/abstract-class-interface.html](http://alecture.blogspot.com/2011/05/abstract-class-interface.html)

