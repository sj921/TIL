# chap 09. java.lang 패키지와 유용한 클래스 (java.lang package & util classes)

출처 : Java의 정석_기초편 (https://github.com/castello/javajungsuk_basic)


- [1 Object클래스](#1-Object클래스)
   - [1-1 Object클래스의 메서드](#1-1-Object클래스의-메서드)
   - [1-2 equals(Object obj)](#1-2-equals-Object-obj)
   - [1-3 hashCode()](#1-3-hashCode)
   - [1-4 toString()](#1-4-toString)
   - [1-5 getClass()](#1-5-getClass)   
- [2 String클래스](#2-String클래스)
   - [2-1 String클래스의 특징](#2-1-String클래스의-특징)
   - [2-2 빈 문자열(empty string)](#2-2-빈-문자열-empty-string)
   - [2-3 String클래스의 생성자와 메서드](#2-3-String클래스의-생성자와-메서드)
   - [2-4 문자열과 기본형간의 변환](#2-4-문자열과-기본형간의-변환)   
- [3 StringBuffer클래스](#3-StringBuffer클래스)
   - [3-1 StringBuffer클래스의 특징](#3-1-StringBuffer클래스의-특징)
   - [3-2 StringBuffer클래스의 생성자와 메서드](#3-2-StringBuffer클래스의-생성자와-메서드)   
- [4 Math & wrapper클래스](#4-Math-wrapper클래스)
   - [4-1 Math클래스](#4-1-Math클래스)
   - [4-2 wrapper클래스](#4-2-wrapper클래스)
   - [4-3 Number클래스](#4-3-Number클래스)

---

## 1 Object클래스

### 1-1 Object클래스의 메서드
- `Object`클래스 : 모든 클래스의 최고 조상. 오직 11개의 메서드만을 가지고 있다.
- notify(). wait() 등은 쓰레드(thread)와 관련된 메서드이다.
![image](https://github.com/sj921/TIL/assets/111365047/0a8caf05-9893-4397-ba8f-b21456aae328)

### 1-2 equals(Object obj)
**Object클래스의 메서드 - equals()**
- 객체 자신(this)와 주어진 객체(obj)를 비교한다. 같으면 true, 다르면 false.
- Object클래스의 equals()는 객체의 주소를 비교한다. (참조변수 값 비교)
```
// 두 객체의 주소를 비교
public boolean equals(Object obj) {
	return (this == obj);
}
```
```
public class Ex9_1 {
	public static void main(String[] args) {
		Value v1 = new Value(10);
		Value v2 = new Value(10);
		
		if(v1.equals(v2)) {
			System.out.println("v1과 v2는 같습니다.");
		} else {
			System.out.println("v1과 v2는 다릅니다.");
		}
	} // main 끝
}

class Value{
	int value;	
	Value(int value){ // 생성자
		this.value = value;
	}
}

[출력결과]
v1과 v2는 다릅니다.
```

▼▼

Ex9-1을 Object의 equals()를 오버라이딩해서 주소가 아닌 value를 비교하도록 변경
```
class Ex9_1 {
	public static void main(String[] args) {
		Value v1 = new Value(10);
		Value v2 = new Value(10);

		if (v1.equals(v2))
			System.out.println("v1과 v2는 같습니다.");
		else
			System.out.println("v1과 v2는 다릅니다.");
	} // main
} 

class Value {
	int value;

	Value(int value) {
		this.value = value;
	}
	
	// Object의 equals()를 오버라이딩해서 주소가 아닌 value를 비교
	public boolean equals(Object obj) {
//		return this == obj;	// 주소 비교. 서로 다른 객체는 항상 거짓
		
		// 참조변수의 형변환 전에는 반드시 instanceOf로 확인해야 함.
		if (!(obj instanceof Value)) return false;
		
		Value v = (Value)obj; // obj를 value로 형변환
		
		return this.value == v.value;
	}
}

[출력결과]
v1과 v2는 같습니다.
```

- 인스턴스 변수(iv)의 값을 비교하도록 equals()를 오버라이딩해야 한다.
```
class Person {
	long id;	// this.id

	public boolean equals(Object obj) {
		if(!(obj instanceof Person))
			return false;
			
		Person p = (Person)obj;
				
		return id == p.id;
	}

	Person(long id) {
		this.id = id;
	}
}

class Ex9_2 {
	public static void main(String[] args) {
		Person p1 = new Person(8011081111222L);
		Person p2 = new Person(8011081111222L);

		if(p1.equals(p2))
			System.out.println("p1과 p2는 같은 사람입니다.");
		else
			System.out.println("p1과 p2는 다른 사람입니다.");
	}
}

[출력결과]
p1과 p2는 같은 사람입니다.
```



### 1-3 hashCode()
### 1-4 toString()
### 1-5 getClass()

---

## 2 String클래스

### 2-1 String클래스의 특징
### 2-2 빈 문자열(empty string)
### 2-3 String클래스의 생성자와 메서드
### 2-4 문자열과 기본형간의 변환

---

## 3 StringBuffer클래스

### 3-1 StringBuffer클래스의 특징
### 3-2 StringBuffer클래스의 생성자와 메서드

---

## 4 Math & wrapper클래스

### 4-1 Math클래스
### 4-2 wrapper클래스
### 4-3 Number클래스

---

