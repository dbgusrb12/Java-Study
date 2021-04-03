Control Flow Statements
===

# 제어문(Control Flow Statements) 이란?

제어문이란 프로그램의 흐름을 개발자가 원하는 방향으로 제어할 수 있도록 해주는 구문이다.   
인터프리터가 바이트코드를 기계어로 번역할 때 번역 흐름을 제어해 준다.

## 조건문 (조건 분기)

특정 데이터의 값에 따라서 코드의 실행 흐름을 제어하는 구문이다.

### if 문

```java
if(조건식) {
    // 조건식이 true 일 때 실행되는 구역
}
```

- 조건식의 결과에 따라 블록(`{}`)의 실행 여부가 결정된다.
- 블록에 들어갈 코드가 한줄이면 블록을 생략 할 수 있다.
- 조건식에는 true/false 를 반환하는 연산식이나, boolean 변수만 들어갈 수 있다.
- 조건식이 true 일 때 블록 구역의 내용이 실행된다.

### if-else 문

```java
if(조건식) {
    // 조건식이 true 일 때 실행되는 구역
} else {
    // 조건식이 false 일 때 실행되는 구역
}
```

- 조건식에 결과에 따라 한개의 블록만 실행 된 후 벗어난다.
- 블록에 들어갈 코드가 한줄이면 블록을 생략 할 수 있다.

### if-else if-else 문

```java
if(조건식A) {
    // 조건식A가 true 일 때 실행되는 구역
} else if(조건식B) {
    // 조건식B가 true 일 때 실행되는 구역
} else {
    // 모든 조건식이 false 일 때 실행되는 구역
}
```

- 조건식이 여러개인 if 문이다.
- 위에 조건식부터 타고 내려오며 true 인 조건식의 블록만 실행 된 후 벗어나고, 모든 조건문이 false 일 때 else 블록이 실행된다.
- 블록에 들어갈 코드가 한줄이면 블록을 생략 할 수 있다.

### 중첩 if 문

```java
if(조건식A) {
    // 조건식이 true 일 때 실행되는 구역
    if(조건식B) {
        // 조건식A 와 조건식B 가 모두 true 일 때 실행되는 구역
    }
}
```

- if 문 내부에서 또 다른 if 문을 사용하는 구문
- 중첩 단계는 제한이 없기 때문에 실행 흐름을 잘 판단하여 작성해야 한다.

```java
public class DecisionMakingStatementsTest {
    public static void main(String[] args) {
        int intVal = 10;
        int intVal2 = 20;
        
        // 블록에 들어갈 코드가 한줄이면 생략 가능
        if(intVal + intVal2 >= 30) System.out.println("Condition is true");
        
        if(intVal + intVal2 > 30) {
            System.out.println("Condition is true");
        }else {
            System.out.println("Condition is false");
        }
        
        if(intVal + intVal2 > 30) {
            intVal++;   // 조건문이 false 이므로 실행되지 않음.
        }else if(intVal + intVal2 < 30) {
            intVal--;   // 조건문이 false 이므로 실행되지 않음.
        }else {
            System.out.println("result is " + (intVal + intVal2));
        }
        
        if(intVal >= 10) {
            if(intVal2 >= 20) {
              System.out.println("intVal is " + intVal + " and intVal2 is " + intVal2);
            }
        }
    }
}
```
```
Condition is true
Condition is false
result is 30
intVal is 10 and intVal2 is 20
```


## 선택문 (선택 처리)

### switch 문

```java
switch(변수) {
    case 값1:
        // 실행 할 코드가 들어간다.
        break;
    case 값2:
        // 실행 할 코드가 들어간다.
        break;
    case 값3:
        // 실행 할 코드가 들어간다.
        break;
    default:
        실행문;
}
```

- 선택문은 조건문의 일종으로 볼 수 있다.
- 하나의 변수에 대해 여러 조건을 선택적으로 실행 할 수 있다.
- `case` 값의 들어가는 값은 상수여야 한다.
- if-else 문이나 if-else if-else 문과는 다르게, 조건에 해당하는 구역만 실행하는게 아닌,   
  조건에 해당하는 구역부터 밑으로 실행하기 때문에, `break` 키워드와 같이 쓴다.
- 모든 조건이 해당하지 않으면 `default` 구역이 실행된다.
- switch 구문의 단점을 개선한 것이 Java 12부터 추가된 switch 연산자이다.(switch 구문이 바뀐게 아닌 switch 연산자가 추가된 것)

```java
public class DecisionMakingStatementsTest {
    public static void main(String[] args) {
        int[] intArray = {1,2,3,6};
        for(int intVal : intArray) {
            switch(intVal) {
                case 1:
                    System.out.println("Condition is 1");
                    break;
                case 2:
                    System.out.println("Condition is 2");
                    break;
                case 3:
                    System.out.println("Condition is 3");   // break; 키워드를 넣지 않으면 이후의 코드까지 실행이 된다.
                case 4:
                    intVal++;
                case 5:
                    intVal++;
                default:
                    System.out.println("Condition is default");
            }
            System.out.println("intVal is " + intVal);
        }
    }
}
```

```
Condition is 1
intVal is 1
Condition is 2
intVal is 2
Condition is 3
Condition is default
intVal is 5
Condition is default
intVal is 6
```



> - [마로의 Java(자바) 정리 - 7. 제어문(Control Flow Statement)-조건문](https://hoonmaro.tistory.com/17)