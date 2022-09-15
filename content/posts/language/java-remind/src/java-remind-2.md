---
title: "(2) 객체지향프로그래밍"
date: 2022-09-15T17:15:23+06:00
hero: /images/posts/language/python/17.jpg
description: 자바 빠르게 리마인드하기
theme: Toha
menu:
  sidebar:
    name: (2) 객체지향프로그래밍
    identifier: java-remind-2
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

### Chapter 6 객체지향프로그래밍 I

<br/>

##### method

<br />

```java
int add(int x, int y) {
  return x + y;
}
void foo() {} // 컴파일러가 return; 을 넣어줌
```

<br />

##### method overloading

<br />

method 이름이 같아야 하고, 매개변수 개수 또는 타입이 달라야 함
return type은 아무런 상관 없음

```java
double add(double x, double y) {
  return x + y;
}
```

<br />

##### variable arguments

<br />

가변인자는 항상 마지막 매개변수여야 함

```java
public PrintStream printf(String format, Object... args) {...}
```

<br />

##### constructor

<br />

생성자의 이름은 클래스 이름과 같아야 함
생성자는 리턴값이 없음. 다만 return type을 void로 명시하지 않고 생략함
현재 아래 생성자 `Person() {}`과 같은 형태가 default constructor임.
class에 생성자가 하나도 정의되어 있지 않을 때, 컴파일러는 `className() {}`를 추가해 컴파일함

```java
class Person {
  Person() {}
  Person(String name, double height) {
    // constructor overloading
  }
}
```

<br />

##### this와 this()

<br />
생성자는 반드시 첫 줄에서 생성자를 호출해야만 한다 (자기 자신(this) 또는 부모(super))
첫 줄에 생성자를 호출하는 문장이 없으면 컴파일러가 super(); 를 넣어준다.
생성자를 포함한 모든 인스턴스 메서드에는 this가 지역변수로 숨겨진 채로 존재함.

```java
Class Person {
  Person() {}
  Person(String name) {
    this(name, 175, 65);
  }
  Person(String name, double height, double weight) {
    this.name = name;
    this.height = height;
    this.weight = weight;
  }
}
```

<br />

##### 변수의 암시적 초기화

<br />

(멤버 변수만 적용, 지역변수는 명시적으로 초기화해야 함)

```java
class InitTest {
  int integer;
  double doublePrecision;
  char character;
  boolean bool;
  String string;
  int[] intArr;
  void bar() {
    System.out.println(integer);
    System.out.println(doublePrecision);
    System.out.println(character);
    System.out.println(string);
    System.out.println(bool);
    System.out.println(intArr);
  }
}
(new InitTest()).bar(); // 0, 0.0, '\u0000', false, null, null

```

<br />

##### initialization block

<br />

- `static {}`: 클래스 초기화 블럭. 클래스가 메모리에 처음 로드될 때 한 번만 수행됨
- `{}`: 인스턴스 초기화 블럭. 인스턴스 변수와 관련한 초기화 작업이 복잡할 때 사용. 생성자보다 먼저 수행됨

```java
class Circle {
  static int count = 0;
  double radius = 1;
  double circumference;
  static {
    count++;
  }
  {
    this.circumference = 2 * Math.pi * this.radius;
  }
  Person () {}
}

```

클래스의 로딩 시기는 JVM의 종류에 따라 다를 수 있다.
프로그램이 시작할 때, 또는 lazy load

<br />

### Chapter 7 객체지향프로그래밍 II

<br />

##### inheritance

<br />

생성자와 초기화 블럭은 상속되지 않는다. 멤버 변수와 멤버 메서드만 상속됨.
Java에서는 오직 단일 상속만을 허용
모든 Java 클래스 상속계층도의 최상위에 있는 조상 클래스: Object class
extends Object 없이 컴파일하면 컴파일러가 extends Object를 추가해줌

```java
class Child extends Parent {
  ...
}
```

<br />

##### overriding

<br />

자손 클래스에서 오버라이딩하는 메서드는 조상 클래스의 메서드와 이름, 매개변수, 반환타입이 같아야 하며,
접근제어자는 조상 클래스의 메서드보다 좁은 범위로 변경할 수 없다 (public > protected > (default) > private)
조상 클래스의 메서드보다 더 많은 수(많은 범위)의 예외를 선언할 수 없다

```java
class Parent {
  String name = "Josh";
  public void sayHello() { System.out.println("Hello"); }
}

class Child extends Parent {
  String name = "Kaitlyn";
  // 조상 클래스의 멤버가 자손 클래스의 멤버와 중복 정의되어 서로 구별해야 할 때만 super를 사용하는 것이 좋다.
  public void sayHello() { System.out.println("Hi"+super.name); }
}
```

<br />

##### package

<br />

package: 물리적인 하나의 디렉터리.
package 선언문은 반드시 소스파일에서 주석과 공백을 제외한 첫 문장이어야 하며, 소스코드에 한 번만 선언될 수 있다.
package를 선언하지 않으면 이름 없는 패키지(unnamed package)에 속하게 된다.

```java
package com.google.translate;
```

<br />

##### import

<br />

모든 소스파일에서 import 문은 package 문 다음, 클래스 선언문 이전에 위치해야 한다.
package.\* 를 하면 컴파일러가 조금의 수고를 더 하겠으나, 실행 시 성능상 차이는 전혀 없다.
package.\* 의 범위가 하위 클래스의 패키지의 클래스까지 포함하지는 않는다.
모든 소스파일에는 묵시적으로 import java.lang.\* 문이 선언되어 있다.

```java
import java.util.*;
```

<br />

##### static import

<br />

static import 문을 사용하면 static member를 호출할 때 클래스 이름을 생략 가능하다.

```java
import static java.lang.System.out;
import static java.lang.Math.*;

class StaticImport {
  public static void main(String[] args)
    out.println(Math.random());
}
```

<br />

##### 제어자

<br />

접근 제어자: `public`, `protected`, `default`, `private`
그 외: `static`, `final`, `abstract`, `native`, `transient`, `synchronized`, `volatile`, `strictfp`

- `static`

  - member variable, method, initialization block에 사용 가능

  ```java
  class Static {
    static int x = 0;
    static {  }
    static int max(int a, int b) {
      return a > b? a : b;
    }
  }
  ```

- `final`

  - member, local variable, method, class에 사용 가능
  - 변수에 사용: 값을 변경할 수 없는 상수가 됨
  - 메서드에 사용: 오버라이딩을 못하게 함
  - 클래스에 사용: 자신을 extend하는 자손 클래스를 정의하지 못하게 함

  ```java
  final class Final {
    final int MAX_SIZE = 127;
    final void getMaxSize() {
      return MAX_SIZE;
    }
  }
  ```

  참고) final 인스턴스 변수의 경우, 선언과 초기화를 동시에 하지 않고 생성자에서 초기화되도록 할 수도 있다.

  ```java
  class Car {
    final int MAX_SPEED;
    Car(int maxSpeed)
      MAX_SPEED = maxSpeed;
  }
  ```

- 접근 제어자

  - public > protected > (default) > private
  - 실제 default 키워드는 없음. 접근 제어자가 지정되어 있지 않으면 default.
  - private: 같은 클래스 내에서만 접근 가능
  - default: 같은 패키지 내에서만 접근 가능
  - protected: 같은 패키지 내에서, 그리고 다른 패키지의 자손 클래스에서 접근 가능
  - public: 접근 제한 없음

- 제어자의 조합
  - 메서드에 static과 abstract를 함께 사용할 수 없다
    - static method는 몸통이 있는 메서드에만 사용할 수 있기 때문
  - 클래스에 abstract와 final을 동시에 사용할 수 없다
    - final은 클래스를 확장할 수 없다는 의미이고, abstract는 상속을 통해 완성돼야 한다는 의미이므로 서로 모순됨
  - abstract method의 접근 제어자는 private일 수 없다
    - 자손 클래스에서 abstract method를 구현해주어야 하는데 접근할 수 없기 때문
  - 메서드에 private과 final을 같이 사용할 필요는 없다
    - 접근 제어자가 private인 method는 오버라이딩 될 수 없기 때문

<br />

##### 추상 클래스

<br />

추상 클래스로 인스턴스를 생성할 수 없음

```java
abstract class Person {
  abstract void sayHello(String name);
}
```

<br />

##### 인터페이스

<br />

추상 클래스보다 추상화 정도가 높음
오직 추상 메서드와 상수만을 멤버로 가질 수 있음
몸통을 갖춘 일반 메서드 혹은 멤버변수를 구성원으로 가질 수 없음
모든 메서드는 public abstract이어야 하며, 이를 생략할 수 있다 (JDK 1.8 이상: static method와 default method는 예외)
모든 멤버 변수는 public static final이어야 하며, 이를 생략할 수 있다

```java
interface Attackable {
  public static final ATTACK_DAMAGE = 45;
  public abstract void attack(Unit target);
}

interface Movable {
  SPEED = 340;
  void Move(Direction direction);
}

// 인터페이스의 상속
interface Fightable extends Attackable { ... }

// 인터페이스의 구현
class Fighter implements Fightable {
  public void attack(Unit target) { ... }
  public void Move(Direction direction) { ... }
}

// 구현하는 인터페이스의 메서드 중 일부만 구현한다면 추상 클래스로 선언
abstract class Fighter implements Fightable {
  public void attack(Unit target) { ... }
}

// 상속과 구현을 동시에 할 수 있음
class Fighter extends Unit implements Fightable { ... }

// (> JDK 1.8) default method, static method
// default method가 새로 추가되어도 해당 인터페이스를 구현한 클래스를 변경하지 않아도 됨
interface Fooable {
  default void bar() {}
  static void foobar() {}
}
```

<br />
