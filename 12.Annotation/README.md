Annotation
===

# 어노테이션 (Annotation) 이란?

어노테이션 (Annotation) 이란 Java 5 부터 등장한 기능으로,   
메타데이터의 한 형태로 분류된다.

사전적 정의로는 주석 이라는 의미를 가지고 있으며,   
주석과는 다른 역할 이지만 주석처럼 해당 요소에 특정한 의미를 부여하거나,   
기능을 주입 할 수 있고, 이런 의미나 기능은 컴파일 타임, 혹은 런타임에 해석 될 수 있다.

# 어노테이션의 용도

어노테이션의 용도는 크게 3가지로 나뉜다.

### 컴파일러에 대한 정보

컴파일러가 오류를 탐지하거나, 경고를 억제하는데 사용한다.

### 컴파일 시간 및 배포 시간 처리

소프트웨어 도구는 어노테이션 정보를 처리하여 코드, XML 파일 등을 생성할 수 있다.

### 런타임 처리

일부 주석은 런타임 시점에 검사 할 수 있다.

# Built-in Annotation

Java 에서 기본적으로 제공하는 어노테이션들이 있다.

## `@Override`

해당 메서드가 오버라이드 되었다는 것을 나타낸다.

만약, 상위 클래스에서 해당 메서드를 찾을 수 없으면 에러를 발생시킨다.

## `@Deprecated`

해당 요소가 더 이상 사용 되지 않으며, 더 이상 사용되지 않아야 함을 나타낸다.

만약, 사용한다면 컴파일 경고를 발생시킨다.

해당 어노테이션의 경우 `Javadoc` 을 사용하여 문서화 해야한다.

`Javadoc` 을 사용 할 때 내부의 어노테이션은 소문자 d로 시작한다.

```java
/**
 * @deprecated  (소문자 d 로 시작함)
 * 보안 상의 문제로 더 이상 사용 되지 않음.
 */
// 대문자 D 로 시작함
@Deprecated
public class DeprecatedClass {
    
}
```

## `@SuppressWarnings`

컴파일러에서 발생 될 특정 경고를 억제하도록 지시한다.

`@SuppressWarnings` 의 요소는 한개 일 때와 여러개 일 때로 나뉘는데,   

- 한 개일때

`@SuppressWarnings("resource")`

- 여러개 일 때,

`@SuppressWarnings({"resource", "unchecked"})`


해당 어노테이션의 요소로 들어갈 목록들은 다음과 같다.

|토큰|설명|
|---|-----|
|all|모든 경고를 억제한다.|
|boxing boxing/unboxing|오퍼레이션과 관련된 경고를 억제한다.|
|cast|캐스트 오퍼레이션과 관련된 경고를 억제한다.|
|dep-ann|권장되지 않는 어노테이션과 관련된 경고를 억제한다.|
|deprecation|권장되지 않는 기능과 관련된 경고를 억제한다.|
|fallthrough|switch 문에서 누락된 break 문과 관련된 경고를 억제한다.|
|finally|리턴되지 않는 마지막 블록과 관련된 경고를 억제한다.|
|hiding|변수를 숨기는 로컬과 관련된 경고를 억제한다.|
|incomplete-switch|switch 문에서 누락된 항목과 관련된 경고를 억제한다(enum case).|
|javadoc|javadoc 경고와 관련된 경고를 억제한다.|
|null|널(null) 분석과 관련된 경고를 억제한다.|
|rawtypes|원시 유형 사용법과 관련된 경고를 억제한다.|
|restriction|올바르지 않거나 금지된 참조 사용법과 관련된 경고를 억제한다.|
resource|닫기 가능 유형의 자원 사용에 관련된 경고 억제한다.|
|serial|직렬화 가능 클래스에 대한 누락된 serialVersionUID 필드와 관련된 경고를 억제한다.|
|static-access|잘못된 정적 액세스와 관련된 경고를 억제한다.|
|static-method|static 으로 선언될 수 있는 메소드와 관련된 경고를 억제한다.|
|super|수퍼 호출을 사용하지 않는 메소드 겹쳐쓰기와 관련된 경고를 억제한다.|
|synthetic-access|내부 클래스로부터의 최적화되지 않은 액세스와 관련된 경고를 억제한다.|
|sync-override|동기화된 메소드를 오버라이드하는 경우 누락된 동기화로 인한 경고 억제한다.|
|unchecked|미확인 오퍼레이션과 관련된 경고를 억제한다.|
|unqualified-field-access|규정되지 않은 필드 액세스와 관련된 경고를 억제한다.|
|unused|사용하지 않은 코드 및 불필요한 코드와 관련된 경고를 억제한다.|

## `@SafeVarargs`

Java7 부터 지원하는 어노테이션으로,   
제너릭 같은 가변인자의 매개변수를 사용할 때의 경고를 무시한다.

## `@FunctionalInterface`

Java8 부터 지원하는 어노테이션으로,   
함수형 인터페이스로 지정한다는 것을 나타낸다.

만약 메서드가 존재하지 않거나, 1개 이상의 메서드 (default 메서드 제외)가 존재할 경우   
컴파일 오류를 발생 시킨다.

# Meta Annotation

Java 5 부터 추가된 기능으로,   
어노테이션을 선언 할 때 사용하는 어노테이션 이다.

## `@Retention`

자바 컴파일러가 어노테이션을 다루는 방법을 기술하며,   
어느 시점까지 영향을 미치는지를 결정한다.

|종류|내용|
|---|---|
|RetentionPolicy.SOURCE|소스 레벨에서만 유지되며 컴파일러에서 무시된다.|
|RetentionPolicy.CLASS|컴파일 시점에 컴파일러에 의해 유지되지만 JVM (Java Virtual Machine)에서는 무시된다.|
|RetentionPolicy.RUNTIME|JVM 에 의해 유지되므로 런타임 환경에서 사용할 수 있다.|

## `@Target`

어노테이션이 적용할 요소를 선택한다.

|종류|내용|
|---|---|
|ElementType.ANNOTATION_TYPE|어노테이션 유형에 적용 할 수 있다.|
|ElementType.CONSTRUCTOR|생성자에 적용 할 수 있다.|
|ElementType.FIELD|필드 또는 속성에 적용 할 수 있다.|
|ElementType.LOCAL_VARIABLE|지역 변수에 적용 할 수 있다.|
|ElementType.METHOD|메서드에 적용 할 수 있다.|
|ElementType.PACKAGE|패키지 선언에 적용 할 수 있다.|
|ElementType.PARAMETER|메소드의 매개 변수에 적용 할 수 있다.|
|ElementType.TYPE|클래스의 모든 요소에 적용 할 수 있다.|

## `@Documented`

해당 어노테이션이 사용될 때마다 해당 요소가 Javadoc 도구를 사용하여   
문서화되어야 함을 나타낸다.

기본적으로 Javadoc 에는 어노테이션 정보가 들어가지 않는다.

## `@Inherited`

해당 어노테이션을 상속을 가능하게 해준다.

## `@Repeatable`

Java8 부터 지원하며, 연속적으로 어노테이션을 선언할 수 있게 해준다.

# 어노테이션 정의하는 방법

어노테이션은 인터페이스의 한 종류로,   
어노테이션을 정의 하는 방법은 인터페이스를 정의 하는 방법과 비슷하다.

`interface` 키워드 앞에 `@` 를 붙여 표시한다.

어노테이션은 암묵적으로 `java.lang.annotation.Annotation` 을 상속받기 때문에   
다른 인터페이스를 상속 받을 수 없다.

```java
public @interface MyAnnotation {
    int num() default 0;
    String name() default "hyunGyu";
}
```

어노테이션 안에 메서드와 유사한 어노테이션 요소가 들어 갈 수 있는데,
내부 요소의 개수에 따라 명칭이 달라지고,   
일정한 규칙이 있다.


- 어노테이션의 요소의 타입은   
`primitive type`, `String`, `enum`, `annotation`, `Class`   
만 허용한다.
- 요소의 `()` 안에 매개 변수를 허용하지 않는다.
- 예외를 선언 할 수 없다.
- 요소를 타입 매개변수로 정의 할 수 없다.
- `default` 키워드를 이용해 기본 값을 가질 수 있다.

### Maker Annotation

Maker 어노테이션은 해당 어노테이션의 요소가 하나도 없는 어노테이션을 의미한다.

단순히 표식으로 사용 되는 어노테이션이며,   
해당 어노테이션은 컴파일러에 어떠한 의미를 전달하는 용도로 쓰인다.

### Single-Value Annotation

Single-Value 어노테이션은 해당 어노테이션의 요소가 하나인 어노테이션을 의미한다.

요소가 하나이기 때문에 값만을 명시하여 데이터를 전달 할 수 있다.   

```java
public @interface MyAnnotation {
    // default 키워드를 사용하여 값이 없을 때 기본으로 들어갈 값을 지정 할 수 있다.
    String value() default "";
}

@MyAnnotation("test annotation") // @MyAnnotation(value = "test annotation") 과 동일하다.
public class MyClass { ... }
```

### Full Annotation

Full 어노테이션은 해당 어노테이션의 요소가 둘 이상인 어노테이션을 의미한다.

데이터는 key-value 형태의 배열로 전달 할 수 있다.

```java
public @interface MyAnnotation {
    // default 키워드를 사용하여 값이 없을 때 기본으로 들어갈 값을 지정 할 수 있다.
    String value() default "";
    int age() default 0;
}

@MyAnnotation(value = "test annotation", age = 10)
public class MyClass { ... }
```

> 웹문서
> - [The Java Tutorials(Annotation)](https://docs.oracle.com/javase/tutorial/java/annotations/index.html)
> - [Java 에서 어노테이션(Annotation)이란?](https://elfinlas.github.io/2017/12/14/java-annotation/)
> - [자바 @SuppressWarnings 사용하기](https://ktko.tistory.com/entry/Java%EC%9D%98-SuppressWarnings-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)