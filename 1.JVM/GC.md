GC (Garbage Collection)
===

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

