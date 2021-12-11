---
title: "[자바의 정석] 8장 예외처리(exception handling)"
category: POST
tag: Java
toc: true
excerpt: ""
---

## 1. 예외처리(exception handling)

### 1.1 프로그램 오류

_오작동을 하거나 비정상적으로 종료되는 경우 이러한 결과를 초래하는 원인을 프로그램 오류 또는 에러라고 한다._

**에러의 종류**

1. 컴파일 에러 - 컴파일 시에 발생하는 에러
2. 런타임 에러 - 실행 시에 발생하는 에러
3. 논리적 에러 - 실행은 되지만, 의도와 다르게 동작하는 것

- 자바는 실행 시 발생할 수 있는 프로그램 오류를 '에러'와 '예외', 두 가지로 구분하였다.  
  **에러** - 프로그램 코드에 의해서 수습될 수 없는 심각한 오류  
  **예외** - 프로그램 코드에 의해서 수습될 수 있는 다소 미약한 오류

### 1.2 예외 클래스의 계층구조

- 자바에서는 실행 시 발생할 수 있는 오류(Exception과 Error)를 클래스로 정의하였다.
- Exception과 Error클래스 역시 Object 클래스의 자손이다.
- 모든 예외의 최고 조상은 Exception클래스이며, 예외 클래스들은 두 그룹으로 나눠질 수 있다.

  1.  Exception클래스와 그 자손들
  2.  RuntimeException클래스와 그 자손들

- Exception 클래스들은 주로 **사용자의 실수와 같은 외적인 요인에 의해 발생하는 예외**
  - 존재하지 않은 파일의 이름 입력 - FileNotFoundException
  - 실수로 클래스의 이름을 잘못 적음 - ClassNotFoundException
  - 입력한 데이터 형식이 잘못된 경우 - DataFormatException 등
- RuntimeException클래스들은 주로 **프로그래머의 실수에 의해서 발생할 수 있는 예외**
  - 배열의 범위를 벗어남 - ArrayIndexOutOfBoundsException
  - 값이 null인 참조변수의 멤버를 호출 - NullPointerException
  - 클래스간의 형변환 실수 - ClassCastException
  - 정수를 0으로 나눔 - ArithmeticException 등

### 1.3 예외처리하기 - try-catch문

실행도중 발생하는 에러는 어쩔 수 없지만, 예외는 프로그래머가 이에 대한 처리를 미리 해주어야 한다.

#### 예외처리

**정의** - 프로그램 실행 시 발생할 수 있는 예외에 대비한 코드를 작성하는 것  
**목적** - 프로그램의 비정상 종료를 막고, 정상적인 실행상태를 유지하는 것

> 에러와 예외는 모두 실행 시(runtime) 발생하는 오류이다.

- 발생한 예외를 처리하지 못하면, 프로그램은 비정상적으로 종료되며, 처리되지 못한 예외는 JVM의 '예외처리기'가 받아서 에러의 원인을 화면에 출력

#### try-catch문

```java
try {
		//예외가 발생할 가능성이 있는 문장
} catch (Exception1 e1) {
		//Exception1이 발생 시, 처리하기 위한 문장
} catch (Exception2 e2) {
		//Exception2이 발생 시, 처리하기 위한 문장
}
```

- 하나의 try 블럭 다음에는 여러 종류의 예외를 처리할 수 있도록 하나 이상의 catch블럭이 올 수 있으며, 이 중 발생한 예외의 종류와 일치하는 단 한 개의 catch블럭만 수행
- 일치하는 catch블럭이 없으면 예외는 처리되지 않는다.
- if문과 달리, try블럭이나 catch블럭 내에 포함된 문장이 하나뿐이어도 괄호 생략 불가
- try블럭 또는 catch블럭에 또 다른 try-catch문이 포함될 수 있다.
- 그러나 catch블럭 내에 또 하나의 try-catch문이 포함된 경우, 같은 이름의 참조변수를 사용해서는 안 된다.

### 1.4 try-catch문에서의 흐름

- try블럭 내에서 예외가 발생한 경우,
  1.  발생한 예외와 일치하는 catch블럭이 있는지 확인
  2.  일치하는 catch블럭을 찾게 되면, catch블럭 내의 문장들을 수행하고 전체 try-catch문을 빠져나가서 그 다음 문장을 계속해서 수행
- try블럭 내에서 예외가 발생하지 않은 경우,
  1.  catch블럭을 거치지 않고 전체 try-catch문을 빠져나가서 수행을 계속

_try블럭에서 예외가 발생하면, 예외가 발생한 위치 이후에 있는 try블럭의 문장들은 수행되지 않으므로, try블럭에 포함시킬 코드의 범위를 잘 선택해야 한다._

### 1.5 예외의 발생과 catch블럭

1. 예외가 발생하면, 발생한 예외에 해당하는 클래스의 인스턴스가 만들어 진다.
2. 첫 번째 catch블럭부터 차례로 내려가면서 catch블럭의 괄호()내에 선언된 참조변수의 종류와 생성된 예외클래스의 인스턴스에 instanceof연산자를 이용해서 검사결과가 true인 catch블럭을 만날 때까지 검사한다.
3. 검사결과가 true인 catch블럭을 찾게 되면 블럭에 있는 문장들을 모두 수행하고 try-catch문을 빠져나간다.

**모든 예외 클래스는 Exception클래스의 자손이므로, catch블럭의 괄호()에 Exception클래스 타입의 참조변수를 선언해 놓으면 어떤 종류의 예외가 발생하더라도 이 catch블럭에 의해서 처리된다.**

#### printStackTrace()와 getMessage()

_예외가 발생했을 때 생성되는 예외 클래스의 인스턴스에는 발생한 예외에 대한 정보가 담겨있고, getMessage()와 printStackTrace()를 통해서 이 정보들을 얻을 수 있다._

**printStackTrace()** - 예외발생 당시의 호출스택에 있었던 메서드의 정보와 예외 메시지를 화면에 출력  
 **getmessage()** - 발생한 예외클래스의 인스턴스에 저장된 메시지를 얻을 수 있다.

#### 멀티 catch블럭

_JDK 1.7부터 여러 catch블럭을 '|'기호를 이용해서, 하나의 catch블럭으로 합칠 수 있게 되었으며, 이를 '멀티 catch블럭'이라 한다._

```java
try {
} catch (ExceptionA | ExceptionB) {
	e.printStackTrace();
}
```

- `|`기호로 연결할 수 있는 예외 클래스의 개수에는 제한이 없다.
- 연결된 예외 클래스가 조상과 자손의 관계에 있다면 컴파일 에러가 발생한다.  
  -> 불필요한 코드는 제거하라는 의미
- 멀티 catch는 하나의 catch블럭으로 여러 예외를 처리하는 것이기 때문에, 멀티 catch블럭 내에서는 실제로 어떤 예외가 발생한 것인지 알 수 없다. 그래서 예외 클래스들의 공통 분모인 조상 예외 클래스에 선언된 멤버만 사용할 수 있다.

### 1.6 예외 발생시키기

1. 먼저, 연산자 new를 이용해서 발생시키려는 예외 클래스를 만든 다음  
   `Exception e = new Exception("고의로 발생시켰음");`
2. 키워드 throw를 이용해서 예외를 발생시킨다.  
   `throw e;`

- Exception인스턴스를 생성할 때, 생성자에 String을 넣어 주면, Exception인스턴스에 메시지로 저장된다.
- Exception클래스들이 발생할 가능성이 있는 문장들에 대해 예외처리를 해주지 않으면 컴파일조차 되지 않는다.
- RuntimeException클래스들에 해당하는 예외는 프로그래머에 의해 실수로 발생하는 것들이기 때문에 예외처리를 강제하지 않는다.  
  -> 만일 예외처리를 필수로 해야 한다면, 참조 변수와 배열이 사용되는 모든 곳에 예외처리를 해줘야 할 것이다.

**컴파일러가 예외처리를 확인하지 않는 RuntimeException클래스들은 'unchecked예외'라고 부르고, 예외처리를 확인하는 Exception클래스들은 'checked예외'라고 부른다.**

### 1.7 메서드에 예외 선언하기

_예외를 처리하는 방법에는 try-catch문을 사용하는 것 외에, 예외를 메서드에 선언하는 방법이 있다._

```java
void method() throws Exception1, Exception2, ... ExceptionN {
	//메서드 내용
}
```

- 메서드에 예외를 선언하려면, 메서드의 선언부에 키워드 throws를 사용해서 메서드 내에서 발생할 수 있는 예외를 적어주기만 하면 된다.
- 모든 예외의 최고 조상인 Exception클래스를 메서드에 선언하면, 이 메서드는 모든 종류의 예외가 발생할 가능성이 있다는 뜻
- 예외를 선언하면, 이 예외뿐만 아니라 그 자손타입의 예외까지도 발생할 수 있기에, 오버라이딩할 때는 단순히 선언된 예외의 개수가 아니라 상속관계까지 고려해야 한다.  
  -> 메서드의 선언부에 예외를 선언함으로써 메서드를 사용하려는 사람이 선언부를 보았을 때, 이 메서드를 사용하기 위해서는 어떠한 예외들이 처리되어져야 하는지 쉽게 알 수 있다.

- Exception클래스의 자손은 반드시 처리해주어야 하는 예외이다.
- RuntimeException클래스의 자손은 예외처리를 해주지 않아도 되기에 메서드를 선언할 때 일반적으로 적지 않는다.  
  -> 보통 반드시 처리해주어야 하는 예외들만 선언한다.
- 예외를 메서드의 throws에 명시하는 것은 예외를 처리하는 것이 아니라, 자신을 호출한 메서드에게 예외를 전달하여 예외처리를 떠맡기는 것
- 예외를 전달받은 메서드는 또다시 자신을 호출한 메서드에게 전달할 수 있으며, main메서드에서도 예외가 처리되지 않으면, 프로그램 전체가 종료

```java
//어떤 클래스의 실행 결과
java.lang.Exception
	at ExceptionExample.method2(ExceptionExample.java:11)
	at ExceptionExample.method1(ExceptionExample.java:7)
	at ExceptionExample.main(ExceptionExample.java:3)
```

위의 결과로부터 알 수 있는 사실

1. 예외가 발생했을 때, 모두 3개의 메서드(main, method1, method2)가 호출스택에 있었다.
2. 예외가 발생한 곳은 제일 윗줄에 있는 method2()이다.
3. main메서드가 method1()을, 그리고 method1()은 method2()를 호출했다.  
   -> 결국 어느 한 곳에서는 반드시 try-catch문으로 예외처리를 해주어야 한다.

**예외가 발생한 메서드 내에서 자체적으로 처리해도 되는 것은 메서드 내에서 try-catch문을 사용해서 처리하고, 메서드에 호출 시 넘겨받아야 할 값을 다시 받아야 하는 경우(메서드 내에서 자체적으로 해결이 안 되는 경우)에는 예외를 메서드에 선언해서, 호출한 메서드에서 처리해야한다.**

### 1.8 finally블럭

_예외의 발생여부에 상관없이 실행되어야 할 코드를 포함시킬 목적으로 사용_

#### 예외가 발생한 경우

'try -> catch -> finally'순으로 실행

#### 예외가 발생하지 않은 경우

'try -> finally'순으로 실행

_try블럭에서 return문이 실행되는 경우에도 finally블럭의 문장들이 먼저 실행된 후에, 현재 실행 중인 메서드를 종료한다._

### 1.9 자동 자원 반환 - try-with-resources문

_입출력에 사용되는 클래스 중에서는 사용했던 자원을 반환하기 위해 사용한 후 꼭 닫아 줘야 하는 것들이 있다._

```java
try {
	fis = new FileInputStream("score.dat");
	dis = new DataInputStream(fis);
		...
} catch (IOException ie) {
	ie.printStackTrace();
} finally {
	dis.close();
}
```

- 위의 코드는 DataInputStream을 사용해서 파일로부터 데이터를 읽는 코드인데, 예외가 발생하더라도 DataInputStream이 닫히도록 finally블럭 안에 close()를 넣었다.
- 문제는 close()가 예외를 발생시킬 수 있다는 것이다.
- finally블럭 안에 try-catch문을 추가해서 close()에서 발생할 수 있는 예외를 처리하면, 우선 코드가 복잡해져 보기에 좋지 않다.
- 더 큰 문제는 try블럭과 finally블럭에서 모두 예외가 발생하면, try블럭의 예외는 무시된다는 것이다.  
  -> 이를 개선하기 위해 try-with-resources문이 추가된 것

#### try-with-resources문

- try-with-resources문의 괄호()안에 객체를 생성하는 문장을 넣으면, 이 객체는 따로 close()를 호출하지 않아도 try블럭을 벗어나는 순간 자동적으로 close()가 호출된다.
- 자동으로 객체의 close()가 호출될 수 있으려면, 클래스가 AutoCloseable이라는 인터페이스를 구현한 것이어야만 한다.

```java
public interface AutoCloseable {
	void close() throws Exception;
}
```

- try문과 자동으로 호출되는 close()에서 동시에 예외가 발생했을 때, close()에서 발생한 예외는 '억제된(suppressed)'라는 의미의 머리말과 함께 출력
- 두 예외가 동시에 발생할 수는 없기 때문에, 실제 발생한 예외를 일반적으로 출력하고, close()에서 발생한 예외는 억제된 예외로 다룬다.
- 억제된 예외에 대한 정보는 실제로 발생한 예외에 저장된다.

#### 억제된 예외와 관련된 메서드

**void addSuppressed(Throwable exception)** - 억제된 예외를 추가  
**Throwable[] getSuppressed()** - 억제된 예외(배열)를 반환

### 1.10 사용자정의 예외 만들기

_기존의 정의된 예외 클래스 외에 필요에 따라 프로그래머가 새로운 예외 클래스를 정의하여 사용할 수 있다._

```java
class MyException extends Exception {
	MyException(String msg) {
		super(msg);
	}
}
```

- Exception클래스는 생성 시에 String값을 받아서 메시지로 저장할 수 있기에, 사용자정의 예외 클래스도 메시지를 저장하려면 String을 매개변수로 받는 생성자를 추가해줘야 한다.
- 기존의 예외 클래스는 주로 Exception을 상속받아서 'checked예외'로 작성하는 경우가 많았지만, 요즘은 예외처리를 선택적으로 할 수 있도록 RuntimeExeption을 상속받아서 작성하는 쪽으로 바뀌어가고 있다.  
  -> 예외처리가 불필요한 경우에도 try-catch문을 넣어서 코드가 복잡해지기 때문에

### 1.11 예외 되던지기(exception re-throwing)

_한 메서드에서 발생할 수 있는 예외가 여럿인 경우, 심지어는 단 하나의 예외에 대해서도 예외가 발생한 메서드와 호출한 메서드 양쪽에서 처리하도록 할 수 있다._  
-> 예외를 처리한 후에 인위적으로 다시 발생시키는 방법을 통해 가능한데, 이것을 '예외 되던지기'라고 한다.

- 먼저 예외가 발생할 가능성이 있는 메서드에서 try-catch문을 사용해서 예외를 처리해주고 catch문에서 필요한 작업을 행한 후에 throw문을 사용해 예외를 다시 발생시킨다.
- 다시 발생한 예외는 이 메서드를 호출한 메서드에게 전달되고 try-catch문에서 또다시 처리한다.
- 하나의 예외에 대해서 예외가 발생한 메서드와 이를 호출한 메서드 양쪽 모두 처리해줘야 할 작업이 있을 때 사용한다.
- 주의할 점은 예외가 발생할 메서드에서는 try-catch문을 사용해서 예외 처리를 해줌과 동시에 선언부에 발생할 예외를 throws에 지정해줘야 한다는 것
- 반환값이 있는 return문의 경우, catch블럭에도 return문이 있어야 한다. 예외가 발생한 경우에도 값을 반환해야 하기 때문이다.
- 또는 catch블럭에서 예외 되던지기를 해서 호출한 메서드로 예외를 전달하면, return문은 없어도 된다.

### 1.12 연결된 예외(chained exception)

_예외 A가 예외 B를 발생시켰다면, A를 B의 '원인 예외'라고 한다._

```java
try {
	startInstall();
	copyFiles();
} catch (SpaceException e) {
	InstallException ie = new InstallException("설치중 예외발생"); // 예외 생성
	ie.initCause(e); // InstallException의 원인 예외를 SpaceException로 지정
	throw ie;		  // InstallException을 발생시킨다.
}
```

1. 먼저 InstallException를 생성한 후에,
2. initCause()로 SpaceException을 InstallException의 원인 예외로 등록한다.
3. throw로 이 예외를 던진다.

- initCause()는 Exception클래스의 조상인 Throwable클래스에 정의되어 있기 때문에 모든 예외에서 사용 가능하다.  
  **Throwable initCause(Throwable cause)** - 지정된 예외를 원인 예외로 등록  
  **Throwable getCause()** - 원인 예외를 반환
- 원인 예외로 등록해서 다시 예외를 발생시키는 이유
  _ 여러가지 예외를 하나의 큰 분류의 예외로 묶어서 다루기 위해
  _ checked예외를 unchecked예외로 바꿈으로서 예외처리를 선택적으로 만들어 억지로 예외처리를 하지 않게 하기 위해  
  `RuntimeException(Throwable cause) // 원인 예외를 등록하는 생성자`
