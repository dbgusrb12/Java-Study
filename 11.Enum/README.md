Enum
===

# 열거형 (Enum) 이란?

JDK 1.5 부터 추가된 `Enum` 은 Enumeration 의 약자이고, **열거형**이라고도 한다.

서로 연관성이 있는 상수들의 집합을 의미하며, Reference type 의 데이터이다.

Java 에서 `final static String`, `final static int` 와 같은 기본 자료형의   
값을 `enum` 으로 대체하여 사용 할 수 있다.

# `enum` 정의 하는 방법

`enum` 을 정의 할때는 `class` 키워드 대신 `enum` 키워드를 사용하고,   
사용 할 상수들을 선언해주면 된다.

```java
public enum Day {
    // 상수 이기 때문에 대문자로 써야한다. (컨벤션)
    MONDAY, TUESDAY, WEDNESDAY
}
```

jdk 1.5 이전에는 클래스나, 인터페이스로 상수 집합을 사용하였으며,   
위의 코드를 클래스로 바꾸면 아래와 같이 된다.

```java
public class Day {
    public final static Day MONDAY = new Day();
    public final static Day TUESDAY = new Day();
    public final static Day WEDNESDAY = new Day();
}
```

## `enum`의 장점

위의 예시와 같이 `enum` 을 사용하게 되면 클래스나 인터페이스를 사용하는 것에 비해   
아래와 같은 장점이 있다.

- 코드가 간결해지고, 가독성이 좋아진다.
- 인스턴스의 생성과 상속을 방지하여, 상수들의 타입 안정성을 보장한다.
- `enum` 키워드를 사용하여 구현 의도가 열거임을 분명하게 나타낼 수 있다.

## 추가 속성 부여

`enum` 내부에 열거된 상수들에 추가 속성을 부여 할 수 있다.

```java
public enum Day {
    MONDAY("월요일"),
    TUESDAY("화요일"),
    WEDNESDAY("수요일");

    private String name;

    // 생성자의 접근 제한자를 private 이 아닌 다른 걸로 선언하면 컴파일 시점에 에러가 난다. 
    // public Day() {
    // }


    Day() {
    }

    Day(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}
```

`enum` 내부 상수에 String 으로 된 한글 이름을 추가 한 뒤,   
`name` 이라는 필드값을 생성 하고, 내부 상수의 속성과 연결 시켜 사용 할 수 있다.   
이 때 필드값보다 상수들을 먼저 정의해야하고, `enum` 에 필드와 메서드가 있다면   
반드시 열거된 상수들 뒤에 세미콜론(`;`)을 붙여줘야 한다.


`enum` 도 특별한 하나의 `class` 이기 때문에 생성자가 필요하다.   
기본적으로 만들지 않아도 default 생성자를 만들어 주긴 하지만,   
`enum` 의 경우 생성자의 접근제어자를 `private` 으로 설정해주어야한다.

명시적으로 접근제어자를 쓰지 않는 경우 기본 `private` 으로 생성되며,   
`private` 으로 설정하는 이유는 `enum` 의 특징 때문이다.

- Day.class 파일

```java
public enum Day {
    MONDAY("월요일"),
    TUESDAY("화요일"),
    WEDNESDAY("수요일");

    private String name;

    // JVM 에서 컴파일을 할 때 private 을 추가한다.
    private Day() {
    }

    private Day(String name) {
        this.name = name;
    }

    public String getName() {
        return this.name;
    }
}
```

`enum` 은 값이 고정적인 상수들의 집합으로써, `enum` 내부가 아닌 다른 외부에서 고정적인   
상수의 값을 변경 할 수 없도록 하여, 모든 값을 런타임 시점이 아닌, 컴파일 시점에서 알 수 있도록   
해야한다.   
그렇게 되면 컴파일 시에 타입 안정성도 보장되고,   
인스턴스를 생성 할 수도, 상속 받을 수도 없으니 선언된 상수는 존재하고, 인스턴스는 존재 하지 않은   
final 한 객체가 되는 것이다.

결국 enum 타입은 인스턴스 생성을 제어하며, **싱글톤(singleton)** 을 일반화한다.

이런 특성 때문에 enum 은 싱글톤을 구현하는 하나의 방법으로 사용되기도 한다.

# `java.lang.Enum` 클래스

`enum` 키워드를 사용해 enum 을 정의 할 때, 기본적으로 `java.lang.Enum` 클래스를   
상속 받는다.

Java 에서는 다중 상속을 지원하지 않기 때문에 정의된 enum 은 다른 클래스를 상속 받을 수 없고,   

상속 받을 때 자식 클래스의 생성자에서 부모 클래스의 생성자를 호출해야 하지만,   
enum 의 생성자는 기본 `private` 이기 때문에 다른 클래스가 정의된 enum 을 상속받을 수도 없다.

따라서 `enum` 은 상속을 할 수도 없고, 상속을 받을 수도 없고,   
인스턴스를 생성 할 수도 없는 클래스가 되는 것이다.

`Enum` 클래스에서는 여러가지 메서드들을 지원한다.

## `valueOf(Class<T> enumType, String name)`

`valueOf` 메서드는 해당 타입의 enum 내부의 해당 name 과 같은 이름의   
상수를 가져온다.

```java
public class EnumTest {
    enum Day {
        MONDAY("월요일"),
        TUESDAY("화요일"),
        WEDNESDAY("수요일");

        private String name;
        
        Day() {
        }
        
        Day(String name) {
            this.name = name;
        }

        public String getName() {
            return name;
        }
    }
    public static void main(String[] args) {
        Day day = Day.valueOf(Day.class, "MONDAY"); 
        System.out.println(day.getName());
    }
}
```
```
월요일
```

## `ordinal()`

`ordinal` 메서드는 해당 enum 타입의 상수가 나열 되어있는 인덱스를 알 수 있다.

```java
public class EnumTest {
    enum Day {
        MONDAY, TUESDAY, WEDNESDAY;
    }
    public static void main(String[] args) {
        Day day = Day.TUESDAY;
        System.out.println(day.ordinal());
    }
}
```
```
1
```

# enum 이 제공하는 메소드

`enum` 키워드를 사용하여 enum 을 정의 하면, 컴파일 시점에 JVM 에서 자동으로 만들어주는   
메서드 들이 존재한다.

## `values()`

`values()` 메서드는 해당 enum 에 정의되어 있는 상수들의 리스트를 반환한다.

```java
public class EnumTest {
    enum Day {
        MONDAY, TUESDAY, WEDNESDAY;
    }
    public static void main(String[] args) {
        Day[] dayList = Day.values();   // Day enum 의 정의 되어 있는 상수들의 배열을 리턴
        for (Day day : dayList) {
            System.out.println(day);
        }
    }
}
```
```
MONDAY
TUESDAY
WEDNESDAY
```

## `valueOf(String name)`


`valueOf` 메서드는 enum 에 정의되어 있는 상수들 중 해당 `name` 과 같은 이름의   
상수를 가져온다.

```java
public class EnumTest {
    enum Day {
        MONDAY("월요일"),
        TUESDAY("화요일"),
        WEDNESDAY("수요일");

        private String name;
        
        Day() {
        }
        
        Day(String name) {
            this.name = name;
        }

        public String getName() {
            return name;
        }
    }
    public static void main(String[] args) {
        Day day = Day.valueOf("MONDAY");
        System.out.println(day.getName());
    }
}
```
```
월요일
```

## enum 내부 메서드 확인 방법

Intellij 에서 컴파일된 클래스의 바이트 코드를 보면 해당 메서드들을 볼 수 있다.   
([Intellij 에서 바이트 코드 보는 방법](https://www.youtube.com/watch?v=XRmxIp8mJ-o))

- `Day.java` 파일
```java
public enum Day {
    MONDAY,
    TUESDAY,
    WEDNESDAY
}
```

- `Day.class` 파일 의 바이트코드 일부
```java
/**
 * 바이트 코드의 일부분을 가져온 것으로, 필요한 바이트코드들만 추출하여 만든 것이다.
 */
public final enum com/company/hg/enumTest/Day extends java/lang/Enum {

  // compiled from: Day.java

  public final static enum Lcom/company/hg/enumTest/Day; MONDAY

  public final static enum Lcom/company/hg/enumTest/Day; TUESDAY

  public final static enum Lcom/company/hg/enumTest/Day; WEDNESDAY

  public static values()[Lcom/company/hg/enumTest/Day;
   L0
    LINENUMBER 3 L0
    GETSTATIC com/company/hg/enumTest/Day.$VALUES : [Lcom/company/hg/enumTest/Day;
    INVOKEVIRTUAL [Lcom/company/hg/enumTest/Day;.clone ()Ljava/lang/Object;
    CHECKCAST [Lcom/company/hg/enumTest/Day;
    ARETURN
    MAXSTACK = 1
    MAXLOCALS = 0

  public static valueOf(Ljava/lang/String;)Lcom/company/hg/enumTest/Day;
   L0
    LINENUMBER 3 L0
    LDC Lcom/company/hg/enumTest/Day;.class
    ALOAD 0
    INVOKESTATIC java/lang/Enum.valueOf (Ljava/lang/Class;Ljava/lang/String;)Ljava/lang/Enum;
    CHECKCAST com/company/hg/enumTest/Day
    ARETURN
   L1
    LOCALVARIABLE name Ljava/lang/String; L0 L1 0
    MAXSTACK = 2
    MAXLOCALS = 1
}
```

위의 바이트코드를 보면 `java.lang.Enum` 클래스를 상속 받고 있는 것을 볼 수 있고,
내부에 `values()` 메서드와 `valueOf(String name)` 메서드가 정의되어 있는 것을 볼 수 있다.

그리고 `valueOf` 메서드의 바이트 코드를 보면, 내부적으로 해당 enum 의 클래스를 가져와,   
`Enum` 클래스의 `valueOf` 메서드를 호출하는 것을 볼 수 있다.

그렇게 해서 enum 타입의 `valueOf` 메서드는 매개변수로 `String name` 만 받아도   
해당 타입의 이름을 가진 상수를 가져 올 수 있는 것이다.

# `EnumSet`

`EnumSet` 은 enum 으로 작동 하기 위한 `Set` 컬렉션이다.

`Set` 인터페이스를 기반으로 하며, enum 요소들을 담아내어 보다 빠르게 결과를 도출해 낼 수 있다.

## `EnumSet` 특징

- `enum` 타입만 들어 갈 수 있고, 들어가는 enum 의 타입은 모두 같은 타입이여야 한다.
- null 을 추가 할 수 없다.
- 쓰레드에 안전 하지 않아 필요하다면 외부에서 동기화를 해야한다.
- 저장되는 값은 해당 enum type 의 ordinal 을 따라간다.
- 모든 메서드는 산술 비트 연산을 사용하여 구현되었기 때문에 연산이 매우 빠르다.

## `EnumSet` 내부 메서드

`EnumSet` 클래스에서 제공하는 메서드들 중 `EnumSet` 인스턴스를 만들 수 있는   
메서드가 여러가지가 있다.

### `of(E e, E e2, ...)`

`of()` 메서드는 매개변수에 정의된 enum 의 필드값을 넣으면 되는데,   
`EnumSet` 클래스에서 정의 되어 있는 메서드는 5개의 매개변수까지 존재하고 그 이후의 것은   
`of(E first, E... rest)` 이런 식으로 정의 되어 있는 것을 볼 수 있다.

```java
public class EnumSetTest {
    enum Day {
        MONDAY,
        TUESDAY,
        WEDNESDAY,
        THURSDAY,
        FRIDAY
    }

    public static void main(String[] args) {
        EnumSet<Day> enumSet1 = EnumSet.of(Day.MONDAY);
        EnumSet<Day> enumSet2 = EnumSet.of(Day.MONDAY, Day.TUESDAY);
        EnumSet<Day> enumSet3 = EnumSet.of(Day.MONDAY, Day.TUESDAY, Day.WEDNESDAY);
        EnumSet<Day> enumSet4 = EnumSet.of(Day.MONDAY, Day.TUESDAY, Day.WEDNESDAY, Day.THURSDAY);
        
        // 순서를 바꿔도 저장 된 것을 보면 ordinal 순으로 저장 된 게 보인다.
        EnumSet<Day> enumSet5 = EnumSet.of(Day.WEDNESDAY, Day.TUESDAY, Day.MONDAY, Day.THURSDAY, Day.FRIDAY);

        System.out.println(enumSet1);
        System.out.println(enumSet2);
        System.out.println(enumSet3);
        System.out.println(enumSet4);
        System.out.println(enumSet5);
    }
}
```
```
[MONDAY]
[MONDAY, TUESDAY]
[MONDAY, TUESDAY, WEDNESDAY]
[MONDAY, TUESDAY, WEDNESDAY, THURSDAY]
[MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY]
```

### `allOf(Class<E> elementType)`

`allOf` 메서드는 해당 타입의 enum 내부에 존재하는 요소들을 전부 가져와 `EnumSet` 인스턴스를 만든다.

```java
public class EnumSetTest {
    enum Day {
        MONDAY,
        TUESDAY,
        WEDNESDAY,
        THURSDAY,
        FRIDAY
    }

    public static void main(String[] args) {
        EnumSet<Day> enumSet = EnumSet.allOf(Day.class);
        
        System.out.println(enumSet);
    }
}
```
```
[MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY]
```

### `copyOf(EnumSet<E> s)`, `copyOf(Collection<E> c)`

`copyOf` 메서드는 해당 `EnumSet` 인스턴스나, 타입이 `Enum` 인 컬렉션을 기반으로   
복사본을 만들어 준다.

```java
import java.util.EnumSet;

public class EnumSetTest {
    enum Day {
        MONDAY,
        TUESDAY,
        WEDNESDAY,
        THURSDAY,
        FRIDAY
    }

    public static void main(String[] args) {
        EnumSet<Day> enumSet = EnumSet.allOf(Day.class);

        EnumSet<Day> copyEnumSet = EnumSet.copyOf(enumSet);

        // 원본을 바꿔도 복사본이 바뀌지 않는다.
        enumSet = EnumSet.noneOf(Day.class);

        System.out.println(copyEnumSet);
    }
}
```
```

```

### `range(E from, E to)`

`range` 메서드는 해당 enum 요소의 범위를 정해 가져와서 반환한다.

반드시 첫번째 매개변수의 ordinal 이 두번째 매개변수의 ordinal 보다 작거나 같아야 한다.

```java
public class EnumSetTest {
    enum Day {
        MONDAY,
        TUESDAY,
        WEDNESDAY,
        THURSDAY,
        FRIDAY
    }

    public static void main(String[] args) {
        EnumSet<Day> enumSet = EnumSet.range(Day.MONDAY, Day.WEDNESDAY);
        
        // range 의 매개변수는 무조건 앞의 ordinal 이 뒤의 ordinal 보다 작거나 같아야 한다.
        // EnumSet<Day> enumSet = EnumSet.range(Day.WEDNESDAY, Day.MONDAY);
        
        System.out.println(enumSet);
    }
}
```
```
[MONDAY, TUESDAY, WEDNESDAY]
```

### `complementOf(EnumSet<E> s)`

`complementOf` 메서드는 해당 enum 타입의 요소를 제외한 요소만 넣어서 반환한다.

```java
import java.util.EnumSet;

public class EnumSetTest {
    enum Day {
        MONDAY,
        TUESDAY,
        WEDNESDAY,
        THURSDAY,
        FRIDAY
    }

    public static void main(String[] args) {
        EnumSet<Day> enumSet = EnumSet.complementOf(EnumSet.range(Day.MONDAY, Day.WEDNESDAY));
        
        System.out.println(enumSet);
    }
}
```
```
[THURSDAY, FRIDAY]
```

### `noneOf(Class<E> elementType)`

`noneOf` 메서드는 해당 타입의 빈 `EnumSet` 을 반환한다.

```java
public class EnumSetTest {
    enum Day {
        MONDAY,
        TUESDAY,
        WEDNESDAY,
        THURSDAY,
        FRIDAY
    }

    public static void main(String[] args) {
        EnumSet<Day> enumSet = EnumSet.noneOf(Day.class);
        
        System.out.println(enumSet);
    }
}
```
```
[]
```

> 웹문서
> - [The Java Tutorials(Enum Types)](https://docs.oracle.com/javase/tutorial/java/javaOO/enum.html)
> - [Java: enum 의 뿌리를 찾아서...](https://www.nextree.co.kr/p11686/)
> - [[Java] Enum의 사용법](https://limkydev.tistory.com/66)