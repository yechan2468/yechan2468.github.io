---
title: "(3) 예외와 I/O"
date: 2022-09-15T19:15:23+06:00
hero: /images/posts/language/python/17.jpg
description: 자바 빠르게 리마인드하기
theme: Toha
menu:
  sidebar:
    name: (3) 예외와 I/O
    identifier: java-remind-3
    parent: java-remind
    weight: 500
tags: ["Java", "자바", "문법"]
categories: ["Java"]
---

> 이 글은 'Java의 정석(남궁성) 3rd edition' 책의 내용을 요약한 것입니다.
> 모든 내용을 요약한 것이 아니라, 저 개인의 자바 언어 문법 공부용, 기록용으로 작성한 글입니다.
> 자바 언어를 리마인드하려는 사람들이 읽을 수도 있으나, 글이 친절하지 않을 수 있습니다.
> 프로그래밍 언어를 접해보지 않은 사람들은 읽지 않는 것이 좋을 것입니다.

<br />

### Chapter 8 예외처리

<br />

##### 예외 클래스의 계층 구조

<br />

> `Object` <-- `Throwable` <-- `Exception`, `Error`

Java는 runtime 프로그램 오류를 Exception과 Error로 나눔.

- Error: 프로그램 코드에 의해서 수습될 수 없는 심각한 오류
  - `OutOfMemoryError`, `StackOverflowError`, ...
- Exception: 프로그램 코드에 의해서 수습될 수 있는 다소 미약한 오류
  - `IndexOutOfBoundsException`, `IOException`, `ClassNotFoundException`, ...

> `Exception` <-- `RuntimeException`

Exception 중에는

- `Exception`을 바로 상속하는 Exception과,
- `RuntimeException`을 바로 상속하는 Exception이 있음

전자의 경우 주로 외부의 영향으로 발생할 수 있는 것, 사용자들의 동작에 의해 발생하는 것들이다. (checked 예외)
(`FileNotFoundException`, `ClassNotFoundException`, `DataFormatException`, ...)
후자의 경우 주로 프로그래머의 실수로 발생할 수 있는 예외들로, 자바 프로그래밍 요소들과 연관이 깊다. (unchecked 예외)
(`ArrayIndexOutOfBoundsException`, `NullPointerException`, `ClassCastException`, ...)

<br />

##### try-catch

<br />

try, catch 블럭 내 문장이 하나뿐이어도 괄호 {}를 생략할 수 없다.

```java
try {

} catch (Exception1 e1) {
  e1.printStackTrace();
  String errorMessage = e1.getMessage();
  throw e1;
} finally {

}
```

`Exception` class의 직계자손 Exception이 발생할 가능성이 있는 문장들에 대해 예외처리를 해주지 않으면 컴파일이 되지 않음

<br />

##### method에 예외 선언하기

<br />

```java
void method() throws Exception1, Exception2, Exception3 { ... }
```

<br />

### Chapter 15 I/O 입출력

<br />

입력 받기

```java
import java.util.*
Scanner scanner = new Scanner(System.in);
String input = scanner.nextLine();
int parsed = Integer.parseInt(input);
int num = scanner.nextInt();
```

<br />
