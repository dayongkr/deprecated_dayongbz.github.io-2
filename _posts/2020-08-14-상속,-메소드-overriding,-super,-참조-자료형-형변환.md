---
title: 상속, 메소드 overriding, super, 참조 자료형 형변환
date: 2020-08-14 14:45:27 +0900
categories: [공부, 자바]
tags: [자바, java, 개발, dev, 공부, study]
---
자바에서는 **상속**이라는 개념이 있다.

상속은 여러 개의 클래스들이 서로 중복되는 변수나 메서드와 같은 것이 있으면 한 클래스에 몰아서 만들고 다른 클래스를 만들 때 상속받고 필요한 변수들 메서드를 추가함으로써 유지 보수하기 더 편하게 만들어주는 개념이다.

앞으로 노트북으로 예를 들면서 설명해보겠다.

```java
// Laptop.java
public class Laptop {
  int screenSize;
  String cpu;

  public void turnOn() {
    System.out.println("컴퓨터가 켜졌습니다.");
  }
}

// Mac.java
public class Mac extends Laptop {
  public static void main(String[] args) {
    Mac mac = new Mac();
      mac.screenSize = 15;
      mac.cpu = "i7-10700";
      mac.turnOn();
      mac.openSafari();
  }

  public void openSafari() {
    System.out.println("사파리 브라우저가 켜졌습니다.");
  }
}
```

위와 같이 상속을 하는 것이다.

보다시피 부모 클래스는 따로 뭐 설정할 것들은 없지만 자식 클래스인 Mac 클래스를 보면 **extends**라는 키워드가 보일 것이다.

extends 키워드를 사용해서 부모 클래스로부터 상속을 받는 것이다.

만약에 생성자에 인자를 넘겨주고 싶으면 아래를 참고하면 된다.

```java
// Laptop.java
public class Laptop {
  public Laptop() {
  }

  public Laptop(int screenSize) {
    this.screenSize = screenSize;
  }

  int screenSize;
  String cpu;

  public void turnOn() {
    System.out.println("컴퓨터가 켜졌습니다.");
  }

  public void checkSpec() {
    System.out.println("화면크기: " + screenSize + "\ncpu: " + cpu);
  }
}

// Mac.java
public class Mac extends Laptop {
  public Mac() {
    super(15);
  }

  public static void main(String[] args) {
    Mac mac = new Mac();
    //mac.screenSize = 15;
    mac.cpu = "i7-10700";
    mac.turnOn();
    mac.openSafari();
    mac.checkSpec();
  }

  public void openSafari() {
    System.out.println("사파리 브라우저가 켜졌습니다.");
  }
}
```

위에 보면 **super**라는 키워드가 보일 것이다. super는 부모 클래스의 변수나 인스턴스를 가져올 때 사용하는데 위에서는 super()라고 사용했으니 부모의 생성자를 실행시킨다.

인자를 안 넘기는 경우에는 super()를 굳이 안 적어도 부모의 기본 생성자가 자동으로 호출되지만 위와 같이 인자를 넘길 경우에는 생성자를 직접 지정해서 자식의 생성자에다가 추가해야 한다.

다음은 **메서드** **overriding** 개념이다.

만약에 부모의 메서드와 동일한 메서드를 자식에 추가할 수 있을까?

정답은 yes이다.

만약에 자식에 부모의 메소드와 동일한 메소드를 추가하면 **자식의 메서드만을 실행시킨다.**

하지만 조심해야 할 것이 **접근 제어자가 부모의 메서드보다 범위가 작아지면 안 된다.**

그러니깐 만약에 부모의 메서드의 접근제어자가 public이면 자식의 메서드도 무조건 public으로 추가해야 한다.

```java
//가능
public class User {
  public static void main(String[] args) {
    Laptop laptop = new Mac();
    laptop.turnOn();
  }
}

//불가능
public class User {
  public static void main(String[] args) {
    Mac laptop = new Laptop();
    laptop.turnOn();
  }
}

```

다음은 매우 중요한 **참조자료형 형 변환이다.**

위에 보다시피 자식 클래스에서 부모 클래스로 형 변환은 되는데 반대는 안된다.

그 이유는 자식은 부모의 모든 변수와 메서드를 사용 가능해서 자식 -> 부모가 가능하지만 부모는 자식이 새로 추가한 변수와 메소드를 사용할 수 없어서 부모 -> 자식이 안된다.

위와 같은 개념을 활용해서 아래와 같이 사용된다.

```java
//가능
public class User {
  public static void main(String[] args) {
    Mac mac = new Mac();
    Samsung samsung = new Samsung();
    Laptop [] laptops = new Laptop[2];
    laptops[0] = mac;
    laptops[1] = samsung;
  }
}
```

위와 같이 서로 다른 클래스들을 하나의 배열 안에 넣을 수 있다.

만약에 어떤 클래스의 인스턴스인지 확인하고 싶으면 아래처럼 하면 된다.

```java
public class User {
  public static void main(String[] args) {
    Mac mac = new Mac();
    Samsung samsung = new Samsung();
    Laptop [] laptops = new Laptop[2];
    laptops[0] = mac;
    laptops[1] = samsung;

    if (latops[0] instanceof Mac) {
      System.out.println("Mac클래스의 인스턴스입니다.");
    }
  }
}
```

**instanceof** 키워드를 사용하면 boolean 타입의 값으로 맞는지 아닌지 알려준다.

물론 Mac 자리에 Laptop을 넣어도 true가 나오니 최하위 클래스를 넣어주는 것이 좋다.