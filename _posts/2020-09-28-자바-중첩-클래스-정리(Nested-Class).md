---
title: 자바 중첩 클래스 정리(Nested Class)
date: 2020-09-28 20:59:08 +0900
categories: [공부, 자바]
tags: [공부, 자바, 정리, java, study]
---

## 중첩 클래스란

클래스 안에 있는 클래스로 코드를 간단하게 표현하기 위해 존재한다. 보통 `UI` 처리를 할 때 `사용자의 입력`이나 `외부의 이벤트`에 대한 처리를 하는 곳에서 가장 많이 사용된다고 한다.

### 종류

중첩 클래스(Nested Class)는 선언 방법에 따라 `Static nested 클래스`와 `Inner 클래스`로 구분된다. 두 클래스의 차이는 static으로 선언되어있는지 여부로 static으로 선언되어있을 경우 `Static nested class`(정적 중첩 클래스)이고 아닐 경우에는 `Inner class`(내부 클래스)라고 한다.

Inner class는 또 한 번 나뉜다. 이름이 있는 내부 클래스는 `Local inner class`(지역 내부 클래스)이고 없을 경우 `anonymous inner class`(익명 내부 클래스)라고 부른다. 근데 일반적으로는 간단하게 `내부 클래스`와 `익명 클래스`로 불린다고 한다.

### 사용되는 이유

1. 한 곳에서만 사용되는 클래스를 논리적으로 묶어서 처리할 필요가 있을 때
2. 캡슐화가 필요할 때(내부 구현을 감추고 싶을 때)
3. 소스의 가독성과 유지보수성을 높이고 싶을 때

첫 번째 이유가 `Static nested class`를 사용하는 이유이고 두 번째 이유가 `Inner class`를 이용하는 이유라고 한다.

---

## Static nested class(정적 중첩 클래스)

### 선언 방법

```java
public class Outer {
  static class StaticNested {
  }
}
```

위와 같이 class 안에 static으로 클래스를 선언하면 된다. Outer.java를 컴파일을 하면 `Outer.class`와 `Outer$StaticNested.class`가 만들어진다.

### 생성 방법

```java
public class Test {
  public static void main(String[] args) {
    Outer.StaticNested staticNested = new Outer.StaticNested();
  }
}
```

위와 같이 생성할 때에는 `$`로 구분하는 것이 아닌 `.`을 찍어서 사용한다.

---

## Inner class & Anonymous class(내부 클래스와 익명 클래스)

### 내부 클래스 선언 방법

```java
public class Outer {
  class Inner {
  }
}
```

선언 방법은 `static`으로 선언 안 했다는 것을 제외하곤 `Static nested class` 선언 방법과 동일하다.

### 내부 클래스 생성 방법

```java
public class Test {
  public static void main(String[] args) {
    Outer outer = new Outer();
    Outer.Inner inner = outer.new Inner();
  }
}
```

위와 같이 `static`으로 선언 안 했기 때문에 다른 클래스에서 생성하려면 복잡하게 생성해야 한다. 하지만 내부 클래스는 보통 다른 클래스에서 전혀 필요가 없을 때 만들어 사용하기 때문에 어차피 이처럼 생성하여 사용할 일은 매우 드문 것 같다.

### 익명 클래스 사용 방법

```java
// EventListener.java
public interface EventListener {
  public void onClick();
}

// Test.java
public class Test {
  public static void main(String[] args) {
    Test test = new Test();
    test.setListener(new EventListener() {
      public void onClick() {
        System.out.println("Clicked");
      }
    });
  }

  private EventListener listener;

  public void setListener(EventListener listener) {
    this.listener = listener;
  }
}
```

위와 같이 `EventListener`를 생성하고 구현하는 것이 익명 클래스이다. 클래스에는 이름이 없지만 메소드는 구현되어 있다. 이렇게 클래스를 생성하면 메모리와 실행 시작 시간을 단축시켜서 성능상 이롭다.

---

## 중첩 클래스 변수 참조

- Static nested class
  - static 변수만 참조 가능
- Inner class
  - 모든 변수 참조 가능
- Outer class(?) 감싸고 있는 클래스
  - Static nested class, Inner class의 모든 변수 참조 가능

---

## 마무리

아직은 Java를 UI 기반으로 개발도 안 해봤고 Java를 기반으로 프로젝트들을 진행해본 적이 없어서 이해하기 비교적 어려웠지만 전체적인 특징들을 이해했으니 나중에 JavaFX를 공부할 때 잘 활용할 수 있을 것 같다.
