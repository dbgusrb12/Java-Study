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
개발자가 미리 예측할 수 없기 때문에 애플리케이션에서 오류에 대한 처리를 할 수 없고,   
애초에 발생하지 않도록 하는게 중요하다.

# `Exception` , `Error` 계층 구조

Java 에서는 에러들을 제어하기 위해   
`Exception` 클래스와 `Error` 클래스를 제공한다.



## `Throwable` 의 자식

`Exception`, `Error` 는 `Throwable` 클래스를 상속받고 있고,   
`Throwable` 클래스는 `Object` 클래스의 자식 클래스이다.

`Error` 클래스에 해당하는 클래스들은 시스템 레벨에서 발생하는 에러이기 때문에,   
애플리케이션이 동작하는 도중에 `Error` 를 처리 할 수 없다.

`Exception` 클래스에 해당하는 클래스들은 개발자가 로직을 추가하여 처리를 할 수 있고,   
`RuntimeException` 을 기준으로 크게 두가지로 나뉘어지는데,   

`Exception` 의 자식 클래스 중   
`RuntimeException` 을 제외한 모든 클래스를 Checked Exception,   
`RuntimeException` 과 해당 하위 클래스들을 Unchecked Exception   
이라고 한다.

### Checked Exception

Checked Exception 은 반드시 처리해야 되는 예외로, 컴파일 단계에서 확인을 한다.

Checked Exception 을 처리하지 않으면 컴파일 자체가 안되는 점은 `Error`와 같지만,   
개발자의 로직으로 처리 할 수 있다는 부분이 다르다.

또한, 컴파일 시점에서 체크를 하기 때문에, 웬만한 IDE 에서는 Syntax Error 처럼 표시해준다.

기본적으로 Checked Exception 은 예외 발생 시 트랜잭션을 roll-back 하지 않고   
예외를 발생시킨다.

### Unchecked Exception

Unchecked Exception 은 명시적인 처리를 강제하지 않는 예외로,   
실행 시점 (`Runtime`) 에 확인을 한다.

개발자의 실수나 부주의로 발생하는 에러가 대부분이고,   
미리 예측하지 못했던 상황에 발생하는 예외가 아니기 때문에,   
명시적으로 예외 처리를 하지 않아도 프로그램이 실행되며,   
예외가 발생되는 시점에 애플리케이션의 구동을 멈춘다.

기본적으로 Unchecked Exception 은 예외 발생 시 트랜잭션을 roll-back 한 후,   
예외를 발생시킨다.


# 예외 처리 방법

Java 에서 제공하는 예외 처리에 대한 방법은 다양하다.

예외 처리에 필요한 키워드들은 `try`, `catch`, `finally`, `throw`, `throws` 가   
있으며, 밑에서 자세하게 살펴보겠다.

## `try-catch-finally`

예외가 발생 할 코드를 미리 체크하여, 에러가 발생 했을 때 실행 할 코드를 지정 할 수 있다.
[변수의 스코프](https://github.com/dbgusrb12/Java-Study/tree/study/02.DataType%2CVariable%2CArray#%EB%B3%80%EC%88%98%EC%9D%98-%EC%8A%A4%EC%BD%94%ED%94%84%EC%99%80-%EB%9D%BC%EC%9D%B4%ED%94%84%ED%83%80%EC%9E%84) 에 따라서 블럭(`{}`) 안에 선언된 변수들은 그 안에서만 유효하다.

```java
try {
    // 예외가 발생 될 수 있는 구문
} catch(예외타입 예외명) {
    // 예외가 발생 됐을 때 실행 될 구문
} catch(예외타입2 예외명) {
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

그리고 `|`를 구분자로 여러 타입을 나열 할 때에도   
앞서 나온 예외 타입의 하위 클래스는 사용 할 수 없다.

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
        // 앞서 나온 예외 타입의 하위 타입은 사용 할 수 없다.
        // catch (Exception | NullPointerException e2) {
        // }
    }
}
```
```
에러가 발생 했습니다. 확인해주세요.
```

### `finally`

`try-catch-finally` 구문에서 마지막 구역에 해당되며,   
`try-catch` 구문이 종료 된 후 실행되는 구문이다.

일반적인 경우 `try-catch` 구문이 종료 된 후 무조건 실행 되지만,   
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
`java.lang.AutoCloseable` 인터페이스나 이 인터페이스를 상속받은   
`java.io.Closeable` 인터페이스를 구현 한 객체만 매개변수로   
들어 갈 수 있다.

OutFile.txt
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
```java
public class TryWithResourcesTest {
    public static void main(String[] args) {
        
        // try-with-resources 문법 
        try (BufferedReader br = new BufferedReader(new FileReader("OutFile.txt"))) { // try 구문의 매개변수로 자원을 넣는다.
            System.out.println(br.readLine());
        } catch (FileNotFoundException e) {
            System.out.println("FileNotFoundException 발생! : " + e.getMessage());
        } catch (IOException e) {
            System.out.println("IOException 발생! : " + e.getMessage());
        }

        // try-catch-finally 구문으로 표현 하려면 이렇게 한다.
        BufferedReader br = null;
        try {
            br = new BufferedReader(new FileReader("OutFile.txt"));
            System.out.println(br.readLine());
        } catch (FileNotFoundException e) {
            System.out.println("FileNotFoundException 발생! : " + e.getMessage());
        } catch (IOException e) {
            System.out.println("IOException 발생! : " + e.getMessage());
        } finally {
            if(br != null) {
                System.out.println("BufferedReader 자원 반납");
                try {
                    br.close();
                } catch (IOException e) {
                    System.out.println("IOException 발생! : " + e.getMessage());
                }
            } else {
                System.out.println("BufferedReader 가 생성되지 않았습니다.");
            }
        }
    }
}
```
```
Value at: 0 = 0
Value at: 0 = 0
BufferedReader 자원 반납
```

하지만 `try-with-resources` 문법에서도 `finally` 구문을 사용 할 수 있다.

```java
public class TryWithResourcesTest {
    public static void main(String[] args) {
        
        try (BufferedReader br = new BufferedReader(new FileReader("OutFile.txt"))) { // try 구문의 매개변수로 자원을 넣는다.
            System.out.println(br.readLine());
        } catch (FileNotFoundException e) {
            System.out.println("FileNotFoundException 발생! : " + e.getMessage());
        } catch (IOException e) {
            System.out.println("IOException 발생! : " + e.getMessage());
        } finally {
            System.out.println("finally 구역 실행");
        }
    }
}
```
```
Value at: 0 = 0
finally 구역 실행
```

`finally` 구문이나 `try-with-resources` 구문 모두   
`try` 또는 `catch` 구문에서 JVM 이 종료되거나, 쓰레드가 중단/중지 되면   
`close()` 가 제대로 실행이 되지 않는 것 같다.

```java
/**
 * 자원 해제를 테스트 하기 위한 클래스로,
 * Closeable 을 구현해 close() 메서드를 오버라이딩 하였다.
 */
public class CloseSample implements Closeable {

    private String sampleStr;

    public CloseSample() {
        this.sampleStr = "자원 해제 테스트";
    }

    public String getSampleStr() {
        return sampleStr;
    }

    @Override
    public void close() throws IOException {
        System.out.println("close 실행");
    }
}

public class TryWithResourcesTest {
    public static void main(String[] args) {
        
        // try-with-resources 구문
        try (CloseSample closeSample = new CloseSample()) {
            System.out.println(closeSample.getSampleStr());
            // 쓰레드를 멈춰놓고 IntelliJ 에서 강제로 프로젝트를 종료시켜 보았다.
            Thread.sleep(5000);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        // try-catch-finally 구문
        CloseSample closeSample = null;
        try {
            closeSample = new CloseSample();
            System.out.println(closeSample.getSampleStr());
            // 쓰레드를 멈춰놓고 IntelliJ 에서 강제로 프로젝트를 종료시켜 보았다.
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            System.out.println("InterruptedException 발생! : " + e.getMessage());
        } finally {
            if (closeSample != null) {
                try {
                    closeSample.close();
                } catch (IOException e) {
                    System.out.println("IOException 발생! : " + e.getMessage());
                }
            } else {
                System.out.println("CodeSample 이 생성되지 않았습니다.");
            }
        }
    }
}
```
- 강제 종료 시키지 않았을 때
```
자원 해제 테스트
close 실행

Process finished with exit code 0
```
- 강제 종료 시켰을 때
```
자원 해제 테스트

Process finished with exit code 130 (interrupted by signal 2: SIGINT)
```

## `throws`

예외처리 회피 방식에 해당하는 `throws` 키워드는 해당 에러를 직접 해결 하는게 아닌   
호출한 쪽으로 예외를 던지며, 그에 대한 회피하는 것이다.

메서드 시그니쳐 뒤에 `throws` 키워드를 붙이고 `,`를 구분자로 내보낼 예외들을 나열한다.

```java
public void methodName() throws 예외타입, 예외타입 {
    // 메서드 구현부
}
```

무책임하게 `throws` 키워드를 남발하는 것은 위험한 일이다.   
호출한 쪽에서 예외를 받아 처리하도록 하거나, 해당 메서드에서 예외를 던지는게   
최선의 방법이라는 확신이 있을때만 사용해야한다.

```java
public class ThrowsTest {
    
    public static void main(String[] args) {
        ThrowsTest throwsTest = new ThrowsTest();
        throwsTest.arithmeticExceptionMethod();
    }

    public void arithmeticExceptionMethod() throws ArithmeticException {
        int intVal = 0;
        int intVal2 = 1;
        System.out.println(intVal2 / intVal);
    }
    
}
```
```
Exception in thread "main" java.lang.ArithmeticException: / by zero
	at com.company.hg.exception.ExceptionTest.arithmeticExceptionMethod(ExceptionTest.java:13)
	at com.company.hg.exception.ExceptionTest.main(ExceptionTest.java:7)
```

`throws` 키워드를 사용해 예외를 회피해도, 결국 호출한 쪽에서 해결하지 않으면   
에러가 난다.

## `throw`

예외 전환 방식에서 많이 사용하는 `throw` 키워드는 에러를 직접 발생 시키는 키워드이다.   
특정 상황에 원하는 에러를 발생시키거나, 특정 에러를 원하는 에러로 변경하여 해당 에러에 맞는   
처리를 할 수 있게 한다.

```java
throw 예외객체;
```

`throw` 키워드 뒤에 들어갈 객체는 `Exception`, `Error` 객체와 같이   
`throwable` 의 하위 객체들만 들어 갈 수 있다.

`Exception`의 하위 클래스의 예외를 발생시킬 수 있고,   
개발자가 직접 정의한 에러를 발생 시킬수도 있다.

`throw` 한 에러를 `catch` 구문을 사용해 잡을 수도 있고, `throws` 키워드를 통해   
예외를 던질 수도 있다.

Checked exception 의 경우 강제로 발생을 시키면 무조건 `catch`, `throws` 를 이용해   
예외 처리를 해야하고 (컴파일 자체가 안된다.),   
Unchecked exception 의 경우 강제로 발생 시켜도 예외 처리를 하지 않아도,   
컴파일과 실행이 가능하고, 해당 메서드의 행이 실행 될 때 예외가 발생해 프로그램이 멈춘다.

```java
/**
 * throw 키워드의 사용 방법에 대한 클래스
 * 해당 클래스의 예외 처리는 예시로 만들어진 샘플 코드입니다.
 */
public class ThrowTest {
    
    public static void main(String[] args) {
        ThrowTest throwTest = new ThrowTest();
        
        throwTest.catchMethod(0, 10);
        throwTest.throwsMethod(0, 20);
    }

    public void catchMethod(int intVal, int intVal2) {
        if (intVal == 0) {
            try {
                // 에러 발생
                throw new ArithmeticException();
            } catch (ArithmeticException e) {
                System.out.println("intVal 의 값이 0입니다.");
            }
        } else {
            System.out.println(intVal2 / intVal);
        }
    }

    public void throwsMethod(int intVal, int intVal2) throws ArithmeticException {
        if (intVal == 0) {
            // 에러 발생
            throw new ArithmeticException();
        } else {
            System.out.println(intVal2 / intVal);
        }
    }

    public void catchAndThrowsMethod(int intVal, int intVal2) throws RuntimeException {
        try {
            // 특정 에러가 발생 했을 때
            System.out.println(intVal2 / intVal);
        } catch (ArithmeticException e) {
            // 다른 에러로 전환 하여 throws 키워드를 통해 내보낼 수 있다.
            throw new RuntimeException();
        }
    }
}
```
```
intVal 의 값이 0입니다.
Exception in thread "main" java.lang.ArithmeticException
	at com.company.hg.exception.ThrowTest.throwsMethod(ThrowTest.java:28)
	at com.company.hg.exception.ThrowTest.main(ThrowTest.java:9)
```

# 커스텀한 예외를 만드는 방법

발생하는 exception 의 타입을 선택할 때, Java 가 제공하는 exception 을 사용 할 수도   
있지만, 개발자가 직접 exception 을 정의해서 사용 할 수도 있다.

다시 처음으로 돌아와서, `Throwable` 클래스를 살펴보겠다.

`Throwable` 클래스의 필드값 중 `detailMessage` 라는 `private` 필드값이 있고,   
`public` 생성자는 4가지가 있다.

```java
/**
 * Throwable 클래스의 일부를 가져온 클래스 
 * 예시를 위해 필요한 값들만 가져온 클래스이다.
 * 
 */
public class Throwable {

    private String detailMessage;
    
    public Throwable() {
        fillInStackTrace();
    }
    
    public Throwable(String message) {
        fillInStackTrace();
        detailMessage = message;
    }
    
    public Throwable(String message, Throwable cause) {
        fillInStackTrace();
        detailMessage = message;
        this.cause = cause;
    }
    
    public Throwable(Throwable cause) {
        // 매개변수 cause 는 chained exception 방식에서 쓰인다. 자세한건 따로 설명하겠다.
        fillInStackTrace();
        detailMessage = (cause==null ? null : cause.toString());
        this.cause = cause;
    }
    
    /* ... */
}
```

이 `Throwable` 클래스를 상속받은 `Exception` 클래스와 그 하위 클래스들의 생성자는   
이런식으로 되어있다.

```java
/**
 * Exception 클래스의 일부를 가져온 클래스 
 * 예시를 위해 필요한 값들만 가져온 클래스이다.
 */
public class Exception extends Throwable {
    public Exception() {
        super();
    }

    public Exception(String message) {
        super(message);
    }

    public Exception(String message, Throwable cause) {
        super(message, cause);
    }

    public Exception(Throwable cause) {
        super(cause);
    }
}
```

커스텀한 예외를 만들 때는 이 `Exception` 클래스나 해당 클래스의 하위 클래스를   
상속받아 작성하면 된다.

Unchecked Exception 인 커스텀 예외를 만들고 싶으면 `RuntimeException` 클래스나   
해당 클래스의 하위 클래스를 상속받아 만들고,   
Checked Exception 을 만들고 싶으면 `RuntimeException` 클래스가 아닌 클래스를   
상속받으면 된다.

어떤 예외 클래스를 상속받느냐에 따라 Checked exception 과 Unchecked exception   
이 나뉘어지기 때문에, 애플리케이션의 동작 방식에 따라 정의하면 된다.

```java
/**
 * custom checked exception 클래스
 */
public class MyCheckedException extends Exception {

    public MyCheckedException() {
        super();
    }

    public MyCheckedException(String message) {
        super(message);
    }

}
```
```java
/**
 * custom unchecked exception 클래스
 */
public class MyUncheckedException extends RuntimeException {

    // custom exception 클래스에 필요한 값을 넣어 사용 할 수 있다.
    private final int ERR_CODE;

    public MyUncheckedException(String message) {
        super(message);
        ERR_CODE = 500;
    }

    public MyUncheckedException(String message, int errorCode) {
        super(message);
        ERR_CODE = errorCode;
    }

    public int getErrCode() {
        return this.ERR_CODE;
    }

}
```

```java
public class CustomExceptionTest {
    public static void main(String[] args) {
        CustomExceptionTest test = new CustomExceptionTest();

        // 해당 예외는 Unchecked exception 이기 때문에 예외처리를 하지 않아도 컴파일 및 실행이 가능하지만
        // 런타임에서 해당 예외에 대한 처리가 없을 경우 프로그램이 종료된다.
        // test.throwsUncheckedException();

        try {
            test.throwsUncheckedException();
        } catch (MyUncheckedException e) {
            System.out.println("error code : " + e.getErrCode() + ", custom error catch");
        }
        
        try {
            test.throwsUncheckedExceptionWithErrCode(400);
        } catch (MyUncheckedException e) {
            System.out.println("error code : " + e.getErrCode() + ", custom error catch");
        }

        // 해당 예외는 Checked exception 이기 때문에 예외 처리를 하지 않으면 컴파일 시점에서 에러가 난다.
        // test.throwsCheckedException();

        try {
            test.throwsCheckedException();
        } catch (MyCheckedException e) {
            System.out.println("custom error catch!");
        }
    }

    public void throwsUncheckedException() throws MyUncheckedException {
        throw new MyUncheckedException("Unchecked exception 발생!");
    }

    public void throwsUncheckedExceptionWithErrCode(int errorCode) throws MyUncheckedException {
        throw new MyUncheckedException("Unchecked exception 발생", errorCode);
    }
    
    public void throwsCheckedException() throws MyCheckedException {
        throw new MyCheckedException("Checked exception 발생!");
    }
}
```
```
error code : 500, custom error catch
error code : 400, custom error catch
custom error catch!
```

# 추가로!

## Chained Exception

- 예외는 다른 예외를 유발할 수 있다. 예외 A가 예외 B를 발생시켰다면, 예외 A는 B의 원인 예외(cause exception)이다.
- 원인 예외를 새로운 예외에 등록한 후 다시 새로운 예외를 발생시키는데, 이를 예외 연결 (exception chaining)이라 한다.
- 예외를 연결하는 이유는 여러 가지 예외를 하나의 큰 분류의 예외로 묶어서 다루기 위함이다.
- checked exception 을 unchecked exception 으로 포장(wrapping)하는데 유용하게 사용되기도 한다.

이러한 관점에서 Chained Exception 을 사용하는데,   
`Throwable` 클래스의 `initCause()` 메서드로 하위 예외를 등록하고,   
`getCause()` 메서드로 하위 예외를 불러 올 수 있다.

```java
public class ChainedExceptionTest {
    public static void main(String[] args) {
        ChainedExceptionTest test = new ChainedExceptionTest();

        try {
            test.chainedExceptionMethod(null);
        } catch (NumberFormatException e) {
            // 해당 stackTrace 가 출력 될 때 원인 예외까지 출력이 된다. (Caused by : 이후)
            e.printStackTrace();
            
            // 원인 예외만 stackTrace 로 찍을 수 있다.
            e.getCause().printStackTrace();
        }
    }

    public void chainedExceptionMethod(String parseStr) throws NumberFormatException {
        try {
            Integer.parseInt(parseStr);
        } catch (NumberFormatException e) {
            e.initCause(new NullPointerException("parseStr is null"));
            throw e;
        }
    }
}
```
```
java.lang.NumberFormatException: null
	at java.base/java.lang.Integer.parseInt(Integer.java:614)
	at java.base/java.lang.Integer.parseInt(Integer.java:770)
	at com.company.hg.exception.ChainedExceptionTest.chainedExceptionMethod(ChainedExceptionTest.java:20)
	at com.company.hg.exception.ChainedExceptionTest.main(ChainedExceptionTest.java:8)
Caused by: java.lang.NullPointerException: parseStr is null
	at com.company.hg.exception.ChainedExceptionTest.chainedExceptionMethod(ChainedExceptionTest.java:22)
	... 1 more
java.lang.NullPointerException: parseStr is null
	at com.company.hg.exception.ChainedExceptionTest.chainedExceptionMethod(ChainedExceptionTest.java:22)
	at com.company.hg.exception.ChainedExceptionTest.main(ChainedExceptionTest.java:8)
```


## 커스텀한 예외의 대한 시선

커스텀한 예외를 만드는 것에 있어서 긍정적인 시선도 있고, 부정적인 시선도 있는데

개발을 할 때 어떤걸 중요하게 여기냐에 따라 적용하는 코드가 달라질 수 있다는 걸   
느꼈던 부분이다.

### 사용자 정의 예외가 필요하다!

- 예외의 이름으로 확실한 정보 전달이 가능하다.
- 상세한 예외 정보를 전달 할 수 있다.
- 예외에 대한 응집도가 향상된다.
- 예외 발생 후처리가 용이하다.
- 예외 생성 비용을 절감한다.

### 표준 예외를 적극적으로 활용하자!

- 표준 예외의 메세지로도 충분히 의미를 전달 할 수 있다.
- 표준 예외를 사용하면 가독성이 높아진다.
- 일일히 예외 클래스를 만들다보면 지나치게 커스텀 예외가 많아질 수 있다.



> 웹문서
> - [The Java Tutorials(Exception)](https://docs.oracle.com/javase/tutorial/essential/exceptions/index.html)
> - [Java 예외(Exception) 처리에 대한 작은 생각](https://www.nextree.co.kr/p3239/)
> - [JAVA의 Exception(예외)이란 무엇인가](https://preamtree.tistory.com/111)
> - [custom exception을 언제 써야 할까?](https://woowacourse.github.io/javable/post/2020-08-17-custom-exception/)
