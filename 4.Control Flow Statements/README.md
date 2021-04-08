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
- 조건식에는 true / false 를 반환하는 연산식이나, boolean 변수만 들어갈 수 있다.
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
- 위에 조건식부터 타고 내려오다가 true 인 조건식의 블록만 실행 된 후 벗어나고, 모든 조건문이 false 일 때는 else 블록이 실행된다.
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

## 반복문

- 일정한 조건까지 구간을 반복적으로 실행시키는 구문이다.
- 중복 코드를 줄일 수 있고, 시간을 절약 할 수 있고, 실수를 줄일 수 있다.


### break

- 반복문, switch 문의 흐름을 제어 하기 위해 쓰이는 키워드이다.
- 자신이 포함된 하나의 반복문, 또는 switch 문을 빠져나온다.

### continue

- 반복문의 흐름을 제어 하기 위해 쓰이는 키워드이다.
- 자신이 포함된 반복문의 끝으로 이동한다.
- continue 이후의 문장들은 실행되지 않는다.
- 반복문을 빠져나오는게 아닌 해당 반복 부분만 빠져 나오는 것이다.

### while 문

```java
while(조건식) {
    // 조건식이 true일 때 동안 반복하여 실행되는 구역
}
```

- 조건식의 결과에 따라 블록(`{}`)의 실행 여부가 결정된다.
- 블록이 실행 된 후 조건식으로 다시 돌아가 실행 여부를 확인한다.
- 조건식이 false 가 되면 루프를 빠져나온다.

```java
public class WhileLoopTest {
    public static void main(String[] args) {
        int initialization = 0;
        int sum = 0;
        while(initialization < 10) {    // while 의 들어가는 조건식은 실행 시킬 조건이다.
            initialization++;
            sum += initialization;
        }
        System.out.println("result is " + sum);

        initialization = 0;
        sum = 0;

        while(true) {       // 조건식에 true 가 들어가면 무한 루프가 돈다.
            initialization++;
            sum += initialization;

            if(initialization == 10) {  // break 키워드를 쓸 조건식은 빠져 나올 조건이다.
                break;      // break 키워드로 반복문을 빠져 나올 수 있다.
            }
        }
        System.out.println("result is " + sum);
        
        initialization = 0;
        sum = 0;
  
        while(initialization < 10) {
          initialization++;
          if(initialization % 2 == 1) { // continue 키워드를 쓸 조건식은 이후의 코드들을 넘어 갈 조건이다.
            continue;       // continue 키워드로 이후의 코드들을 실행 시키지 않을 수 있다.
          }
          sum += initialization;
        }
        System.out.println("result is " + sum);

        // 조건식을 잘못 세우면 무한 루프에 빠진다.
//      while(true) {
//        initialization++;
//        if(initialization % 2 == 0) {
//          System.out.println("continue 실행");
//          continue;
//        }
//        if(initialization == 10) {
//          System.out.println("break 실행");
//          break;
//        }
//        sum += initialization;
//      }
//      System.out.println("result is " + sum);
    }
}
```

```
result is 55
result is 55
result is 30
```

### do-while 문

```java
do {
    // 실행 할 구역
}while(조건식);
```

- while 문과는 다르게 블록 구역의 내용이 먼저 실행 된 후 조건식을 확인한다.
- 조건식이 true 일 때 블록 구역으로 다시 돌아가 실행한다.
- 조건식이 false 가 되면 루프를 빠져나온다.

```java
public class DoWhileLoopTest {
    public static void main(String[] args) {
        int initialization = 0;
        int sum = 0;

        do {
            initialization++;
            sum += initialization;
        }while(initialization < 10);
        System.out.println("result is " + sum);
        
        do { // do-while 문은 while 의 조건식이 무엇이든 do 의 블록 영역이 한번은 실행된다.
            System.out.println("한번은 실행된다.");
        }while(false);
    }
}
```

```
result is 55
한번은 실행된다.
```

### for 문

```java
for(초기값; 조건식; 증감식) {
    // 조건식이 true 일 때 동안 반복하여 실행되는 구역    
}
```

- 주로 반복의 횟수를 알고 있을 때 쓰인다.
- 초기값과 조건식, 증감식에 따라 블록(`{}`)의 실행 여부가 결정된다.
- 처음엔 초기값과 조건식으로 블록의 실행 여부를 판단한다.
- 그 이후엔 증감식을 거치고, 변경된 값과 조건식으로 블록의 실행 여부를 판단한다.

```java
public class ForLoopTest {
    public static void main(String[] args) {
        int sum = 0;
        for(int initialization = 1; initialization < 11; initialization++) {
            sum += initialization;
        }
        System.out.println("result is " + sum);

      int initialization = 1;   // 초기 값에 들어갈 변수를 밖에서 선언 가능하다.
      int odd = 0;
      int even = 0;

      // 1부터 10까지 홀수의 합과 짝수의 합
      for(; ; initialization++) { // 조건식이 없으면 무한루프가 돈다.
          if(initialization % 2 == 1) {
              odd += initialization;
              continue;
          }
          even += initialization;

          if(initialization == 10) {    // 무한 루프를 빠져나오기위해 break 키워드 사용
              break;
          }
      }
      System.out.println("odd is " + odd + " and even is " + even);
      System.out.println("initialization is " + initialization);    // 초기 값을 밖에서 선언하면 변경 된 초기 값 역시 밖에서 사용 가능
    }
}
```

```
result is 55
odd is 25 and even is 30
initialization is 10
```

### 향상된 for 문

```java
for(자료형 변수명 : 배열 or 컬렉션) {
    // 배열, 컬렉션의 길이 만큼 실행되는 영역
}
```

- for 문이 배열이나 컬렉션과 많이 쓰기 때문에 같이 쓸 수 있도록 개량 되었다.
- 배열이나 컬렉션의 0번 인덱스부터 접근하며, 0번 인덱스의 값이 사용 할 변수에 대입된다.

```java
public class ForEachLoopTest {
    public static void main(String[] args) {
        int[] intArray = {1,2,3,4,5};
        for(int intVal : intArray) {    // intArray 배열을 돌면서 인덱스의 값을 intVal 에 대입시킨다.
            System.out.println("result is " + intVal);
        }
    }
}
```

```
result is 1
result is 2
result is 3
result is 4
result is 5
```

> - [마로의 Java(자바) 정리 - 7. 제어문(Control Flow Statement)-조건문](https://hoonmaro.tistory.com/17)
> - [[JAVA] break문, continue문](https://josephian.tistory.com/218)