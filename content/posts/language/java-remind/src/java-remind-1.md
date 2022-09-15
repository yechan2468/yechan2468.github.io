---
title: "(1) 변수와 제어문"
date: 2022-09-15T15:15:23+06:00
hero: /images/posts/language/python/17.jpg
description: 자바 빠르게 리마인드하기
theme: Toha
menu:
  sidebar:
    name: (1) 변수와 제어문
    identifier: java-remind-1
    parent: java-remind
    weight: 500
tags: ["Java", "자바", "문법"]
categories: ["Java"]
---

> 이 글은 'Java의 정석(남궁성) 3rd edition' 책의 내용을 요약한 것입니다.
> 모든 내용을 요약한 것이 아니라, 저 개인의 자바 언어 문법 공부용, 기록용으로 작성한 글입니다.
> 자바 언어를 리마인드하려는 사람들이 읽을 수도 있으나, 글이 친절하지 않을 수 있습니다.
> 프로그래밍 언어를 접해보지 않은 사람들은 읽지 않는 것이 좋을 것입니다.

<br/>

### Chapter 1 자바를 시작하기 전에

<br/>

```java
// Hello.java
class Hello {
  public static void main(String[] args) {
    System.out.println(“Hello, world.”);
  }
}
```

- 모든 자바 어플리케이션은 적어도 하나의 main method를 가져야 함 <br />
- 소스파일의 이름은 그 파일의 public class name과 대소문자까지 일치해야 함<br />
- 하나의 소스 파일에 둘 이상의 public class가 들어갈 수 없음<br />

<br/>

### Chapter 2 변수

<br/>

##### 변수 선언

<br/>

```java
int x=0, y=0;
```

변수의 타입이 같은 경우는 컴마로 구분해 여러 변수를 한 줄에 선언 가능<br />

<br/>

##### 자바 코딩 컨벤션

<br/>

클래스 이름의 첫 글자는 대문자, 변수와 메서드의 첫 글자는 소문자<br />
snake case가 아닌 camel case 또는 Paskal case 사용<br />
상수의 이름은 대문자 `MAX_NUMBER`<br />

<br/>

##### 변수 종류

<br/>

- 기본형 변수: boolean, char, byte, short, int, long, float, double
- 참조형 변수: 나머지
- 상수
  ```java
  // 선언과 동시에 초기화해야 **함**
  final int MAX_SPEED = 130;
  ```
- 리터럴

<br/>

##### 리터럴의 접미사

<br/>

결론: long(L)과 float(F)만 신경 써주자<br />
접두사와 접미사는 대소문자를 구분하지 않음<br />

- 정수형(Long type)
  ```java
  // 숫자 1과 소문자 l이 헷갈리므로 대문자 L로 적어주는 것이 좋음
  long a = 123L;
  long b = 0b0101L;
  long c = 077L;
  long d = 0xFFL;
  // 정수형 리터럴의 중간에 구분자(\_)를 넣어 숫자를 편하게 읽을 수 있음
  long a = 100_000_000L;
  long hex = 0xFFFF_FFFF_FFFF_FFFFL;
  ```
- 실수형(float)
  ```java
  float a = 3.14f;
  double b = 3.0e8D;
  // 접미사 d는 생략 가능 (실수형에서는 double이 기본 자료형)
  double c = 3.1415926535;
  ```
- 문자, 문자열

  ```java
  String str1 = new String("abc"); // immutable.
  // 참고: mutable한 문자열을 다루려면 StringBuffer 클래스를 사용
  String str2 = "xyz";
  String str3 = "";
  char ch1 = 'a';
  char ch2 = ''; // compile error: char는 비어 있는 문자 ‘’를 허용하지 않음

  // 덧셈 연산자로 다른 타입의 피연산자와 concatenate 가능
  true + “” + null == “truenull”
  7 + 7 + “” == “14”
  “” + 7 + 7 == “77”
  ```

<br/>

### Chapter 3 연산자

<br/>

- 덧셈 연산자
  ```java
  byte a = 10;
  byte b = 20;
  // + 연산자는 두 개의 피연산자의 자료형을 int형으로 변환 후 연산을 수행함.
  byte c = a + b; // compile error: 명시적으로 형 변환 필요
  byte c = (byte)(a + b);
  ```
- 등가비교 연산자

  ```java
  String str1 = "abc";
  String str2 = new String("abc");

  // == 연산자는 객체 안의 값을 검사하는 것이 아니라, 서로 같은 객체인지 검사. (참조변수의 값)
  System.out.println(str1 == "abc") // true
  System.out.println(str2 == "abc") // false
  System.out.println(str1.equals("abc")) // true
  System.out.println(str2.equals("abc")) // true
  ```

<br/>

### Chapter 4 조건문과 반복문

<br/>

java에는 조건문(if, switch), 반복문(for, while, do while;)이 존재.

<br />

##### switch statement

<br />

switch의 조건식은 결과값이 반드시 정수여야 함.

```java
switch (condition) {
  case 1:
  // code block
  break;

  // code blocks

  default:
  // code block
}
```

<br />

##### for statement, enhanced for statement

<br />

enhanced for statement는 배열이나 컬렉션에 저장된 요소들을 읽어오는 용도로만 사용할 수 있다.

```java
int[] arr = {1, 3, 5, 7, 9};

for (int i=0; i<arr.length; i++) {
System.out.println(arr[i]);
}

for (int elem: arr) {
System.out.println(elem);
}

```

<br />

##### labeled loop

<br />

```java
Loop1: for (int i=0; i<5; i++) {
  for (int j=0; j<5; j++) {
    if (arr[i] * arr[j] == 49) {
      break Loop1;
    }
  }
}
```

<br/>

### Chapter 5 배열

<br/>

##### 배열의 선언과 생성

<br />

```java
int[] scores;
int scores[];

scores = new int[10];
int scores = new int[0]; // 길이가 0인 배열도 가능
```

<br />

##### 배열의 초기화

<br />

for문 또는 `new type[] {}` 표현을 통해 가능

```java
int[] scores = new int[5];
for (int i=0; i<scores.length; i++)
  scores[i] = 2*i + 1;

int[] scores = new int[] {1, 3, 5, 7, 9};
```

<br />

##### 배열의 초기화 시 `new type[]`을 생략 가능한 경우

<br />

아래와 같이 생략 가능하다.
참고: 매개변수로 배열을 받을 때에도 `new type[]`을 생략 가능하다.

```java
int[] scores = new int[] {1, 3, 5, 7, 9};
int[] scores = {1, 3, 5, 7, 9};

int sum(int[] arr) {...}
int result = sum(new int[] {1, 3, 5, 7, 9});
int result = sum({1, 3, 5, 7, 9});
```

위의 경우와 달리, 배열의 선언과 생성을 따로 할 때는 `new type[]`을 생략 불가능하다.

```java
int[] scores;
scores = {1, 3, 5, 7, 9}; // compile error
```

<br />

##### 배열의 길이

<br />

```java
int len = scores.length; // arr.length는 read-only
```

<br />

##### 배열의 복사

<br />

for문으로 가능하지만, System.arraycopy 메서드 사용하면 편리

```java
System.arraycopy(srcArr, srcIndex, destArr, destIndex, length)
char[] abc = {'a', 'b', 'c', 'd'};
char[] num = {1, 2, 3, 4, 5, 6, 7};
System.arraycopy(abc, 0, num, 0, abc.length); // abcd567
System.arraycopy(abc, 0, num, 2, 3); // ababc67
```

<br />

##### 다차원 배열

<br />

```java
int[][] table;
int table[][];
int[] table[];

int[][] table = new int[7][10]; // row-major order
```

<br />

##### 가변 배열

<br />

```java
int[][] table = new int[3][];
table[0] = new int[3];
table[1] = new int[55];
table[2] = new int[999];
```

<br/>
