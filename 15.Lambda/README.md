Lambda
===

# 람다식 (Lambda Expressions) 이란?

람다식은 함수형 프로그래밍을 지원하기 위해 Java 8 부터 추가된 기능이며,      
메서드를 하나의 식(expression) 으로 표현한 것 이다.   

이렇게 메서드를 람다식으로 표현하면, 메서드의 이름과, 리턴 값이 없어지므로,   
익명 함수 (anonymous function) 라고도 한다.

# 람다식 사용법

람다식은 기본적으로 

```java
(매개변수..) -> { 실행 코드 }
```

형식으로 이루어져 있으며, 컴파일 시점에 익명 구현 객체로 생성 된다.

```java
public String join(String str, String str2) {
    return str + str2;
}
```

해당 메서드를 람다식으로 표현한다면,

```java
(String str, String str2) -> { return str + str2; }
```

이런 식으로 표현 될 수 있다.

```java
(str, str2) -> str + str2
```
실행되는 코드가 한줄이라면 `{}` 와 `return` 키워드를 제거 할 수 있고,   
매개 변수의 타입이 추론 가능하다면 매개변수의 타입도 제거 할 수 있다.

```java
a -> System.out.println(a);
```
매개 변수가 한 개일 때에는 `()` 를 제거 할 수 있지만, 매개 변수가 없거나, 두 개 이상인 경우   
`()` 를 꼭 써야한다.

# 함수형 인터페이스

람다식은 실제로는 익명 클래스의 객체와 동일하다.

```java
public interface TestInterface {
    void print(String str, String str2);
}

public class LambdaTest {
    public static void main(String[] args) {
        // 익명 클래스로 구현한 TestInterface
        TestInterface testInterface = new TestInterface() {
            @Override
            public void print(String str, String str2) {
                System.out.println(str + str2);
            }
        };

        // 람다식으로 구현한 TestInterface
        TestInterface testLambdaInterface = (str, str2) -> System.out.println(str + str2);

        testInterface.print("안녕", "하세요.");
        testLambdaInterface.print("안녕", "하세요.");
        
    }
}
```
```
안녕하세요.
안녕하세요.
```

람다식으로 표현된 객체는 익명 클래스의 객체와 동일한 기능을 하는 것을 볼 수 있다.   
하지만 인터페이스의 선언된 메서드가 두 개 이상일 경우 인터페이스의 모든 메서드를 재정의 해야 하고,   
람다식으로 여러 개의 메서드를 표현 할 수 없기 때문에,   
Java 에서는 하나의 메서드만 선언된 인터페이스로 람다식을 다룬다.

이렇게 하는 것이 객체 지향 언어인 Java 의 기존 규칙을 깨뜨리지 않고,   
자연스럽게 함수형 프로그래밍을 지원 할 수 있다.

람다식을 다루기 위해 선언한 인터페이스를 **함수형 인터페이스 (Functional Interface)** 라고 한다.

함수형 인터페이스는 `@FunctionalInterface` 어노테이션으로 명시 할 수 있으며,   
`@FunctionInterface` 어노테이션으로 함수형 인터페이스에 대한 형식을 미리 검사 할 수 있다.

함수형 인터페이스의 추상 메서드는 오직 하나만 선언 되어 있어야 하지만,   
`static` 메서드와 `default` 메서드는 개수의 제약이 없다.

# Variable Capture

람다식 내부에서 외부의 지역 변수를 사용하려고 할 때 두가지 제약 조건이 존재한다.   
이 두가지 조건을 만족하는 지역 변수만 람다식 내부에서 사용 할 수 있다.

- 사용하고자 하는 외부의 지역 변수는 `final` 로 선언되어 있어야 한다.
- `final` 로 선언되어 있지 않은 변수는 `final` 처럼 동작해야 한다. (`Effectively final`)

## `Effectively final`

Java 7 에서 익명 클래스의 인스턴스로 넘겨지는 모든 변수들은 `final` 이여야만 한다.   
`final` 이 아닌 변수를 넘기면 컴파일 에러가 발생한다.

익명 클래스에 컨텍스트를 넘겨주는 것을 클로저라고 하고,   
컴파일러는 이 필요한 정보를 복사해서 넘겨주는데 이걸 `Variable capture` 라고 한다.

Java 8 에서 `Effectively final` 이라는 개념이 도입되면서,   
컴파일러에서 해당 변수가 변경 되지 않았다고 판단되면 `final` 이 아니여도, 그 변수를   
`final` 로 해석 할 수 있다.

# 메서드 레퍼런스, 생성자 레퍼런스

메서드 레퍼런스, 생성자 레퍼런스는 람다식에서 쓰이는 축약 표현으로,   
해당 람다식의 내용이 그저 다른 메서드로 전달 하는 목적만 있다면,   
해당 메서드, 생성자 참조를 통해 데이터를 전달 할 수 있다.

이런 형식은 자주 사용되는 패턴의 람다식을 줄여서 간단하게 사용 할 수 있는 방법이다.

## 메서드 레퍼런스

메서드 레퍼런스는 `static` 메서드 레퍼런스와, 인스턴스 메서드 레퍼런스가 있다.

`static` 메서드 레퍼런스는

`클래스이름::정적메서드이름`

으로 표현 되고, 람다식의 파라미터들이 정적 메서드의 파라미터 값으로 들어간다.

```java
(String s) -> System.out.println(s)
// 을
System.out::println
// 와 같이 표현 할 수 있다.
```

인스턴스 메서드 레퍼런스는 `static` 메서드 레퍼런스와 비슷하게

`클래스이름::인스턴스메서드이름`

으로 표현되고,   
람다식의 첫번쨰 파라미터가 메서드의 수신자,   
나머지 파라미터가 해당 인스턴스 메서드의 파라미터 값으로 들어간다.

```java
public interface Sample {
    boolean equals(String a, String b);
}

public class MethodReferenceTest {
    public static void main(String[] args) {
        // 두 메서드는 같은 기능을 한다.
        Sample sample = (String a, String b) -> a.equals(b);
        // 해당 람다식의 첫번쨰 파라미터가 수신자가 되고, 나머지 파라미터들이 메서드의 파라미터가 된다.
        Sample sample2 = String::equals;

        System.out.println(sample.equals("안녕", "안녕"));
        System.out.println(sample2.equals("안녕", "하세요"));
    }
}
```
```
true
false
```

## 생성자 레퍼런스

생성자 레퍼런스는 

`클래스이름::new`

로 표현되고, 람다식의 파라미터들은 해당 생성자의 파라미터로 들어간다.

이 때, 파라미터의 개수와 같은 생성자가 없다면, 컴파일 에러가 난다.

```java
public class TestClass {
    
    private String name;
    
    public TestClass(String name) {
        this.name = name;
    }
}

public interface ConstructorTest {
    
    TestClass getTestClass(String name);

}

public class SampleClass {
    public static void main(String[] args) {
        // 생성자 레퍼런스를 이용하여 객체를 받아 올 수 있다. (이 때 해당 파라미터 개수 만큼의 생성자가 없으면 에러 발생)
        ConstructorTest constructorTest = TestClass::new;
        TestClass testClass = constructorTest.getTestClass();
    }
}
```

> 웹문서
> - [The Java Tutorials(Lambda Expressions)](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html)
> - [java 8 람다식이란?](https://effectivesquid.tistory.com/entry/java-8-%EB%9E%8C%EB%8B%A4%EC%8B%9D%EC%9D%B4%EB%9E%80)
> - [(Java) 람다 캡처링과 final 제약조건](https://perfectacle.github.io/2019/06/30/java-8-lambda-capturing/)