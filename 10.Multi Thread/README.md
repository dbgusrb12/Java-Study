Multi Thread
===

# 멀티 쓰레드 (Multi Thread) 란?

멀티 쓰레드란, 하나의 프로세스 내에 여러개의 쓰레드가 동시에 작업을 수행 하는   
것을 말한다.


## 프로세스 (Process) 란?

사용자가 작성한 프로그램이 운영체제에 의해 메모리 공간을 할당받아 실행 중인 것을 말한다.   
이런 프로세스는 독립적인 런타임 리소스, 사용되는 데이터의 메모리 그리고 쓰레드로 구성된다.

프로세스는 그 자체로 독립적인 실행 환경이 있어서, 프로그램 또는 응용 프로그램이라고   
부르기도 한다.

하지만 사용자 입장에서 하나의 응용프로그램 이라고 생각한 것들이 사실은 여러 프로세스들로   
이루어진 경우도 있어 완전히 하나의 프로그램이다! 라고 보기엔 조금 무리가 있을 수 있다.

JVM 은 대부분 단일 프로세스로 실행하며, `ProcessBuilder` 객체를 이용해 프로세스를   
추가 하여 사용 할 수 있고, 이런 방식을 멀티 프로세싱(Multi-processing) 이라고 한다.   
~~이 부분은 좀 더 공부가 필요하다.~~

## 쓰레드 (Thread) 란?

쓰레드는 **경량 프로세스** 라고도 불린다.   
프로세스와 쓰레드 모두 실행 환경을 가지고 있지만, 새 프로세스를 만드는 것에 비해   
새 쓰레드를 만드는 것이 더 적은 리소스를 요구하기 때문에 훨씬 가볍다.

쓰레드란 프로세스 내에서 실제로 작업을 수행하는 주체이다.   
모든 프로세스에는 한개 이상의 쓰레드가 존재하고, 두개 이상의 쓰레드를 가지는   
프로세스를 멀티 쓰레드 프로세스 (multi-threaded process) 라고 한다.

프로세스 내의 쓰레드들은 해당 프로세스의 리소스와 메모리를 공유하기 때문에   
효율적이지만, 반대로 생각해보면 공유를 하기 때문에 통신에 문제가 생길 수 있다.

ex) 같은 데이터를 다른 쓰레드에서 동시에 읽어와 서로 다른 값으로 바꾸면 후에 처리되는 값이 적용된다.


# `Thread` 클래스와 `Runnable` 인터페이스

Java 에서 쓰레드를 생성하는 방법은 두가지가 있는데,

첫번째는 `Thread` 클래스를 상속받는 방식이고,   
두번째는 `Runnable` 인터페이스를 구현하는 방식이다.

결국 `Thread` 클래스 자체가 `Runnable` 인터페이스를 구현한 클래스라서,   
어느 방식을 쓰던지 상관은 없지만,   
Java 에서 클래스는 다중 상속이 불가능하기 때문에, 다른 클래스를 상속받아야 하는   
상황이면 `Runnable` 인터페이스를 구현하고, 그렇지 않으면 아무거나 써도 상관 없다.

`Thread` 클래스를 상속받으면 다른 클래스를 상속받을 수 없으므로,   
일반적으로 `Runnable` 인터페이스를 구현하는 방법으로 스레드를 생성한다고 한다.

하지만 Java 에서 `Thread` 클래스 내부에 쓰레드 관리를 위한 메서드들을   
정의해놨기 때문에 유용하게 사용할 수 있는 클래스이다.   
어떤 방식을 사용 할 지는 개발하는 어플리케이션에 대해 여러가지를 고려하여 적용하면   
될거 같다.

Oracle 공식 가이드에 따르면,   
어플리케이션에서 `Thread` 객체를 생성 할 때 사용하는 기본 전략이 두가지가   
있다고 한다.

- 쓰레드의 생성과 관리를 직접 제어해야 될 경우 어플리케이션이 비동기 작업을 할 때   
  `Thread` 클래스를 인스턴스화 하여 사용한다.
- 쓰레드 관리를 추상화 해야 될 경우 `Executor` 인터페이스를 사용한다.

본문에선 이 두가지 전략 중 첫번째 방식을 위주로 볼 것이며, 추후 시간이 될 때   
`Executor` 인터페이스를 사용하는 전략에 대해 공부하고, 추가하곘다.

```java
/**
 * Thread 클래스를 상속 받아서 쓰레드를 생성하는 클래스
 */
public class ExtendsThread extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(getName());  // Thread 클래스의 getName() 메서드 호출 현재 쓰레드의 이름을 반환한다.
            try {
                Thread.sleep(50);           // 쓰레드를 0.5 초 동안 멈춘다.
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

/**
 * Runnable 인터페이스를 구현해서 쓰레드를 생성하는 클래스
 */
public class ImplementsRunnable implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            // Runnable 인터페이스는 추상 메서드인 run() 메서드 밖에 없기 때문에 현재 쓰레드를 가져와 이름을 출력해야한다.
            System.out.println(Thread.currentThread().getName());
            try {
                Thread.sleep(50);       // 쓰레드를 0.5 초 동안 멈춘다.
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

public class ThreadTest {
    public static void main(String[] args) {
        Thread extendsThread = new ExtendsThread();                         // Thread 클래스를 상속 받았을 때
        Thread implementsRunnable = new Thread(new ImplementsRunnable());   // Runnable 인터페이스를 구현 했을 때

        // 쓰레드를 생성하고, start 메서드를 통해 쓰레드를 실행시킨다.
        extendsThread.start();
        implementsRunnable.start();
    }
}
```
```
Thread-0
Thread-1
Thread-1
Thread-0
Thread-1
Thread-0
Thread-1
Thread-0
Thread-1
Thread-0
```

위 코드의 결과를 보면 알 수 있듯, 컴퓨터의 성능에 따라 두 개의 쓰레드가 처리하는   
속도가 다를 수 있기 때문에, 쓰레드 두개를 실행하고, 0.5초씩 멈춘다고 하더라도   
순서대로 실행되지 않을 수 있다.

# 쓰레드의 상태

`Thread` 클래스 내부에 쓰레드의 상태에 대한 상수가 나열되어 있는데,   
`enum` 으로 되어 있으며, `Thread.State` 로 정의 되어있다.

```java
/**
 * Thread 클래스 내부의 State enum 을 추출하였다.
 */
public enum State {
    NEW,
    RUNNABLE,
    BLOCKED,
    WAITING,
    TIMED_WAITING,
    TERMINATED;
}
```



# Main Thread

Java 플랫폼에서는 메모리 관리, 신호 처리와 같은 작업을 수행하는   
**시스템 쓰레드(System Thread)** 들은 하나 이상의 멀티 쓰레드로 이루어져 있지만,

실제로 개발자 관점에서 보면,   
**메인 쓰레드(Main Thread)** 라고 하는 하나의 쓰레드로부터 프로그램이 실행된다.

이 메인 쓰레드에서 쓰레드를 추가 할 수 있고, 추가한 쓰레드를 관리 할 수 있다.



> 웹문서
> - [The Java Tutorials(Concurrency)](https://docs.oracle.com/javase/tutorial/essential/concurrency/procthread.html)
> - [스레드의 개념](http://tcpschool.com/java/java_thread_concept)