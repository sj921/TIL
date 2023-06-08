# chap 08. 예외처리 (exception handling)

출처 : Java의 정석 (https://github.com/castello/javajungsuk_basic)

<br>

## 프로그램 오류
  - 컴파일 에러(compile-time error) : 컴파일할 때 발생하는 에러 → 실행x
    - Java 컴파일러의 기능 : 구문체크, 번역, 최적화
  - 런타임 에러(runtime error) : 실행할 때 발생하는 에러 → 실행o & 프로그램 종료o 
    - 에러(error) : 프로그램 코드에 의해서 수습될 수 없는 심각한 오류
    - 예외(exception) : 프로그램 코드에 의해서 수습될 수 있는 다소 미약한 오류
  - 논리적 에러 : 작성 의도와 다르게 동작 → 실행o & 프로그램 종료x 

<br>

## 예외 클래스의 계층 구조
  - RuntimeException클래스들 (+자손클래스) : 프로그래머의 실수로 발생하는 예외 → 예외 처리 선택
  - Exception클래스들 (+자손클래스) : 사용자의 실수와 같은 외적인 요인에 의해 발생하는 예외 → 예외 처리 필수
<br>
  <img src="https://github.com/sj921/TIL/assets/111365047/730bad63-c18a-41f8-8c75-5f4116c8dde2" width="400" height="auto"/>
  <img src="https://github.com/sj921/TIL/assets/111365047/b6bf630e-b3cc-44bd-8a59-efe4ddc92f6b" width="400" height="auto"/>
<br>

<br><br>

## 예외 처리하기 : try-catch문

**예외 처리(exception handling)**
- 정의 : 프로그램 실행 시 발생할 수 있는 예외의 발생에 대비한 코드를 작성하는 것
- 목적 : 프로그램의 비정상 종료를 막고, 정상적인 실행 상태를 유지하는 것

:potted_plant: 에러와 예외는 모두 실행 시(Runtime) 발생하는 오류이다.

<br>

```
try {
	// 예외가 발생할 가능성이 있는 문장들을 넣는다.
} catch (Exception1 e1) {
	// Exception1이 발생했을 경우, 이를 처리하기 위한 문장을 적는다.
} catch (Exception2 e2) {
	// Exception2가 발생했을 경우, 이를 처리하기 위한 문장을 적는다.
} catch (ExceptionN eN) {
	// ExceptionN이 발생했을 경우, 이를 처리하기 위한 문장을 적는다.
}
```
:potted_plant: if문과 달리, try-catch문에서는 블럭 내에 포함된 문장이 하나뿐이어도 괄호{}를 생략할 수 없다.

<br>

## try-catch문에서의 흐름
- try블럭 내에서 예외가 발생한 경우,
  1. 발생한 예외와 일치하는 catch블럭이 있는지 확인한다.
  2. 일치하는 catch블럭을 찾게 되면, 그 catch블럭 내의 문장들을 수행하고 전체 try-catch문을 빠져나가서 그 다음 문장을 계속해서 수행한다. 만일 일치하는 catch블럭을 찾지 못하면 예외는 처리되지 못한다.
  3. 예외의 최고 조상인 Exception을 처리하는 catch블럭은 모든 종류의 예외를 처리할 수 있다. (반드시 마지막 catch블럭이어야 한다.)
- try블럭 내에서 예외가 발생하지 않는 경우,
  1. catch블럭을 거치지 않고 전체 try-catch문을 빠져나가서 수행을 계속한다.

<br>

```
class Ex8_4 {
	public static void main(String args[]) {
		System.out.println(1);			
		System.out.println(2);
		try {
			System.out.println(3);
//			System.out.pr intln(args[0]); // ArrayIndexOutOfBoundsException
			System.out.println(0/0);
			System.out.println(4); 	// 실행되지 않는다.
		} catch (ArithmeticException ae)	{
			if (ae instanceof ArithmeticException) 
				System.out.println("true");	
			System.out.println("ArithmeticException");
		} catch (ArrayIndexOutOfBoundsException abe){
			System.out.println("ArrayIndexOutOfBoundsException");
		} catch (Exception e){	// 모든 예외의 최고 조상인 Exception
			System.out.println("Exception");
		}	// try-catch의 끝
		System.out.println(6);
	}	// main메서드의 끝
}
```

```
[출력 결과]
1
2
3
true
ArithmeticException
6
```

<br>

## 예외의 발생과 catch블럭
- 발생한 예외 객체를 catch블럭의 참조변수로 접근할 수 있다.
  - printStackTrace() : 예외 발생 당시의 호출 스택(Call Stack)에 있었던 메서드의 정보와 예외 메시지를 화면에 출력한다.
  - getMessage() : 발생한 예외클래스의 인스턴스에 저장된 메시지를 얻을 수 있다.

```
class Ex8_5 {
	public static void main(String args[]) {
		System.out.println(1);			
		System.out.println(2);

		try {
			System.out.println(3);
			System.out.println(0/0); // 예외발생!!!
			System.out.println(4);   // 실행되지 않는다.
		} catch (ArithmeticException ae)	{
			ae.printStackTrace(); // 참조변수 ae를 통해, 생성된 ArithmeticException인스턴스에 접근할 수 있다.
			System.out.println("예외메시지: " + ae.getMessage());
		}	// try-catch의 끝 (참조변수ae의 유효 범위(Scope))
		
		System.out.println(6);
	}	// main메서드의 끝
}
```

```
[출력 결과]
1
2
3
java.lang.ArithmeticException: / by zero
	at Ex8_5.main(Ex8_5.java:8)
예외메시지: / by zero
6
```

<br>

## 멀티 catch블럭
- 멀티 catch블럭 : 내용이 같은 catch블럭을 하나로 합친 것 (JDK1.7부터 적용)

```
try {
	...
} catch (ExceptionA e) {
  e.printStackTrace();
} catch (ExceptionB e2) {
  e.printStackTrace();
} 

/*	▼▼▼▼▼	*/

try {
	...
} catch (ExceptionA | ExceptionB e) {
  e.printStackTrace();
} 
```

:potted_plant:  Multi Catch문 사용시 주의 사항
- 멀티 Catch문에 사용된 예외들은 예외의 상속관계에서 부모와 자식관계에 있으면 안된다.
  - 부모 타입의 catch문 하나만으로도 처리가 가능하다.
- 멀티 Catch문에 사용된 예외들의 공통된 조상의 멤버만 사용할 수 있다.
  - instanceof 연산을 통해 어느 예외의 인스턴스인지 판단 후 캐스팅해준 후 메서드를 사용한다.

<br>

### 예외 발생시키기

1. 연산자 new를 이용해서 발생시키려는 예외 클래스의 객체를 만든 다음
   - Exception e = new Exception("고의로 발생시켰음");	
2. 키워드 throw를 이용해서 예외를 발생시킨다.
   - throw e;

```
class Ex8_6 {
	public static void main(String args[]) {
		try {
			Exception e = new Exception("고의로 발생시켰음");
			throw e;	 // 예외를 발생시킴
		//  throw new Exception("고의로 발생시켰음.");

		} catch (Exception e)	{
			System.out.println("에러 메시지: " + e.getMessage());
			e.printStackTrace();
		}
		System.out.println("프로그램이 정상 종료되었음.");
	}
}
```

```
[출력 결과]
에러 메시지: 고의로 발생시켰음
java.lang.Exception: 고의로 발생시켰음
	at Ex8_6.main(Ex8_6.java:4)
프로그램이 정상 종료되었음.
```

<br>

## checked예외, unchecked예외

- checked예외 : 컴파일러가 예외 처리 여부를 체크 (예외 처리 필수)
  - Exception과 그 자손들 :  사용자가 발생시키는 예외
- unchecked예외 : 컴파일러가 예외 처리 여부를 체크 안함 (예외 처리 선택)
  - RuntimeException과 그 자손들 : 프로그래머의 실수로 발생시키는 예외
```
 class Ex8_7 {
	public static void main(String[] args) {
		
		// throw new Exception();		// Exception을 고의로 발생시킨다.
	
		try {	// Exception과 그 자손(checked 예외)은 반드시 예외처리를 해줘야 한다.(필수)
			throw new Exception();		// Exception을 고의로 발생시킨다.
		} catch (Exception e) {}
		
		// RuntimeException과 그 자손은 예외처리를 해주지 않아도 컴파일이 된다.(선택)
		throw new RuntimeException();
				
	}
} 
```

<br>

## 메서드에 예외 선언하기
- 예외를 처리하는 방법
  1. try-catch문 (직접 처리)
  2. 예외 선언하기 (예외 떠넘기기(알리기))
  3. 은폐(덮기) (빈 catch)
- 예외 선언 : 메서드가 호출 시 발생 가능한 예외를 호출하는 쪽에 알리는 것

```
void method() throws Exception1, Exception2, ... , ExceptionN {
	// 메서드의 내용
}
```
:potted_plant:  예외를 발생시키는 키워드 throw와 예외를 메서드에 선언할 때 쓰이는  throws를 잘 구별할 것!

<br>

- Exception은 모든 예외의 최고 조상으로 모든 종류의 예외가 발생할 가능성이 있다
- 예외를 선언하면, 이 예외뿐만 아니라 그 자손 타입의 예외까지도 발생할 수 있다. (오버라이딩할 때는 단순히 선언된 예외의 개수가 아니라 상속관계까지 고려해야 한다.)
```
void method() throws Exception {
	// 메서드의 내용
}	
```

<br>


### 메서드에 예외 선언하기 예제1
```
class Ex8_9 {
	public static void main(String[] args) throws Exception {
		method1();	 // 같은 클래스내의 static멤버이므로 객체생성없이 직접 호출가능.
  	}	// main메서드의 끝

	static void method1() throws Exception {
		method2();
	}	// method1의 끝

	static void method2() throws Exception {
		throw new Exception();
	}	// method2의 끝
}
```
```
[출력 결과]
Exception in thread "main" java.lang.Exception
	at Ex8_9.method2(Ex8_9.java:11)
	at Ex8_9.method1(Ex8_9.java:7)
	at Ex8_9.main(Ex8_9.java:3)
```

위의 결과로부터 다음과 같은 사실을 알 수 있다.
1. 예외가 발생했을 때, 모두 3개의 메서드(main, method1, method2)가 호출스택에 있었으며,
2. 예외가 발생한 곳은 제일 윗 줄에 있는 method2라는 것과
3. main메서드가 method1()을, 그리고 method1()은 method2()를 호출했다는 것을 알 수 있다.
- 예외를 단순히 전달만하면 프로그램이 예외로 인해 비정상적으로 종료된다.
- 따라서 어느 한 곳에서는 반드시 try-catch문으로 예외처리를 해주어야 한다. 
 
<br>


### 메서드에 예외 선언하기 예제2

```
// 떠넘기기
class Ex8_10 {
	public static void main(String[] args) {
		try {
			File f = createFile(args[0]);
			System.out.println( f.getName()+"파일이 성공적으로 생성되었습니다.");
		} catch (Exception e) {
			System.out.println(e.getMessage()+"다시 입력해 주시기 바랍니다.");
		}
	}	// main메서드의 끝

	static File createFile(String fileName) throws Exception {
		if (fileName==null || fileName.equals(""))
			throw new Exception("파일이름이 유효하지 않습니다.");
		File f = new File(fileName);		//  File클래스의 객체를 만든다.
     	// File객체의 createNewFile메서드를 이용해서 실제 파일을 생성한다.
		f.createNewFile();
		return f;		// 생성된 객체의 참조를 반환한다.
	}	// createFile메서드의 끝
	
}	// 클래스의 끝 
```
```
// 직접 처리하기
class Ex8_10 {	
	public static void main(String[] args) {
			File f = createFile("");
			System.out.println( f.getName()+"파일이 성공적으로 생성되었습니다.");
	}	// main메서드의 끝
	
	static File createFile(String fileName)  {
		try {
			if (fileName==null || fileName.equals(""))
				throw new Exception("파일이름이 유효하지 않습니다.");
			
		} catch (Exception e) {
			fileName = "제목없음.txt";
		}
		
		File f = new File(fileName);		//  File클래스의 객체를 만든다.
		// File객체의 createNewFile메서드를 이용해서 실제 파일을 생성한다.
		
		try {
			f.createNewFile();			
		} catch (IOException e) {
			e.printStackTrace();
		}		
		return f;		// 생성된 객체의 참조를 반환한다.
	}	// createFile메서드의 끝
	
}	// 클래스의 끝 
```

<br>

## finally블럭
- 예외의 발생 여부와 관계없이 항상 수행되어야 하는 문장들을 넣는다.
- finally블럭은 try-catch문의 맨 마지막에 위치해야 한다.
```
try {
	// 예외 발생 가능성 있는 문장들을 넣는다.
} catch (Exception1 e1) {
	// 예외 처리를 위한 문장을 넣는다.
} finally {
	// 예외의 발생여부에 관계없이 항상 수행되어야하는 문장들을 넣는다.
	// finally블럭은 try-catch문의 맨 마지막에 위치해야한다.
}
```
:potted_plant:  try 또는 catch블럭에서 return문을 만나도 finally블럭은 수행된다.

<br>

## 사용자 정의 예외 만들기
- 우리가 직접 예외 클래스를 정의할 수 있다
- 조상은 Exception과 RuntimeException 중에서 선택 (되도록 선택처리가 가능한 RuntimeException을 사용)
- 문자열을 매개변수로 받는 생성자를 추가하여 메시지를 얻을 수 있다

```
class MyException extends Exception {	// 필수처리 예외(try-catch문 필요)
	MyException(String msg) { // 문자열을 매개변수로 받는 생성자
		super(msg); // 조상인 Exception클래스의 생성자를 호출한다.
	}
}
```

### 사용자 정의 예외 만들기 예제
```
class Ex8_11 {
	public static void main(String args[]) {
		try {
			startInstall();		// 프로그램 설치에 필요한 준비를 한다.
			copyFiles();		// 파일들을 복사한다
		} catch (SpaceException e)	{
			System.out.println("에러 메시지 : " + e.getMessage());
			e.printStackTrace();
			System.out.println("공간을 확보한 후에 다시 설치하시기 바랍니다.");
		} catch (MemoryException me)	{
			System.out.println("에러 메시지 : " + me.getMessage());
			me.printStackTrace();
			System.gc();         // Garbage Collection을 수행하여 메모리를 늘려준다.
			System.out.println("다시 설치를 시도하세요.");
		} finally {
			deleteTempFiles();		// 프로그램 설치에 사용된 임시파일들을 삭제한다.
		} // try의 끝
	} // main의 끝

   static void startInstall() throws SpaceException, MemoryException { 
		if(!enoughSpace()) 		// 충분한 설치 공간이 없으면..
			throw new SpaceException("설치할 공간이 부족합니다.");
		if (!enoughMemory())		// 충분한 메모리가 없으면..
			throw new MemoryException("메모리가 부족합니다.");
   } // startInstall메서드의 끝

   static void copyFiles() { /* 파일들을 복사하는 코드를 적는다. */ }
   static void deleteTempFiles() { /* 임시파일들을 삭제하는 코드를 적는다.*/ }
   
   static boolean enoughSpace()   {
		// 설치하는데 필요한 공간이 있는지 확인하는 코드를 적는다.
		return false;
   }
   
	static boolean enoughMemory() {
		// 설치하는데 필요한 메모리공간이 있느지 확인하는 코드를 적는다.
		return true;
   }
} // ExceptionTest클래스의 끝

class SpaceException extends Exception {
	SpaceException(String msg) {
	   super(msg);	
   }
}

class MemoryException extends Exception {
	MemoryException(String msg) {
	   super(msg);	
   }
}
```

```
[실행 결과]
에러 메시지 : 설치할 공간이 부족합니다.
SpaceException: 설치할 공간이 부족합니다.
	at Ex8_11.startInstall(Ex8_11.java:22)
	at Ex8_11.main(Ex8_11.java:4)
공간을 확보한 후에 다시 설치하시기 바랍니다.
```

<br>



## 예외 되던지기 (exception re-throwing)
- 예외를 처리한 후에 다시 예외를 발생시키는 것
- 호출한 메서드와 호출된 메서드 양쪽 모두에서 예외처리하는 것

```
class Ex8_12 {
	public static void main(String[] args) {
		try  {
			method1();		
		} catch (Exception e)	{
			System.out.println("main메서드에서 예외가 처리되었습니다."); // 2) 처리
		}
	}	// main메서드 끝

	static void method1() throws Exception {	//  예외 선언
		try {
			throw new Exception();	// 예외 발생
		} catch (Exception e) {
			System.out.println("method1메서드에서 예외가 처리되었습니다.");	// 1) 처리
			throw e;			// 2) 다시 예외를 발생시킨다.
		}
	}	// method1메서드 끝
}
```

```
[출력 결과]
method1메서드에서 예외가 처리되었습니다.
main 메서드에서 예외가 처리되었습니다.
```

<br>

## 연결된 예외(chained exception)
- 한 예외가 다른 예외를 발생시킬 수 있다
- 예외A가 예외B를 발생시키면 A는 B의 원인 예외(cause exception)
  - Throwable initCause(Throwable cause) : 지정한 예외를 원인 예외로 등록
  - Throwable getCause() : 원인 예외를 반환

- 연결된 예외를 사용하는 이유
  - 이유1 : 여러 예외를 하나로 묶어서 다루기 위해서
  - 이유2 : checked예외를 unchecked예외로 변경하려 할 때


```
public class Throwable implement Serializable {
	...
	// 예외 안에 또다른 예외를 포함
	private Throwable cause = this; // 객체 자신(this)을 원인 예외로 등록 (원인예외를 저장하기 위한 iv)
	...
	
	public synchronized Throwable initCause(Throwable cause) { 
		this.cause = cause; // cause를 원인 예외로 등록
		return this;
	}
	...
}
```


```
// 필수 예외이므로 둘 다 try-catch문이 필요하다
static void startInstall() throws SpaceException, MemoryException {
	if(!enoughSpace())	// 충분한 설치 공간이 없으면...
		throw new SpaceException("설치할 공간이 부족합니다.");
		
	if(!enoughMemory())	// 충분한 메모리가 없으면...
		throw new MemoryException("메모리가 부족합니다.");	
}

/*	▼▼▼▼▼	*/

// RuntimeException으로 MemoryException을 감싸면 선택 예외처리로 바뀌어 선언부에 선언하지 않아도 된다.
static void startInstall() throws SpaceException {
	if(!enoughSpace())	// 충분한 설치 공간이 없으면...
		throw new SpaceException("설치할 공간이 부족합니다.");
		
	if(!enoughMemory())	// 충분한 메모리가 없으면...
		throw new RuntimeException(new MemoryException("메모리가 부족합니다."));	
} // startInstall메서드의 끝
```

* RuntimeException(Throwable cause) // 원인 예외를 등록하는 생성자

```
class Ex8_13 {
	public static void main(String[] args) {
		try {
			install();
		} catch(InstallException e) { 
			// spaseException(원인 예외)가 InstallException에 포함(initCause())되어 연결된 예외가 되었다.
			e.printStackTrace();
		} catch(Exception e) {
			e.printStackTrace();
		}
	} // main의 끝
	
	static void install() throws InstallException {
		try {
			startInstall(); // 프로그램 설치에 필요한 준비를 한다.
			copyFiles(); // 파일들을 복사한다.
		} catch (SpaceException2 e) {
			InstallException ie = new InstallException("설치 중 예외발생"); // 예외 생성
			ie.initCause(e); // 예외를 연결
			throw ie;
		} catch (MemoryException2 me) {
			InstallException ie = new InstallException("설치 중 예외발생"); // 예외 생성
			ie.initCause(me); // 예외를 연결
			throw ie;
		} finally {
			deleteTempFiles(); // 프로그램 설치에 사용된 임시파일들을 삭제한다.
		} // try의 끝 
	}
	
	static void startInstall() throws SpaceException2, MemoryException2 {
		if(!enoughSpace()) {	// 충분한 설치 공간이 없으면..
			throw new SpaceException2("설치할 공간이 부족합니다.");
		}
		if(!enoughMemory()) {	// 충분한 메모리가 없으면..
			throw new MemoryException2("메모리가 부족합니다.");
			// throw new RuntimeException(new MemoryException("메모리가 부족합니다.")); 
		}
	} // startInstall메서드의 끝
	
	static void copyFiles() {/* 파일들을 복사하는 코드 */}
	static void deleteTempFiles() {/* 임시파일들을 삭제하는 코드 */}
	
	static boolean enoughSpace() {
		// 설치하는데 필요한 공간이 있는지 확인하는 코드
		return true;
	}
	static boolean enoughMemory() {
		// 설치하는데 필요한 메모리공간이 있는지 확인하는 코드
		return false;
	}
}

class InstallException extends Exception {
	InstallException(String msg){
		super(msg);
	}
}
class SpaceException2 extends Exception {
	SpaceException2(String msg){
		super(msg);
	}
}
class MemoryException2 extends Exception {
	MemoryException2(String msg){
		super(msg);
	}
}

```

```
[출력 결과]
// 발생 예외
InstallException: 설치 중 예외발생			  // 대략 정보
	at Ex8_13.install(Ex8_13.java:22)
	at Ex8_13.main(Ex8_13.java:5)
// 원인 예외
Caused by: MemoryException2: 메모리가 부족합니다.	// 세부 정보
	at Ex8_13.startInstall(Ex8_13.java:35)
	at Ex8_13.install(Ex8_13.java:15)
	... 1 more
```
