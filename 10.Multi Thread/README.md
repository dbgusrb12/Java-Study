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
                Thread.sleep(500);          // 쓰레드를 0.5 초 동안 멈춘다.
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
                Thread.sleep(500);      // 쓰레드를 0.5 초 동안 멈춘다.
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

[`Object` 클래스의 내장 메서드](https://github.com/dbgusrb12/Java-Study/tree/master/6.Inheritance#object-%ED%81%B4%EB%9E%98%EC%8A%A4%EC%9D%98-%EB%82%B4%EC%9E%A5-%EB%A9%94%EC%84%9C%EB%93%9C) 에 있는 메서드 중 쓰레드의 상태를 변화 시키는 메서드는   
`wait`, `notify`, `notifyAll` 이다.

```java
/**
 * Thread 클래스 내부의 State enum 을 추출하였다. 
 * 자세한 설명은 Thread 클래스의 State enum 을 보면 된다.
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

## `NEW`

쓰레드의 객체가 생성되고, 실행되기 전 상태를 의미한다.

쓰레드 객체가 `new` 키워드를 통해 생성은 됐지만, `start()` 메서드가   
호출 되지 않은 상태이다.

## `Runnable`

쓰레드가 실행중인 상태를 의미한다.

`start()` 메서드가 호출되거나, 멈춰있던 쓰레드가 다시 실행 될 때의 상태이다.

## `BLOCKED`

사용하고자 하는 쓰레드 객체의 락이 풀릴때까지 기다리는 상태를 의미한다.

쓰레드가 실행 중지 상태이고, 모니터 락이 풀리기를 기다리는 상태이다.   
쓰레드의 동기화와 관련된 개념이고, 동기화에 대한 설명은 뒤에서 하겠다.

## `WAITING`

쓰레드가 대기중인 상태를 의미한다.

이 상태로 변하는 시점은

`Object.wait()`,   
`Thread.join()`,   
`LockSupport.park()`

위의 메서드 들이 호출된 시점이다.

## `TIMED_WAITING`

위의 `WAITING` 상태와 같은 상태이다.

다른 점은 특정 시간만큼만 대기하고, 시간이 지나면 다시 쓰레드를 실행한다.

이 상태로 변하는 시점은

`Thread.sleep(long millis)`,   
`Object.wait(long timeoutMillis)`,   
`Object.wait(long timeoutMillis, int nanos)`,   
`Thread.join(long millis)`,   
`LockSupport.parkNanos(Object blocker, long nanos)`,   
`LockSupport.parkUntil(Object blocker, long deadline)`

위의 메서드 들이 호출된 시점이다.

## `TERMINATED`

쓰레드가 종료 된 상태를 의미한다.

쓰레드가 주어진 역할을 다 수행했거나, 강제로 종료를 했을 때 의 상태이다.

# 쓰레드의 우선 순위

쓰레드의 우선 순위는 `Thread` 클래스의 필드 값과 메서드를 보면 알 수 있다.

```java
/**
 * Thread 클래스의 일부분을 가져온 클래스이다.
 */
public class Thread implements Runnable {
  
    private int priority;
    
    public static final int MIN_PRIORITY = 1;
  
    public static final int NORM_PRIORITY = 5;
  
    public static final int MAX_PRIORITY = 10;

    public final void setPriority(int newPriority) {
        ThreadGroup g;
        checkAccess();
        if (newPriority > MAX_PRIORITY || newPriority < MIN_PRIORITY) { 
            throw new IllegalArgumentException();
        }
        if ((g = getThreadGroup()) != null) {
            if (newPriority > g.getMaxPriority()) {
                newPriority = g.getMaxPriority();
            }
            setPriority0(priority = newPriority);
        }
    }

    public final int getPriority() {
        return priority;
    }
  
    public final void checkAccess() {
        SecurityManager security = System.getSecurityManager();
        if (security != null) {
            security.checkAccess(this);
        }
    }
}
```

위의 예제 코드를 보면,

`static final` 로 선언된 3개의 필드값을 볼 수 있는데,   
이 3개의 값은 모든 쓰레드에서 공통으로 쓰이는 필드값이다.

먼저 `MIN_PRIORITY` 는 쓰레드가 가질 수 있는 우선 순위의 최소 값을 나타낸 것이며,   
`MAX_PRIORITY` 는 쓰레드가 가질 수 있는 우선 순위의 최대 값을 나타낸 것이고,   
`NORM_PRIORITY` 는 쓰레드가 생성 될 때 생기는 우선 순위의 기본 값을 나타낸 것이다.

해당 쓰레드의 우선 순위는 `priority` 의 값을 통해 알 수 있고,   
`priority` 는 `getPriority()` , `setPriority()` 메서드를 통해 수정하고, 조회 할 수 있다.

쓰레드의 우선 순위는 절대값이 아닌 상대적인 값이다.

우선 순위가 10인 쓰레드가 우선 순위가 1인 쓰레드보다 10배 빠르게 수행 되는 것이 아닌,   
실행 큐에 좀 더 많이 포함되어, 작업 시간을 더 많이 할당 받는 것이다.

# Main Thread

Java 플랫폼에서는 메모리 관리, 신호 처리와 같은 작업을 수행하는   
**시스템 쓰레드(System Thread)** 들은 하나 이상의 멀티 쓰레드로 이루어져 있지만,

실제로 개발자 관점에서 보면,   
**메인 쓰레드(Main Thread)** 라고 하는 하나의 쓰레드로부터 프로그램이 실행된다.

이 메인 쓰레드에서 쓰레드를 추가 할 수 있고, 추가한 쓰레드를 관리 할 수 있다.

그리고 이 Main Thread 의 우선 순위는 5이다. (기본값)

# 동기화 (Synchronize)

프로세스 내부의 쓰레드들은 해당 프로세스의 리소스와 메모리를 공유하여 통신한다.

이러한 통신 방식은 효율적이지만, 쓰레드 간섭 이나 메모리 일관성 오류가 발생 할 수 있다.

그래서 이런 에러들을 방지하는데 필요한 개념이 바로 동기화 이다.

Java 에서 이런 동기화를 하는 방법은 3가지가 있는데,

첫번째는 `synchronized` 키워드를 사용하는 방법,   
두번째는 `Atomic` 클래스를 사용하는 방법,   
마지막으로 세번째는 `volatile` 키워드를 사용하는 방법이다.

```java
public class Counter {
    private int count = 0;

    public void increment() {
        count++;
    }

    public void decrement() {
        count--;
    }

    public int getCount() {
        return count;
    }
}

public class CountThread extends Thread {

    private Counter counter;
    private boolean flag;
  
    public CountThread(Counter counter, boolean flag) {
        this.counter = counter;
        this.flag = flag;
    }
  
    @Override
    public void run() {
        for (int i = 0; i < 10000; i++) {
            if (flag) {
                counter.increment();
            } else {
                counter.decrement();
            }
        }
    }
}

public class SynchronizeTest {

    public static void main(String[] args) {
        Counter counter = new Counter();
    
        Thread thread1 = new CountThread(counter, true);
        Thread thread2 = new CountThread(counter, false);
    
        thread1.start();
        thread2.start();
    
        try {
            thread1.join(); // join 메서드를 사용하여 실행 결과 출력을 쓰레드가 다 실행 된 후로 바꿔준다.
            thread2.join();
            System.out.println(counter.getCount()); // 실행 결과 출력
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```
```
-523
```

위 코드를 보면 0이 아닌 계속 다른 값이 나오는 것을 확인 할 수 있다.   
동시에 같은 데이터를 사용해 읽고 쓰다 보면 쓰레드의 간섭이 심해져 원하는 데이터가 나오지 않는다.

## `synchronized` 키워드

동기화를 하기 위해 필요한 키워드로, 메서드 선언부에 들어가거나,   
메서드 구현부에서 원하는 로직을 블럭(`{}`)으로 묶어 사용 할 수도 있다.

### synchronized method

`synchronized` 키워드가 메서드 선언부에 들어갔을 때 synchronized method 라고 한다.

synchronized method 는 쓰레드가 해당 메서드로 들어오는 순간   
인스턴스에 lock 을 걸고, 메서드에서 빠져나가는 순간 lock 을 해제한다.

lock 이 걸린 인스턴스의 메서드는 다른 쓰레드에서 접근 할 수 없다.   

다른 쓰레드에선 lock 이 해제 될 때까지 기다린 후 해제되면 lock 을 걸고,   
작업을 실행한다.

synchronized method 에서 lock 이 걸리는 주체는 해당 메서드 뿐만 아닌   
해당 메서드를 가지고있는 인스턴스도 lock 에 걸린다.

```java
public class Counter {
    private int count = 0;
  
    public synchronized void increment() {  // 메서드 선언부에 synchronized 키워드를 붙인다.
        count++;
    }
  
    public synchronized void decrement() {
        count--;
    }
  
    public int getCount() {
        return count;
    }
}

public class CountThread extends Thread {

    private Counter counter;
    private boolean flag;
  
    public CountThread(Counter counter, boolean flag) {
        this.counter = counter;
        this.flag = flag;
    }
  
    @Override
    public void run() {
        for (int i = 0; i < 10000; i++) {
            if (flag) {
                counter.increment();
            } else {
                counter.decrement();
            }
        }
    }
}

public class SynchronizeTest {

    public static void main(String[] args) {
        Counter counter = new Counter();
    
        Thread thread1 = new CountThread(counter, true);
        Thread thread2 = new CountThread(counter, false);

        thread1.start();
        thread2.start();
  
        try {
            thread1.join(); // join 메서드를 사용하여 실행 결과 출력을 쓰레드가 다 실행 된 후로 바꿔준다.
            thread2.join();
            System.out.println(counter.getCount()); // 실행 결과 출력
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```
```
0
```

### synchronized block

`synchronized` 키워드가 메서드 구현부에 들어가는 것을 synchronized block 이라고 한다.

synchronized method 가 해당 인스턴스와 메서드 모두 lock 을 거는거라면,   
synchronized block 은 매개변수에 해당하는 인스턴스와, 해당 블럭(`{}`) 내부의 로직을   
잠궈준다.

위의 예제를 synchronized block 구문을 사용해 바꾼다면,

```java
public class Counter {
    private int count = 0;
  
    public void increment() {  // 메서드 선언부에 synchronized 키워드를 붙인다.
        synchronized (this) {
            count++;
        }
    }
  
    public void decrement() {
        synchronized (this) {
            count--;
        }
    }
  
    public int getCount() {
        return count;
    }
}

public class CountThread extends Thread {

    private Counter counter;
    private boolean flag;
  
    public CountThread(Counter counter, boolean flag) {
        this.counter = counter;
        this.flag = flag;
    }
  
    @Override
    public void run() {
        for (int i = 0; i < 10000; i++) {
            if (flag) {
                counter.increment();
            } else {
                counter.decrement();
            }
        }
    }
}

public class SynchronizeTest {

    public static void main(String[] args) {
        Counter counter = new Counter();
    
        Thread thread1 = new CountThread(counter, true);
        Thread thread2 = new CountThread(counter, false);

        thread1.start();
        thread2.start();
  
        try {
            thread1.join(); // join 메서드를 사용하여 실행 결과 출력을 쓰레드가 다 실행 된 후로 바꿔준다.
            thread2.join();
            System.out.println(counter.getCount()); // 실행 결과 출력
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

이런 식으로 바꿔줄 수 있다.

synchronized block 의 매개변수와 블럭을 잠그는 구문이기 때문에,   
매개변수에 `this` 가 들어가고, synchronized block 구문 외의 로직이   
없다면 synchronized method 와 같은 기능을 한다.

## `Atomic` 클래스

`Atomic` 클래스를 이용해 동기화 처리를 할 수 있다.

`synchronized` 의 단점은 해당 로직이 다른 쓰레드의 침범을 받지 않아도   
일단 lock 을 걸고 시작하기 때문에 비용이 비싸다는 것이다.

자원 경합이 무조건 일어난다는 가정 하에 독점적으로 자원을 잠그는 행위를   
비관적 잠금(Pessimistic locking) 이라고 한다.

반면에 `Atomic` 클래스는 사용 할 때 내부적으로 CAS(Compare And Swap) 알고리즘을 통한   
낙관적 잠금 연산을 지원한다.

쓰레드 사이에 같은 자원으로 경쟁 할 일이 잦지 않다는 가정 하에 lock 을   
미리 걸지 않고, 문제가 발생할때만 자원을 잠그는 행위를   
낙관적 잠금(Optimistic Lock)이라고 한다.

무조건 lock 을 거는 것 보다 문제를 감지하는 것이 비용이 더 적게 들고,   
효율적이라고 볼 수 있다.

`Atomic` 클래스들은 일종의 Wrapping 클래스 이고,   
`java.util.concurrent.atomic` 패키지에 정의되어 있다.

primitive type, reference type 모두 지원하며, 다양한 종류가 있기 때문에   
전부 설명을 하진 않고, `AtomicInteger` 클래스만 예시로 작성하였다.

```java
public class AtomicCounter {
    private AtomicInteger count = new AtomicInteger(0);
  
    public void increment() {
        count.incrementAndGet();
    }
  
    public void decrement() {
        count.decrementAndGet();
    }
  
    public int getCount() {
        return count.get();
    }
}

public class AtomicThread extends Thread {
    private AtomicCounter counter;
    private boolean flag;
  
    public AtomicThread(AtomicCounter counter, boolean flag) {
        this.counter = counter;
        this.flag = flag;
    }
  
    @Override
    public void run() {
        for (int i = 0; i < 10000; i++) {
            if (flag) {
                counter.increment();
            } else {
                counter.decrement();
            }
        }
    }
}

public class AtomicTest {
    public static void main(String[] args) {
        AtomicCounter counter = new AtomicCounter();

        Thread thread1 = new AtomicThread(counter, true);
        Thread thread2 = new AtomicThread(counter, false);

        thread1.start();
        thread2.start();

        try {
            thread1.join(); // join 메서드를 사용하여 실행 결과 출력을 쓰레드가 다 실행 된 후로 바꿔준다.
            thread2.join();
            System.out.println("result is " + counter.getCount()); // 실행 결과 출력
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

## `volatile` 키워드

`volatile` 키워드는 변수의 변경에 대한 가시성을 보장하는 키워드 이다.

멀티 쓰레드 어플리케이션은 성능상의 이유로 Main Memory 에서 변수를 읽고,   
CPU cache 에 저장하게 된다.

이 때 여러 쓰레드들이 같은 변수를 Read 할 때 CPU cache 에 저장한 값이   
특정 쓰레드에서 변경을 하면 데이터 불일치가 생기기 때문에,   
변수를 애초에 Main Memory 에서 직접 읽고, 쓸 수 있게 선언을 해주는 키워드이다.

```java
public class ShareObject {
    public int count = 0;
}
```

`count` 라는 변수를 Thread1, Thread2 에서 읽어온다면,   

- Main Memory : `count = 0`
- Thread1 의 CPU cache : `count = 0`
- Thread2 의 CPU cache : `count = 0`

Thread1 에서 `count` 변수의 값을 바꾼다면,

- Main Memory : `count = 0`
- Thread1 의 CPU cache : `count = 5`
- Thread2 의 CPU cache : `count = 0`

이런 상황에서, Thread1 의 CPU cache 에 존재하는 `count` 변수의   
변화를 언제 적용하는지 알 수 없는 상황에서 Thread2가 `count` 변수를   
Read 한다면 `count`의 값은 0일수 밖에 없다.

하지만 `volatile` 키워드를 붙인다면,

```java
public class ShareObject {
    public volatile int count = 0;
}
```
- Main Memory : `count = 0`
- Thread1 의 count : `count = 0`
- Thread2 의 count : `count = 0`

Thread1 에서 `count` 변수 값 변경
- Main Memory : `count = 5`
- Thread1 의 count : `count = 5`
- Thread2 의 count : `count = 5`

Main Memory 에서 직접 READ, WRITE 하기 때문에 값이 일정 할 수 있다.

# 데드락 (Deadlock)

데드락(Deadlock) 이란, 둘 이상의 쓰레드가 서로의 lock 을 획득 하기 위해   
영원히 대기 상태에 빠지는 현상을 말한다.

예를 들어,   
A 라는 쓰레드가 A-1 이라는 lock 을 가지고 있고,   
B 라는 쓰레드가 B-1 이라는 lock 을 가지고 있다고 했을 때,

쓰레드 A 가 A-1 을 가지고 있는 상태로 B-1 을 획득하기 위해 대기하고,   
쓰레드 B 가 B-1 을 가지고 있는 상태로 A-1 을 획득하기 위해 대기할 때

서로 lock 을 획득 하지 못한 채로 교착 상태(Deadlock) 에 빠지게 된다.

허리를 숙여 인사를 하면 상대방이 대답을 할 때 까지 아무말도 못하는 세상이 있다고 가정하자.   
이런 세상에서 서로 동시에 허리를 숙여 인사를 하면 어떻게 될까?

```java
public class StrangeWorld {
    static class Person {
        private final String name;
    
        public Person(String name) {
            this.name = name;
        }
    
        public String getName() {
            return this.name;
        }
    
        public synchronized void bow(Person bower) {
            System.out.println(bower.getName() + " 이(가) " + this.name + "한테 허리를 숙여 인사를 한다.");
            bower.bowBack(this);
        }
    
        public synchronized void bowBack(Person bower) {
            System.out.println(this.name + "이(가) " + bower.getName() + " 의 인사를 받는다.");
            System.out.println(this.name + " : 네! 안녕하세요~");
        }
    }

    public static void main(String[] args) {
        final Person hyunGyu = new Person("hyunGyu");
        final Person nick = new Person("nick");
    
        Thread thread = new Thread(new Runnable() {
            @Override
            public void run() {
                hyunGyu.bow(nick);
            }
        });
    
        Thread thread2 = new Thread(new Runnable() {
            @Override
            public void run() {
                nick.bow(hyunGyu);
            }
        });
        thread.start();
        thread2.start();
    }
}
```
```
hyunGyu 이(가) nick한테 허리를 숙여 인사를 한다.
nick 이(가) hyunGyu한테 허리를 숙여 인사를 한다.
```

허리를 숙인 채로 평생 아무것도 못한채로 교착 상태에 빠지고,   
이 상황에서 다른 사람이 와서 인사를 한다면 다른 사람 역시 교착 상태에 빠질 것이다.

이러한 상황을 **데드락(Deadlock)** 이라고 한다.

> 웹문서
> - [The Java Tutorials(Concurrency)](https://docs.oracle.com/javase/tutorial/essential/concurrency/procthread.html)
> - [스레드의 개념](http://tcpschool.com/java/java_thread_concept)
> - [[Java] Thread(스레드)에 관한 고찰 - 스레드 관련 코드를 어떻게 짜야할까](https://pjh3749.tistory.com/268)
> - [자바 volatile 키워드](https://parkcheolu.tistory.com/16)