---
layout: post
title: "[Kotlin] Data Class를 핥아보자!"
date: 2023-05-21 00:00:00
tags:
categories: kotlin
noindex: true
image: /assets/img/[Kotlin]-Data-Class를-핥아보자/Untitled.png

---
# [Kotlin] Data Class를 핥아보자!

오늘은 코틀린의 편리한 클래스중 하나인 `Data Class`를 핥아?보도록 하겠다.

## Data Class의 선언

Data Class는 다음과 같이 선언 할 수 있다.

```kotlin
data class foo(
  val bar: String
)

// 생각보다 별게 없다.
```

## Data Class의 특징

- 코틀린의 데이터 보관 목적 클래스이다.
- 상속 받을 수 없다.
- 클래스의 프로퍼티를 자동으로 생성해 준다.
    - `toString` , `hashCode()` , `equals()` , `copy()`
    - 메서드를 오버라이드 하여 직접 구현 할 수도 있다.

## Java 와의 비교

```java
public class JavaPersonDto {
    
    private final String name;
    private final int age;

    public JavaPersonDto(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        JavaPersonDto that = (JavaPersonDto) o;
        return age == that.age && Objects.equals(name, that.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }

    @Override
    public String toString() {
        return "JavaPersonDto{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

Lombok 을 이용하여 코드를 줄일 수 있지만
극적인 효과를 위해 IDE의 힘을 빌려 작성했다.

```kotlin
data class PersonDto(
  val name: String,
  val age: Int
)

// 이게 다에요.. 진짜로!!
```

놀랍게도 위 두코드는 같은 기능을 수행한다.

그렇다면 코틀린이 진짜로 일을 했는지 테스트 해보자

```kotlin
fun main() {
  val personDto1 = PersonDto("GGos3", 100)
  val personDto2 = PersonDto("GGos3", 100)
  
  // 클래스 내의 toString 메서드를 이용해 객체를 String 타입으로 만들어 준다.
  println(personDto1)

  // 클래스 내의 equals 메서드를 이용해 비교해준다.
  println(personDto1 == personDto2)
}

// 실행결과
PersonDto(name=GGos3, age=100)
true
```

코틀린은 일을 빼먹지 않고 잘 한것 같다. ~~누구보다 낫네~~

## 마무리

오늘은 이렇게 코틀린의 Data Class에 대해 살짝쿵 핥아 봤는데, 
Java 코드에 비해 너무 양심없게 코드를 줄여버린게 아닌가.. 하는 생각이 들 정도로 편리했다.