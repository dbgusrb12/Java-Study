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


> 웹문서
> - [The Java Tutorials(I/O)](https://docs.oracle.com/javase/tutorial/essential/io/)
> - [[개념정리] 버퍼(BUFFER)란? 버퍼 개념](https://dololak.tistory.com/84)