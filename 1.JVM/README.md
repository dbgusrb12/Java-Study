JVM (Java Virtual Machine)
==========================

# **JVM** 이란

_Java Bytecode_ 를 OS에 맞게 해석해주는 역할을 한다.

## **바이트코드** 란 

*특정 하드웨어가 아닌 가상 컴퓨터에서 돌아가는 실행 프로그램을 위한 이진 표현법* 이다.   
하드웨어가 아닌 *소프트웨어에 의해 처리* 되기 때문에 보통 기계어보다 더 _추상적_ 이라고 한다.   

## 컴파일 하는 방법
```
$ javac main.java
```
*.class* 파일을 **자바 바이트코드**라고 하는데,   
_Java compiler_ 를 통해 *.java* 파일을 *.class* 파일로 변환 시켜준다.   
(이렇게 변환 시키는것을 컴파일 한다고 한다.)   

## 실행 하는 방법
```
$ java main
```
source code(##.java) => javac compile (`$ javac ##.java`) => bytecode(##.class)로 변환 => JVM => OS에 맞게 binary code로 변환 => 프로그램 실행

## Java 프로그램 실행 과정

>   1. 프로그램이 실행되면 JVM은 OS로부터 프로그램이 필요로 하는 메모리를 할당받고,   
>      할당받은 메모리를 용도에 따라 여러 영역으로 나누어 관리한다.
>   2. 자바 컴파일러(javac)가 자바 소스 코드(.java)를 읽어들여 Java Bytecode(.class)로 변환시킨다.
>   3. Class Loader를 통해 class파일들을 JVM으로 로딩한다.
>   4. 로딩된 class파일들은 Execution engine을 통해 해석된다.
>   5. 해석된 Bytecode는 Runtime Data Areas에 배치되어 실질적인 수행이 이루어진다.   
>
>   이러한 실행과정 속에서 JVM은 필요에 따라 Thread Synchronization과 GC같은 관리작업을 수행한다.

때문에 JAVA는 OS에 종속적이지 않고, 어느 디바이스든 java 파일만 있다면 JVM 위에서 실행 할 수 있다.  
~~하지만 JVM은 OS에 맞게 설치해야 된다고 한다.~~


# **JVM** 구성 요소

## Class Loader

Java compiler가 컴파일한 클래스(.class 파일)를 로드하는 모듈.   
Runtime 시 동적으로 클래스를 로드한다.   

클래스를 처음으로 참조할 때, 해당 클래스를 로드하고 링크해주는 역할을 수행한다.   
jar파일 내 저장된 클래스들을 JVM 위에 올리고 사용하지 않는 클래스는 삭제한다.

## Execution Engine

클래스 로더가 로드한 클래스(bytecode 로 이루어져 있다.)들을 해석하여 Binary Code로 변환하고, 실행한다.

초기 JVM이 나왔을 당시에는 한줄씩 해석하는 Interpreter 방식 이였기 때문에 속도가 느리다는 단점이 있었지만   
JIT(Just In Time) Compiler 방식을 도입해 이 단점을 해결했다.

### **JIT Compiler** 란

Interpreter 방식의 단점을 보완하기 위해 도입된 컴파일러이다.   
Bytecode 전체를 컴파일하여 Native Code로 변경하고 캐시에 보관해   
한번 컴파일된 코드는 빠르게 수행할 수 있게 된다.   

JIT 컴파일러가 컴파일하는 과정은 bytecode를 인터프리팅 하는 것보다 훨씬 오래 걸리므로 한번만 실행되는   
코드라면 컴파일하지 않고 인터프리팅 하는 것이 유리하다.

따라서 JIT Compiler를 사용하는 JVM들은 Interpreter 방식을 사용하다 내부적으로 해당 메소드가 얼마나   
자주 수행되는지 체크하고, 일정 기준를 넘어가면 JIT Compiler 방식으로 실행된다.


## Runtime Data Area

JVM이 프로그램을 수행하기 위해 OS로부터 별도로 할당받은 메모리 공간이다.

크게 5가지 영역으로 나눌 수 있는데   
*PC Register, JVM Stack, Native Method Stack* 이 세 영역은 Thread 마다 생성 되며,   
*Method Area, Heap* 이 두 영역은 모든 Thread에게 공유된다.

- **PC Register**

    Thread가 시작될 때 생성되며, Thread가 어떤 명령어로 실행되어야 할지에 대한 기록을 하는 부분이다.
    
    현재 수행중인 JVM명령의 주소를 갖는다.

- **JVM Stack**

    프로그램 실행 과정에서 임시로 할당되었다가 메소드를 빠져나가면 바로 소멸되는 
    특성의   
    데이터를 저장하기 위한 영역이다.

    각종 형태의 변수나 임시 데이터, Thread나 메소드의 정보를 저장한다. 메소드 호출    
    시마다 각각의 스택 프레임이 생성된다.

    메소드 수행이 끝나면 프레임 별로 삭제를 하고, 메소드 안아서 사용되는 값들을 저장하고,   
    호출된 메소드의 매개변수, 지역변수, 리턴 값 및 연산 시 일어나는 값들을 임시로 저장한다.

- **Native Method Stack**

    자바 프로그램이 컴파일되어 생성되는 Bytecode가 아닌 실제 실행할 수 있는 기계어로   
    작성된 프로그램을 실행시키는 영역이다.

    JAVA 언어가 아닌 다른 언어로 작성된 코드를 위한 공간이고, JAVA Native Interface를   
    통해 Bytecode로 전환하여 저장한다.

    JAVA가 아닌 일반 프로그램처럼 커널이 스택을 잡아 독자적으로 프로그램을 실행시키는 영역   
    으로, 이 부분을 통해 C code를 실행시켜 Kernel에 접근 가능하다.

- **Method Area**

    *Class Loader* 가 클래스를 처음 메모리 공간에 올릴 때 초기화되는 대상을 저장하기 위한 메모리 공간이다.

    Java 프로그램은 main 메소드의 호출에서부터 계속된 메소드의 호출로 흐름을 이어가기 때문에   
    Method Area에 올라가게 되는 메소드의 Bytecode는 프로그램의 흐름을 구성하는 Bytecode이다.

    JVM이 읽어들인 ***클래스와 인터페이스에 대한 Runtime 상수 풀, 멤버 변수(필드값),   
    클래스변수(Static 변수), 생성자, 메소드*** 를 저장하는 영역이다.

    각 클래스와 인터페이스의 상수, 메소드와 필드에 대한 모든 레퍼런스를   
    **Runtime Constant Pool**이라는 별도의 관리 영역에 저장하여 중복을 막는 역할을 수행하고,   
    JVM은 이곳을 통해 해당 메소드나 필드의 실제 메모리상 주소를 찾아 참조한다.


- **Heap**

    객체를 저장하는 가상 메모리 공간이다.

    new 연산자로 생성된 객체와 배열을 저장한다. Method Area 영역에 올라온 클래스들만 객체로   
    생성할 수 있다.

    Heap 영역은 3가지 영역으로 나눌 수 있는데,

    + New / Young Generation

        Eden이란 영역에 객체들이 최초로 생성되고, Survivor 0/ Survivor 1에서 참조되는 객체들이   
        저장된다. 시간이 지나 우선순위가 낮아지면 Old로 옮겨진다.   
        이때 Minor GC가 발생한다.
    
    + Old Generation

        New / Young에서 저장되었던 객체 중 오래된 객체가 넘어와 저장된다.   
        여기서 객체가 사라질 때 Major GC(Full GC)가 발생한다.

    + Permanent Generation

        생성된 객체 정보의 주소값이 저장된다.   
        *Class Loader* 에 의해 로드되는 클래스, 메소드 등에 대한 meta 정보가 저장되는 영역으로,   
        JVM에 의해 사용된다.   
        ~~JDK8 버전 부터는 이 영역 대신 Metaspace라는 영역이 추가됐다고 한다.(추가 공부 필요)~~

# **JDK**와 **JRE**의 차이

## **JRE** 란 

Java Runtime Environment 의 약자로, 자바 프로그램을 실행시켜주는 환경을 구성해주는 도구

## **JDK** 란

Java Development Kit 의 약자로, 자바 개발을 위해 필요한 툴킷을 제공하는 도구.   
JRE가 포함 되어있다.


# GC (Garbage Collection)

## **GC**란

Garbage Collection이란 말 그대로 Garbage를 모으는 작업으로,   
이미 할당된 메모리에서 더이상 *사용하지 않는 메모리* (Heap과 Method Area에서   
더이상 사용되지 않는 Object) 를 해제하는 행동을 의미한다.

## Stop-The-World

GC를 실행하기 위해 JVM이 자체적으로 애플리케이션의 실행을 멈추는 것을 의미한다.   
stop-the-world가 발생하면 GC를 실행하는 thread 외의 나머지 thread들은 모두 작업을 멈춘다.   
GC 튜닝을 하는 대부분의 경우는 이 stop-the-world의 시간을 단축시키는 것이다.

## Weak Generational Hypothesis

GC가 작업을 할 때 더 이상 필요 없는 Object를 찾는 전제 조건.
- 대부분의 객체는 금방 접근 불가능 상태(unreachable)가 된다.
- 오래된 객체에서 젊은 객체로의 참조는 아주 적게 존재한다.

## Young 영역의 구성

객체가 생성되면 제일 먼저 담기는 Young 영역은 3가지 영역(Eden 영역, Survivor 영역 2개)으로 나뉜다.

- 새로 생성한 대부분의 객체는 Eden 영역에 위치한다.
- Eden 영역에서 처음 GC가 발생 한 후 살아남은 객체는 Survivor 영역 중 하나로 이동한다.
- 그 다음 Eden 영역에서 다시 GC가 발생 하면 이미 살아남은 객체들이 있는 Survivor 영역으로 계속 쌓인다.
- 하나의 Survivor 영역이 가득 차게 되면 그중 살아남은 객체들을 다른 Survivor 영역으로 넘기고 기존 Survivor 영역을 비운다.
- 이 과정을 반복하고, 계속 살아남아 있는 객체들은 Old 영역으로 이동한다.

## Old 영역에 대한 GC

Old 영역은 기본적으로 데이터가 가득 차게 되면 GC를 실행한다.   
JDK7을 기준으로 5가지 방식이 있는데,

### **Serial GC**

mark-sweep-compact 라는 알고리즘을 사용하는 GC로, Old 영역에 살아있는 객체를 식별(mark)하고,   
Heap의 앞부분부터 확인해 살아있는 객체만 남긴 후(Sweep), 각 객체들이 연속되게 쌓이게끔 Heap의 가장   
앞부분부터 채워서 객체가 존재하는 부분과 없는 부분으로 나눈다.(Compaction)   

메모리가 작거나 CPU 코어 개수가 적을 때 적합한 방식이다.

### **Parallel GC**(Throughput GC)

Serial GC와 기본 알고리즘이 같지만, Serial GC는 GC를 처리하는 Thread가 하나이고, Parallel GC   
는 GC를 처리하는 Thread가 여러개이다.

메모리가 충분하고, 코어의 개수가 많을 때 유리하다.

### **Parallel Old GC**(Parallel Compacting GC)

JDK 5 update 6부터 제공한 방식으로, Parallel GC와 다르게 Old 영역 의 알고리즘이   
Mark-Summary-Compaction 단계를 거친다.   
Summary 단계는 앞서 GC를 수행한 영역에 대해서 별도로 살아 있는 객체를 식별한다는 점에서   
Mark-Sweep-Compaction 알고리즘의 Sweep 단계와 다르며, 약간 더 복잡한 단계를 거친다.

### **Concurrent Mark & Sweep GC**(CMS GC)

CMS 방식은 기존 Parallel GC와 같이 멀티 Thread로 운영되지만, Minor GC를 수행하는 알고리즘이 다르고,   
애플리케이션이 구동되는 중에 백그라운드에서 Thread 를 따로 만들어서 GC를 수행하기 때문에 stop-the-world   
시간이 짧다.

Initial Mark 단계, Concurrent Mark 단계, Remark 단계, Concurrent Sweep 단계를 거치는데,   
Initial Mark 단계와 Remark 단계에서만 stop-the-world 가 발생하고, 나머지 두 단계는 애플리케이션의   
다른 Thread가 실행되고 있는 상황에서 진행한다.

1. Initial Mark 단계에서는 Class Loader에서 가장 가까운 객체 중 살아있는 객체만 찾고 끝낸다.
2. Concurrent Mark 단계에서는 방금 살아있다고 확인한 객체에서 참조하고 있는 객체들을 따라가면서   
확인한다.
3. Remark 단계에서는 Concurrent Mark 단계에서 새로 추가되거나 참조가 끊긴 객체를 확인한다.
4. Concurrent Sweep 단계에서는 이전 단계들에서 Mark 된 정보들을 기반으로 쓰레기를 정리하는 작업을   
실행한다.

CMS GC는 별도의 Compaction 단계가 존재하지 않기 때문에, Memory fragmentation이 발생할 수 있다.   
조각난 메모리가 많아 Compaction 작업을 실행하면 다른 GC 방식의 stop-the-world 시간보다 길기 때문에,   
Compaction 작업이 얼마나 자주, 오랫동안 수행되는지 확인해야한다.


### **G1(Garbage First) GC**

기존 GC와는 다르게 young / old 영역으로 나뉘지 않고, Region 이라는 각각의 영역으로 나뉘어 지고,   
Region 별로 순차적으로 GC 작업이 진행된다. Garbage First 라는 의미는 Garbage로만 꽉 찬    
Region부터 Collection을 시작한다는 의미로 발견 되자 마자 즉각 정리한다.


> 웹문서
> - [자바가상머신, JVM(Java Virtual Machine)이란 무엇인가?](https://asfirstalways.tistory.com/158)
> - [JVM이란?](https://medium.com/@lazysoul/jvm-%EC%9D%B4%EB%9E%80-c142b01571f2)
> - [Java Garbage Collection](https://d2.naver.com/helloworld/1329)
> - [Java - Garbage Collection(GC,가비지 컬렉션) 란?](https://coding-start.tistory.com/206)
> - [가비지 컬렉터(GC)에 대하여](https://velog.io/@litien/%EA%B0%80%EB%B9%84%EC%A7%80-%EC%BB%AC%EB%A0%89%ED%84%B0GC)