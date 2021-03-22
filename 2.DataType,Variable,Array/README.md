Data type, Variable, Array
===

# Data type 이란?
자바 언어가 처리할 수 있는 데이터의 종류를 뜻하며, 자바의 데이터 타입은 기본형(Primitive type)과 참조형   (Reference type)으로 나뉘어 진다.   
기본형은 메모리에 값 자체가 저장되고, 참조형의 경우 Heap 영역에 주소값이 참조된 형태로 Java GC에 의해 관리가 된다.

# 기본형 (Primitive type)
자바에서 기본 자료형은 반드시 사용하기 전에 선언 되어야하고, 자료형의 길이는 모든 OS에서 같다.   
기본형은 정수형, 실수형, 문자형, 논리형으로 나눠진다.   
null 값을 가질 수 없으며, primitive type을 클래스화 한 Wrapper class (reference type) 에서는 null 값을 가질 수 있다.

## 정수형

### byte

- 길이가 8bit인 부호있는 정수형 타입으로, 범위는 -128 ~ 127 까지이다.
- default 값은 0이며, Wrapper class 는 Byte 이다.

### short

- 길이가 16bit인 부호있는 정수형 타입으로, 범위는 -32768 ~ 32767 까지이다.
- default 값은 0이며, Wrapper class 는 Short 이다.
### int

- 길이가 32bit인 부호있는 정수형 타입으로, 범위는 -2147483648 ~ 2147483647 까지이다.
- default 값은 0이며, Wrapper class 는 Integer 이다.

### long

- 길이가 64bit인 부호있는 정수형 타입으로, 범위는 -9223372036854775808 ~ 9223372036854775807 까지이다.
- default 값은 0L 이며, L 은 long 타입의 리터럴이다. 
- Wrapper class 는 Long 이다.

## 실수형

### float

- 길이가 32bit인 부호있는 실수형 타입으로, 범위는 1.40239846e-45f ~3.40282347e+38f 까지이다.
- default 값은 0.0f 이며, f 는 float 타입의 리터럴이다.
- Wrapper class 는 Float 이다.

### double

- 길이가 64bit인 부호있는 실수형 타입으로, 범위는 4.94065645841246544e-324~1.79769313486231570e+308 까지이다.
- default 값은 0.0d 이며, d 는 double 타입의 리터럴이다.
- Wrapper class 는 Double 이다.

## 문자형

### char

- 길이가 16bit인 유니코드 문자형 타입으로, 범위는 \u0000 ~ \uffff (0 ~ 2^15-1) 까지이다.
- default 값은 \u0000이며, Wrapper class 는 Character 이다.

## 논리형

### boolean

- 길이가 1bit인 논리형 타입으로, true 혹은 false로 나뉜다.
- default 값은 false 이며, Wrapper class 는 Boolean 이다.

## String 클래스

String 클래스는 참조형에 속하지만, 기본형처럼 사용한다.   
불변(immutable)하는 객체이고, 데이터를 바꾸는 메소드들은 새로운 String 클래스를 만드는 것이다.   
기본형의 비교는 `==` 연산자를 사용하지만, String 클래스 간의 비교는 `equals()` 메소드를 사용해야한다.

## 리터럴 (Literal)

```java
public class Literal {
    public static void main(String[] args) {
        int binary = 0b11;              // 2진수
        int octal = 015;                // 8진수
        int decimal = 15;               // 10진수
        int hexaDecimal = 0x11;         // 16진수
        long longType = 15L;            // long 타입

        float floatType = 1.23f;        // float 타입
        double doubleType = 1.23d;      // double 타입
        double dTypeSkip = 1.23;        // 뒤에 붙는 d를 생략해도 된다.
        double exponential = 5.34e3;    // 지수 형식으로 표현 할 수 있다.

        char alphabet = 'a';            // 문자 하나
        char korean = '현';             // 한글도 가능하다.
        char unicode = '\u1234';        // 유니코드 형식

        boolean bool = true;            // true or false
        boolean isEqual = 3 == 4;       // 논리 연산자를 이용한 초기화도 가능하다.
    }
}
```
프로그램에서 직접 표현한 값을 의미하며, 자료형마다 리터럴을 다르게 표현한다.   

정수형에서는 0b로 시작하면 2진수, 0으로 시작하면 8진수, 0x로 시작하면 16진수 int형으로 컴파일 되며,   
long 타입에서는 뒤에 L, l 을 붙여 표현한다.   

실수형에서는 소수점 형태나 지수 형태로 표현한 값들이며, float 타입은 뒤에 f, double 타입은 뒤에 d를 붙여   
표현한다. 기본 double 타입으로 컴파일 되어 뒤에 d를 붙이는 것을 생략해도 되지만, float 타입의 경우 f를   
꼭 붙여줘야 한다.

문자형에서는 단일 인용부호('')로 표현한 값들이며, 문자가 들어가거나, \u 이후 4자리 16진수 값으로 표현된다.

# 참조형 (Reference type)

`java.lang.Object` 클래스를 상속받거나, 선언한 자료형이 기본형이 아닐 경우 참조형이 된다.   
참조형은 클래스형(Class type), 인터페이스형(Interface type), 배열형(Array type), Enum type 등이 있다.   
new 연산자를 통해 생성되는 인스턴스들로, Heap 영역에 주소값이 저장되어 관리된다.

## 클래스형(Class type)

``` java
class Student {
    private String name;
    Student(name) {
        this.name = name;
    }
    public int getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
}

public class ClassType {
    public static void main(String[] args) {
        Student a = new Student("hyungyu");
        Student b = new Student("hyukjae");
        System.out.println(a);           // result is com.sample.Student@2f92e0f4
        System.out.println(a.getName()); // result is hyungyu
        a = b;
        System.out.println(a.getName()); // result is hyukjae
        b.setName("HyunGyu");
        System.out.println(a.getName()); // result is HyunGyu
    }
}
```
a 를 출력했을 때 나오는 값은 a 라는 인스턴스의 해쉬코드 값이고, 주소값과는 별개인 인스턴스 고유의 값이지만, 주소값이 같으면 해쉬코드 값이 같다.   
Student 클래스의 데이터 타입은 참조형이고, 그중에서 클래스형이다.   
주소값을 참조하고 있는 클래스형 이기 때문에, `a = b` 를 했을 때 b 와 a 의 주소값이 같아지고,   
따라서 b의 값을 바꾸면 a 의 값도 바뀐다.

## 인터페이스형(Interface type)

```java
interface Person {
    void walk();
    void run();
}
```
interface도 자료형(참조형)이기 때문에, 자신을 구현한 객체의 주소를 가질 수 있다.

## 배열형(Array type)

```java
public class ArrayType {
    public static void main(String[] args) {
        int[] integerArray1 = new int[5];   // 배열 선언 방식 [] 안에 넣을 길이를 정한다.
        int integerArray2[] = new int[5];   // [] 의 위치는 자료형 뒤, 변수명 뒤 어디든 상관 없다.
        int[][] arraySample = null;
    }
}
```
배열형은 기본형 배열, 참조형 배열 모두 만들 수 있다.   
자료형에 대해 `[]`를 선언하여 배열을 지정 할 수 있고, 주소값을 가지고 있기 때문에 클래스형의 특징과 일치한다.   
`[][]` 대괄호를 중첩하여 다중 배열로 사용할 수 있다.




> 웹문서
> - [자바 자료형 정리(Java Data Type)](https://jdm.kr/blog/213)
> - [Java data type 정리](https://hyungjoon6876.github.io/jlog/2018/07/05/java-data-type.html)
> - [[JAVA]자바_리터럴(literal)이란?](https://mine-it-record.tistory.com/100)