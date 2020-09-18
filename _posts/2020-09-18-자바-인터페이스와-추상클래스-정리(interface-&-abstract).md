---
title: 자바 인터페이스와 추상클래스 정리(interface & abstract)
date: 2020-09-18 20:12:37 +0900
categories: [공부, 자바]
tags: [자바, 정리, 공부, 기초]
---

## 인터페이스와 추상클래스

---

오늘 정리할 내용은 인터페이스와 추상클래스로 보통 실무에서는 많은 개발자들이 동시에 개발하다보니깐 매개 변수와 메소드 이름을 짓는데 혼동을 줄이기 위해 사용한다고 한다.\
즉 앞으로 개발하는데 혼동을 줄이기 위해 미리 간단하게 정할 것을 정하는 용도로 사용하는 것 같다.\
아직 자바 뉴비로써 아직 사용할 일이 없지만 필요성은 이해가 간다.

### 인터페이스 / interface

---

먼저 인터페이스에 대해 알아보겠다.

#### 선언하기

```java
public interface ProfileEditor {
  public void editName(String name);
  public void editAge(int age);
}
```

인터페이스를 선언하는 방법은 매우 간단하다.\
기존에 타입을 적는 부분에 `interface`를 적고 클래스와 동일하게 변수나 메소드들을 선언해주면 된다.\
하지만 `주의`할부분이 있다.\
interface로 선언했으면 메소드를 선언하되 `구현을 하면 안된다`.\
즉 `interface abstract method`는 `body`를 가지면 안된다.

#### 구현하기

```java
public class profileEditorImpl implements ProfileEditor {
  public void editName(String name){
    // 구현 생략
  }
  public void editAge(int age){
    // 구현 생략
  }
}
```

인터페이스를 사용 및 구현하는 방법은 `implements` 키워드를 사용하면 된다.\
기존에 `extends` 키워드를 사용하는 것이 아니다.\
그리고 주의할 점은 위처럼 interface에 포함된 `모든 메소드`를 `구현`해야한다.
