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
        list.add("테스트");
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

> 웹문서
> - [The Java Tutorials(Generics)](https://docs.oracle.com/javase/tutorial/java/generics/index.html)