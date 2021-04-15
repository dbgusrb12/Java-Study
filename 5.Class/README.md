Class
===

# 클래스(Class) 란?

객체를 정의하기 위한 틀 또는 설계도 이다.   
클래스는 객체의 상태를 나타내는 필드(field)와 객체의 행동을 나타내는 메소드(method)로 구성 된다.   
필드(field)는 클래스에 포함된 변수(variable)를 의미하고,   
메소드(method)는 어떠한 특정 작업을 수행하기 위한 명령문의 집합이다.

# 객체(Object) 란?

실생활에서 우리가 인식 할 수 있는 사물 이다.

학생을 예로 들어 설명해보면,   
학생이라는 개념은 클래스이고, 학생이라는 개념에 속해있는 사람들은 객체이다.

즉, 김철수 라는 아이는 학생이라는 클래스에서 파생된 하나의 객체인 것이다.

이러한 객체들이 프로그래밍의 중심이 되어 객체의 상태와 행동을 구체화시키는 형태의 프로그래밍을   
**객체 지향 프로그래밍(OOP, Object-Oriented Programming)** 이라고 한다.

# 인스턴스(Instance) 란?

자바에서 클래스를 사용하기 위해서는 해당 클래스의 타입에 맞는 객체를 생성해야한다.   
클래스로부터 객체를 선언하는 과정을 인스턴스화 라고 하고,   
이렇게 생성된 해당 클래스 타입의 객체를 인스턴스 라고 한다.

즉, 인스턴스 란 메모리에 할당된 객체를 의미한다.

# 접근 제어자

객체 지향에서 사용자가 굳이 알 필요가 없는 정보는 사용자로부터 숨겨야 한다는   
정보 은닉(data hiding)이란 개념이 있다.   
접근 제어자 란 Java 에서 이러한 정보 은닉을 구체화 할 수 있는 방법이다.

## private

`private` 키워드로 선언을 한 데이터는 같은 클래스 내에서만 접근이 가능하다.

## default

`default` 키워드로 선언을 한 데이터는 같은 패키지 내에서만 접근이 가능하다.   
`default` 키워드는 생략이 가능하다.

## protected

`protected` 키워드로 선언을 한 데이터는 같은 패키지 내에서 접근이 가능하고,   
다른 패키지라도 선언 한 데이터가 속해있는 클래스를 상속받은 자식 클래스에서 접근이 가능하다.

## public

`public` 키워드로 선언을 한 데이터는 어디에서든 접근이 가능하다.


# 클래스, 메서드 정의 시 키워드 순서

|순서|내용|
|---|---|
|1|Annotations|
|2|접근자 (public/protected/private)|
|3|abstract|
|4|static|
|5|final|
|6|transient|
|7|volatile|
|8|default|
|9|synchronized|
|10|native|
|11|strictfp|

# 클래스 정의하는 방법

```java
접근제어자 class 클래스이름 {
    
}
```

이것 외 에도 여러 키워드를 가지고 쓸 수 있다.([클래스, 메서드 정의 시 키워드 순서](#클래스-메서드-정의-시-키워드-순서))


# 클래스의 구성요소

## 필드 (Field)

클래스에 포함된 변수를 의미한다.

객체의 상태를 추상화 한 값으로,  
접근제어자를 `private` 으로 설정해놓고, 메소드(Getter/Setter)를 이용해 값을 가져오거나 변경한다.

## 생성자 (Constructor)

객체 생성과 동시에 필드값들을 초기화 시켜줄 수 있게 해주는 메소드이다.   
생성자의 이름은 해당 클래스의 이름과 같아야 하며, 리턴 타입을 쓰지 않는다.   

기본적으로 아무 생성자도 구현하지 않았다면, 컴파일 시점에 매개 변수가 아무것도 없는 기본 생성자가 만들어진다.

*리턴 값이 클래스명과 같아서 생략하는 것인줄 알았는데 모든 생성자의 리턴값이   
void 이기 때문에 생략하는 거라고 한다.*

- .java 파일

```java
public class Member {
    
    private Integer id;

    private String name;

    private Integer age;
    
}
```

- 컴파일 후 .class 파일

```java
public class Member {
    private Integer id;
    private String name;
    private Integer age;

    public Member() {
    }
}
```

### 생성자 정의하는 방법

```java
접근제어자 해당클래스이름(초기화 할 변수...) {
    초기화 명령문;
}
```

## 메서드 (Method)

기본적으로 메서드란 어떠한 특정 작업을 수행하기 위한 명령문의 집합이다.   
메서드의 사용 목적은 중복 코드 방지, 모듈화로 인한 코드의 가독성 향상 등이 있다.   
하나의 메서드가 하나의 기능만을 수행하도록 작성하는걸 권장한다.   

객체 클래스 내의 쓰임새는 행동을 추상화 한 값으로,   
내부에서 쓰는 메서드는 접근제어자를 `private`으로 설정하고,   
외부에서 접근하는 메서드는 접근제어자를 `public`으로 설정한다.

### 메서드 정의하는 방법

#### 접근 제어자

해당 메소드에 접근할 수 있는 범위를 명시합니다.

#### 반환 타입(return type)

메소드가 모든 작업을 마치고 반환하는 데이터의 타입을 명시합니다.

#### 메소드이름 

메소드를 호출하기 위한 이름을 명시합니다.

#### 매개변수 목록(parameters)

메소드 호출 시에 전달되는 인수의 값을 저장할 변수들을 명시합니다.

#### 구현부

메소드의 고유 기능을 수행하는 명령문의 집합입니다.

```java
접근제어자 리턴타입 메소드이름(매개변수...) {
    /* 구현부 */
    실행 할 코드;
    return 리턴 할 데이터;    // 리턴 타입에 맞게 리턴한다. 리턴 타입이 void 일 경우, 생략해도 된다.
    /* 구현부 */
}
```

## 정리

```java
/**
 * 클래스 정의 하는 방법을 설명하기 위한 클래스이다.
 * 첫번째로, 접근제어자가 들어간다.
 * 두번째로, class 라는 키워드가 들어간다.
 * 세번째로, 해당 class 의 이름이 들어간다.
 */
public class ClassDefinitionTest {
    /*
      처음에 들어 갈 값들은 객체의 상태를 추상화 한 필드 값들이 먼저 들어온다.
      필드값의 선언 방법은
      접근제어자 자료형 필드값이름;
      으로 구성되어 있다.
    */

    private String fieldName1;

    private String fieldName2;

    private String fieldName3;

    /*
      두번째로 들어갈 값들은 객체를 생성 할 수 있게 해주는 생성자이다.
      생성자의 이름은 해당 클래스의 이름과 같아야 된다.
      생성자는 리턴 타입이 없다.
      오버로딩이 가능하기 때문에 매개 변수가 다른 여러개의 생성자를 만들 수 있다.
      생성자는 필드값(멤버 변수)의 초기값을 담당한다.
      기본 생성자(매개 변수가 없는 생성자)는 생성자가 아무것도 없을 때 정의를 하지 않아도 컴파일 시점에 자동으로 정의되어 만들어진다.
      생성자의 선언 방법은
      접근제어자 클래스이름(매개변수...) {
         실행 될 코드;
      }
      로 구성되어 있다.
    */

    public ClassDefinitionTest() {
    }

    public ClassDefinitionTest(String fieldName1) {
        this.fieldName1 = fieldName1;
    }

    public ClassDefinitionTest(String fieldName1, String fieldName2) {
        this.fieldName1 = fieldName1;
        this.fieldName2 = fieldName2;
    }

    /*
      마지막으로 들어갈 값들은 객체의 행동을 추상화한 메서드들이 들어간다.
      메서드의 선언 방법은
      접근제어자 리턴타입 메소드이름(매개변수...) {
         실행 될 코드;
         return ;
      }
      로 구성되어 있다.
    */

    public void classMethod(String fieldName1) {
        if(fieldName1.equals("안녕하세요")) {
            this.fieldName1 = fieldName1;
        }
    }

}
```

# 객체 만드는 방법

`new` 키워드와 생성자를 통해서 객체를 만들 수 있다.

```java
public class CreateObjectTest {

    public static class CreateObject {
        private String objectName;

        public CreateObject() {
        }

        public CreateObject(String objectName) {
            this.objectName = objectName;
        }
    }

    public static void main(String[] args) {
        CreateObject object = new CreateObject();               // new 키워드 뒤에 기본 생성자 호출

        String objectName = "객체";
        CreateObject object2 = new CreateObject(objectName);    // new 키워드 뒤에 매개변수 생성자 호출
    }
}
```

# `new` 키워드

`new` 키워드는 클래스 타입의 인스턴스(객체)를 생성해주는 역할을 담당한다.   
`new` 키워드를 통해 메모리(Heap 영역)에 데이터를 저장할 공간을 할당받고   
그 공간의 참조값을 객체에게 반환하고, 생성자를 호출하게 된다.

# `this` 키워드

Java 에서 사용하는 `this` 키워드는 객체 자신을 나타낸다.   
해당 class 의 코드 블록(`{}`) 안에서만 사용 가능하며,    
주로 3가지 용법으로 사용하는데,

## `this.변수명`

그 객체의 변수에 접근하는 방법이다. 주로 생성자, Getter/Setter 메서드에서 쓰인다.

```java
public class ThisKeywordTest {
    
    private String field1;
    
    private String field2;

    public ThisKeywordTest(String field1, String field2) {
        this.field1 = field1;   // 매개 변수의 변수명과 필드 값의 변수명이 같으면 코드 블럭 안에서는 매개 변수가 우선으로 적용된다.
        this.field2 = field2;   // 그래서 this 키워드를 이용해 객체의 필드값에 접근해 대입한다.
    }

    public String getField1() {
        return field1;
    }

    public void setField1(String field1) {
        this.field1 = field1;
    }

    public String getField2() {
        return field2;
    }

    public void setField2(String field2) {
        this.field2 = field2;
    }
    
}
```

## `this(매개변수...)`

해당 클래스의 생성자를 호출 하는 방법이다.
여러개의 생성자가 오버로딩 되어 있을 때 중복 코드를 줄일 수 있는 방법이다.

```java
public class ThisKeywordTest {

    private String field1;

    private String field2;

    public ThisKeywordTest() {
        this("안녕하세요", "반갑습니다.");          // 생성자 안에서 다른 생성자를 호출 할 수 있다.
    }

    public ThisKeywordTest(String field1, String field2) {
        this.field1 = field1;
        this.field2 = field2;
    }

}
```

## `this`

`this` 키워드 그 자체를 쓰는 방법으로, 해당 객체의 참조값을 전달 할 때 쓰인다.

```java
public class ThisKeywordTest {

    private String field1;

    private String field2;

    public ThisKeywordTest(String field1, String field2) {
        this.field1 = field1;
        this.field2 = field2;
    }

    /**
     * 메서드를 호출 할 경우 생성 되었던 해당 객체의 참조값을 얻을 수 있다.
     * @return
     */
    public ThisKeywordTest getThisInstance() {
        return this;
    }

}
```

# 추가로!

## 메서드 오버로딩과 오버라이딩

### 오버로딩(Overloading)

메서드는 해당 메서드의 이름으로만 형식을 지정하는게 아닌 메서드의 이름과 매개변수의 형식을   
묶어 구분을 하는데, 이 형식을 **Method signature** 라고 한다.

이러한 점 때문에 Java 에서는 한 클래스 내에 이미 사용하려는 이름과 같은 이름을 가진   
메소드가 있더라도 매개변수의 개수 또는 타입이 다르면 (Method signature 가 다르면),   
같은 이름을 사용해서 메소드를 정의할 수 있는데, 그것을 **오버로딩(Overloading)** 이라고 한다.

### 오버로딩의 조건

- 메서드의 이름이 같아야한다.
- 메서드의 리턴 타입이 같아야한다.
- 메서드의 매개변수 개수, 매개변수 자료형이 달라야 한다.

### 오버로딩을 사용하는 이유

- 같은 기능을 사용하는 메서드를 하나의 이름으로 사용 할 수 있다.
- 메소드의 이름을 절약 할 수 있다.

```java
public class OverloadingTest {

    public static void main(String[] args) {
        OverloadingMethod overloadingMethod = new OverloadingMethod();
        
        System.out.println(overloadingMethod.concat("hello", 1));   // concat(String, int) 메서드 실행
        System.out.println(overloadingMethod.concat(1, 0));         // concat(int, int) 메서드 실행
        System.out.println(overloadingMethod.concat(1.0, 0));       // concat(double, int) 메서드 실행
    }

    public static class OverloadingMethod {
        public String concat(String strVal, int intVal) {
            return strVal + intVal;
        }

        public String concat(int intVal, int intVal2) {
            return String.valueOf(intVal) + intVal2;
        }

        public String concat(double doubleVal, int intVal) {
            return String.valueOf(doubleVal) + intVal;
        }
    }
}
```

```
hello1
10
1.00
```

### 오버라이딩(Overriding)

부모 클래스의 메서드를 자식 클래스에서 재정의 하여 사용하는 것으로,   
오버라이딩 할 메서드의 리턴값, 메서드 이름, 매개변수 가 모두 같아야 한다.

오버라이딩에 대해서는 다음 주차에서 더 알아보도록 하겠다.



> 웹문서
> - [클래스의 개념](http://www.tcpschool.com/java/java_class_intro)
> - [클래스의 선언](http://tcpschool.com/java/java_class_declaration)
> - [클래스의 구성](http://www.tcpschool.com/java/java_class_component)
> - [[스터디_자바 기본] 21. 생성자(Constructor)](https://programmer-seva.tistory.com/79)
> - [Java this 키워드](https://library1008.tistory.com/4)
> - [[Java]오버로딩 & 오버라이딩(Overloading & Overriding)](https://hyoje420.tistory.com/14)