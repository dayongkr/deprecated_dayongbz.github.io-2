---
title: Object 클래스, Object의 메소드
date: 2020-08-20 17:42:50 +0900
categories: [공부, 자바]
tags: [자바, java, 개발, dev, 공부, study]
---
### **Object 클래스 소개 및 자주 쓰는 메소드 정리**

모든 클래스의 부모 클래스는 Object이다.

Object 클래스에는 다양한 메소드가 존재한다.

<table style="border-collapse: collapse; width: 100%; height: 140px;" border="1" data-ke-style="style12"><tbody><tr style="height: 20px;"><td style="width: 50%; text-align: center; height: 20px;"><b>메소드</b></td><td style="width: 50%; text-align: center; height: 20px;"><b>설명</b></td></tr><tr style="height: 20px;"><td style="width: 50%; height: 20px;">protected Object clone()</td><td style="width: 50%; height: 20px;">객체의 복사본을 만들어서 리턴</td></tr><tr style="height: 20px;"><td style="width: 50%; height: 20px;"><b>public boolean equals(Object obj)</b></td><td style="width: 50%; height: 20px;"><b>현재 객체와 매개 변수로 넘겨받은 객체가 같은지 확인 / 같으면 true 다르면 false 리턴</b></td></tr><tr style="height: 20px;"><td style="width: 50%; height: 20px;">protected void finalize()</td><td style="width: 50%; height: 20px;">현재 객체가 더 이상 쓸모가 없어졌을 때 가비지 컬렉터에 의해서 이 메소드가 호출</td></tr><tr style="height: 20px;"><td style="width: 50%; height: 20px;">public <b>Class&lt;?&gt;</b> getClass()</td><td style="width: 50%; height: 20px;">현재 객체의 Class 클래스의 객체를 리턴</td></tr><tr style="height: 20px;"><td style="width: 50%; height: 20px;"><b>public int hashCode()</b></td><td style="width: 50%; height: 20px;"><b>객체에 대한 해시 코드 값을 리턴한다.&nbsp;</b></td></tr><tr style="height: 20px;"><td style="width: 50%; height: 20px;"><b>public String toString()</b></td><td style="width: 50%; height: 20px;"><b>객체를 문자열로 표현하는 값을 리턴</b></td></tr></tbody></table>

> 자바의 신에 정리되어있는 표를 참고했습니다.

위에서 제일 많이 쓰는 메소드인 toString(), hashCode(), equals()에 대해서 알아보겠다.

먼저 알아보기 전에**Class <?>라는** 것을 처음 봐서 알아보니 **제네릭 리터럴**로 **<?>와 같은 와일드카드**를 사용해서 **어떠한 클래스인지 모를 때** 사용하는 것이다.

### **toString()**

먼저 가장 많이 사용되는 메소드는 **toString()**이다.

```java
public class ObjectTest {
  public static void main(String[] args) {
    ObjectTest obj = new ObjectTest();
    obj.checkToString();
  }

  public void checkToString() {
    System.out.println(this);
    System.out.println(toString());
    System.out.println(this + "");
  }
}

// 실행 결과
ObjectTest@5f184fc6
ObjectTest@5f184fc6
ObjectTest@5f184fc6
```

toString()을 사용하면 아래의 결과와 같이 타입 + @ + 메모리 주소 값이 나온다.

그리고 위의 코드를 보면 언제 toString이 실행되는지 볼 수 있다.

1. 객체를 출력할 때
2. toString을 직접 실행할 때
3. 문자열과 더해질 때

위와 같은 상황일 때 toString 메소드가 실행된다.

하지만 위와 같이 결과가 나오면 사람들 입장에서는 뭐하는 객체인지 모른다.

그래서 toString을 사용해야 하는 상황이 온다면 보통 overriding을 해서 사용하게 된다.

```java
public class ObjectTest {
  int num = 0,age = 20;
  String name = "dayong";

  public static void main(String[] args) {
    ObjectTest obj = new ObjectTest();
    System.out.println(obj);
  }

  @Override
  public String toString() {
    return "ObjectTest{" +
            "num=" + num +
            ", age=" + age +
            ", name='" + name + '\'' +
            '}';
  }
}

// 실행결과
ObjectTest{num=0, age=20, name='dayong'}
```

intellij가 생성해준 toString이다.

ide가 생성해주는 toString을 사용하던지 아니면 본인이 직접 오버 라이딩해서 사용해도 된다.

### **equals()**

다음은 equals로 현재 객체와 매개변수로 받은 객체와 같은지 비교하는 메소드이다.

보통 기본자료형들은 == 또는 != 와 같은 연산자로 비교를 하는데 객체들을 이와 같이 비교하면 객체의 메모리 주소가 같은지 확인하는데 이것을 **shallow compare**이라고 한다.

그럼 객체의 안에 있는 값들이 같은지 **deep compare**은 어떻게 할까?

바로 **equals** 메소드를 overriding 해서 사용하는 것이다.

```java
import java.util.Objects;

public class ObjectTest {
  int num = 0, age = 20;
  String name = "dayong";

  public static void main(String[] args) {
    ObjectTest obj1 = new ObjectTest();
    ObjectTest obj2 = new ObjectTest();
    System.out.println(obj1 == obj2);
    System.out.println(obj1.equals(obj2));
  }

  @Override
  public boolean equals(Object o) {
    if (this == o) return true;
    if (o == null || getClass() != o.getClass()) return false;
    ObjectTest that = (ObjectTest) o;
    return num == that.num &&
            age == that.age &&
            Objects.equals(name, that.name);
  }
}

// 실행결과
false
true
```

\== 연산자를 사용했을 때에는 메모리 주소를 비교하기 때문에 false가 나오고 overrding 한 equals를 사용했을 때에는 안의 값들을 비교하기 때문에 true가 나왔다.

위와 같이 오버 라이딩을 안하면 객체의 hashCode()값을 비교하기 때문에 equals() 메소드를 사용하려면 반드시 오버라이딩을 하고 사용해야 한다.

### **hashCode()**

객체에 대한 해시 코드 값을 리턴한다.

hashCode는 equals메소드를 오버라이딩할 때 같이 오버라이딩한다.

왜냐하면 equals가 객체가 같다고 했는데 hashCode에서 반환하는 hash값이 서로 다르면 안 되기 때문이다.

```java
import java.util.Objects;

public class ObjectTest {
  int num = 0, age = 20;
  String name = "dayong";

  public static void main(String[] args) {
    ObjectTest obj1 = new ObjectTest();
    ObjectTest obj2 = new ObjectTest();
    System.out.println(obj1.hashCode());
    System.out.println(obj2.hashCode());
  }

  @Override
  public boolean equals(Object o) {
    if (this == o) return true;
    if (o == null || getClass() != o.getClass()) return false;
    ObjectTest that = (ObjectTest) o;
    return num == that.num &&
            age == that.age &&
            Objects.equals(name, that.name);
  }

  @Override
  public int hashCode() {
    return Objects.hash(num, age, name);
  }
}

// 실행 결과
-1338725353
-1338725353
```

위와 같이 오버라이딩을 하면 해쉬값이 같아지는 것을 확인할 수 있다.

equals메소드와 hashCode메소드는 **둘 다 규칙이라는 게 존재하기** 때문에 웬만하면 **ide에서 생성해주는 것을 그대로 사용**하는 것이 좋다.
