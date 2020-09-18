---
title: 자바 인터페이스와 추상클래스 정리(interface & abstract)
date: 2020-09-18 20:12:37 +0900
categories: [공부, 자바]
tags: [자바, 정리, 공부, 기초]
---

## 인터페이스와 추상클래스

---

오늘 정리할 내용은 인터페이스와 추상 클래스로 보통 실무에서는 `많은 개발자들이 동시에` 개발하다 보니깐 매개 변수와 메소드 이름을 짓는데 `혼동을 줄이기 위해` 사용한다고 한다.\
즉 앞으로 개발하는데 혼동을 줄이기 위해 미리 간단하게 정할 것을 정하는 용도로 사용하는 것 같다.\
아직 자바 뉴비로써 아직 사용할 일이 없지만 필요성은 이해가 간다.

### 인터페이스 / interface

---

먼저 인터페이스에 대해 알아보겠다.

#### 인터페이스 선언하기

```java
public interface ProfileEditorInterF {
  public void editName(String name);
  public void editAge(int age);
}
```

인터페이스를 선언하는 방법은 매우 간단하다.\
기존에 타입을 적는 부분에 `interface`를 적고 클래스와 동일하게 변수나 메소드들을 선언해주면 된다.\
하지만 `주의`할 부분이 있다.\
interface로 선언했으면 메소드를 선언하되 `구현을 하면 안 된다`.\
즉 `interface abstract method`는 `body`를 가지면 안 된다.

#### 인터페이스 구현하기

```java
public class ProfileEditor implements ProfileEditorInterF {
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
그리고 주의할 점은 위처럼 interface에 포함된 `모든 메소드`를 `구현`해야 한다.

### 추상화 클래스 / abstract class

---

다음 `abstract` 클래스로 인터페이스와 용도는 같지만 다른 부분이 꽤 있다.

#### 추상화 클래스 선언하기

```java
public abstract class ProfileEditorAbstract {
  public void editName(String name) {
    // 구현 생략
  }
  public abstract void editAge(int age);
}
```

선언 방법은 `class` 앞에 `abstract` 키워드만 추가하면 된다.\
그리고 코드를 보면 `editName` 메소드는 이미 구현이 되어있다.\
그렇다 추상화 클래스는 메소드 구현도 가능하다.\
그리고 구현하지 않을 메소드는 타입 앞에 `abstract`를 적어주면 추상화 메소드로 선언된다.\
추상화 클래스는 `static`이나 `final` 메소드가 있어도 된다고 하니 `interface`에 비해 자유로운 것 같다.

#### 추상화 클래스 상속하기

```java
public class ProfileEditor extends ProfileEditorAbstract {
  public void editAge(int age) {
        // 구현 생략
    }
}
```

그렇다 부제목에 보면 `구현`이 아니라 `상속`으로 적혀있다시피 일반적인 클래스와 같이 `extends`키워드로 상속해서 사용한다.\
그리고 추상화 클래스 또한 구현하지 않은 추상화 메소드는 구현을 해야 하고 이미 구현한 메소드는 오버라이딩이 필요 없으면 굳이 구현하지 않아도 된다.

### 정리

---

|                                  | 인터페이스 | abstract 클래스 | 클래스     |
| :------------------------------- | :--------- | :-------------- | :--------- |
| 선언 시 사용하는 예약어          | interface  | abstract class  | class      |
| 구현 안 된 메소드 포함 가능 여부 | 가능(필수) | 가능            | 불가       |
| 구현된 메소드 포함 가능 여부     | 불가       | 가능            | 가능(필수) |
| static 메소드 선언 가능 여부     | 불가       | 가능            | 가능       |

해당 정리는 `자바의 신 1`에 나와있는 것을 그대로 옮긴 것이다!
고로 신뢰해도 좋을 것 같다!
