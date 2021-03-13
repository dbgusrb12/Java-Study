JVM (Java Virtual Machine)
==========================

## **JVM** 이란

_Java Byte Code_ 를 OS에 맞게 해석해주는 역할을 한다.

### **Byte Code** 란 

*특정 하드웨어가 아닌 가상 컴퓨터에서 돌아가는 실행 프로그램을 위한 이진 표현법* 이다.   
하드웨어가 아닌 *소프트웨어에 의해 처리* 되기 때문에 보통 기계어보다 더 _추상적_ 이라고 한다.   

### 컴파일 하는 방법
```
$ javac main.java
```
*.class* 파일을 **Java Byte Code**라고 하는데,   
_Java compiler_ 를 통해 *.java* 파일을 *.class* 파일로 변환 시켜준다.   
(이렇게 변환 시키는것을 컴파일 한다고 한다.)   

### 실행 하는 방법
```
$ java main
```
source code(##.java) => javac compile (`$ javac ##.java`) => byte code(##.class)로 변환 => JVM => OS에 맞게 binary code로 변환 => 프로그램 실행

### Java 프로그램 실행 과정

>   1. 프로그램이 실행되면 JVM은 OS로부터 프로그램이 필요로 하는 메모리를 할당받고,   
>      할당받은 메모리를 용도에 따라 여러 영역으로 나누어 관리한다.
>   2. 자바 컴파일러(javac)가 자바 소스코드(.java)를 읽어들여 Java Byte Code(.class)로 변환시킨다.
>   3. Class Loader를 통해 class파일들을 JVM으로 로딩한다.
>   4. 로딩된 class파일들은 Execution engine을 통해 해석된다.
>   5. 해석된 Byte Code는 Runtime Data Areas에 배치되어 실질적인 수행이 이루어진다.   
>
>   이러한 실행과정 속에서 JVM은 필요에 따라 Thread Synchronization과 GC같은 관리작업을 수행한다.

때문에 JAVA는 OS에 종속적이지 않고, 어느 디바이스든 java 파일만 있다면 JVM 위에서 실행 할 수 있다.  
~~하지만 JVM은 OS에 맞게 설치해야 된다고 한다.~~


## **JVM** 구성 요소

### Class Loader

Java compiler가 컴파일한 클래스(.class 파일)를 로드하는 모듈.   
Runtime 시 동적으로 클래스를 로드한다.   

클래스를 처음으로 참조할 때, 해당 클래스를 로드하고 링크해주는 역할을 수행한다.   
jar파일 내 저장된 클래스들을 JVM 위에 올리고 사용하지 않는 클래스는 삭제한다.

### Execution Engine

클래스 로더가 로드한 클래스(Byte code로 이루어져 있다.)들을 해석하여 Binary Code로 변환하고, 실행한다.

초기 JVM이 나왔을 당시에는 한줄씩 해석하는 Interpreter 방식 이였기 때문에 속도가 느리다는 단점이 있었지만   
JIT(Just In Time) Compiler 방식을 도입해 이 단점을 해결했다.

#### **JIT Compiler** 란

Interpreter 방식의 단점을 보완하기 위해 도입된 컴파일러이다.   
Byte Code 전체를 컴파일하여 Native Code로 변경하고 캐시에 보관해   
한번 컴파일된 코드는 빠르게 수행할 수 있게 된다.   

JIT 컴파일러가 컴파일하는 과정은 byte code를 인터프리팅 하는 것보다 훨씬 오래 걸리므로 한번만 실행되는   
코드라면 컴파일하지 않고 인터프리팅 하는 것이 유리하다.

따라서 JIT Compiler를 사용하는 JVM들은 Interpreter 방식을 사용하다 내부적으로 해당 메소드가 얼마나   
자주 수행되는지 체크하고, 일정 기준를 넘어가면 JIT Compiler 방식으로 실행된다.


### Runtime Data Area

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

    자바 프로그램이 컴파일되어 생성되는 Byte Code가 아닌 실제 실행할 수 있는 기계어로   
    작성된 프로그램을 실행시키는 영역이다.

    JAVA 언어가 아닌 다른 언어로 작성된 코드를 위한 공간이고, JAVA Native Interface를   
    통해 Byte Code로 전환하여 저장한다.

    JAVA가 아닌 일반 프로그램처럼 커널이 스택을 잡아 독자적으로 프로그램을 실행시키는 영역   
    으로, 이 부분을 통해 C code를 실행시켜 Kernel에 접근 가능하다.

- **Method Area**

    *Class Loader* 가 클래스를 처음 메모리 공간에 올릴 때 초기화되는 대상을 저장하기 위한 메모리 공간이다.

    Java 프로그램은 main 메소드의 호출에서부터 계속된 메소드의 호출로 흐름을 이어가기 때문에   
    Method Area에 올라가게 되는 메소드의 Byte Code는 프로그램의 흐름을 구성하는 Byte Code이다.

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

## **JDK**와 **JRE**의 차이

### **JRE** 란 

Java Runtime Environment 의 약자로, 자바 프로그램을 실행시켜주는 환경을 구성해주는 도구

### **JDK** 란

Java Development Kit 의 약자로, 자바 개발을 위해 필요한 툴킷을 제공하는 도구.   
JRE가 포함 되어있다.