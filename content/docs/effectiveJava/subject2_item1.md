---
title: "Item1"
date: 2023-06-02T09:49:45Z
weight: 1
draft: false
---

# Creating and Destroying Objects Item1
* * *

## **Item 1 - 생성자 대신 정적 팩터리 메서드를 고려**  
### **정적 팩토리 메서드란 무엇인가?**
* 정적 팩터리 메서드는 클래스에 대한 public static 메서드로 해당 클래스의 인스턴스를 반환
* 일반적으로 valueOf, of, getInstance, newInstance 등의 네이밍 컨벤션을 사용

### **정적 팩토리 메서드의 장점**

**1.이름을 가질 수 있다**  
메서드 이름을 통해 생성될 객체의 의미를 명확히 할수 있어서 가독성이 좋아진다.  
한 클래스에 시그니처가 같은 생성자가 여러개 필요할 것 같으면, 생성자를 정적 팩터리 메서드로 바꾸고 각각의 
차이를 잘 드러내는 이름을 지어주자 
```java
public class Model {
    private String title;
    private int number;
    
    private Model(String title, int number) {
        this.title = title;
        this.number = number;
    }
    
    public static Model createModel(String title, int number) {
        return new Model(title, number);
    }
}
```  
  
**2.호출될 때마다 인스턴스를 새로 생성하지 않아도 됨**  
같은 객체가 자주 요청되는 상황이라면 인스턴스를 재사용하여 메모리를 줄일 수 있다.  
가비지컬렉션에 부담을 줄일 수 있었 성능향상  
또한 불변클래스는 인스턴스를 미리 만들어 놓거나 새로 생성한 인스턴스를 캐싱하여 재활용한다.
```java
Integer a = Integer.valueOf(10);   // 범위 내의 값에 대한 Integer 객체 생성
Integer b = Integer.valueOf(10);   // 동일한 범위 내의 값을 가지는 객체 재활용

System.out.println(a == b);        // true (같은 객체 재활용)
```  
  
**3.반환 타입의 하위 타입객체를 반환할 수 있는 능력이 있고  
입력 매개변수의 따라 매번 다른 클래스의 객체를 반환할 수 있다.**    
생성자와는 달리 반환타입의 하위 객체를 반환할수 있는 유연성이다.  
생성자를 사용할 경우 반환타입은 항상 인스턴스를 생성하는 클래스의 정확한타입이다.  
그러나 정적 팩토리메서드를 사용하면 반환타입의 하위 타입객체를 반환 할 수 있어 더 구체적이고 특화된 객체를 반환할 수 있다.  
```java
public interface Shape {
   ...
}

public class Circle implements Shape {
   ...
}

public class Square implements Shape {
   ...
}

public class ShapeFactory {
    public static Shape createShape(String shapeType) {
        if (shapeType.equalsIgnoreCase("circle")) {
            return new Circle();
        } else if (shapeType.equalsIgnoreCase("square")) {
            return new Square();
        }
        return null;
    }
}

```

**4.정적 팩토리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.**  

이는 정적 팩터리 메서드가 인터페이스, 추상 클래스 또는 다른 클래스의 서브 클래스를 반환할 수 있는 유연성을 제공한다는 의미
예제코드드 3번과 같으며,  createShape 정적 팩터리 메서드는 입력에 따라 "Circle" 또는 "Square"와 같은 다양한 도형 객체를 반환.  
 이 때, 정적 팩터리 메서드는 인터페이스인 "Shape"을 반환하고, 실제 반환되는 객체의 클래스는 메서드 호출 시점에 결정된다.  
 따라서 반환될 객체의 클래스가 작성 시점에 존재하지 않아도 되며, 메서드 호출 시에 적절한 클래스의 인스턴스를 반환할 수 있다  
반환될 객체의 클래스가 작성 시점에 존재하지 않아도 되므로, 유연한 인스턴스 생성과 다형성을 구현할 수 있다.이를 통해 런타임에 객체의 타입을 결정하고, 객체 생성과 관련된 다양한 기능을 구현할 수 있다


### **정적팩토리 메소드의 단점**

**1.상속을 하려면 public 이나 protected 생성자가 필요하니 정적팩토리 메서드만 제공하면 하위 클래스를 만들수없다.**  
하위 클래스를 만들려면 해당 클래스의 생성자에 접근할 수 있어야 한다.   
하지만 정적 팩터리 메서드는 해당 클래스 내에서만 호출할 수 있으며, 하위 클래스에서 직접 호출할 수 없다.   
따라서 정적 팩터리 메서드만을 제공하고 생성자에 대한 접근을 제한하면, 하위 클래스에서 새로운 객체를 생성하는 것이 불가능해진다.   
<br>
<br>

### **정적 팩터리 메서드에 흔히 사용되는 명명**    

1. valueOf : 매개변수 기반으로 Integer객체를 생성하고 반환    
2. of : 주어진 매개변수를 기반으로 객체를 생성하고 반환
3. create :  예를 들어, Thread 클래스의 createThread 메서드는 새로운 Thread 객체를 생성하여 반환
4. parse : 해당 타입의 객체로 변환하여 반환
5. builder : 빌더 패턴을 사용하여 복잡한 객체를 생성하는데 사용











