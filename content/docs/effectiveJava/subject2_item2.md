---
title: "Item2"
weight: 2
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---

# Creating and Destroying Objects Item2
* * *

## **Item 2 - 생성자에 매개변수가 많다면 빌더를 고려하라**   
### **빌더패턴**       
빌더 패턴은 객체 생성을 편리하고 가독성 있게 만들기 위해 사용되는 디자인 패턴   
원하는 매개변수만 설정하여 객체를 생성할 수 있음  
```java
Person person = new PersonBuilder()
                .withName("John")
                .withAge(30)
                .withEmail("john@example.com")
                .build();

```   

### **점층적 생성자 패턴**   
이 패턴은 선택적 매개변수를 갖는 생성자를 여러 개 만들고, 필요에 따라 원하는 매개변수를 선택하여 객체를 생성하는 방식   
그러나 매개변수가 많은 경우에는 매개변수 조합에 따라 모든 생성자를 구현하는 것이 번거로움   
타입이 같은 매개변수가 여러개 있으면 찾기 어려운 버그로 이어질수있음   
컴파일에러가 아닌 런타임에 엉뚱한 동작을 하게 된다.   
```java
  public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public Person(String name, int age, String address) {
        this.name = name;
        this.age = age;
        this.address = address;
    }
    
    public Person(String name, int age, String address, String phoneNumber) {
        this.name = name;
        this.age = age;
        this.address = address;
        this.phoneNumber = phoneNumber;
    }
```    



### **자바빈즈 패턴**   
매개변수가 없는 생성자로 객체를 만든 후 setter를 호출해 원하는 매개변수의 값을 설정하는 방식   
자바빈즈패턴에서는 객체하나를 만들려면 메서드를 여러개 호출해야하고 , 객체가 완전히 생성되기전까지 일관성이 무너진상태에 놓이게된다.    
또한 자바빈즈 패턴에서는 클래스를 불변으로 만들 수 없어 스레드 안정성을 얻으려면 개발자가 직접 추가 작업을 해줘야함   
```java
 public Person() {
        // 기본 생성자
    }
    
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    public int getAge() {
        return age;
    }
    
    public void setAge(int age) {
        this.age = age;
    }
```   

### **빌더패턴**   
점층적 생성자패턴의 안정성과 자바빈즈패턴의 가독성을 겸비한 빌더패턴 등장.   
빌더패턴은 불변클래스를 설계할때 유용하다. 불변클래스는 한번 생성되면 그 상태가 변경되지 않는 클래스로 객체의 일관성과 안정성을 보장할수있음.   
불변클래스는 모든 필드를 final로 선언하고 외부에서 직접 접근할수 없도록 읽기전용으로 만들어짐.   


### **빌더패턴은 계층적으로 설계된 클래스와 함께 쓰기 좋다**   
**빌더 클래스**   
 -> 객체의 속성을 설정하고 최종 객체를 반환하는 메서드 제공   
**제품 클래스**   
 -> 생성할 객체를 나타내는 클래스, 이 클래스는 빌더 클래스에 의해 설정된 속성을 가짐   

```java
public abstract class Pizza {

    public static abstract class Builder<T extends Builder<T>> {
        protected List<String> toppings;

        public T toppings(List<String> toppings) {
            this.toppings = toppings;
            return self();
        }

        //하위클래스에서 재정의하여 this를 반환받도록 함   
        //self() 하위클래스에서 형변환 없이 메서드 연쇄를 지원함 
        protected abstract T self();

        public abstract Pizza build();
    }

}

```
각 파위 클래스의 빌더가 정의한 build메서드는 해당하는 구체 하위 클래스를 반환하도록 함    
하위 클래스의 메서드가 상위 클래스의 메서드가 정의한 반환타입이 아닌,   
 그 하위 타입을 반환하는 기능을 **"공변반환타이핑"** 이라 한다.   
이 기능을 이용하면 클라이언트가 형 변환에 신경쓰지 않고도 빌더를 사용할수있다.

```java
public static class ChicagoStylePizza extends Pizza {
        private boolean hasSauce;

        public static class Builder extends Pizza.Builder<Builder> {
            private boolean hasSauce = false;

            public Builder hasSauce() {
                this.hasSauce = true;
                return this;
            }

            @Override
            public ChicagoStylePizza build() {
                return new ChicagoStylePizza(this);
            }
        }

        private ChicagoStylePizza(Builder builder) {
            super(builder);
            this.hasSauce = builder.hasSauce;
        }
    }
}
```

빌더패턴 사용 

```java
     Pizza newyorkPizza = new Pizza.NYStylePizza.Builder()
                .size(Pizza.Size.SMALL)
                .toppings(Arrays.asList("Cheese", "Pepperoni"))
                .build();

        Pizza chicagoPizza = new Pizza.ChicagoStylePizza.Builder()
                .hasSauce()
                .toppings(Arrays.asList("Sausage", "Mushrooms"))
                .build();
```

### **빌더패턴 단점아닌 단점**    
객체를 만들려면 , 그에 앞서 빌더부터 만들어야됨, 빌더 생성비용이 크지는 않지만 성능에 민감한 상황에서 문제가 될 수 있음.    
또한 점층적 생성자 패턴보다는 코드가 장황해서 매개변수가 4개 이상은 되어야 제값함.   
하지만 프로그럄이 커질수록 매개변수가 많아지는 경향이 있음, 유연성을 위하여 빌더패턴으로 시작하는게 나을 수 있다.



