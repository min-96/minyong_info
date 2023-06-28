---
title: "Item3"
weight: 3
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---
# Creating and Destroying Objects Item3
* * *

## **Item 3 - private 생성자나 열거 타입으로 싱글턴임을 보증**

### **싱글턴이란?**
인스턴스를 오직 하나만 생성할 수 있는 클래스를 말한다.   
무상태(stateless)객체나 설계상 유일해야 되는 시스템 컴포넌트   

> **Item3에서는 싱글턴을 디자인 패턴을 구현하는 두가지 방법 제시**


### **방법 1 :  private 생성자와 public static final 필드를 사용**
static final 필드 INSTANCE를 사용하여 하나의 인스턴스를 만들며,이는 클래스가 로드될 때 한 번만 생성된다   
생성자는 private로 선언되어 있어서 클래스 외부에서는 new 키워드를 통해 Singleton 클래스의 객체를 만들 수 없다   
 이로 인해 Singleton.INSTANCE를 통해서만 Singleton 클래스의 인스턴스에 접근할 수 있다.
```java
public class Singleton {
    public static final Singleton INSTANCE = new Singleton();

    private Singleton() {
    
    }

}
```

구현이 간결하다는 장점이 있다. 또한, 직렬화와 관련하여 추가 작업이 필요하지 않다.        
그러나 이 방식은 reflection을 통한 공격에 취약할 수 있다.    
즉, reflection을 사용하여 private 생성자를 호출하여 싱글턴이 아닌 추가 인스턴스를 생성할 수 있다   
이 문제는 생성자에서 두 번째 객체 생성을 확인하고 예외를 던짐으로써 완화할 수 있다   



### **방법 2 : private 생성자와 함께 static factory method를 사용**

이 방식은 publuc static final 필드를 사용하는 방식과 비슷하다   
```java
public class Singleton {
    private static Singleton INSTANCE = null;

    private Singleton() {}

  // lazy initialization
    public static Singleton getInstance() {
        if (INSTANCE == null) {
            INSTANCE = new Singleton();
        }
        return INSTANCE;
    }

    // 이 메서드를 호출하여 싱글턴이 아닌 인스턴스를 반환하도록 변경할 수 있음
    public static Singleton getNewInstance() {
        return new Singleton();
    }
}
```
*  api를 변경하지 않고도 싱글턴이 아니게 변경할수있다. 팩토리 메서드가 반환하는 인스턴스 종류를 변경하기만 됨.
* 원하는 경우에만 싱글턴을 만들수있다. 이걸 lazy initialization이라 부름.
* 제네릭 싱글턴 팩토리로 만들수있음
* 메서드 참조를 공급자로 사용할수있음

### **제테릭 싱글턴 팩토리**
```java
public class GenericSingletonFactory {
    private static UnmodifiableList<?> INSTANCE = null;

    private GenericSingletonFactory() {}

    @SuppressWarnings("unchecked")
    public static <T> UnmodifiableList<T> getInstance() {
        if (INSTANCE == null) {
            INSTANCE = new UnmodifiableList<>();
        }
        return (UnmodifiableList<T>) INSTANCE;
    }
}
```
### **메서드 참조를 공급자로 사용** 
```java
public class SingletonUser {
    public static void main(String[] args) {
        Supplier<Singleton> singletonSupplier = Singleton::getInstance;
        Singleton singleton = singletonSupplier.get();
        singleton.someMethod();
    }
}
```

### **문제 1 : 하지만 이 두 방법 모두 reflection 공격에 취약함**   

### **reflection 이란?** 
Reflection은 런타임에 클래스, 인터페이스, 메서드 및 변수와 같은 객체를 검사하거나 수정할 수 있게 하는 강력한 기능
Reflection API를 사용하면 런타임에 클래스의 객체를 만들고, 메서드를 호출하고, 변수의 값을 바꿀 수 있다.

그러나 Reflection은 그 강력함 때문에 부주의하게 사용될 경우 보안 문제를 초래할 수 있다.
private나 protected로 선언된 멤버에 접근할 수 있으므로, 이를 악용하면 싱글턴 패턴과 같이 멤버의 개수나 상태를 통제하려는 디자인 패턴을 깨트리는 것이 가능하다.
이러한 가능성을 “Reflection을 통한 공격"이라 부름

**reflection을 해결**
```java
public class Singleton {
    public static final Singleton INSTANCE = new Singleton();
    private Singleton() {
        if (INSTANCE != null) {
            throw new IllegalStateException("Already instantiated");
        }
    }
}
```

### **문제 2 : 직렬화 처리**
Singleton이 Serializable 인터페이스를 구현한다면, 직렬화와 역직렬화 과정에서 새로운 인스턴스가 생성될수 있다.   
자바의 직렬화/역직렬화 프로세스는 기본적으로 객체의 상태를 저장하고 복원하는 방법입니다. 역직렬화 과정에서는
해당 객체의 새로운 인스턴스가 생성된다. 
```java
import java.io.*;

public class Singleton implements Serializable {
    public static final Singleton INSTANCE = new Singleton();

    private Singleton() {
        // Prevent form the reflection api.
        if (INSTANCE != null) {
            throw new IllegalStateException("Already created.");
        }
    }
    
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        // 직렬화
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("singleton.bin"));
        oos.writeObject(Singleton.INSTANCE);
        oos.close();

        // 역직렬화
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream("singleton.bin"));
        Singleton singleton2 = (Singleton) ois.readObject();
        ois.close();

        // 출력 결과 : false
        System.out.println(Singleton.INSTANCE == singleton2);
    }
}
```
readObject() 메서드는 싱글턴 객체의 새로운 인스턴스를 생성합니다. 마지막 줄에서 비교하는 == 연산의 결과가 false로 나타나는 이유   
이것은 싱글턴이라는 원칙, 즉 동일 클래스에 대해 생성되는 인스턴스가 오직 하나임을 보장하는 원칙을 위배하는 것

** 

