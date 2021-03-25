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
        // 기본적으로 변수를 선언 할 때에는 
        // 자료형 변수명; 으로 선언을 하고, 
        // 초기화 할 때에는
        // 변수명 = 값; 으로 초기화가 이루어진다.
        // 밑의 예시들 처럼 선언과 동시에 초기화가 가능하다.
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

*정수형* 에서는 0b로 시작하면 2진수, 0으로 시작하면 8진수, 0x로 시작하면 16진수 int형으로 컴파일 되며,   
long 타입에서는 뒤에 L, l 을 붙여 표현한다.   

*실수형* 에서는 소수점 형태나 지수 형태로 표현한 값들이며, float 타입은 뒤에 f, double 타입은 뒤에 d를 붙여 표현한다.    
기본 double 타입으로 컴파일 되어 뒤에 d를 붙이는 것을 생략해도 되지만, float 타입의 경우 f를 꼭 붙여줘야 한다.

*문자형* 에서는 단일 인용부호('')로 표현한 값들이며, 문자가 들어가거나, \u 이후 4자리 16진수 값으로 표현된다.

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


# 변수의 스코프와 라이프타임
변수의 스코프는 변수에 접근할 수 있는 유효 범위/영역을 말하며, 일반적으로 선언된 블록(`{}`) 내에서만 엑세스 될 수 있다.   
라이프타임은 변수가 메모리에서 살아있는 시간을 의미한다.

멤버 변수(Instance variable), static 변수(Class variable), 지역 변수(Local variable) 로 나뉘어 지고,

```java
public class VariableScope {
    int a = 10;         // 멤버 변수
    static int b = 25;  // static 변수

    public static void main(String[] args) {
        VariableScope variableTest = new VariableScope();
        int a = 25;     // 지역 변수
        System.out.println("result is " + a);              // result is 25
        System.out.println("result is " + variableTest.a); // result is 10

        System.out.println("result is " + b);              // result is 25
        int b = 10;     // 지역 변수
        System.out.println("result is " + b);              // result is 10
    }
}
```

## 멤버 변수 (Instance Variable)

클래스 내부와 모든 메소드 및 블록 외부에서 선언된 변수이며, static 메소드를 제외한 클래스 어디서든 사용 할 수 있고,   
라이프타임은 객체가 메모리에 남아 있을 때까지 이다.

## static 변수 (Class Variable)

클래스 내부와 모든 메소드 및 블록 외부에서 static으로 선언된 변수 이며, 클래스 어디서든 사용 할 수 있고,   
라이프타임은 프로그램이 끝날때 까지, 혹은 클래스가 메모리에 로드되는 동안 이다.

## 지역 변수 (Local Variable)

멤버 변수, 혹은 static 변수를 제외한 모든 변수 이며, 선언 된 블록 내에서 사용 할 수 있고,   
라이프타임은 컨트롤이 선언 된 블록을 떠날때까지 이다.

# 타입 변환

Java 의 타입 변환에는 자동 타입 변환(promotion)과 강제 타입 변환(casting)이 있다.

## 자동 타입 변환 (Promotion)

크기가 작은 자료형을 큰 자료형에 대입할 때, 자동으로 작은 자료형이 큰 자료형으로 변환되는 현상이다.   
기본형에서만 이루어지며, 타입의 크기는   
`byte(1) < short(2) < int(4) < long(8) < float(4) < double(8)` 순서이다.   
float 타입이 4byte고 long 타입이 8byte지만 더 큰 타입으로 구분 되는 이유는 float 타입의 표현 범위가 더 넓어서 이다.

```java
public class PromotionTest {
    public static void main(String[] args) {
        byte byteVal = 4;
        int intVal = byteVal;       // 자동 타입 변환으로 byte 타입 데이터의 값이 int 타입으로 바뀐다.
        System.out.println("result is " + intVal);     // result is 4
        double doubleVal = intVal;   // 정수형 타입을 실수형 타입으로 변환할 때는 뒤에 .0이 붙은 실수형으로 표현된다.
        System.out.println("result is " + doubleVal);    // result is 4.0
        char charVal = 'A';
        intVal = charVal;   // 문자 타입을 int형으로 변환할 때는 유니코드 값이 저장된다.
        System.out.println("result is " + intVal);      // result is 65
    }
}
```
음수가 저장될 수 있는 byte, int 타입 등은 char 타입으로 자동 타입 변환 할 수 없다.(강제 타입 변환은 가능하다.)

## 강제 타입 변환 (Casting)

크기가 큰 자료형은 더 작은 자료형으로 자동 타입 변환을 할 수 없기 때문에 강제로 변환을 해줘야 하고, 이걸 강제 타입 변환(Casting) 이라고 한다.   
이 과정에서 데이터의 손실이 있을 수 있다.
```java
public class CastingTest {
    public static void main(String[] args) {
        int intVal = 103029770;
        // byte byteVal = intVal;   // 컴파일 에러가 난다. 
        byte byteVal = (byte) intVal;   // 캐스팅을 하여 강제로 타입을 변환 시켜준다.
        System.out.println("result is " + byteVal); // result is 10
        intVal = 65;
        char charVal = (char) intVal;
        System.out.println("result is " + charVal); // result is "A"
        double doubleVal = 3.14;
        intVal = (int) doubleVal;
        System.out.println("result is " + intVal);  // result is 3
    }
}
```
정수형 타입을 더 낮은 정수형 타입으로 캐스팅 할 때는 낮은 정수형 타입의 byte 크기에 따라 저장되고, 남은 byte는 버려지게 된다.   

intVal 의 값이 103029770 이고, 이걸 int 타입의 크기인 4byte로 표현하면   
00000110 00100100 00011100 00001010 이고,    
byte 타입의 크기는 1byte 이기 때문에 앞의 3byte가 삭제되고, 남은 1byte로만 계산이 되서    
~~00000110 00100100 00011100~~ 00001010 => 10 이 저장된다.    
intVal 의 값이 1byte로 표현되는 값이라면 캐스팅을 해도 값이 유지된다.

실수형 타입을 정수형 타입으로 캐스팅 할 때는 소수점 이하 부분은 버려지고, 정수 부분만 저장된다.

자바에서는 데이터 값 검사를 위해 Byte.MIN_VALUE 나 Byte.MAX_VALUE 같은 최대값, 최소값 상수를 제공한다.


# 1차 및 2차 배열 선언
배열이란 같은 자료형의 데이터들을 연속된 공간에 저장하기 위한 자료구조이다.    
연관된 데이터를 그룹화하여 묶어주는 역할을 하며, 중복된 변수의 선언을 줄여주고, 반복문 등을 이용하여 계산과 같은 과정을 쉽게 처리 할 수 있다.


```java
public class ArrayTest {
    public static void main(String[] args) {
        int[] array1;
        int array2[];
        int[] array3 = new int[5];
        array3[0] = 1;
        array3[1] = 2;
        array3[2] = 3;
        array3[3] = 4;
        array3[4] = 5;
        int[] array4 = {1, 2, 3, 4, 5};
        int[] array5 = new int[]{1, 2, 3, 4, 5};

        int[][] sampleArray1;
        int sampleArray2[][];
        int[][] sampleArray3 = new int[2][3];
        int[][] sampleArray4 = {{1, 2, 3}, {4, 5, 6}};
        int[][] sampleArray5 = new int[][]{{1, 2, 3}, {4, 5, 6}};
    }
}
```
다양한 방법으로 배열은 선언 및 초기화 할 수 있고, 배열에 접근할 때 처음은 0부터 시작한다.   
예시 코드를 기준으로    
1차원 배열은 스택 영역에 배열의 주소값이 담겨있고, 주소값이 가리키고 있는 힙영역부터 배열의 길이만큼 할당이 되어 있다.   
2차원 배열은 스택 영역에 배열의 주소값이 담겨있고, 주소값이 가리키고 있는 힙영역부터 배열의 길이만큼 하위 배열의 주소값이 담겨있다.

# 타입 추론


> 웹문서
> - [자바 자료형 정리(Java Data Type)](https://jdm.kr/blog/213)
> - [Java data type 정리](https://hyungjoon6876.github.io/jlog/2018/07/05/java-data-type.html)
> - [[JAVA]자바_리터럴(literal)이란?](https://mine-it-record.tistory.com/100)
> - [[Java] 변수의 스코프와 라이프타임](https://league-cat.tistory.com/411)
> - [3. Java 자바 - 자동 타입 변환, 강제 타입 변환](https://kephilab.tistory.com/27)
> - [[Java] 배열(Array) 선언 및 초기화 하기](https://ifuwanna.tistory.com/231)