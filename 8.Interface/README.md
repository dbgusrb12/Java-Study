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

인터페이스 본문에는 

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
  해당 인터페이스와 관계 된 모든 인터페이스에 메서드를 구현해야한다.

```java
public interface InterfaceParent {
    void printParent();
}

public interface ChildInterface extends InterfaceParent {
    void printChild();
}
```


기존에 사용하던 인터페이스에 기능을 추가해야 되는 상황이 올 때,   
해당 인터페이스에 메서드를 추가하면 인터페이스를 구현하는 모든 클래스들에 추가 된   
메서드를 전부 재정의 해야된다.

> 웹문서
> - [The Java Tutorials(Interfaces)](https://docs.oracle.com/javase/tutorial/java/IandI/createinterface.html)
> - [자바 인터페이스(Java Interface)는 무엇인가?](https://interconnection.tistory.com/129)
> - [[JAVA] 인터페이스(Interface)](https://velog.io/@codemcd/%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4Interface)
> - [[JAVA] 자바 인터페이스란?(Interface)_이 글 하나로 박살내자](https://limkydev.tistory.com/197)