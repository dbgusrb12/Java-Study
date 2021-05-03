Interface
===

# 인터페이스(Interface) 란?

여러 객체들 사이에서 공통적인 부분을 모아서 추상화 한 것이다.

## 인터페이스의 특징

- 추상메서드로 이루어져 있다.
- 객체가 어떻게 구성되어야 하는지 정리한 설계도 역할을 한다.
- 객체의 다형성을 높혀준다.
- 개발 코드를 직접 수정하지 않고도, 사용하고 있는 객체만 변경할 수 있다.
- 인터페이스는 클래스와 다르게 다중 상속이 가능하다. (인터페이스는 상속이 아니라 구현한다고 한다.)
- 인터페이스를 구현하는 클래스는 반드시 인터페이스의 추상 메서드를 구현 해야한다.


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

인터페이스의 구성은 Java 8 이전에는 상수, 추상메서드만 가질 수 있었지만,   
Java 8 이후부터는 기본 메서드, 정적 메서드를 가질 수 있게 되었다.

추상메서드는 구현부가 없고,   
기본 메서드는 `default` 키워드를 통해 정의되고,   
정적 메서드는 `static` 키워드를 통해 정의된다.

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

- 인터페이스에서 다른 인터페이스를 상속 받을 때는 `extends` 키워드를 사용한다.

- 클래스에서 인터페이스를 상속받을 수 없고, 인터페이스에서 인터페이스를 구현 할 수 없다.

인터페이스의 구성과 클래스의 상속을 생각해보면 이해가 편하다.

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


> 웹문서
> - [The Java Tutorials(Interfaces)](https://docs.oracle.com/javase/tutorial/java/IandI/createinterface.html)
> - [자바 인터페이스(Java Interface)는 무엇인가?](https://interconnection.tistory.com/129)
> - [[JAVA] 인터페이스(Interface)](https://velog.io/@codemcd/%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4Interface)
> - [[JAVA] 자바 인터페이스란?(Interface)_이 글 하나로 박살내자](https://limkydev.tistory.com/197)