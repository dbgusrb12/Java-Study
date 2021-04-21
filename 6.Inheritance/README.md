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

### `extends` 키워드를 통해 이루어진다.

```java
class 자식클래스 extends 부모클래스 {}
```
  
## Single Inheritance

한번에 여러 클래스를 상속 받을 수 없다.

```java
class 자식클래스 extends 부모클래스A, 부모클래스B {} // 불가능
```
  
## 모든 클래스는 `Object` 클래스의 서브 클래스이다.

부모 클래스가 명시적으로 작성되어 있지 않은 모든 클래스는 암묵적으로 `Object` 클래스를 상속받는다.
  
```java
// 명시적인 상속 관계가 없는 클래스는 Object 클래스를 상속 받는다.
class 클래스이름 /* extends Object */ {} // 생략 된 코드 
```

## Multi-level inheritance

상속 받은 클래스를 상속 받아 클래스를 구현 할 수 있다. (상속의 상속의 상속...)

```java
// 상속의 횟수는 제한이 없다.
class A {}
class B extends A {}
class C extends B {}
```

## 부모 클래스의 `private` 멤버를 제외한 모든 멤버를 상속 받는다.

생성자는 멤버가 아니므로 상속받지 않는다.
  
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
- `@Override` 어노테이션을 넣으면 부모 클래스의 메서드가 없을 때 컴파일 에러가 난다.   
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

# 추상 클래스 (Abstract Class)

추상 클래스 (Abstract Class) 란 클래스들의 공통적인 필드나 메서드를   
추출하여 만들어진 클래스이다.

## 추상 클래스를 사용하는 이유?

- 공통적인 메서드나 필드를 하나로 묶어 유지보수성을 높이고, 통일성을 유지 할 수 있다.
- 추상 클래스를 상속받는 클래스를 구현 할 때 시간을 절약 할 수 있다.
- 규격에 맞춰 클래스를 구현 할 수 있다.

## 추상 클래스 특징

### 추상 클래스 정의

`class` 키워드 앞에 `abstract` 키워드를 사용하여 표현한다.
```java
abstract class 클래스명 {}
```

### 추상 메서드 정의

추상 메서드는 리턴타입 앞에 `abstract` 키워드를 사용하여 표현하고,   
메서드 시그니쳐는 존재하지만, 메서드의 구현부는 존재하지 않는다.

**추상 메서드가 하나라도 존재하면 추상 클래스이다.**

```java
abstract class 클래스명 { 
    abstract 리턴타입 메서드명(파라미터...);
}
```

### 추상 클래스 상속

추상 클래스를 상속받는 클래스를 구현 할 때는 해당 추상 클래스의 추상 메서드를   
반드시 오버라이딩 하여 구현해야 한다.

```java
public class AbstractTest {
    public static void main(String[] args) {
        // 각자 오버라이딩 하여 구현 한 메서드를 호출한다.
        Backend backendDeveloper = new Backend();
        backendDeveloper.codeSample();
        Frontend frontendDeveloper = new Frontend();
        frontendDeveloper.codeSample();
        
        // 추상 클래스는 객체 생성이 불가능하다.
        // Developer developer = new Developer();
        
        // 하지만 Anonymous Class 표현식을 써 추상 클래스를 직접 정의 할 수 있다.
        Developer developer = new Developer() {
            @Override
            public void codeSample() {
                System.out.println("skeleton code");
            }
        };
        developer.codeSample();
    }
}

abstract class Developer {

    public String language;
  
    public abstract void codeSample();

}

class Backend extends Developer {

    public Backend() {
        this.language = "Java";
    }
  
    @Override
    public void codeSample() {
        // 추상 클래스를 상속 받을 때는 반드시 추상 메서드를 오버라이딩 하여 구현해야 된다.
        System.out.println("public static void main(String[] args) {}");
    }
    
}

class Frontend extends Developer {

    public Frontend() {
        this.language = "Vue.js";
    }
  
    @Override
    public void codeSample() {
        System.out.println("new Vue({router, store, render: h => h(app)})");
    }
    
}
```
```
public static void main(String[] args) {}
new Vue({router, store, render: h => h(app)})
skeleton code
```

# `final` 키워드

Java 에서 `final` 키워드는 오직 한번만 할당 할 수 있는 entity 를   
정의 할 때 사용하는 키워드이다.

키워드를 사용하는 곳에 따라 약간씩 다른 기능을 한다.

## `final` 변수

변수의 앞에 `final` 키워드가 붙는 경우로, primitive type, reference type 에   
따라 다르다.

### Primitive type

`final` 키워드가 붙은 해당 변수는 한번 초기화하면 변경 할 수 없는 상수값이 된다.

```java
public class FinalTest {
    public static void main(String[] args) {
        // final primitive type 초기화 방법
        final int intVal = 1;
        final int intVal2;
        intVal2 = 10;
      
        // intVal = 10;
        // intVal2 = 20;
        /* final 키워드가 붙은 primitive type 은 한번 초기화하면 변경 할 수 없다. */
    }
}
```

### Reference type

`final` 키워드가 붙은 해당 변수는 다른 참조 값을 가리키도록 변경 할 수 없다.
  
```java
public class FinalTest {
    public static void main(String[] args) {
        // final reference type 초기화 방법
        final Person person = new Person("HyunGyu", 27);
        final Person person2;
        person2 = new Person("HyunGyu", 27);

        // person = new Person("hyun gyu", 10);
        /* final 키워드가 붙은 reference type 은 다른 참조 값을 가리키도록 변경 할 수 없다. */

        // 객체의 속성은 변경 가능하다
        person.setAge(10); 
        person2.setName("hyun gyu");

        System.out.println("person name is " + person.getName() + ", age is " + person.getAge());
        System.out.println("person2 name is " + person2.getName() + ", age is " + person2.getAge());
    }
}
class Person {

    private String name;

    private int age;

    public Person() {
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

}
```

### 메서드의 파라미터

파라미터에 `final` 키워드가 붙으면 메서드 구현부에서 변수 값을 변경 할 수 없다.

### 멤버 변수
- 클래스의 멤버 변수에 `final` 키워드가 붙으면 선언과 동시에 초기화를 하거나,   
  인스턴스 초기화 블록에서 초기화를 하거나, 생성자에서 초기화를 한다.
- `static` 키워드가 같이 붙으면 선언과 동시에 초기화를 하거나, 정적 초기화 블록   
  에서 초기화를 해야한다.
- `final` 키워드가 붙어있기 때문에 한가지 방법으로만 초기화 할 수 있다.
  
```java
/**
 * final 키워드가 있는 멤버 변수의 초기화 방법들을 위한 클래스로,
 * final 멤버 변수의 중복 초기화로 인한 컴파일 에러를 무시하고 작성하였다.
 */
public class FinalFieldTest {
    final static String name = "HyunGyu";   // static final 변수 선언과 동시에 초기화
    final int age = 20;                     // final 멤버 변수 선언과 동시에 초기화

    static {
        // static final 변수 정적 초기화 블록을 사용해 초기화
        this.name = "HyunGyu";
    }

    {
        // final 멤버 변수 인스턴스 초기화 블록을 사용해 초기화
        this.age = 20;
    }

    public FinalFieldTest() {
        // final 멤버 변수 생성자를 사용해 초기화
        this.age = 20;
    }

    public FinalFieldTest(int age) {
        // final 멤버 변수를 생성자를 이용해 초기화를 할 경우 모든 생성자에 초기화 로직이 있어야한다.
        this.age = age;
    }
}
```


## `final` 메서드

`final` 키워드가 붙은 메서드는 오버라이딩 할 수 없음 을 의미한다.

```java
class Parent {
    public final void print() {
        System.out.println("재정의를 할 수 없습니다.");
    }
}

class Child extends Parent {

    // 컴파일 에러가 난다.
    @Override
    public void print() {
      System.out.println("재정의 합니다.");
    }
}
```

## `final` 클래스

`final` 키워드가 붙은 클래스는 상속 받을 수 없음 을 의미한다.   
주로, Util 형식의 클래스나 상수 값을 모아놓은 클래스에서 쓰인다.

```java
final class Parent {
    String name;
    String age;
}

/**
 * final 키워드가 붙은 클래스를 상속 받으려고 시도할 때 컴파일 에러가 난다.
 */
class Child extends Parent {}
```

# `Object` 클래스

`Object` 클래스는 기본적으로 Java 에서 모든 클래스의 부모 클래스, 즉 최상위 클래스이다.

최상위 클래스이기 때문에 모든 클래스를 받을 수 있고, ([다형성](#다형성-polymorphism))   
모든 클래스에서 `Object` 클래스의 멤버를 사용 할 수 있다.

## `Object` 클래스의 내장 메서드

여러가지 메서드들 중 `final` 메서드가 아닌 메서드들은 오버라이딩이 가능하다.

|접근지시자(final 여부)|리턴타입|메서드명(파라미터)|설명|
|---|---|---|---|
|public final|Class|getClass()|해당 객체의 클래스 객체를 반환한다.|
|public|int|hashCode()|해당 객체의 해쉬코드 값을 반환한다.|
|public|boolean|equals(Object obj)|해당 객체와 파라미터의 객체가 같은지 판단한다.|
|protected|Object|clone()|해당 객체의 복사본을 만들어 반환한다.|
|public|String|toString()|해당 객체의 대한 정보를 문자열로 반환한다.|
|public final|void|wait()|가지고 있는 고유 락을 해제하고, notify(), notifyAll() 메서드 호출 전까지 쓰레드를 잠들게 한다.|
|public final|void|wait(long timeout)|지정된 시간만큼 쓰레드를 잠들게한다. (timeout 은 1/1000 초 단위이다.)|
|public final|void|wait(long timeout,int nanos)|지정된 시간만큼 쓰레드를 잠들게한다. (nanos 는 1/10<sup>9</sup> 초 단위이다.)
|public final|void|notify()|wait 상태인 쓰레드 중 임의로 하나를 골라 깨운다.|
|public final|void|notifyAll()|wait 상태인 쓰레드를 모두 꺠운다.|
|protected|void|finalize()|객체가 소멸되는 시점에 가비지 컬렉터에 의해 자동으로 호출 되는 메서드이다.



> 웹문서
> - [The Java Tutorials(Inheritance)](https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html)
> - [메소드 오버라이딩](http://www.tcpschool.com/java/java_inheritance_overriding)
> - [다형성의 개념](http://tcpschool.com/java/java_polymorphism_concept)
> - [다이나믹 메소드 디스패치](https://velog.io/@maigumi/Dynamic-Method-Dispatch)
> - [[JAVA]Static Method Dispatch, Dynamic Method Dispatch, Double Dispatch](https://defacto-standard.tistory.com/413_)
> - [[Java] java final 키워드](https://gmlwjd9405.github.io/2018/08/06/java-final.html)
> - [자바에서 final에 대한 이해](https://advenoh.tistory.com/13)
> - [Object class in Java](https://www.javatpoint.com/object-class)