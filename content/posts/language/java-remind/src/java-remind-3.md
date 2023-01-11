---
title: "(3) 더 나은 코드를 위한 Java 문법"
date: 2022-09-16T21:33:23+06:00
hero: /images/posts/language/python/17.jpg
description: 자바 빠르게 리마인드하기
theme: Toha
menu:
  sidebar:
    name: (3) 더 나은 코드를 위한 Java 문법
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

### Chapter 12 지네릭스, 열거형, 애너테이션

<br />

##### Generics의 제한

<br />

JDK 1.5부터 처음 도입되었다.
다양한 타입의 객체들을 다루는 메서드나 컬렉션 클래스에 컴파일 시 타입 체크를 해주는 기능.
타입 안정성을 제공하고, 타입 체크와 타입 캐스팅을 생략할 수 있으므로 코드가 간결해진다.
컴파일 시 컴파일러가 필요한 곳에 형변환을 해준 뒤 지네릭 타입을 제거하므로, 이전 버전 소스코드와 호환성이 유지된다.

```java
class Box<T> {
  T item;
  void setItem(T item) { this.item = item; }
  T getItem() { return item; }
}

Box<String> b = new Box<String>();
b.setItem("ABC");
b.setItem(new Object()); // compile error
// String item = (String) b.getItem();
String item = b.getItem;
```

Generics가 도입되기 이전의 코드와 호환을 위해, generic class인데도 아래와 같이 예전 방식으로 객체를 생성하는 것이 허용된다. 다만 경고가 발생한다.

```java
Box b = new Box();
b.setItem("ABC"); // 경고
b.setItem(new Object()); // 경고
```

static member에 타입 변수를 사용할 수 없다.
타입 변수가 인스턴스 변수로 간주되기 때문이다.
static member는 타입 변수에 지정된 타입, 즉 대입된 타입의 종류에 관계 없이 동일한 것이어야 한다.

```java
class Box<T> {
  static T item; // compile error
  static int compare(T t1, T t2) { ... } // compile error
}
```

또한, generic type의 배열을 생성하는 것은 허용되지 않는다.
new 연산자는 컴파일 시간에 타입 변수가 정확히 무엇인지 알아야 하는데, compile time에 타입 변수의 타입을 알 수 없기 때문이다.
꼭 generic 배열을 생성해야 할 필요가 있을 때는, new 연산자 대신 `Reflection API`의 `newInstance()`와 같이 동적으로 객체를 생성하는 메서드로 배열을 생성하거나, Object 배열을 생성해서 복사한 후 `T[]`로 형변환하는 방법 등을 사용한다.

```java
class Box<T> {
  T[] itemArr;
  T[] toArray() {
    return new T[itemArr.length]; // compile error
  }
}
```

cont'd

<br />

### Chapter 14 람다와 스트림

<br />

##### Lambda Expression

<br />

JDK 1.8부터 추가됨.
메서드를 하나의 식(expression)으로 표현한 것.
메서드를 람다식으로 표현하면 메서드의 이름과 반환값이 없어지므로, 람다 식을 anonymous function이라고도 한다.

```java
int[] arr = new int[10];

int method(int) {
  return (int)(Math.random()*5) + 1;
}

Arrays.setAll(arr, (i) -> (int)(Math.random()*5) + 1);
```

모든 메서드는 클래스에 포함되어 있어야 하므로, 간단한 메서드라고 하더라도 클래스를 새로 만들어야 하고, 객체도 생성해야 메서드를 호출할 수 있다.
람다식은 이러한 과정 없이 메서드의 역할을 대신해줄 수 있다.

##### 람다 식의 생략

반환 값이 있는 메서드의 경우 return문 대신 식으로 대신할 수 있다.

```java
(int a, int b) -> { return a > b? a : b; }
(int a, int b) -> a > b? a : b
```

람다식에 선언된 매개변수의 타입은 추론이 가능한 경우는 생략할 수 있다.
다만 여러 개의 매개변수 중 어느 하나의 타입만 생략하는 것은 허용하지 않는다.

```java
(int a, int b) -> a > b? a : b
(a, b) -> a > b? a : b
```

람다식에 선언된 매개변수가 하나뿐인 경우는 괄호`()`를 생략할 수 있다.
단, 매개변수의 타입이 있으면 괄호`()`를 생략할 수 없다.

```java
(a) -> a * a
a -> a * a
```

괄호`{}` 안의 문장이 하나일 때에는 괄호`{}`를 생략할 수 있다.
그러나 괄호`{}` 안의 문장이 return문일 경우 괄호`{}`를 생략할 수 없다.

```java
(String name) -> { System.out.println("hello "+name); }
(String name) -> System.out.println("hello "+name)
```

##### 함수형 인터페이스

람다 식은 익명 클래스의 객체와 동일하다.

```java
(int a, int b) -> a > b? a : b

new Object() {
  int function(int a, int b) {
    return a > b? a : b;
  }
}
```

람다식으로 정의된 익명 객체의 메서드를 호출하기 위해서는 참조변수가 있어야 한다.

<br />
