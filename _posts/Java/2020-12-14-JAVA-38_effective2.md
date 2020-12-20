---
title: "이펙티브 자바 - 2 생성자에 매개변수가 많다면 빌더를 고려하라"
categories:
  - Java
read_time: false
tags:
  - Java
comments:
  - true
toc: true
toc_sticky: true
---
> 이 포스팅은 이펙티브 자바를 공부한 내용을 바탕으로 작성한 포스팅입니다. 

---

* 지금까지 일반적인 public 생성자를 통해 객체를 생성하는 방법과 정적 팩토리르 이용하여 객체를 생성하는 방법에 대해 다뤄봤다.
* 이 두 방식에는 제약이 있는데, 매개변수가 많을 때 대응하기가 어렵다는 것이다.
* 일반적으로 생성자를 통해 객체를 생성할 때, 선택적 매개변수가 많은 경우 매개변수의 개수에 따라 생성자를 여러개 오버로딩하여 생성한다.
* 정적 팩토리 메소드 역시 메소드 안에서 private등의 생성자를 호출하기 때문에 위의 방식으로 작성될 것이다.
* 이에 따른 단점을 생각해보자.

---

## 매개변수가 많은 경우
* 매개변수가 많으면 클라이언트 측에서 생성자를 호출할 때, 위치에 따라 실수가 일어날 수 있다.
* 이는 컴파일 오류가 아닌 논리적 흐름에 따른 오류가 발생하여 에러 원인을 찾기가 매우 어려워지는 결과를 야기할 수 있다.
* 점층적 생성자 패턴(다양한 생성자를 오버로딩)을 사용할 경우 코드가 매우 길어지고 헷갈릴 수 있다.

```java
// effective java 예제 코드
public class NutritionFacts{
  private final int servingSize;
  private final int servings;
  private final int calories;
  private final int fat;
  private final int sodium;
  private final int carbohydrate;

  public NutritionFacts(int servingSize, int servings){
    this(servingSize, servings, 0);
  }
  public NutritionFacts(int servingSize, int servings, int calories){
    this(servingSize, servings, calories, 0);
  }
  // ... 점층적으로 생성자를 여러개 만든다.
}
```

* 클라이언트 측은 일일이 매개변수의 개수도 세봐야할 것이고, 어느 곳에 어느 값이 들어가는지도 정확히 확인을 해야한다.
* 선택 매개변수가 많을 때는 자바빈즈 패턴을 생각해볼 수 있다.

---

## 자바빈즈 패턴
* 자바빈즈 패턴은 디폴트인 매개변수가 없는 생성자 한개만 만든 후, setter를 통해 매개변수의 값을 설정하는 방식이다.

```java
// effective java 예제 코드
public class NutritionFacts{
  private int servingSize = -1;
  private int servings = -1;
  private int calories = 0;
  private int fat = 0;
  private int sodium = 0;
  private int carbohydrate = 0;
  
  public NutritionFacts(){}; // 기본생성자 한개만 생성
  public void setServingSize(int servingSize){
    this.servingSize = servingSize;
  }
  public void setCalories(int calories){
    this.calories = calories;
  }
  // ...

}
```

* 이 경우, 점층적 생성자 패턴의 단점은 사라지게 된다.
* 하지만, 가독성이 좋다는 것 빼고는 치명적인 단점들이 존재한다.
* 먼저, 객체 하나를 만들기 위해 여러 메소드를 호출해야 된다.
* 이 말은, __객체가 완전히 생성되기 전까지 일관성이 무너진 상태가 된다는 것__
* 일관성이 깨진다는 말은, 심각한 오류가 발생할 수 있고, 객체가 완전히 생성되기 전까지 버그가 들어가는 등의 문제가 생길 수 있다.
* 스레드 안정성을 얻기위해 추가작업을 해야 한다.
* 이런 문제 때문에 점층적 생성자 패턴의 안전성과 가독성을 겸비한 빌더 패턴을 고려해본다.

---

## 빌더패턴
* 빌더 패턴은, 클라이언트가 직접 객체를 만드는 것이 아니라, 클래스 안에 정의되어 있는 빌더 객체를 얻는다.
* 그 후, 빌더 객체가 제공하는 일종의 매개변수 setter를 호출하여 매개변수 값을 설정한다.
* 마지막으로 빌더 객체가 제공하는 build 메소드를 호출해 우리가 필요한 객체를 얻는다.(보통은 불변인 객체)
* 코드로 한번 살펴보자.

```java
// effective java 예제 코드
public class NutritionFacts {
    private final int servingSize;
    private final int servings;
    private final int calories;
    private final int fat;
    
    public static class Builder{
        private final int servingSize;
        private final int servings; 
        private int calories = 0; // 선택 매개변수
        private int fat = 0; // 선택 매개변수

        public Builder(int servingSize, int servings) {
            this.servingSize = servingSize;
            this.servings = servings;
        }
        public Builder calories(int calories) {
            this.calories = calories;
            return this;
        }
        public Builder fat(int fat) {
            this.fat = fat;
            return this;
        }
        public NutritionFacts build() {
            return new NutritionFacts(this);
        }
    }
    private NutritionFacts(Builder builder) {
        servingSize = builder.servingSize;
        servings = builder.servings;
        calories = builder.calories;
        fat = builder.fat;
    }
}
```

* 예제 코드가 조금 길긴하지만 이해하기 어렵지는 않을 것이다.
* static 클래스인 Builder의 객체를 생성하며 필수매개변수를 넣어주고, 이후 빌더의 세터메소드들은 빌더 자신을 반환하기 때문에, 연쇄적인 호출이 가능하다.
* 메소드 호출이 흐르듯 연결된다는 뜻으로 플루언트 API 혹은 메소드 연쇄라고 한다.
* 마지막으로 클라이언트는 build 메소드를 호출하게 되고, Builder를 매개변수로 가지고 있는 NutritionFacts의 생성자를 호출하게 된다.
* 클라이언트 측에서 호출하는 코드를 살펴보자.

```java
NutritionFacts candy = new NutritionFacts.Builder(180, 2) // 빌더 객체 생성
            .calories(200) // calories 값 set -> 다시 빌더객체 반환하므로 연쇄 호출 가능
            .fat(15) // fat 값 set
            .build(); // build 메소드를 통해 NutritionFacts 객체 생성
```

* 이때, 잘못된 매개변수를 일찍 발견하기 위해 빌더의 생성자와 메소드에서 입력 매개변수를 검사하고,
* build 메소드가 호출하는 생성자에서 여러 매개변수에 걸친 불변식을 검사하자.

> 불변 : 어떠한 변경도 허용하지 않는다(ex. 불변 객체)
> 불변식 : 정해진 기간동안 반드시 만족해야 하는 조건 (ex. 리스트 크기는 0 이상인데, 어느 한순간 음수가 되면 불변식이 깨진 것)

---

## 계층적으로 설계된 클래스에서의 빌더 패턴
* 빌더 패턴은 계층적으로 설계된 클래스와 함께 쓰기에 좋다.
* 각 계층의 클래스에 관련 빌더를 멤버로 정의한다.
* 추상 클래스는 추상 빌더, 구체 클래스는 구체 빌더

```java
// effective java 예제 코드
// Pizza 추상 클래스 - 빌더도 추상 빌더
public abstract class Pizza {
    public enum Topping {HAM, MUSHROOM, ONION}
    final Set<Topping> toppings;

    abstract static class Builder<T extends Builder<T>> { // 빌더 클래스 - 재귀적 한정 타입
        EnumSet<Topping> toppings = EnumSet.noneOf(Topping.class); // 빈 Enumset 생성

        public T addTopping(Topping topping) { // 빌더 반환
            toppings.add(Objects.requireNonNull(topping));
            return self();
        }

        abstract Pizza build();

        protected abstract T self();
    }

    Pizza(Builder<?> builder) {
        toppings = builder.toppings.clone();
    }
}
```

* self 메소드는 구체 클래스에서 정의하겠지만, 형 변환 없이 메소드 연쇄를 지원할 수 있게 된다.
* self 타입이 없는 자바를 위한 이런 방법을 __simulated self-type__ 이라 한다.
* Pizza 클래스의 하위 클래스 한가지만 작성해보자.

```java
public class NyPizza extends Pizza {
    public enum Size {SMALL, MEDIUM, LARGE}
    private final Size size;

    public static class Builder extends Pizza.Builder<Builder>{
        private final Size size;

        public Builder(Size size) { // 필수 매개변수는 size
            this.size = Objects.requireNonNull(size);
        }

        @Override
        public NyPizza build() {
            return new NyPizza(this);
        }

        @Override
        protected Builder self() {
            return this; // 하위 클래스에서 정의해서 this를 반환하므로, NyPizza의 Builder 객체가 반환.
        }
    }

    private NyPizza(Builder builder) {
        super(builder);
        size = builder.size;
    }
}
```

* 각 하위 클래스의 빌더가 정의한 build 메소드는 하위 클래스를 반환하도록 선언해서, 
* 클라이언트는 형변환에 신경쓰지 않고 빌더를 사용할 수 있게 된다.
* 또한 생성자로는 누릴 수 없는 이점으로, 가변인수 매개변수를 여러개 사용할 수 있다.
* 클라이언트쪽 코드를 살펴보자.

```java
NyPizza pizza = new NyPizza.Builder(NyPizza.Size.LARGE)
            .addTopping(Pizza.Topping.ONION)
            .addTopping(Pizza.Topping.HAM) // 생성자랑 다르게 메소드 여러번 호출 가능
            .build(); // NyPizza가 반환되므로 형변환 신경 안써도 됨.
```

---

> 생성자나 정적 팩토리가 처리해야 할 매개변수가 많다면 빌더 패턴을 선택하는 게 더 낫다.
> API는 시간이 지날수록 매개변수가 많아지는 경향이 있으므로, 애초에 빌더로 시작하는 편이 나을때가 많다.

---

## 출처
* Effective Java 3/E , Joshua Bloch
