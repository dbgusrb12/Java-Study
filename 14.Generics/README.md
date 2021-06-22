Generics
===

# 제네릭 (Generics) 이란?

제네릭 (Generics) 은 Java 5 부터 제공하고,   
코드의 재사용성을 높일 수 있는 기능이다.

클래스나 메서드에서 사용 할 다양한 타입의 객체를 내부가 아닌 외부에서 설정하는 방법   


## 제네릭을 사용하는 이유?

제네릭을 사용하여 작성하는 코드는 제네릭을 사용하지 않은 일반 코드에 비해 다양한 이점이 있다.

### 컴파일 시점에 타입 체크를 할 수 있다.

자바 컴파일러는 컴파일 시점에 타입 체크를 굉장히 엄격하게 적용하고, type safety 하지 않으면 에러를 발생시킨다.

자바 어플리케이션에서는 언제 발생할 지 모르는 런타임 에러를 수정하는 것 보다 컴파일 시점에 에러를 수정하는게 더 쉽고,   
제네릭을 사용하면 타입을 체크하고, 올바른 타입이 아니면 컴파일 시점에서 에러를 발생시키기 때문에, 어플리케이션을 실행 할 때   
에러를 체크 할 수 있다.

### 강제 형변환을 할 필요가 없다.

제네릭을 사용하지 않으면 해당 타입의 객체가 아닌 Object 타입의 객체가 반환되기 때문에 강제 캐스팅을 해줘야 하지만,   
제네릭을 사용하면 해당 타입의 객체로 반환해주기 때문에 타입 캐스팅에 드는 비용을 줄일 수 있다.

```java
public class GenericsTest {
    public static void main(String[] args) {
        List list = new ArrayList();
        list.add("테스트");
        // String 클래스로 형변환을 해줘야한다.
        String str = (String) list.get(0);

        List<String> stringList = new ArrayList<>();
        stringList.add("테스트");
        // 제네릭을 사용하면 String 클래스로 형변환을 할 필요가 없다.
        String strVal = stringList.get(0);
    }
}
```

### 개발자가 일반 알고리즘을 구현 할 수 있게 한다.

제네릭을 사용함으로써 개발자 입장에서   
다양한 유형의 컬렉션에서 작동하고, 커스터마이징 할 수 있으며,   
type safety 하고 가독성이 좋은 일반 알고리즘을 구현할 수 있다.

# 제네릭 사용법

제네릭은 클래스에서 사용하는 제네릭 클래스,   
메서드에서 사용하는 제네릭 메서드 두가지가 있다.

제네릭을 사용 하려면 사용 할 제네릭을 선언 해야 하는데,   
제네릭을 선언 할 때에는 `<>` 내부에 사용 할 제네릭을 `,`를 구분자로 나열한다.


## 제네릭 클래스

클래스 내부에서 사용하는 제네릭을 선언하기 위해서는   
클래스 이름 뒤에 사용 할 제네릭을 선언 한다.

```java
public class GenericClass<T> {

    T t;

    public GenericClass() {
    }

    public void setT(T t) {
        this.t = t;
    }

    public T getT() {
        return this.t;
    }
}

/**
 * 콤마를 구분자로 여러 제네릭을 선언 할 수 있다.
 * @param <K>
 * @param <V>
 */
public class MultiGenericClass<K, V> {
    K key;
    V value;

    public MultiGenericClass(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public K getKey() {
        return key;
    }

    public V getValue() {
        return value;
    }
}

public class GenericsTest {
    public static void main(String[] args) {
        // 해당 제네릭 클래스를 인스턴스화 시킬 때 타입이 명시되어 있지 않으면 Object 타입으로 정해진다.
        GenericClass<Integer> genericClass = new GenericClass<Integer>();
        genericClass.setT(10);
        System.out.println("result is : " + genericClass.getT());

        // Java 7 부터 다이아몬드 표기법 이라는 타입 추론 방식이 등장했다.
        // 앞에 제네릭에 타입이 미리 정해져 있다면, 뒤쪽 <> 에 아무것도 쓰지 않아도 타입 추론이 이루어져 해당 타입으로 선언 할 수 있다.
        GenericClass<Integer> genericClass2 = new GenericClass<>();
        genericClass2.setT(10);
        System.out.println("result is : " + genericClass2.getT());

        // 여러 제네릭을 사용하는 클래스도 타입 추론을 사용 할 수 있다.
        MultiGenericClass<String, Integer> multiGenericClass = new MultiGenericClass<>("age", 10);
        System.out.println("key : " + multiGenericClass.getKey() + ", value : " + multiGenericClass.getValue());
    }
}
```
```
result is : 10
result is : 10
key : age, value : 10
```

### 제네릭 타입 Naming Conventions

제네릭 타입은 단일 대문자로 짓는걸 규칙으로 정해놨고,   

일반적으로 사용하는 제네릭 타입들의 이름은 다음과 같다.

|타입인자|의미|
|---|---|
|T|Type|
|E|Element (컬렉션에서 많이 사용한다.)|
|K|Key|
|V|Value|
|N|Number|

## 제네릭 메서드

메서드 내부에서 사용하는 제네릭을 선언하기 위해서는   
메서드의 리턴타입 앞에 사용 할 제네릭을 선언 한다.

```java
public class MultiGenericClass<K, V> {
    K key;
    V value;

    public MultiGenericClass(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public K getKey() {
        return key;
    }

    public V getValue() {
        return value;
    }
}

public class GenericsTest {
    public static void main(String[] args) {
        MultiGenericClass<String, Integer> p1 = new MultiGenericClass<>("age", 10);
        MultiGenericClass<String, Integer> p2 = new MultiGenericClass<>("age", 10);

        GenericsTest genericsTest = new GenericsTest();
        System.out.println("result is : " + genericsTest.compare(p1, p2));
    }
    public <K, V> boolean compare(MultiGenericClass<K, V> p1, MultiGenericClass<K, V> p2) {
        return p1.getKey().equals(p2.getKey()) && p1.getValue().equals(p2.getValue());
    }
}
```
```
result is : true
```

# 바운디드 타입 (Bounded Type)

제네릭이 존재하는 타입에서 제네릭으로 사용 할 수 있는 타입을 제한하고 싶을 경우   
사용하는 방법이다.

예를 들면 제네릭을 사용 해야 하지만 숫자 형식의 타입만 받아야 하는 상황이 있을 때,   
이 바운디드 타입을 사용하여, 타입을 제한한다.

바운디드 타입은 `extends` 키워드를 사용해 표현한다.

제한해야 되는 타입이 `interface` 라고 `implements` 키워드를 사용하지 않는다.   
만약 다중으로 제한을 해야되면 `extends` 키워드 이후 들어가는 타입들을 `&` 구분자로   
나누어 표현한다.   
이 때, `extends` 키워드 이후 들어가는 목록들 중 클래스 타입이 있다면 인터페이스 보다   
앞 쪽에 나열되어야 한다.

```java
/**
 * 바운디드 타입을 이용해 Number 클래스의 하위 클래스과, Constable 인터페이스를
 * 상속받거나 구현한 타입만 들어 올 수 있도록 제한한다.
 * @param <T>
 */
public class BoundedType<T extends Number & Constable> {
    private T t;

    public BoundedType() {
    }

    public void setT(T t) {
        this.t = t;
    }

    public T getT() {
        return this.t;
    }
}

public class GenericsTest {
    public static void main(String[] args) {
        
        // 해당 바운디드 타입의 범위에서 벗어나지 않으면 정상적으로 인스턴스가 생성된다.
        BoundedType<Integer> boundedType = new BoundedType<>();

        // 해당 바운디드 타입의 범위에서 벗어나는 경우 컴파일 에러가 발생한다.
        // BoundedType<String> boundedType = new BoundedType<String>();
    }
}
```

# 와일드카드 (Wildcard)


> 웹문서
> - [The Java Tutorials(Generics)](https://docs.oracle.com/javase/tutorial/java/generics/index.html)