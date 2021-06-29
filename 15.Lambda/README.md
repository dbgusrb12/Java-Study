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



> 웹문서
> - [The Java Tutorials(Lambda Expressions)](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html)
> - [java 8 람다식이란?](https://effectivesquid.tistory.com/entry/java-8-%EB%9E%8C%EB%8B%A4%EC%8B%9D%EC%9D%B4%EB%9E%80)