# chap 08. 예외처리 (exception handling)

출처 : Java의 정석 (https://github.com/castello/javajungsuk_basic)

<br>

### 프로그램 오류
  - 컴파일 에러(compile-time error) : 컴파일할 때 발생하는 에러 <span style="color: yellowgreen"> → 실행x </span>
    - Java 컴파일러의 기능 : 구문체크, 번역, 최적화
  - 런타임 에러(runtime error) : 실행할 때 발생하는 에러 <span style="color: yellowgreen"> → 실행o & 프로그램 종료o </span>
    - 에러(error) : 프로그램 코드에 의해서 수습될 수 없는 심각한 오류
    - 예외(exception) : 프로그램 코드에 의해서 수습될 수 있는 다소 미약한 오류
  - 논리적 에러 : 작성 의도와 다르게 동작 <span style="color: yellowgreen"> → 실행o & 프로그램 종료x </span>

<br>

### 예외 클래스의 계층 구조
  - RuntimeException클래스들 (+자손클래스) : 프로그래머의 실수로 발생하는 예외 → 예외 처리 선택
  - Exception클래스들 (+자손클래스) : 사용자의 실수와 같은 외적인 요인에 의해 발생하는 예외 → 예외 처리 필수
<br>
  <img src="https://github.com/sj921/TIL/assets/111365047/730bad63-c18a-41f8-8c75-5f4116c8dde2" width="400" height="auto"/>
  <img src="https://github.com/sj921/TIL/assets/111365047/b6bf630e-b3cc-44bd-8a59-efe4ddc92f6b" width="400" height="auto"/>
<br>

<br><br>

### 예외 처리 구문 : try-catch문

**예외 처리(exception handling)**
- 정의 : 프로그램 실행 시 발생할 수 있는 예외의 발생에 대비한 코드를 작성하는 것
- 목적 : 프로그램의 비정상 종료를 막고, 정상적인 실행 상태를 유지하는 것

<br>

**try-catch문에서의 흐름**
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
1
2
3
true
ArithmeticException
6
```

<br>

:potted_plant: if문과 달리, try-catch문에서는 블럭 내에 포함된 문장이 하나뿐이어도 괄호{}를 생략할 수 없다.

<br>

### 예외의 발생과 catch블럭
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
1
2
3
java.lang.ArithmeticException: / by zero
	at Ex8_5.main(Ex8_5.java:8)
예외메시지: / by zero
6
```

<br>

**Multi catch블럭**
- Multi catch블럭 : 내용이 같은 catch블럭을 하나로 합친 것 (JDK1.7부터 적용)

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
- Multi Catch문에 사용된 예외들은 예외의 상속관계에서 부모와 자식관계에 있으면 안된다.
  - 부모 타입의 catch문 하나만으로도 처리가 가능하다.
- Multi Catch문에 사용된 예외들의 공통된 조상의 멤버만 사용할 수 있다.
  - instanceof 연산을 통해 어느 예외의 인스턴스인지 판단 후 캐스팅해준 후 메서드를 사용한다.

<br>

### 예외 발생시키기







<br>






