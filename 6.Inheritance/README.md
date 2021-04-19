Inheritance
===

# 상속(Inheritance) 이란?

Oracle 공식 가이드에 따르면,   
**Java 에서 클래스는 다른 클래스에서 파생될 수 있으므로 해당 클래스에서 필드와 메서드를 상속한다.**   
라고 나와있다.   

상속(Inheritance) 이란 기존에 존재하던 클래스를 기반으로 새로운 클래스를 구현하는 것이다.

# 상위 클래스 (superclass)

상속을 할 때 기반이 되는 클래스이며,   
기본 클래스 (basic class), 부모 클래스 (parent class) 라고도 한다.

# 하위 클래스 (subclass)

상속을 할 때 파생된 클래스를 말하며,   
파생 클래스 (derived class), 확장 클래스 (extended class), 자식 클래스 (child class) 라고도 한다.

# 상속의 특징

- `extends` 키워드를 통해 이루어진다.
  
  ```java
  class 자식클래스 extends 부모클래스 {}
  ```
  
- 한번에 여러 클래스를 상속 받을 수 없다. (single inheritance)
  
  ```java
  class 자식클래스 extends 부모클래스A, 부모클래스B {} // 불가능
  ```
  
- 부모 클래스가 명시적으로 작성되어 있지 않은 모든 클래스는 암묵적으로 `Object` 클래스를 상속받는다.   
  (모든 클래스는 `Object` 클래스의 서브 클래스이다.)
  
  ```java
  // 명시적인 상속 관계가 없는 클래스는 Object 클래스를 상속 받는다.
  class 클래스이름 /* extends Object */ {} // 생략 된 코드 
  ```

- 상속 받은 클래스를 상속 받아 클래스를 구현 할 수 있다. (Multi-level inheritance)

  ```java
  // 상속의 횟수는 제한이 없다.
  class A {}
  class B extends A {}
  class C extends B {}
  ```

- 자식 클래스는 부모 클래스의 private 멤버를 제외한 모든 멤버를 상속 받는다.   
  (생성자는 멤버가 아니므로 상속받지 않는다.)

# `super` 키워드

`this` 키워드와 비슷한 기능으로, 부모의 멤버나 생성자에 접근 할 수 있는 키워드이다.   

## `super()`

부모 클래스의 생성자를 호출하는 메서드이다.

자식 클래스는 생성 될 때 부모 클래스의 기본 생성자를 호출하도록 되어있기 때문에,   
기본적으로 명시되어 있는 게 없으면 컴파일러가 자동으로 super()를 자식 클래스 생성자의 첫줄에 호출한다.

## `super.변수명`

클래스 내에 멤버 변수와 지역 변수의 이름이 같을 때 `this` 키워드를 사용하여 구분 하는 것처럼,   
부모 클래스의 멤버 변수를 참조 할 때 쓰인다.

## `super.메서드명`

자식 클래스에서 부모 클래스의 메서드에 접근 할 때 쓰이는 방법이다.   

```java
class Parent {
    int age;
    private String name;
    
    public Parent() {
        this.age = 20;
        this.name = "HyunGyu";
    }
    
    public void parentMethod() {
        System.out.println(this.name);  // HyunGyu
    }
}

class Child extends Parent {
    int age;
    
    public Child() {
        // super() 를 명시적으로 작성하지 않아도 컴파일 시점에 super() 를 호출한다.
        this.age = 10;
    }
    
    public void childMethod(int age) {
        System.out.println("age is " + age);                // age is 0
        System.out.println("this.age is " + this.age);      // this.age is 10
        System.out.println("super.age is " + super.age);    // super.age is 20

        super.parentMethod();   // 부모 클래스의 메서드를 호출 할 수 있다.
    }
}

class SuperKeywordTest {
    public static void main(String[] args) {
        Child child = new Child();
        child.childMethod(0);
    }
}
```

```
age is 0
this.age is 10
super.age is 20
HyunGyu
```


# 오버라이딩 (Overriding)

**오버라이딩 (Overriding)** 이란 상속 관계에 있는 부모 클래스의 정의 되어 있는 메서드를   
자식 클래스에서 같은 메서드 시그니쳐 (Method signature)를 갖는 메서드로 재정의 하여 사용 하는 것을 말한다.

## 오버라이딩의 조건

- 오버라이딩 할 메서드가 부모 클래스에 존재해야 한다.
- 메서드 이름, 파라미터, 리턴 타입이 모두 같아야 한다.
- 접근 제어자는 부모 클래스의 메서드 보다 넓거나, 같아야 한다.
- 부모 클래스의 메서드보다 더 큰 범위의 예외를 선언 할 수 없다.
- @Override 어노테이션을 넣으면 부모 클래스의 메서드가 없을 때 컴파일 에러가 난다.   
  (오타를 방지 할 수 있다.)

## 오버라이딩을 사용하는 이유

하위 클래스가 상위 클래스로부터 물려받은 메서드를 그대로 사용해야 한다는 제약을 벗어나   
상위 클래스의 기본적인 동작 방법을 재정의 하여 사용 할 수 있다.

```java
public class OverridingTest {
    public static void main(String[] args) {
        Parent obj = new Parent();
        obj.print();
        Child obj2 = new Child();
        obj2.print();
    }
}

class Parent {
    
    public void print() {
        System.out.println("부모 클래스의 메서드 입니다.");
    }
    
}

class Child extends Parent {

    @Override
    public void print() {
        System.out.println("자식 클래스의 메서드 입니다.");
    }
    
}
```
```
부모 클래스의 메서드 입니다.
자식 클래스의 메서드 입니다.
```

# 다형성 (Polymorphism)

Java 에서의 다형성은 동일한 네이밍을 가지고 여러가지 액션을 취할 수 있는 테크닉을 말한다.   
부모 클래스의 타입으로 자식 클래스의 인스턴스를 참조 할 수 있고,   
앞에서 배운 오버라이딩, 오버로딩 등이나, 뒤에서 배울 메서드 디스패치 같은 것들 역시   
다형성의 방법이다.

# 메서드 디스패치 (Method Dispatch)

프로그램이 어떤 메서드를 호출 할 것인가를 결정하여 그것을 실행 시키는 과정으로,   

- 정적 메서드 디스패치(Static Method Dispatch)
- 동적 메서드 디스패치(Dynamic Method Dispatch)
- 더블 디스패치(Double Dispatch)

가 존재한다.

## 정적 메서드 디스패치 (Static Method Dispatch)

컴파일 시점에 컴파일러가 특정 메서드를 호출 할 것을 명확하게 알고 있는 경우이다.

정적 메서드 디스패치 같은 경우 컴파일 시 생성된 바이트코드에도 이 정보가 남아있으며,   
오버로딩이나, 자식 클래스의 타입으로 생성된 자식 인스턴스 같은 경우   
정적 디스패치 (Static Dispatch) 라고 볼 수 있다.

```java
public class StaticDispatchTest {
    public static void main(String[] args) {
        Parent obj = new Parent();
        obj.print();        // 컴파일 시점에 print() 메서드는 Parent 클래스의 메서드를 가리키고있다.
        Child obj2 = new Child();
        obj2.print();       // 컴파일 시점에 print() 메서드는 Child 클래스의 메서드를 가리키고있다.
        // (IntelliJ 기준) print() 에 마우스를 올렸을 때 각각 클래스의 메서드를 참조하고 있는게 보인다.
    }
}

class Parent {

    public void print() {
        System.out.println("부모 클래스의 메서드 입니다.");
    }

}

class Child extends Parent {

    @Override
    public void print() {
        System.out.println("자식 클래스의 메서드 입니다.");
    }

}
```

## 동적 메서드 디스패치 (Dynamic Method Dispatch)

컴파일 시점에 어떤 메서드를 호출 할 지 알 수 없고,   
런타임 시점에 특정 메서드를 호출하는 경우이다.   
런타임 시점에 도달해야 실행 할 메서드를 알 수 있기 때문에 바이트 코드에 남지 않으며,   
할당된 인스턴스의 타입을 보고 메서드를 실행 한다.

부모의 타입으로 생성된 자식 인스턴스의 경우   
동적 디스패치 (Dynamic Dispatch) 라고 볼 수 있다.

```java
public class DynamicDispatchTest {
    public static void main(String[] args) {
        Parent obj = new Child();
        obj.print();        // 컴파일 시점에 print() 메서드는 Parent 클래스의 메서드를 가리키고 있다.
        // (IntelliJ 기준) 마우스를 올렸을 때 Parent 클래스의 메서드를 참조하고 있는게 보인다.
    }
}
class Parent {
    public void print() {
        System.out.println("부모 클래스의 메서드 입니다.");
    }
}

class Child extends Parent {
    @Override
    public void print() {
        System.out.println("자식 클래스의 메서드 입니다.");
    }
}
```

## 더블 디스패치 (Double Dispatch)

더블 디스패치(Double Dispatch) 는 동적 디스패치 (Dynamic Dispatch) 를   
두번 시도하는 경우이다.

밑의 예시는 [토비님의 유튜브 강의](https://www.youtube.com/watch?v=s-tXAHub6vg&feature=youtu.be) 에서 참고한 예제코드이다.
```java
public class DoubleDispatchTest {
    
    
    interface Language {
        void useOn(Idea idea);
    }
  
    static class Java implements Language {
        @Override
        public void useOn(Idea idea) {
            idea.use(this);
        }
    }
  
    static class Kotlin implements Language {
        @Override
        public void useOn(Idea idea) {
            idea.use(this);
        }
    }
  
    interface Idea {
        void use(Java java);
    
        void use(Kotlin kotlin);
    }
  
    static class IntelliJ implements Idea {
        @Override
        public void use(Java java) {
            System.out.println("use Java in IntelliJ");
        }
    
        @Override
        public void use(Kotlin kotlin) {
            System.out.println("use Kotlin in IntelliJ");
        }
    }
  
    static class Eclipse implements Idea {
        @Override
        public void use(Java java) {
            System.out.println("use Java in Eclipse");
        }
  
        @Override
        public void use(Kotlin kotlin) {
            System.out.println("use Kotlin in Eclipse");
        }
    }
    public static void main(String[] args) {
        List<Language> languageList = Arrays.asList(new Java(), new Kotlin());
        List<Idea> ideaList = Arrays.asList(new IntelliJ(), new Eclipse());
    
        languageList.forEach(language -> ideaList.forEach(idea -> language.useOn(idea)));
    }
}
```

```
use Java in IntelliJ
use Java in Eclipse
use Kotlin in IntelliJ
use Kotlin in Eclipse
```

1. Language 인터페이스의 구현체중 어떤 클래스의 useOn 메소드를 사용할지 (Dynamic Dispatch 한번 사용)

2. useOn에 인자로 선택된 Idea 인터페이스에서(Idea 로 구현된 클래스) 어떤 use 메소드를 사용할지 (Dynamic Dispatch 한번 사용)

> 웹문서
> - [The Java Tutorials(Inheritance)](https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html)
> - [메소드 오버라이딩](http://www.tcpschool.com/java/java_inheritance_overriding)
> - [다형성의 개념](http://tcpschool.com/java/java_polymorphism_concept)
> - [다이나믹 메소드 디스패치](https://velog.io/@maigumi/Dynamic-Method-Dispatch)
> - [[JAVA]Static Method Dispatch, Dynamic Method Dispatch, Double Dispatch](https://defacto-standard.tistory.com/413_)