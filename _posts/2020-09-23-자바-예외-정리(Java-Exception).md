---
title: 자바 예외 정리(Java Exception)
date: 2020-09-23 20:36:05 +0900
categories: [공부, 자바]
tags: [공부, 자바, 정리, study, java]
---

개발을 하다 보면 `예상한` 혹은 `예상치 못한 일`이 발생하는 것을 막아주기 위해 `안전장치를 하는 것`을 말한다. 이미 `try`, `catch`, `finally` 개념은 다른 언어에서 접해본 적이 있고 많이 알려진 문법이라 처음 알게 된 부분을 정리해보겠다!

## 두개 이상의 catch 가능

```java
public void multiCatch(){
  int[] arr = new int[5];
  try {
    System.out.println(arr[5]);
  } catch (Exception e) {
    System.out.println(arr.length);
  } catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("ArrayIndexOutOfBoundsException occured");
  }
}
```

위와 같이 하나의 `try` 뒤에 `catch`를 여러 개 작성할 수 있다. 하지만 위의 코드는 컴파일 과정에서 에러 메시지가 출력된다. 왜냐하면 모든 예외의 부모는 `java.lang.Exception` 클래스라서 이미 `catch(Exception e)`부분에서 예외를 처리했는데 `불필요하게 catch`를 한 번 더 사용했기 때문이다. 즉 중복되는 예외처리를 하면 안 되고 여러 개의 `catch`를 사용할 때에는 `Exception 클래스는` 맨 밑에 써주는 게 안전하다.

---

## 예외의 종류

- checked exception
- error
- runtime exception / unchecked exception

`error`와 `runtime exception`을 제외하면 모든 예외는 `checked exception`이다. 그래서 그 둘만 알면 모든 예외 종류를 알 수 있다.

### error

`error`는 자바 프로그램 밖에서 발생한 예외를 말한다. 즉 하드웨어가 고장 나서 자바 프로그램이 정상적으로 동작하지 못하는 경우 같은 것이다. `error`는 프로세스에 영향을 줘서 프로그램이 멈추어버린다. 하지만 `Exception`은 쓰레드에만 영향을 줘서 계속 실행되어있다.

### runtime exception / unchecked exception

`runtime exception`는 컴파일할 때 예외를 발생하지 않아서 실행 시에 발생하고 `RuntimeException`을 확장한 예외이다. 이처럼 컴파일 시에 체크를 하지 않기 때문에 `unchecked exception`이라고 부르는 것이다.

---

## 모든 예외의 부모 클래스는 java.lang.Throwable 클래스

`RuntimeException` 클래스는 `Exception` 클래스를 확장했고 `Exception`, `Error` 클래스는 `Throwable` 클래스를 확장했다. 그래서 Throwable 클래스에서 선언된 아래와 같은 메소드들을 `Exception`, `Error` 클래스들을 확장한 예외들에서도 사용할 수 있다.

| 이름              | 설명                                                                                                            |
| :---------------- | :-------------------------------------------------------------------------------------------------------------- |
| getMessage()      | 예외 메시지를 String 형태로 제공 받는다.                                                                        |
| toString()        | 예외 메시지와 예외 클래스 이름도 같이 제공받는다.                                                               |
| printStackTrace() | 첫 줄에는 예외 메시지를 출력, 두 번째 줄부터는 예외가 발생하게 된 메소드들의 호출 관계(Stack Trace)를 출력한다. |

## 예외를 발생 시켜보자

```java
throw new Exception("Number is over than 12");
```

예외를 발생시키려면 위의 코드처럼 `throw new 예외 객체 생성자()`를 작성하면 된다.

```java
public class ThrowTest {
  public static void main(String args[]){
    ThrowTest test = new ThrowTest();
    try {
      test.throwException(0);
    } catch(Exception e) {
      e.printStackTrace();
    }
  }

  public void throwException(int num) throws Exception {
    if(num = 0) {
      throw new Exception("number is zero");
    }
  }
}
```

그리고 `throws` 키워드를 메소드를 선언할 때 추가하면 해당 메소드를 호출하는 메소드로 예외 처리를 위임한다. 그래서 예외를 발생시키는 메소드에는 `try-catch 문`을 사용하지 않아도 된다. 하지만 해당 메소드를 호출한 메소드는 해당 메소드를 호출할 때마다 `try-catch 문`을 사용해야 한다. 그 때문에 더 복잡해지기 때문에 웬만하면 예외를 발생시킬 때 예외 처리를 하는 것이 좋다.

## RuntimeException을 활용하기

```java
throw new RuntimeException("runtime exception");
```

실행 시에 예외가 발생할 것 같을 때 `RuntimeException`을 확장해서 사용하면 더 편리하다. 왜냐하면 `RuntimeException`으로 확장해서 예외를 발생시키면 try-catch 문을 사용하지 않아도 컴파일 시에 예외가 발생하지 않기 때문이다.

---

예외 처리는 아직 이해가 안 되어도 앞으로 공부할 내용에서 더 자세히 나온다고 한다. 그때 한 번 더 정리 할 수 있으려면 하려고 한다. 근데 자바가 첫 번째 언어가 아니라서 그런지 어느 정도는 이해가 간다. 물론 다른 언어에서도 예외처리는 잘 안 해보긴 했지만... ㅎㅎ
