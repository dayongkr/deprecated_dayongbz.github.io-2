---
title: 자바 어노테이션 정리(Annotation)
date: 2020-09-29 20:18:34 +0900
categories: [공부, 자바]
tags: [공부, 자바, 정리, study, java]
---

## 어노테이션란(Annotation)

어노테이션은 클래스나 메소드 등의 선언시 `@`를 사용하는 것을 말한다.

## 용도

- 컴파일러에게 정보를 알려주거나
- 컴파일할 때와 설치(deployment)시의 작업을 지정하거나
- 실행할 때 별도의 처리가 필요할 때

---

## 미리 정해져 있는 어노테이션

자바에는 정해져 있는 어노테이션은 3개가 있다. 하지만 어노테이션을 선언하기 위한 `메타 어노테이션`이라는 것이 4개 더 있다.

### 어노테이션 종류

- @Override
  - 해당 메소드가 부모 클래스에 있는 메소드를 `Override` 했다는 것을 명시적으로 선언
  - 보통 오버라이딩할 때에 매개 변수등을 빼먹지 않도록 사용
- @Deprecated
  - 컴파일러에게 더 이상 사용하지 않는 클래스 또는 메소드라고 알려주려고 사용
- @SupressWarnings
  - 경고를 해 줄 필요가 없다고 컴파일러에게 이야기 해주려고 사용

---

## 어노테이션 선언(메타 어노테이션)

위에서 이미 설명했다시피 메타 어노테이션은 어노테이션을 선언할 때 사용하고 총 4개가 존재한다.

- @Target
- @Retention
- @Documented
- @Inherited

### @Target

어노테이션을 어떤 것에 적용할지를 선언할 때 사용한다.

```java
@Target(ElementType.METHOD) // 메소드 선언시
@Target(ElementType.CONSTRUCTOR) // 생성자 선언시
@Target(ElementType.FIELD) // enum 상수를 포함한 필드 값 선언시
@Target(ElementType.PACKAGE) // 패키지 선언시
@Target(ElementType.LOCAL_VARIABLE) // 지역 변수 선언시
@Target(ElementType.TYPE) // 클래스, 인터페이스, enum 등 선언시
```

### @Retention

얼마나 오래 어노테이션 정보가 유지되는지를 다음과 같이 선언한다.

```java
@Retention(RetentionPolicy.RUNTIME) // 실행시 어노테이션 정보가 가상 머신에 의해서 참조 가능
@Retention(RetentionPolicy.SOURCE) // 어노테이션 정보가 컴파일시 사라짐
@Retention(RetentionPolicy.CLASS) // 클래스 파일에 있는 어노테이션 정보가 컴파일러에 의해서 참조 가능.
```

### @Documented

해당 어노테이션에 대한 정보가 `Javadocs(API)` 문서에 포함된다는 것을 선언한다.

### @Inherited

모든 자식 클래스에서 부모 클래스의 어노테이션을 사용 가능하다는 것을 선언한다.

### 직접 선언해보자

```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target(ElementType.METHOD) // 1
@Retention(RetentionPolicy.RUNTIME) // 2

public @interface Annotation { // 3
    public int number(); // 4
    public String text() default "This is first annotation"; // 5
}

```

1. 해당 어노테이션이 `메소드`에서 사용할 수 있다고 선언했다.
2. 해당 어노테이션은 `실행시`에 참조하게 된다.
3. 어노테이션은 `@interface`로 선언한다.
4. 이와 같이 메소드 처럼 어노테이션 안에 선언해두면 해당 어노테이션을 사용할 때 해당 항목에 대한 타입으로 값을 지정할 수 있다.
5. `default` 예약어를 사용하면 값을 지정하지 않아도 뒤에오는 값이 `default` 값으로 지정된다.

## 마무리

자바의 신 뒤에 나오는 내용들은 보통 앞으로 사용하게될 개념들을 훑어보자는 식으로 나오기 때문에 읽을 때 그냥 외우려고 보는 것 보다는 최대한 이해하려는데 초점을 맞췄다. 그래서 블로그에 정리할 때에도 최대한 기초적이 개념들만 적고 있다. 그래서 어노테이션을 정리할 때에도 `어노테이션 정보 확인하기`, `어떻게 활용되는지`에 대한 내용들도 있었지만 나중에 좀 더 자바를 활용할 수 있을 때 보는 것이 더 좋겠다고 생각되어서 빼게 되었다.
