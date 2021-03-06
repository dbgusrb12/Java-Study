Operator
===

# Operator 란?

어떠한 기능 또는 어떤 대상체에 계산과 같은 처리를 수행하는 문자 또는 기호.   
Java 에서의 연산자는 크게 단항, 이항, 삼항, 대입 연산자로 나뉘고,   
이항 연산자는 산술, 비교, 논리 연산자로 나뉜다.

- 연산자(Operator): 어떠한 기능을 수행하는 기호
- 피연산자(Operand): 연산자의 작업 대상

# 연산자의 종류

## 단항 연산자

피연산자가 하나만 필요한 연산자이다.

### 비트 연산자 (`~`)

- 1의 보수를 구해주는 연산자이다.
- 2진수로 표현 했을 때 각 자리수를 반전 시킨다. (0은 1로, 1은 0으로)
- 피연산자의 크기가 4byte 보다 작으면 4byte(int 형)으로 변환 후 연산을 수행한다.
- 양수일 경우 부호는 음수가 되고 절대값이 1 증가한다.
- 음수일 경우 부호는 양수가 되고 절대값이 1 감소한다.
    
### 논리 연산자 (`!`)

- NOT 연산자로 논리값을 부정하는 연산자이다.
- true 값은 false 가 되고, false 값은 true 가 된다.

### 부호 연산자 (`+`, `-`)

- 값의 부호를 바꾸는 연산자이다. 
- '+' 연산자의 경우는 형식적으로 제공한다.
- '-' 연산자의 경우는 부호를 -로 바꿔주는 연산자인데,   
  다르게 말해 2의 보수를 구하는 연산자이기도 하다.

### 증감 연산자 (`++`, `--`)

- 피연산자의 값을 1 증가, 감소 시키는 연산자이다.
- 전위에 올 경우 값이 참조되기 전에 증감시킨다.
- 후위에 올 경우 값이 참조 된 후에 증감시킨다.

### 형변환 연산자 (`(type)`)

- 피연산자의 자료형을 바꿔주는 연산자이다.
- 피연산자 앞에 붙으며, 전 주에 배웠던 type casting 이 그 예시이다.

```java
public class UnaryOperatorTest {
    public static void main(String[] args) {
        int positiveVal = 10;
        int negativeVal = -10;
        boolean trueVal = true;
        boolean falseVal = false;
        
        System.out.println("result is " + positiveVal + " to binary string is " + Integer.toBinaryString(positiveVal));
        // result is 10 to binary string is 1010
        
        System.out.println("result is " + ~positiveVal + " to binary string is " + Integer.toBinaryString(~positiveVal));
        // result is -11 to binary string is 11111111111111111111111111110101
        // 4byte 이하인 경우 4byte(int 형)으로 변환 하여 연산한다. ~연산자는 2진수로 표현한 각 자리수를 반전시킨다.
        // 0 -> 1, 1 -> 0
        
        System.out.println("result is " + !trueVal);        // result is false
        System.out.println("result is " + !falseVal);       // result is true
        
        System.out.println("result is " + (-positiveVal));  // result is -10
        System.out.println("result is " + (+negativeVal));  // result is -10 +연산자는 형식적으로 제공되어있다.
        System.out.println("result is " + (-negativeVal));  // result is 10
        
        System.out.println("result is " + (positiveVal++)); // result is 10
        // 후위 증가 연산자로 인해 print 가 된 후 연산이 실행된다. 현재 시점에서 intVal 의 값은 11이다.
        System.out.println("result is " + positiveVal);     // result is 11
        System.out.println("result is " + (++positiveVal)); // result is 12
        
        double doubleVal = (double)positiveVal;
        System.out.println("result is " + doubleVal);       // result is 12.0
    }
}
```

## 이항 연산자

피연산자가 두 개 필요한 연산자 이다.

### 산술 연산자 (`+`, `-`, `*`, `/`, `%`)

- 사칙연산(`+`, `-`, `*`, `/`)과 나머지 연산(`%`)을 하는 가장 기본이 되는 연산자이다.   
- 피연산자의 크기가 4byte 보다 작으면 4byte(int 형)으로 변환 후 연산을 수행한다.
- 연산하기 전에 두 피연산자 중 더 큰 자료형으로 자료형을 일치 시킨다.
- 정수와 정수 나눗셈 시 정수로 나와야 하므로 소수는 버려지고 정수만 출력된다.

```java
public class ArithmeticOperatorTest {
    public static void main(String[] args) {
        byte byteVal = 10;
        byte byteVal2 = 20;
        // 4byte 이하의 자료형은 int 형으로 변환한 다음 연산을 수행하기 때문에 에러 발생
        // byte byteVal3 = byteVal + byteVal2;
        byte byteVal3 = (byte)(byteVal + byteVal2);     // 결과값을 형변환해야 에러가 안난다.
        System.out.println("result is " + byteVal3);    // result is 30
        
        double doubleVal = 15;
        Object result = byteVal + doubleVal;
        // 자료형이 다를 경우 두 피연산자 중 더 큰 자료형으로 변환 되어 연산이 진행된다.
        System.out.println("instance of Double ? " + (result instanceof Double));
        // instance of Double ? true
        
        int intVal = 7;
        int intVal2 = 3;
        double doubleVal = intVal;

        // 정수와 정수 나눗셈 시 정수로 나와야 하므로 소수는 버려지고 정수만 출력된다.
        System.out.println("result is " + intVal / intVal2);    // result is 2
        System.out.println("result is " + intVal % intVal2);    // result is 1
        System.out.println("result is " + doubleVal / intVal2); // result is 2.3333333333333335
        System.out.println("result is " + doubleVal % intVal2); // result is 1.0
    }
}
```

### Shift 연산자 (`<<`, `>>`, `>>>`)

- 비트를 이동시키는 연산자이다.
- 비트를 직접 다루기 때문에 정수형 데이터에서만 사용 가능하다.
- 2진수로 표현 했을 때 각 자리를 오른쪽, 또는 왼쪽으로 이동시킨다.   
  빈자리의 경우 `<<`, `>>>` 연산자는 0의 값으로 채우고, ~~>> 연산자는 빈자리를 1로 채운다.~~   
  `>>` 연산자는 음수 일 때는 빈자리를 1로 채우고, 양수일 때는 빈자리를 0으로 채운다. ([2진수의 음수 표현법](#2진수의-음수-표현법))

```java
public class ShiftOperatorTest {
    public static void main(String[] args) {
        int intVal = 8;
        int intVal2 = -8;
        int result;
        
        System.out.println("intVal to binary string is " + Integer.toBinaryString(intVal));
        // intVal to binary string is 1000
        
        result = intVal >> 2;
        System.out.println("result is " + result + " to binary string is " + Integer.toBinaryString(result));
        // result is 2 to binary string is 10
        
        result = intVal << 2;
        System.out.println("result is " + result + " to binary string is " + Integer.toBinaryString(result));
        // result is 32 to binary string is 100000
        
        result = intVal >>> 2;
        System.out.println("result is " + result + " to binary string is " + Integer.toBinaryString(result));
        // result is 2 to binary string is 10
        
        System.out.println("intVal2 to binary string is " + Integer.toBinaryString(intVal2));
        // intVal2 to binary string is 11111111111111111111111111111000
        
        result = intVal2 >> 2;
        System.out.println("result is " + result + " to binary string is " + Integer.toBinaryString(result));
        // result is -2 to binary string is 11111111111111111111111111111110
        
        result = intVal2 << 2;
        System.out.println("result is " + result + " to binary string is " + Integer.toBinaryString(result));
        // result is -32 to binary string is 11111111111111111111111111100000
        
        result = intVal2 >>> 2;
        System.out.println("result is " + result + " to binary string is " + Integer.toBinaryString(result));
        // result is 1073741822 to binary string is 111111111111111111111111111110
    }
}
```

### 관계(비교) 연산자 (`>`, `<`, `>=`, `<=`, `==`, `!=`)

- 두 피연산자의 크기와 값을 비교하는 연산자이다.
- 반환값은 `true`, `false` 이다.
- `<`, `>`,  `<=`, `>=` 연산자는 두 피연산자 사이의 크기를 미만, 초과, 이하, 이상으로 비교하며,   
`!=`, `==`은 두 피연산자의 값을 비교하여 같은지 다른지 판별한다.

```java
public class ComparisonOperatorTest {
    public static void main(String[] args) {
        int intVal = 10;
        int intVal2 = 20;

        System.out.println("result is " + (intVal > intVal2));  // result is false
        System.out.println("result is " + (intVal < intVal2));  // result is true
        System.out.println("result is " + (intVal >= intVal2)); // result is false
        System.out.println("result is " + (intVal <= intVal2)); // result is true
        System.out.println("result is " + (intVal == intVal2)); // result is false
        System.out.println("result is " + (intVal != intVal2)); // result is true
    }
}
```

### 논리 연산자

- 논리를 연산하는 연산자이다.
- 조건의 참, 거짓을 비교하는 **조건 논리 연산자** 와 **비트 논리 연산자** 로 나뉜다.

AND, OR, XOR 진리표    
  
|a|b|AND|OR|XOR|
|:---:|:---:|:---:|:---:|:---:|
|0|0|0|0|0|
|0|1|0|1|1|
|1|0|0|1|1|
|1|1|1|1|0|

#### 조건 논리 연산자 (`&&`, `||`)

- 두 피연산자 사이의 조건을 논리 연산 하는 연산자이다.
- 피연산자의 타입은 boolean 값이여야한다.
- `&&` 연산자는 AND 연산, `||` 연산자는 OR 연산이다.
- 반환값은 `true`, `false` 이다.

#### 비트 논리 연산자 (`&`, `|`, `^`)

- 두 피연산자 사이의 비트를 연산 수행하여, 결과를 정수로 반환하는 연산자이다.
- 비트를 직접 다루기 때문에 정수형 데이터에서만 사용 가능하다.
- `&` 연산자는 AND 연산, `|` 연산자는 OR 연산, `^` 연산자는 XOR 연산이다.
- `~` 연산자는 비트 논리 연산자이지만, 단항 연산자이다. ([위쪽에 설명 참조](#비트-연산자-))
- 두 피연산자를 2진수로 변환하여 각 자릿수를 비교한다.

```java
public class LogicalOperatorTest {
    public static void main(String[] args) {
        int intVal = 6;
        int intVal2 = 4;

        System.out.println("result is " + (intVal > intVal2 && intVal <= intVal2)); // result is false
        System.out.println("result is " + (intVal > intVal2 || intVal <= intVal2)); // result is true

        System.out.println("result is " + (intVal & intVal2));  // result is 4
        // 0110 = 6 
        // 0100 = 4
        // 0100 = 4(2진수로 변환한 각 비트의 자리수를 AND 연산 한다.)
        System.out.println("result is " + (intVal | intVal2));  // result is 6
        // 0110 = 6
        // 0100 = 4
        // 0110 = 6(2진수로 변환한 각 비트의 자리수를 OR 연산 한다.)
        System.out.println("result is " + (intVal ^ intVal2));  // result is 2
        // 0110 = 6
        // 0100 = 4
        // 0010 = 2(2진수로 변환한 각 비트의 자리수를 XOR 연산 한다. => 두 값이 다를 때 true)
    }
}
```

### instanceof 연산자 (`instanceof`)

- 객체의 타입을 확인하는 데 사용하는 연산자이다.
- 기본형(Primitive type)에는 사용 할 수 없으며,   
  사용하고 싶으면 primitive type 을 클래스화 한 Wrapper class 를 이용해 사용한다.

```java
public class InstanceOfOperatorTest {
    class Car {}
    class Bus extends Car{}
    
    public static void main(String[] args) {
        Car car = new Car();
        Bus bus = new Bus();

        System.out.println("car instanceof Car : " + (car instanceof Car));
        // car instanceof Car : true
        System.out.println("bus instanceof Car : " + (bus instanceof Car));
        // bus instanceof Car : true
        // 조상 타입의 클래스는 true 를 얻으며,
        System.out.println("car instanceof Bus : " + (car instanceof Bus));
        // car instanceof Bus : false
        // 자손 타입의 클래스는 false 를 얻는다.
        System.out.println("bus instanceof Bus : " + (bus instanceof Bus));
        // bus instanceof Bus : true
        
        // int 형은 primitive type 이라서 instanceof 연산자 사용이 불가능하다.
//        int intVal = 10;
//        boolean result = intVal instanceof Integer;
        
        // primitive type 을 클래스화 한 wrapper class 를 이용해 연산자 사용이 가능하다.
        Integer intVal = 10;
        boolean result = intVal instanceof Integer;
        System.out.println("result is " + result);  // result is true
    }
}
```

### 대입 연산자 (`=`)

- 오른쪽 피연산자의 값 또는 수식을 왼쪽 피연산자에 대입하는 연산자이다.
- 대입 연산자는 다른 산술 연산자와 같이 쓸 수 있다.

```java
public class AssignmentOperatorTest {
    public static void main(String[] args) {
        int intVal = 10;    // intVal 변수에 10을 대입한다.
        
        // 대입 연산자는 산술 연산자와 같이 쓸 수 있다.
        intVal += 10;
        System.out.println("result is " + intVal);  // result is 20
        intVal -= 10;
        System.out.println("result is " + intVal);  // result is 10
        intVal *= 10;
        System.out.println("result is " + intVal);  // result is 100
        intVal /= 10;
        System.out.println("result is " + intVal);  // result is 10
        intVal %= 10;
        System.out.println("result is " + intVal);  // result is 0
    }
}
```

### 화살표 연산자 (`->`)
- Java 8 에서 등장한 연산자로, 메소드를 피연산자로 가지고 있는 연산자이다.
- (매개변수) `->` 메소드 형식으로 이루어져 있으며, 람다식의 기초가 되는 연산자이다.
- Functional Interface 나 람다식의 개념은 후에 다룰 람다식 주제에서 상세하게 다룰 예정이다. ~~아직 공부를 안해서 자세히 모르겠다..~~

## 삼항 연산자

- 피연산자가 세 개 필요한 유일한 연산자이다.
- 조건식 ? 조건식이 true 일 때 값 : 조건식이 false 일 때 값 의 구조로 사용된다.

```java
public class TernaryOperatorTest {
    public static void main(String[] args) {
        int intVal = 10;
        int intVal2 = 20;
        System.out.println("result is " + (intVal > intVal2 ? intVal : intVal2));
        // result is 20
        System.out.println("result is " + (intVal < intVal2 ? intVal : intVal2));
        // result is 10
    }
}
```

# 연산자의 우선 순위

`()` : 우선 순위 변경을 위해 사용하는 연산자

`[]` : 배열의 크기나 첨자를 나타낼 때 사용하는 연산자

이 두 연산자가 최우선 연산자에 속해있으며, 우선 순위는 다음과 같다.

|우선 순위|연산자|내용|
|---|----|---|
|1|최우선 연산자|`()`, `[]`|
|2|[단항 연산자](#단항-연산자)|`!`, `~`, `+`, `-`, `++`, `--`|
|3|[곱셈 / 나눗셈 연산자(산술 연산자)](#산술-연산자------)|`*`, `/`, `%`|
|4|[덧셈 / 뺄셈 연산자(산술 연산자)](#산술-연산자------)|`+`, `-`|
|5|[Shift 연산자](#shift-연산자---)|`>>`, `<<`, `>>>`|
|6|[관계(비교) 연산자 (크기)](#관계비교-연산자------)|`<`, `<=`, `>`, `>=`
|7|[관계(비교) 연산자 (값)](#관계비교-연산자------)|`==`, `!=`|
|8|[비트 AND 논리 연산자](#비트-논리-연산자---)|`&`|
|9|[비트 XOR 논리 연산자](#비트-논리-연산자---)|`^`|
|10|[비트 OR 논리 연산자](#비트-논리-연산자---)|<code>&#124;</code>|
|11|[논리곱 연산자](#조건-논리-연산자--)|`&&`|
|12|[논리합 연산자](#조건-논리-연산자--)|<code>&#124;&#124;</code>|
|13|[삼항 연산자](#삼항-연산자)|`? :`|
|14|[대입 / 복합 대입 연산자](#대입-연산자-)|`=`, `+=`, `-=`, `*=`, `/=`, `%=`, `<<=`, `>>=`, `&=`, `^=`, `~=`|

우선 순위가 같은 연산자는 왼쪽에서 오른쪽으로 결합된다.

# switch 연산자

- Java 12 부터 preview 로 업데이트 된 기능이다.
- Java 13 에서도 preview 지만, `yield` 키워드가 도입 되었고, `switch` 연산의 `case` 안에서 블록(`{}`)을 사용할 때 값을 리턴 할 수 있다.
- `switch` 의 `case` 레이블에서 [화살표 연산자(`->`)](#화살표-연산자--) 를 사용 할 수 있고, 여러 개의 case 를 `,`를 구분자로 나열 할 수 있다.
- case 구문에서 `:` 키워드와 `->` 키워드를 동시에 사용 할 수 없다.

```java
public class SwitchOperatorTest {

    public static void main(String[] args) {
        int[] intArray = {1, 2, 3};
        for(int intVal : intArray) {
            System.out.println("previousVersion12   : " + previousVersion12(intVal));
            System.out.println("version12           : " + version12(intVal));
            System.out.println("afterVersion12      : " + afterVersion12(intVal));
        }
        for(int intVal : intArray) {
            // 이전 시간에 배웠던 타입 추론과도 같이 쓸 수 있다.
            var anonymousVal = switch(intVal) {
                case 1 -> "String value";
                case 2 -> 10;
                case 3 -> 'A';
                default -> throw new IllegalStateException("Unexpected value: " + intVal);
            };
            Object result = anonymousVal;
            System.out.println("typeInference       : result is " + anonymousVal + " and type is " + result.getClass().getName());
        }
    }

    /**
     * Java 12 버전 이전의 switch 문
     * @param intVal
     * @return
     */
    public static String previousVersion12(int intVal) {
        String result;
        switch(intVal) {
            case 1:
                result = "result is " + 1;
                break;
            case 2:
                result = "result is " + 2;
                break;
            case 3:
                result = "result is " + 3;
                break;
            default:
                throw new IllegalStateException("Unexpected value: " + intVal);
        }
        return result;
    }

    /**
     * Java 12 버전 에서의 switch 연산자 (preview)
     * @param intVal
     * @return
     */
    public static String version12(int intVal) {
        String result = switch (intVal) {
            case 1, 2, 3 -> "result is " + intVal;
            default -> throw new IllegalStateException("Unexpected value: " + intVal);
        };
        return result;
    }

    /**
     * Java 13 버전 에서의 switch 연산자 (preview)
     * @param intVal
     * @return
     */
    public static String afterVersion12(int intVal) {
        // switch 연산자를 직접 return 하는 등 자유롭게 사용 할 수 있다.
        return switch(intVal) {
            case 1, 2, 3 -> {
                if(intVal > 1) {
                    yield "result is " + intVal + " and intVal > 1";
                }else {
                    yield "result is " + intVal + " and intVal <= 1";
                }
            }
            default -> throw new IllegalStateException("Unexpected value");
        };
    }
}
```

```
previousVersion12   : result is 1
version12           : result is 1
afterVersion12      : result is 1 and intVal <= 1
previousVersion12   : result is 2
version12           : result is 2
afterVersion12      : result is 2 and intVal > 1
previousVersion12   : result is 3
version12           : result is 3
afterVersion12      : result is 3 and intVal > 1
typeInference       : result is String value and type is java.lang.String
typeInference       : result is 10 and type is java.lang.Integer
typeInference       : result is A and type is java.lang.Character
```

# 추가로!

## 2진수의 음수 표현법

2진수에서 **최상위비트(MSB : Most Significant Bit)** 를 부호비트라고 한다.   
부호비트가 0이면 양수, 1이면 음를 나타낸다.

10진수 1이 2진수에서는 00000001 이다. (1byte 기준)   
그렇다면 10진수 -1은 2진수에서 어떻게 표현될까?

위에서 설명한대로만 쓴다면 10000001이 맞겠지만, 10진수 -1의 2진수는 11111111이다.

## 2진수 음수 계산

10진수 1과 -1이 있다. 두개를 더하면(1 + (-1)) 0이 된다.   
이런 식으로 00000001과 더해서 0이 되는 수가 -1이 된다는 것이고,

```
  00000001
 +11111111
 ---------
 100000000
 
1byte 기준 범위를 초과하는 자리의 값은 제외 시키기 때문에 결과는 제일 앞의
1을 버린 00000000 => 10진수로 표현하면 0이 된다.
```
-1을 2진수로 표현하면 11111111 이 된다는 것을 알 수 있다.

## 보수란?

보수는 각 자리의 숫자의 합이 어느 일정한 수가 되게 하는 수이다.   
각 진법의 보수에는 같은 수의 보수법과 같은 수 -1의 보수법이 있다.   

10진수 7에 대한 10의 보수라고 하면 7과 더해서 10이 되는 수를 의미한다.   
따라서 10진수 7에 대한 10의 보수는 3이라는 것을 알 수 있게 되고,   
같은 방식으로, 10진수 7에 대한 9의 보수는 2라는 것도 알 수 있게 된다.

위 두 방식의 관계를 보게 되면, 10진수에 대한 10의 보수는 9의 보수 + 1 과 같다는 걸 알 수 있다.

이 방식을 2진법에 적용하게 되면, 2진법에 어떤 수에 대한 1의 보수는   
모든 자릿수의 값을 반대로 뒤집어 주면 나온다.   
즉, 2진수 00000001 에 대한 1의 보수는 각 자릿수의 값을 뒤집은 11111110 이고,
여기에 1을 더한 11111111이 2진수 00000001에 대한 2의 보수가 된다.

따라서 2진수에서 2의 보수는 더해서 0이 되는 수가 된다.

:: 모든건 컴퓨터의 2진법 계산을 기준으로 작성되었고, 1byte 를 기준으로 작성되었다.

> 웹문서
> - [자바(Java)의 기초 박살내기 - 연산자(Operator)](https://raccoonjy.tistory.com/9)
> - [자바의 연산자 및 연산자 우선순위](https://toma0912.tistory.com/66)
> - [자바(Java) instanceof 사용법](https://improver.tistory.com/140)
> - [[Java-21]화살표 연산자(->)그리고 람다 원리](https://catch-me-java.tistory.com/30)
> - [3 Switch Expressions](https://docs.oracle.com/en/java/javase/13/language/switch-expressions.html)
> - [[기본원리] 2진수의 음수표현법!](https://namsieon.com/229)
> - [컴퓨터의 음수 표현법(보수법, 컴퓨터 뺄셈 연산의 원리)](https://zapiro.tistory.com/entry/%EC%BB%B4%ED%93%A8%ED%84%B0%EC%9D%98-%EC%9D%8C%EC%88%98-%ED%91%9C%ED%98%84%EB%B2%95%EB%B3%B4%EC%88%98%EB%B2%95)