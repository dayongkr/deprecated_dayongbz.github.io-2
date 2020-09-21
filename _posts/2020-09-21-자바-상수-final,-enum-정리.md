---
title: 자바 상수 final, enum 정리
date: 2020-09-21 20:10:22 +0900
categories: [공부, 자바]
tags: [자바, 공부, 정리]
---

`상수`는 변수와 달리 값이 `바뀌지 않는 것`을 의미한다. 자바에서도 상수라는 개념이 있다. 바로 `final`, `enum` 키워드를 사용해서 상수를 선언하는데 자세히 알아보도록 하자!

---

## final

`final`은 `변수`뿐만 아니라 `클래스`, `메소드`에서도 선언할 수 있다. `final`로 선언하면 각각 어떻게 달라지는지 알아봅시다!

### 클래스 / Class

```java
public final class FinalClass {

}
```

클래스에서 final로 선언하는 방법은 접근제어자와 class 예약어 사이에 추가하면 된다.

```java
public class FinalClassChild extends FinalClass {

}
// error: cannot inherit from final FinalClass
```

final로 선언된 클래스를 상속받을 수 없다. 그래서 더 `확장해서는 안 되는` 클래스나 누군가 이 클래스를 상속받아서 `내용을 변경해서는 안 되는` 클래스를 선언할 때 final로 선언한다.

### 메소드 / Method

메소드의 경우에는 final로 선언하면 더 `Overriding(오버라이딩)`을 할 수 없다.

```java
public class FinalMethodClass {
  public final void finalMethod() {
    System.out.println("hello");
  }
}
```

```java
public class FinalMethodClassChild extends FinalMethodClass {
  public void finalMethod() {
    System.out.println("bye");
  }
}
```

위와 같이 오버라이딩 하면 해당 메소드는 final이기 때문에 override 할 수 없다고 오류가 뜨게 된다. `자바의 신` 저자에 따르면 final로 선언된 클래스는 간혹 보여도 메소드는 거의 보지 못했다고 한다.

### 변수 / Variable

변수에다가 final을 사용하는 이유는 클래스와 메소드에서 사용하는 이유와 아주 다르다. 변수에다가 사용하면 `더 바꿀 수 없다`가 돼버린다.

```java
public class FinalVariable {
  final int instanceVariable; // -> final int instanceVariable = 0;
  //error: variable instanceVariable not initialized in the default constructor
}
```

그래서 `인스턴스 변수`나 static으로 선언된 `클래스 변수`는 선언과 함께 값을 초기화해야 한다. 안 그러면 위의 코드처럼 에러가 발생한다.

```java
public class FinalVariable {
  public void method(final int parameter) {
    final int localVariable;
  }
}
```

하지만 위의 코드처럼 지역 변수와 매개 변수는 예외이다. 어차피 매개 변수는 초기화돼서 넘어왔고 지역 변수는 메소드를 선언하는 블록 안에서만 참조되므로 다른 곳에서 변경할 일이 없기 때문이다.

```java

public class FinalVariable {
  public void method(final int parameter) {
    final int localVariable;
    localVariable = 1;
    localVariable = 2;
    parameter = 3
  }
}
```

하지만 위의 코드처럼 작성하면 당연히 바뀌면 안 되는 값이 바뀌었으니 `localVariable = 2`, `parameter = 3` 코드 때문에 에러가 발생한다.

그리고 `참조형`도 final로 선언이 가능한데 하지만 객체 안의 값들은 final로 선언이 안 되어있기 때문에 당연히 바꿀 수 있다.

---

## enum 클래스

enum이란 상수의 집합이다.

### 간단하게 사용해보기

```java
public enum EnumClass {DOG, CAT}
```

위와 같이 선언하면 순서대로 상숫값이 `0부터 1만큼 증가`하면서 설정된다. 즉 DOG는 0, CAT은 0으로 설정된다.

```java
public enum EnumClass {
  DOG(43),CAT(89);

  private final int value;
  EnumClass(int value) {
    this.value = value;
  }
  public int getValue() {
    return value;
  }
}
```

`불규칙한 값`을 상숫값으로 설정하고 싶으면 위와 같이 `인스턴스 변수`와 `생성자`를 추가로 추가해주면 된다. 즉 enum 클래스도 일반 클래스와 같이 생성자를 따로 만들지 않으면 `컴파일러`가 알아서 `기본 생성자`를 추가해준다는 사실을 알 수 있다.

```java
public class EnumTest {
  public static void main(String args[]) {
    EnumClass animal = EnumClass.DOG;
    System.out.println(animal); // DOG
    System.out.println(animal.getValue()); // 43
  }
}
```

그리고 사용 방법은 위와 같다.

### enum 클래스의 부모는 java.lang.Enum

enum 클래스는 java.lang.Enum을 상속받고 있다. 따로 extends 키워드를 사용하지 않아도 이 또한 컴파일러가 알아서 추가해서 상속받기 때문이다. 그래서 상속받은 메소드에 대해서 알아보려고 한다.

| 메소드               | 내용                                                    |
| :------------------- | :------------------------------------------------------ |
| compareTo(E e)       | 매개 변수로 enum 타입과의 순서(ordinal) 차이를 리턴한다 |
| getDeclaringClass()  | 클래스 타입의 enum을 리턴한다.                          |
| name()               | 상수의 이름을 리턴한다.                                 |
| ordinal()            | 상수의 순서를 리턴한다.                                 |
| valueOf(String name) | 매개 변수로 받은 문자열과 동일한 상수를 리턴한다.       |
| values()             | 모든 상수를 배열로 리턴한다.                            |

#### compareTo(E e)

제일 헷갈리는 메소드인 compareTo만 자세히 알아보겠다.

```java
public class EnumTest {
  public static void main(String args[]) {
    EnumClass dog = EnumClass.DOG;
    EnumClass cat = EnumClass.CAT;
    System.out.println(dog.compareTo(cat)) // -1

  }
}
```

compareTo 메소드는 매개 변수로 받은 상수를 기준으로 `앞에 있으면 음수`, `같으면 0`, `뒤에 있으면 양수`를 반환한다. 위의 코드 같은 경우에는 `CAT이 기준`이고 DOG가 CAT `바로 앞에` 있다 보니깐 `-1`이 반환되었다.

---

아직 java를 활용해보진 않아서 어떤 상황에 유용한지는 잘 모르겠지만 재밌는 개념인 것 같다. 실무에서 유용하게 사용된다고 하니깐 알면 좋은 개념인 것 같다.
