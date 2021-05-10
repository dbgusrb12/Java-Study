Exception
===

# 예외 (Exception) 란?

프로그램이 실행 되는 도중에 프로그램의 정상적인 흐름을 방해하는 것을   
`Exception` 이라고 한다.

개발자가 구현한 로직에서 발생 하며, 발생할 상황을 미리 예측하여 처리 할 수 있다.

예외는 개발자가 직접 처리 할 수 있기 때문에, 예외에 대한 종류나 처리 방법등을 명확히   
알고 적용하는 것이 중요하다.

# 오류 (Error) 란?

시스템에 비정상적인 현상이 생길 때 발생하는 것이다.   
이러한 `Error` 는 시스템 레벨에서 발생하는 것이라 심각한 오류지만,   
개발자가 미리 예측할 수 없기 때문에 애플리케이션에서 오류에 대한 처리를 신경 쓰지 않아도 된다.

# `Exception` , `Error` 계층 구조

## `Throwable` 의 자식

`Exception`, `Error` 는 `Throwable` 클래스를 상속받고 있고,   
`Throwable` 클래스는 `Object` 클래스의 자식 클래스이다.

`Error` 클래스에 해당하는 클래스들은 시스템 레벨에서 발생하는 에러이기 때문에,   
애플리케이션이 동작하는 도중에 `Error` 를 처리 할 수 없고, 컴파일 자체가 되지 않는다.

`Exception` 클래스에 해당하는 클래스들은 개발자가 로직을 추가하여 처리를 할 수 있고,   
`RuntimeException` 을 기준으로 크게 두가지로 나뉘어지는데,   

`Exception` 의 자식 클래스 중   
`RuntimeException` 을 제외한 모든 클래스를 Checked Exception,   
`RuntimeException` 과 해당 하위 클래스들을 Unchecked Exception   
이라고 한다.

## Checked Exception

Checked Exception 은 반드시 처리해야 되는 예외로, 컴파일 단계에서 확인을 한다.

Checked Exception 을 처리하지 않으면 컴파일 자체가 안되는 점은 `Error`와 같지만,   
개발자의 로직으로 처리 할 수 있다는 부분이 다르다.

또한, 컴파일 시점에서 체크를 하기 때문에, 웬만한 IDE 에서는 Syntax Error 처럼 표시해준다.

기본적으로 Checked Exception 은 예외 발생 시 트랜잭션을 roll-back 하지 않고   
예외를 발생시킨다.

## Unchecked Exception

Unchecked Exception 은 명시적인 처리를 강제하지 않는 예외로,   
실행 시점 (`Runtime`) 에 확인을 한다.

개발자의 실수나 부주의로 발생하는 에러가 대부분이고,   
미리 예측하지 못했던 상황에 발생하는 예외가 아니기 때문에,   
명시적으로 예외 처리를 하지 않아도 프로그램이 실행되며,   
예외가 발생되는 시점에 애플리케이션의 구동을 멈춘다.

기본적으로 Unchecked Exception 은 예외 발생 시 트랜잭션을 roll-back 한 후,   
예외를 발생시킨다.


# 예외 처리 방법

Java 에서 제공하는 예외 처리는 다양한 방법들이 있다.

## `try-catch-finally`

예외가 발생 할 코드를 미리 체크하여, 에러가 발생 했을 때 실행 할 코드를 지정 할 수 있다.
[변수의 스코프](https://github.com/dbgusrb12/Java-Study/tree/study/2.DataType%2CVariable%2CArray#%EB%B3%80%EC%88%98%EC%9D%98-%EC%8A%A4%EC%BD%94%ED%94%84%EC%99%80-%EB%9D%BC%EC%9D%B4%ED%94%84%ED%83%80%EC%9E%84) 에 따라서 블럭(`{}`) 안에 선언된 변수들은 그 안에서만 유효하다.

```java
try {
    // 예외가 발생 될 수 있는 구문
} catch(예외타입 예외명) {
    // 예외가 발생 됐을 때 실행 될 구문
} catch(예외타입2 예외염) {
    // 예외2가 발생 됐을 때 실행 될 구문
} finally {
    // try-catch 블럭을 지나 마지막으로 실행 될 코드
}
```

### `try`

`try-catch-finally` 구문에서 첫번째 구역에 해당되며,   
예외를 발생 시킬수 있는 부분을 지정하는 구역이다.

`try` 블럭 내에서 예외가 발생 할 경우 해당 예외는 `try` 블럭과 연결된   
예외 처리기 `catch` 블럭에서 처리된다.

### `catch`

`try-catch-finally` 구문에서 두번째 구역에 해당되며,   
예외가 발생 했을 때 해당 예외를 처리 할 수 있는 로직이 있는 구역이다.

`catch` 구문의 매개변수로 들어가는 값은 `Exception` 클래스 거나,   
해당 클래스를 상속받고 있어야한다.

```java
public class CatchTest {
    public static void main(String[] args) {
        int[] intArr = new int[5];
        try {
            // intArr 의 길이보다 큰 index 에 접근하려고 할 때 ArrayIndexOutOfBoundsException 이 발생한다.
            System.out.println(intArr[6]);
        } catch (ArrayIndexOutOfBoundsException e) {    // ArrayIndexOutOfBoundsException 은 Exception 클래스의 하위 클래스이다.
            // ArrayIndexOutOfBoundsException 이 발생 했을 때 처리할 로직을 구현
            System.out.println("배열의 길이보다 큰 인덱스 입니다.");
        }
        try {
            System.out.println(intArr[6]);
        } catch (Exception e) {
            // 상위 클래스의 예외로도 처리 할 수 있다.
            System.out.println("배열의 길이보다 큰 인덱스 입니다.");
        }
    }
}
```
```
배열의 길이보다 큰 인덱스 입니다.
```

`try` 바로 다음에 오게되며, 그 사이에 어떠한 코드도 있을 수 없고,   
하나의 `try` 구문에 여러개의 `catch` 구문이 올 수 있다.

```java
public class MultipleCatchTest {
    public static void main(String[] args) {
        int[] intArr = new int[5];
        
        // try-catch 사이에는 어떠한 코드도 있어선 안된다.
//        try {
//            System.out.println(intArr[6]);
//        }
//        intArr[0] = 5;
//        catch (Exception e) {
//            System.out.println("배열의 길이보다 큰 인덱스 입니다.");
//        }
        
        try {
            // 하나의 try 구문에 여러개의 catch 구문을 사용 할 수 있다.
            System.out.println(intArr[6]);
        } catch (ArithmeticException e1) {
            // 첫번째 catch 구문부터 타고 내려오며,
            System.out.println("0으로 나눌 수 없습니다.");
        } catch (ArrayIndexOutOfBoundsException e2) {
            // 발생한 Exception 을 처리 할 수 있는 구문을 실행한다.
            System.out.println("배열의 길이보다 큰 인덱스 입니다.");
        }
        
    }
}
```
```
배열의 길이보다 큰 인덱스 입니다.
```

여러개의 `catch` 구문을 사용 할 때, 앞서 나온 예외 타입의 하위 클래스는   
사용 할 수 없다. (위에서 나온 상위 예외 타입의 구문에서 미리 처리 되기 때문에 실행 되지 않는 코드가 된다.)

```java
public class TryCatchTest {
    public static void main(String[] args) {
        int[] intArr = new int[5];
        
        try {
            System.out.println(intArr[6]);
        } catch (Exception e) {
            System.out.println("Exception 발생!");
        }
        // 위에서 처리된 예외에 포함되는 예외는 다시 catch 할 수 없다. 
//        catch (ArrayIndexOutOfBoundsException e2) {
//            System.out.println("ArrayIndexOutOfBoundsException 발생!");
//        }
        
    }
}
```

여러개의 `catch` 구문에서 사용하는 코드가 같을 때 `|` 를 구분자로 여러 타입을 나열 할 수 있다.

이 때, `catch` 구문의 매개변수 예외타입은 암시적으로 `final` 키워드가 포함된다. (변경 불가)

```java
public class MultipleExceptionCatchTest {
    public static void main(String[] args) {
        int[] intVal = new int[5];
        
        try {
            System.out.println(intVal[6]);
        } catch (ArrayIndexOutOfBoundsException | ArithmeticException e) {
            // 하나의 catch 구문에서 |를 구분자로 여러 예외를 나열해 처리 할 수 있다.
            System.out.println("에러가 발생 했습니다. 확인해주세요.");
            
            // e는 암시적으로 final 키워드가 붙어있기 때문에 변경이 불가능 하다. (예시를 위한 코드)
            // e = new ArrayIndexOutOfBoundsException();
        }
    }
}
```
```
에러가 발생 했습니다. 확인해주세요.
```

### `finally`

`try-catch-finally` 구문에서 마지막 구역에 해당되며,   
`try-catch` 구문이 종료 된 후 실행되는 구문이다.

일반적인 경우 `try-catch` 구문이 종료 된 후 무조건 실행 되며,   
`try` 또는 `catch` 구문에서 JVM 이 종료되거나, 쓰레드가 중단/중지 되면   
`finally` 구역은 실행되지 않을 수 있다.

```java
public class FinallyTest {

    public static void main(String[] args) {
        System.out.println("finallyMethod 실행 전");
        finallyMethod();
        System.out.println("finallyMethod 실행 후");
    }

    public static void finallyMethod() {
        try {
            System.out.println("try 블럭 실행");
            return;
        } catch (Exception e) {
            System.out.println("catch 블럭 실행");
        } finally {
            // try 구문이 실행 된 후 return 이 됐지만, finally 구문이 실행 되는 것을 볼 수 있다.
            System.out.println("finally 블럭 실행");
        }
    }

}
```
```
finallyMethod 실행 전
try 블럭 실행
finally 블럭 실행
finallyMethod 실행 후
```

`try-catch` 구문과 같이 필수로 들어가야 하는 구문은 아니며,   
주로 사용했던 자원을 반환하거나, 해제 할 때 사용한다. (자원 누수 방지)

```java
public class FinallyTest {

    private static final int SIZE = 10;

    public static void main(String[] args) {
        FinallyTest finallyTest = new FinallyTest();
        finallyTest.writeList();

    }

    public void writeList() {
        PrintWriter out = null;
        List<Integer> list = new ArrayList<>();
        for (int i = 0; i < SIZE - 1; i++) {
            list.add(i);
        }
        try {
            System.out.println("try 구문 실행");

            out = new PrintWriter(new FileWriter("OutFile.txt"));
            for (int i = 0; i < SIZE; i++) {
                out.println("Value at: " + i + " = " + list.get(i));
            }
        } catch (IndexOutOfBoundsException e) {
            System.out.println("IndexOutOfBoundsException 발생! : "
                    + e.getMessage());
        } catch (IOException e) {
            System.out.println("IOException 발생! : " + e.getMessage());
        } finally {
            if (out != null) {
                System.out.println("PrintWriter 자원 반납");
                out.close();
            } else {
                System.out.println("PrintWriter 가 생성되지 않았습니다.");
            }
        }
    }

}
```
```
try 구문 실행
IndexOutOfBoundsException 발생! : Index 9 out of bounds for length 9
PrintWriter 자원 반납
```
- OutFile.txt
```text
Value at: 0 = 0
Value at: 1 = 1
Value at: 2 = 2
Value at: 3 = 3
Value at: 4 = 4
Value at: 5 = 5
Value at: 6 = 6
Value at: 7 = 7
Value at: 8 = 8

```

## `try-with-resources`

Java 7 부터 지원되는 `try-with-resources` 구문은   
한개 이상의 자원을 사용하는 `try-catch` 구문에서 사용 할 수 있다.

```java
try (사용하고자하는자원 자원명 = 초기화) {
    
} catch ... 생략
```

`try` 구문의 매개변수로 사용하고자 하는 자원을 선언과 동시에 초기화를 하면,   
`try-catch` 구문이 종료 된 후 매개 변수의 자원을 자동으로 close 한다.

하지만, 모든 타입의 자원이 매개변수로 들어 갈 수 있는 것이 아니라,   
`java.lang.AutoCloseable` 인터페이스를 구현 한 객체만 매개변수로   
들어 갈 수 있다.


> 웹문서
> - [The Java Tutorials(Exception)](https://docs.oracle.com/javase/tutorial/essential/exceptions/index.html)
> - [JAVA의 Exception(예외)이란 무엇인가](https://preamtree.tistory.com/111)
