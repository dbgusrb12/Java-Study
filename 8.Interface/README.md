Interface
===

# 인터페이스(Interface) 란?

여러 객체들 사이에서 공통적인 부분을 모아서 추상화 한 것이다.

## 인터페이스의 특징

- 추상메서드로 이루어져 있다.
- 객체가 어떻게 구성되어야 하는지 정리한 설계도 역할을 한다.
- 객체의 다형성을 높혀준다.
- 개발 코드를 직접 수정하지 않고도, 사용하고 있는 객체만 변경할 수 있다.
- 인터페이스는 클래스와 다르게 다중 상속이 가능하다.
- 인터페이스를 구현하는 클래스는 반드시 인터페이스의 선언 된 모든 메서드를 구현 해야한다.


# 인터페이스 정의하는 방법

```java
접근제어자 interface 인터페이스명1 extends 인터페이스명2, 인터페이스명3 {
    
}
```

클래스와 선언 형태는 같으며, `class` 키워드 대신 `interface` 키워드가 들어간다.

.java 파일로 작성되며, 컴파일러(javac)를 통해 .class 파일로 컴파일 된다.

인터페이스는 다중 상속, 다중 구현이 가능하며, `,`를 구분자로 나열한다.

## 인터페이스의 구성

인터페이스는 상수, 추상메서드, 기본메서드, 정적메서드를 가질 수 있다.

Java 8 이전에는 상수, 추상메서드만 가질 수 있었지만,   
Java 8 이후부터는 기본메서드, 정적메서드를 가질 수 있게 되었다.

추상메서드는 구현부가 없고,   
기본메서드는 `default` 키워드를 통해 정의되고,   
정적메서드는 `static` 키워드를 통해 정의된다.

인터페이스의 모든 메서드들은 `public` 키워드를 암시적으로 가지고 있고,   
모든 인스턴스 변수들은 `public static final` 키워드를 암시적으로 가지고 있어,   
생략 가능하다.

### 추상메서드

추상메서드는 메서드 시그니쳐는 존재하지만, 구현부는 존재하지 않는 메서드를 말한다.   
인터페이스에서는 추상메서드들을 나열하고, 구현하는 구현체에서 이 추상메서드들을   
정의하여 사용한다.

추상메서드는 `abstract` 키워드를 사용해 나타내는데,   
인터페이스를 컴파일 하는 시점에 `public abstract` 키워드를 붙여주므로,   
이 키워드들을 생략 할 수 있다.

```java
public interface InterfaceTest {
    // public abstract 가 interface 안에 메서드들의 기본값이므로 생략이 가능하다.
    // public abstract void print(String message);
    void print(String message);
}
```

# 인터페이스 구현하는 방법

- 인터페이스를 구현 할 때는 `implements` 키워드를 사용한다.

- 인터페이스에서 인터페이스를 구현 할 수 없다.

[인터페이스의 구성](#인터페이스의-구성) 과 [클래스의 상속](https://github.com/dbgusrb12/Java-Study/tree/master/6.Inheritance) 을 생각해보면 이해가 편하다.

밑의 두 인터페이스를 예시로 들면,

```java
public interface Walkable {
    void walk();
}
public interface Flyable {
    void fly();
}
```

interface 는 `extends` 키워드를 사용해 ***상속*** 을 받고,
```java
public interface Movable extends Walkable, Flyable {
}
```

클래스는 `implements` 키워드를 사용하여 ***구현*** 을 한다.
```java
public class Human implements Walkable, Flyable {
    @Override
    public void walk() {
        System.out.println("뚜벅뚜벅");
    }

    @Override
    public void fly() {
        System.out.println("날지 못합니다..");
    }
}
```

# 인터페이스 레퍼런스를 통해 구현체를 사용하는 방법

인터페이스는 `new` 키워드를 사용해 객체를 생성 할 수 없다.   
하지만, 구현해놓은 구현체를 빌려 객체를 생성 할 수 있다.

인터페이스 레퍼런스를 통해 구현체를 사용 할 경우, 구현체의 다른 메서드들은 사용 할   
수 없고, 인터페이스로부터 재정의 한 메서드만 사용 할 수 있다.

```java
public interface Walkable {
    void walk();
}

public class Human implements Walkable{

    private String name;

    private int age;

    @Override
    public void walk() {
        System.out.println("뚜벅뚜벅");
    }

    public void fly() {
        System.out.println("날지 못합니다..");
    }
}

public class InterfaceTest {
    public static void main(String[] args) {
        Walkable walkable = new Human();

        // 인터페이스 레퍼런스로 구현체를 사용 할 경우, 구현체의 재정의 한 메서드를 사용 할 수 있다.
        walkable.walk();

        // Walkable 인터페이스로 선언된 객체는 Human 클래스의 다른 메서드를 사용하지 못한다. (컴파일 에러)
        // walkable.fly();
    }
}
```

# 인터페이스 상속

- 인터페이스에서 다른 인터페이스를 상속 받을 때는 `extends` 키워드를 사용한다.

- 클래스에선 `extends` 키워드로 인터페이스를 상속받을 수 없다. (무조건 `implements` 키워드로 구현만 가능)

- 클래스에서 인터페이스를 구현 할 때 해당 인터페이스에 상속받는 인터페이스들이 있다면,
  해당 인터페이스가 상속하는 모든 인터페이스의 메서드까지 구현해야한다.

```java
public interface InterfaceParent {
    void printParent();
}

public interface ChildInterface extends InterfaceParent {
    void printChild();
}

/**
 * 구현 할 인터페이스에 상속 관계가 있으면 그 관계에 해당하는 인터페이스까지 모두
 * 오버라이딩 해야한다.
 */
public class ImplementTest implements ChildInterface {
    @Override
    public void printParent() {
        System.out.println("부모 인터페이스 오버라이딩");
    }
    @Override
    public void printChild() {
        System.out.println("자식 인터페이스 오버라이딩");
    }
}
```


## 기존 인터페이스에 새로운 메서드를 추가하는 상황이 올때?

기존에 사용하던 인터페이스에 기능을 추가해야 되는 상황이 올 때,   
해당 인터페이스에 메서드를 추가하면 인터페이스를 구현하는 모든 클래스들에 추가 된   
메서드를 전부 재정의 해야된다.

이런 번거로운 과정을 줄이고, 이전 버전과의 호환성을 맞추기 위해   
사용하는 방법으로는 두가지가 있는데,

첫번째는 **인터페이스 상속**을 이용한 방법이고,   
두번째는 **default method** 를 이용한 방법이다.

Java 8 이전에는 인터페이스 상속을 이용해 해당 인터페이스를 상속받은   
인터페이스를 생성 후 생성한 인터페이스에 메서드를 추가하는 방식을 사용 했다.

그리고 Java 8 이후부터는 default method 를 이용해 인터페이스에 기능을   
추가하는 방식을 사용한다.

하지만 인터페이스 상속을 이용해 추가를 한다면, 구조적으로 복잡도가 올라갈 수 있어서   
이렇게 할 만한 가치가 있는지에 대해서는 개발중인 어플리케이션에 대해서 여러가지를   
고려해봐야 한다.

# Default Method (기본메서드)

Java 8 부터 등장한 인터페이스의 default method 는 [기존 버전의 호환성 문제](#기존-인터페이스에-새로운-메서드를-추가하는-상황이-올때) 때문에   
나오게 되었다.

일반적인 인터페이스의 메서드와는 다르게 메서드 시그니쳐만 존재하는게 아닌 구현부도 존재하는   
메서드이다.

default method 는 구현 하는 클래스에서 오버라이딩을 하지 않아도 되며,   
오버라이딩 하지 않으면 인터페이스의 메서드가 호출되고, 오버라이딩 하게 되면 해당 메서드가   
호출된다.

`default` 키워드로 표현할 수 있으며, `default` 키워드가 접근 제어자 default 를   
나타내는게 아니다. (인터페이스의 메서드에 기본 접근 제어자는 `public` 이다.)

```java
public interface Walkable {
    void walkSound();
    public default void walk() { // public 접근제어자는 생략 할 수 있다.
        System.out.println("걸을 수 있는 다리가 있다.");
    }
}


public class Human implements Walkable {

    @Override
    public void walkSound() {
        System.out.println("뚜벅뚜벅");
    }

    public static void main(String[] args) {
        Human human = new Human();
        human.walkSound();
        human.walk();       // default method 는 오버라이딩을 하지 않아도 사용 할 수 있다.
    }
}
```
```
뚜벅뚜벅
걸을 수 있는 다리가 있다.
```

# Static Method (정적메서드)

인터페이스의 정적 메서드 역시 Java 8 에서 등장한 기능이다.   
인터페이스에서 정적 메서드를 정의 할 수 있다.

정적 메서드는 오버라이딩이 불가능 하며, 인터페이스명.메서드명 으로 호출 할 수 있다.

```java
public interface StaticMethodTest {
    static void print() {
        System.out.println("정적메서드 호출");
    }
    default void defaultPrint() {
        System.out.println("기본메서드 호출");
    }
}

public class StaticMethodTestImpl implements StaticMethodTest {
    public static void main(String[] args) {
        StaticMethodTestImpl impl = new StaticMethodTestImpl();
        impl.defaultPrint();        // 인터페이스의 메서드는 기본적으로 호출 할 수 있지만,
        StaticMethodTest.print();   // 정적 메서드의 경우 인터페이스명.메서드명 으로 호출해야한다.
    }
}
```
```
기본메서드 호출
정적메서드 호출
```

# Private Method

Java 9 부터 등장한 private method 는 메서드의 접근 지시자가 `private` 인   
메서드를 말한다.

private method 는 상속이 불가능 하므로, 오버라이딩도 할 수 없고, 인터페이스 내부에서만   
사용 가능하다.

```java
public interface PrivateMethodTest {
  
    private void print() {
        System.out.println("private method 호출!");
    }
    
    default void printSample() {
        print();
    }
}

public class PrivateMethodTestImpl implements PrivateMethodTest {
    public static void main(String[] args) {
        PrivateMethodTestImpl impl = new PrivateMethodTestImpl();
        impl.printSample();
    }
}
```
```
private method 호출!
```

# 추가로!

## `Interface` 와 `Abstract Class` 의 차이?

### 추상 클래스

- 추상 메서드가 한개이상 존재하는 클래스
- `abstract` 키워드를 사용하여 표현하며, 추상 메서드는 자손에서 정의를 해야한다.
- 상속을 위한 클래스이므로 객체를 생성 할 수 없다.
- 자손 클래스에서 완성을 유도하기 때문에 **미완성 설계도** 라고도 표현한다.

### 인터페이스

- 추상 메서드의 집합으로 이루어져있다.
- 추상 클래스와는 다르게 다중 상속, 다중 구현이 가능하다.
- 추상 클래스가 미완성 설계도라면, 인터페이스는 **기본 설계도** 이다.

### 차이점

추상 클래스와 인터페이스의 가장 큰 차이점은 **사용 용도** 이다.

추상 클래스는 `is-A` 관계이고,   
인터페이스는 `has-A` 관계이다.

- `is-A` : ~ 는 ~이다.
- `has-A` : ~ 는 ~를 할 수 있다.

이런 관계를 생각 해 보면

`HyunGyu 는 개발자이고, 자바를 다룰 수 있다.` 

는
```java
class HyunGyu extends Developer implements Java
```
가 되는 것이다.


> 웹문서
> - [The Java Tutorials(Interfaces)](https://docs.oracle.com/javase/tutorial/java/IandI/createinterface.html)
> - [자바 인터페이스(Java Interface)는 무엇인가?](https://interconnection.tistory.com/129)
> - [[JAVA] 인터페이스(Interface)](https://velog.io/@codemcd/%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4Interface)
> - [[JAVA] 자바 인터페이스란?(Interface)_이 글 하나로 박살내자](https://limkydev.tistory.com/197)
> - [Private Methods in Interface – Java 9](https://howtodoinjava.com/java9/java9-private-interface-methods/)