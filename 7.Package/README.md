Package
===

# 패키지(package) 란?

패키지란 클래스, 인터페이스등의 자바 파일 그룹화를 위해 사용된다.   
물리적으로 하나의 디렉토리를 의미하며, 계층 구조는 `.`으로 구분된다.

### FQCN(Fully Qualified Class Name)

- 패키지는 클래스의 일부분이고, 클래스를 유일하게 만들어주는 식별자 역할을 한다.   
- 같은 이름의 클래스일지라도, 패키지가 다르면 충돌을 피할 수 있다.

### Naming Conventions

- 패키지 이름은 모두 소문자여야 한다. (클래스나, 인터페이스 등의 이름과 충돌나지 않기 위해)
- 숫자로 시작하면 안된다.
- 자바의 예약어를 사용하면 안된다. (`int`, `static`)
- `java`, 또는 `javax` 같은 패키지는 자바 언어 자체의 패키지 이므로 사용 할 수 없다.
- 일반적으로 회사의 도메인을 뒤집고, 프로젝트 이름을 쓰는 식으로 패키지를 시작한다.   
  (example.com => com.example.my_project_name)
- `$` , `_`를 제외한 특수 문자는 사용 할 수 없다.

# `package` 키워드

- 소스 파일의 첫번째 행에 선언되어야 한다.
- 패키지 선언은 소스파일 하나당 한개만 있어야 한다.
- 패키지 이름과 위치한 폴더의 이름이 같아야한다.
- `package` 키워드가 선언되지 않은 소스파일은 이름 없는 패키지로 끝난다.   
  소규모 또는 임시 응용 프로그램용이거나 개발 프로세스를 막 시작할 때만 사용할 수 있다.

```java
package com.example.hyungyu;

public class PackageTest {
}
```

## `package` 키워드가 포함된 클래스 컴파일

패키지가 선언된 클래스 파일을 명령어로 컴파일 할 때에는   

`javac 자바파일`  

이 아닌   

`javac -d 경로명 자바파일`

로 클래스 파일이 들어갈 경로를 지정해야 패키지 폴더가 자동으로 생성된다.

```
javac -d . ClassName.java               // 현재 폴더에 생성
javac -d ../bin ClassName.java          // 현재 폴더와 같은 디렉토리 레벨인 bin 폴더에 생성
javac -d ~/Desktop/test ClassName.java  // 지정한 폴더에 생성
```

```java
package com.test.compile_test;

/**
 * 자바 컴파일을 위한 클래스입니다.
 */
public class CompileTest {
    public static void main(String[] args) {
        System.out.println("컴파일 테스트입니다.");
    }
}
```
```zsh
~/Desktop/workspace/compile_test ❯ javac -d . ../test/CompileTest.java
~/Desktop/workspace/compile_test ❯ cd com/test/compile_test
~/Desktop/workspace/compile_test/com/test/compile_test ❯ java CompileTest   
오류: 기본 클래스 CompileTest을(를) 찾거나 로드할 수 없습니다.
원인: java.lang.NoClassDefFoundError: com/test/compile_test/CompileTest (wrong name: CompileTest)

~/Desktop/workspace/compile_test ❯ java com.test.compile_test.CompileTest
컴파일 테스트입니다.
```

위 코드를 보면 compile_test 라는 디렉토리에서 만들어놓은 .java 파일을 컴파일한다.   
자바는 가장 상위 디렉토리(root) 에서 실행해야 한다는 약속이 있기 때문에,   
컴파일을 한 디렉토리가 아닌, 해당 하위 패키지에서 실행을 할 경우 에러가 나는 것을 볼 수 있다.

따라서, 최상위 루트 디렉토리에서 `java 패키지명.클래스명` 으로 실행을 해야한다.

# `import` 키워드

`import` 키워드는 다른 패키지의 클래스들을 가져와서 사용할 때 쓰는 키워드이다.   
같은 패키지나 `java.lang` 패키지의 클래스는 따로 가져올 필요 없이 소스 코드에서   
사용 할 수 있다.

다른 패키지의 클래스를 가져오는 방법은 두가지가 있다.

## FQCN(Fully Qualified Class Name) 으로 가져오기

```java
package com.example.keyword;

/**
 * import 키워드를 사용하지 않고 
 * 다른 패키지의 클래스를 사용 할 때
 */
public class ImportKeywordTest {
    public static void main(String[] args) {
        com.example.import_keyword.ExportClass exportClass = new ExportClass();
        System.out.println(exportClass);
    }
}

package com.example.import_keyword;

public class ExportClass {
    
    String name;
    
    String age;
    
}
```

## `import` 키워드를 사용해 가져오기

가져와야 하는 클래스의 `import 풀패키지명.클래스명;` 으로 가져 올 수 있으며,   
`package` 키워드의 뒤에 오게된다.

asterisk(`*`) 라는 와일드 카드를 사용 할 수 있고, 해당 패키지의 모든 클래스를 가져온다.   
와일드 카드를 사용 할 경우, 해당 패키지에 속해 있는 클래스만 모두 가져오는 것이기 때문에   
해당 패키지의 하위 패키지에 속해 있는 클래스들은 가져오지 못한다.

패키지의 구조는 계층적으로 보이지만, 구분을 위한 구조이고, 각각 별개의 패키지이다.   
파일 디렉토리로 봤을 때는 `java.awt` 폴더 하위에 `color` 폴더가 포함되어 있지만,   
패키지 구조로 봤을 때 `java.awt` 패키지와 `java.awt.color` 패키지는 서로 다른 패키지이다.

```java
package com.example.keyword;

// com.example.import_keyword 패키지에 있는 ExportClass를 가져온다.
import com.example.import_keyword.ExportClass;

// import_keyword 패키지에 속해있는 클래스를 모두 가져온다.
import com.example.import_keyword.*;

/**
 * import 키워드를 사용해 다른 패키지의 클래스를 사용 할 때
 */
public class ImportKeywordTest {
    public static void main(String[] args) {
        ExportClass exportClass = new ExportClass();
        System.out.println(exportClass);

//        Child child = new Child();
        // Child 클래스는 com.example.import_keyword 패키지가 아닌 그 하위에
        // com.example.import_keyword.child 패키지에 있으므로
        // 컴파일 에러가 난다.
    }
}

package com.example.import_keyword;

public class ExportClass {
    
    String name;
    
    int age;
    
}

package com.example.import_keyword.child;

public class Child {
    
    String name;
    
    int age;

}
```

## `static` 자원 가져오기

`static` 자원도 `import static` 키워드로 가져 올 수 있다.

```java
package com.example.hyungyu;

import static com.company.hg.test.StaticClass.staticMethod;
import static com.company.hg.test.StaticClass.CLASS_NAME;

public class PackageTest {
    public static void main(String[] args) {
        System.out.println(CLASS_NAME);
        staticMethod();
    }
}

package com.example.hg.test;

public class StaticClass {
    public static final String CLASS_NAME = "StaticClass";
    
    public static void staticMethod() {
        System.out.println("static method");
    }
}
```

```
StaticClass
static method
```

# Classpath

클래스패스란 JVM 이나 자바 컴파일러에서 사용하는 파라미터로,   
클래스나 패키지를 찾을때 사용하는 경로를 의미한다.

앞서 말했던 것처럼 자바는 가장 상위 디렉토리(root) 에서 실행해야 한다는 약속이 있다.   
그래서 이 상위 디렉토리를 지정해 주는 변수가 바로 classpath 인 것이다.

기본적으로 클래스패스를 따로 설정하지 않으면 현재 디렉토리가 지정되어 클래스를 찾아나간다.

JVM 의 클래스 로더는 클래스패스를 기준으로 클래스 파일들을 찾아나가며   
프로젝트 마다 루트 디렉토리가 다르고, 라이브러리가 들어있는 디렉토리 역시 다르기 때문에   
여러 경로들을 콜론(`:`) 또는 세미콜론(`;`)을 구분 값으로 나뉘어진다.

unix 기반 OS는 콜론(`:`), window 는 세미콜론(`;`)을 구분 값으로 가진다.


클래스패스를 설정 하는 방법은 두가지가 있는데,

## CLASSPATH 환경 변수를 사용
 
CLASSPATH 라는 환경 변수를 만들어서 JVM 의 클래스 로더로 하여금 이 환경 변수를   
참조해 클래스를 찾는다.

### MAC OS

mac 에서 CLASSPATH 환경 변수를 설정 할 때에는   
직접 설정을 하거나, 파일을 만들어 관리 하는 방법이 있다.

#### 직접 명령어를 이용해 설정
```zsh
~ > export CLASSPATH={경로명}:{경로명}
```

#### 파일을 이용해 설정

- 설정을 적용 할 파일을 vi 에디터로 만든다.

```
// sample_profile 파일

export CLASSPATH={경로명}:{경로명}
```

- source 명령어를 이용해 파일 실행 및 환경 변수 적용

```zsh
~ > source {경로명}/파일명
```

### Window

window 에서 CLASSPATH 환경 변수를 설정 할 때에는

1. 내 컴퓨터 에서 속성 OR 제어판 시스템 등록 정보
2. 고급탭 클릭
3. 환경 변수 버튼 클릭
4. 시스템 변수에서 아래의 두 변수가 있다면 편집, 없다면 새로 만들기 버튼 클릭
5. JAVA_HOME 환경 변수 설정
```
변수 이름 : JAVA_HOME
변수 값 : JDK 가 설치되어 있는 폴더 전체 경로
```
6. CLASSPATH 환경 변수 설정
```
변수 이름 : CLASSPATH
변수 값 : .;%JAVA_HOME%\lib\tools.jar
```

기본적인 Java 설치 후 환경 변수 설정을 예시로 한 것이며,   
변수 값에 대한 설명은   
```
. : 현재 디렉토리 경로부터 찾는다.
; : 구분자
%JAVA_HOME% : 미리 설정해 놓은 JAVA_HOME 환경 변수의 값을 대입한다.
\lib\tools.jar : 기본적인 jdk 라이브러리의 루트 디렉토리를 설정하여, java 라이브러리 클래스들을 찾는다.
윈도우에서 디렉토리 경로는 백슬래시(\)로 구분한다.
```

CLASSPATH 에 새로운 경로를 추가하고 싶다면, 맨 뒤에 `;`을 구분자로 한 뒤   
원하는 경로를 입력해주면 된다.

## -classpath 옵션을 사용

`java` 나 `javac` 명령어를 사용 할 때 `-classpath` 옵션을 사용해   
classpath 를 설정하는 방법이다.

### 명령어로 컴파일 할 때

`javac -classpath 경로명 자바파일`   

로 클래스패스로 설정할 경로를 지정하여 컴파일 할 수 있다.

### 명령어로 클래스 파일을 실행 할 때   

`java -classpath 경로명 클래스파일`   

로 클래스패스로 설정할 경로를 지정하여 실행 할 수 있다.

경로명은 앞서 말했던 것과 마찬가지로   
unix 기반 OS는 콜론(`:`), window 는 세미콜론(`;`)을 구분 값으로 가진다.

`-classpath` 옵션은 줄여서 `-cp` 로도 사용 할 수 있으며, 기능은 그대로이다.

## 우선순위

환경변수 CLASSPATH 와 java 옵션 `-classpath` 가 둘 다 설정되어 있으면   
java 옵션인 `-classpath` 가 적용된다.

> 웹문서
> - [9. Java 자바 - 패키지(package)](https://kephilab.tistory.com/52)
> - [The Java Tutorials(Packages)](https://docs.oracle.com/javase/tutorial/java/package/index.html)
> - [7주차 과제: 패키지](https://kils-log-of-develop.tistory.com/430)
> - [macOS Java 환경변수(PATH) 설정 방법](https://whitepaek.tistory.com/28)
> - [[JAVA] javac 및 java 실행시 주의사항](https://kjk3071.tistory.com/entry/JAVA-javac-%EB%B0%8F-java-%EC%8B%A4%ED%96%89%EC%8B%9C-%EC%A3%BC%EC%9D%98%EC%82%AC%ED%95%AD)