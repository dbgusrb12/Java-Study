I/O
===

# I/O (Input/Output) 란?

I/O 란 Input / Output 의 약자로,   
자바 어플리케이션을 기준으로 읽어 오는 것을 Input 이라고 하고,   
내보내는 것을 Output 이라고 한다.

# 스트림 (Stream) 이란?

자바 어플리케이션과 외부 장치 사이에 데이터를 주고 받을 때 그 사이를 연결해주는   
다리 역할이 필요한데, 그 역할을 해주는 것을 `스트림(stream)` 이라고 한다.

`스트림(stream)` 은 단방향 통신이기 때문에,   
Input 을 위해서는 입력 관련 stream 이 필요하고,   
Output 을 위해서는 출력 관련 stream 이 필요하다.

어떤 데이터를 다루는지에 따라 많은 종류의 stream 이 있어,   
상황에 맞는 stream 을 사용 할 수 있다.

# 버퍼 (Buffer) 란?

Java 의 입출력에서 사용되는 `버퍼(buffer)`는 대부분 CPU 와 보조기억장치 사이에서 사용되는   
임시 저장 공간을 의미한다.

기술이 발전함에 따라 CPU 는 속도가 빨라졌지만, 보조 기억 장치는 그대로인 상황에서   
입출력 전송의 속도 차이가 발생 하고, 그것을 보완하기 위해 사용한다.

버퍼는 CPU 내부에 있는 캐시메모리 보다는 느리지만 보조 기억 장치보다 훨씬 빠른 주기억 장치(RAM)를   
이용하며, 보조 기억 장치는 주 기억 장치의 버퍼로 마련해둔 공간에 데이터를 보내 쌓아 둔다.   
CPU 는 처리가 빠르므로 밀려있는 다른일을 처리한 후 시간이 남을때 가끔 버퍼를 확인하여 데이터가   
모두 쌓였는지 확인하고 모두 쌓였다면 가져와 한꺼번에 처리한다.

이런 방법을 사용해 CPU 는 100퍼센트의 효율로 연산을 할 수 있게 된다.

# 채널 (channel) 이란?

`채널(channel)` 은 스트림과 비슷한 개념이지만, 스트림과는 다르게 양방향 통신이다.

버퍼를 기반으로 데이터를 주고 받으며, 입력/출력을 동시에 할 수 있다.

Java 4 부터 버퍼 기반의 채널 방식으로 데이터를 입/출력 하는 클래스가 등장했다.

# `InputStream` 과 `OutputStream`

단방향 통신인 스트림 (stream) 중   
입력 관련 스트림을 `InputStream` 이라고 하고,   
출력 관련 스트림을 `OutputStream` 이라고 한다.

스트림도 자원의 일종이기 때문에 사용하고 난 뒤 꼭 `close` 메서드로 자원을 닫아줘야 한다.

## `InputStream`

`InputStream` 은 입력 관련 스트림을 가리키는 개념적인 의미로도 쓰이지만,   
모든 입력 관련 스트림들의 최상위 클래스이기도 하다.

추상 클래스인 `InputStream` 클래스는 바이트 기반으로 데이터를 주고받으며,   
다형성을 이용해 자식 클래스의 스트림 인스턴스를 생성하여 상황에 맞는 `InputStream` 을 사용 할 수 있다.

`read` 메서드로 해당 데이터를 바이트 단위로 읽어 올 수 있으며,   
매개변수에 `Byte[]` 을 집어넣어 기본 읽어오는 데이터를 늘릴 수 있다.

대표적인 `InputStream` 의 종류는 다음과 같다.

### `FileInputStream`

파일에서 바이트 단위로 자료를 읽는 스트림이다.

### `ByteArrayInputStream`

바이트 배열 메모리에서 바이트 단위로 자료를 읽는 스트림이다.

### `PipedInputStream`

멀티 쓰레드에서 사용하는 스트림으로, 한 쓰레드에서 읽어 들인 데이터를   
다른 쓰레드에게 전달 할 때 사용한다.

해당 스트림의 특성 때문에 `PipedInputStream` 과 `PipedOutputStream` 이   
한 세트가 되어 실행된다.

## `OutputStream`

`OutputStream` 은 출력 관련 스트림을 가리키는 개념적인 의미로도 쓰이지만,   
모든 출력 관련 스트림들의 최상위 클래스이기도 하다.

`OutputStream` 역시 `InputStream` 과 같은 추상클래스 이며, 바이트 기반의 데이터를 주고받는다.

`OutputStream` 에서는 `write` 메서드로 해당 데이터를 바이트 단위로 쓸 수 있으며,   
매개변수에 `Byte[]` 을 집어넣어 기본 쓰는 데이터를 늘릴 수 있다.

대표적인 `OutputStream` 의 종류는 다음과 같다.

### `FileOutputStream`

`FileInputStream` 과 한 쌍으로,   
파일에서 바이트 단위로 자료를 쓰는 스트림이다.

### `ByteArrayOutputStream`

`ByteArrayInputStream` 과 한 쌍으로,   
바이트 배열 메모리에서 바이트 단위로 자료를 쓰는 스트림이다.

### `PipedOutputStream`

`PipedInputStream` 과 한 세트로,   
이 클래스는 쓰레드 사이에서 `PipedInputStream` 에서 읽은 데이터를   
`PipedOutputStream` 으로 내보내기 때문에 `PipedInputStream` 과 항상   
같이 쓰인다.


# Byte 와 Character 스트림

데이터를 어떤 단위 기반으로 입/출력을 하는지에 따라서 종류가 나뉘는데,   
입출력의 단위가 1byte 라면 Byte Stream 이라고 하고,   
입출력의 단위가 2byte(`char`형) 라면 Character Stream 이라고 한다.

## Byte Stream

Java 의 스트림 클래스들은 기본적으로 Byte 단위로 스트림을 전송하며,   
모든 바이트 기반 스트림은 `InputStream` 과 `OutputStream` 클래스의 자손들이다.

위에서 살펴봤던 스트림들 모두 데이터의 단위가 1byte 기반인 Byte Stream 이다.

## Character Stream

Java 에서 가장 작은 타입은 `char` 타입으로, 2byte 이다.

바이트 기반 스트림에서는 1byte 씩 주고 받기 때문에 문자를 처리하기 용이하지 않다는 단점이 있다.

이 단점을 해결하기 위해 Java 에서는 문자 기반 스트림을 지원한다.   
문자 기반 스트림 (Character Stream) 은 오직 문자 기반으로 된 데이터를 처리하기 위해   
만들어진 스트림이다.

이 스트림의 최상위 클래스에는 `Reader`, `Writer` 가 있다.

바이트 기반 스트림의 `InputStream` 이 `Reader` 이고,   
바이트 기반 스트림의 `OutputStream` 이 `Writer` 이다.

대표적인 `Reader`, `Writer` 의 종류는 다음과 같다.

### `FileReader` , `FileWriter`

바이트 기반 스트림의 `FileInputStream` 과 `FileOutputStream` 이랑   
같은 기능을 하는 클래스이고, 문자 기반 스트림이다.

### `CharArrayReader` , `CharArrayWriter`

바이트 기반 스트림의 `ByteArrayInputStream` 과 `ByteArrayOutputStream` 이랑   
같은 기능을 하는 클래스이고, 문자 기반 스트림이다.

### `PipedReader` , `PipedWriter`

바이트 기반 스트림의 `PipedInputStream` 과 `PipedOutputStream` 이랑   
같은 기능을 하는 클래스이고, 문자 기반 스트림이다.

# 보조 스트림

스트림의 기능을 보완하기 위해 나온 보조스트림은,   
그 자체로 스트림의 기능을 하지 못하지만, 스트림을 기반으로 생성을 해 스트림의   
기능을 향상 시키거나, 새로운 기능을 추가 할 수 있다.

Java 의 Stream 중 생성자에 다른 스트림이 들어가는 생성자만 존재하는 클래스가   
보조 스트림이다.

```java
/**
 * BufferedInputStream 클래스의 생성자만 가져온 클래스이다.
 */
public class BufferedInputStream extends FilterInputStream {
    public BufferedInputStream(InputStream in) {
        this(in, DEFAULT_BUFFER_SIZE);
    }

    public BufferedInputStream(InputStream in, int size) {
        super(in);
        if (size <= 0) {
            throw new IllegalArgumentException("Buffer size <= 0");
        }
        buf = new byte[size];
    }
}
```

# 표준 스트림 (`System.in`, `System.out`, `System.err`)

자바에서는 콘솔과 같은 표준 입출력 장치를 위한 스트림을 미리 정의해 놓았는데,   
거기에 해당하는 클래스가 `System` 클래스 이다.

`System` 클래스 내부에 `static final` 로 선언된 스트림 변수 3가지가 있는데,   
`InputStream` 클래스 타입인 `in`,   
`PrintStream` 클래스 타입인 `out` 과 `err` 가 있다.

`System.in` 은 콘솔에서 자바 어플리케이션으로 읽어들일때 사용하고,   
`System.out` 은 자바 어플리케이션에서 콘솔로 내보낼 때 사용한다.
`System.err` 역시 `out` 과 같이 내보낼 때 사용하지만, 에러를 표시할 때 사용하므로   
글자의 색깔이 다르다.

# 파일 읽고 쓰기

파일을 읽고 쓸 때 사용하는 스트림은 여러가지가 있는데,   
그 중 메인 스트림으로는 `FileReader`, `FileWriter` 를 사용하고,   
보조 스트림 으로는 `BufferedReader`, `BufferedWriter` 를 사용하여 파일 복사 예시를 만들어 보았다.

- OutFile.txt
```text
테스트를 위한 파일입니다.
이 파일의 복사본을 만들 예정입니다.
복사본은 해당 파일과 같은 형식입니다.
```

- java 파일
```java
public class FileStreamTest {
    public static void main(String[] args) {
        String originFileName = "OutFile.txt";
        String copyFileName = "OutFile_copy.txt";

        try (
            BufferedReader in = new BufferedReader(new FileReader(originFileName));
            BufferedWriter out = new BufferedWriter(new FileWriter(copyFileName));
        ) {
            String line = "";
            while((line = in.readLine()) != null) {
                out.write(line);
                out.newLine();
            }
            System.out.println("복사 완료!");
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

실행 한 결과 OutFile_copy.txt 라는 파일이 생겼고, 내용을 보면

- OutFile_copy.txt
```text
테스트를 위한 파일입니다.
이 파일의 복사본을 만들 예정입니다.
복사본은 해당 파일과 같은 형식입니다.
```

같다는 것을 확인 할 수 있다.

> 웹문서
> - [The Java Tutorials(I/O)](https://docs.oracle.com/javase/tutorial/essential/io/)
> - [[개념정리] 버퍼(BUFFER)란? 버퍼 개념](https://dololak.tistory.com/84)