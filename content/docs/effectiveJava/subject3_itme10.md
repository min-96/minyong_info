---
title: "Itme10"
weight: 10
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---

# Common methods for all objects Itme10

* * *

## **Item10 - equals를 재정의할 때는 일반규약을 따르자**

> **Item10은 equals()를 재정의 할 때 주의해야 될 상황에 대해서 설명한다.**

### **equals 메소드란 ?**
사용자가 입력한 두 문자열이 동일한지 확인하거나, 두 객체가 같은 데이터를 나타내는지 비교할 때 equals() 메서드를 사용한다

## **equal()를 재정의해야 하는 경우**

### **객체의 논리적 동등성 비교가 필요한 경우**

객체의 논리적 동등성 비교가 필요한 경우는 주로 객체의 내부 상태나 속성이 같은지를 기준으로 판단해야할때이다.

```java 
public class User {
    private String name;
    private String email;

    @Override
    public boolean equals(Object obj) {
        if (this == obj) {
            return true;
        }
        if (obj == null || getClass() != obj.getClass()) {
            return false;
        }
        User user = (User) obj;
        return Objects.equals(name, user.name) &&
               Objects.equals(email, user.email);
    }
    }
}

```

User객체가 이름과 이메일이 모두 동일한 경우에만 논리적으로 동등된다고 판단되어 equals메소드로 재정의해야된다.    

### **클래스의 동등성 비교를 지원해야하는 경우**

클래스가 컬렉션에 저장되거나 검색되는 등의 동작을 수행하는 경우, 객체의 동등성 비교를 지원하기 위해 equals() 메서드를 재정의해야한다.HashSet, HashMap과 같은 컬렉션 클래스는 객체의 중복을 확인하거나, 특정 객체를 검색할 때 equals() 메서드를 사용한다.   

위에서 사용한 코드를 재사용해 HashSet에 저장하고 중복을 확인해보자
```java 
public class Main {
    public static void main(String[] args) {
        User user1 = new User("Alice", 25);
        User user2 = new User("Bob", 30);
        User user3 = new User("Alice", 25);

        Set<User> hashSet = new HashSet<>();
        hashSet.add(user1);
        hashSet.add(user2);
        hashSet.add(user3);

        System.out.println(hashSet.size());  // 출력 결과: 2
    }
}
``` 
equals() 메서드의 재정의를 통해 동일한 내부 상태를 가지는 User 객체를 동등하다고 판단하였기 때문에, 컬렉션 클래스에서 객체의 동등성 비교를 정확하게 수행할 수 있다.


## **equals()를 재정의할때 일반규약**


### **반사성**
반사성은 모든 객체나 요소는 자기 자신과 동등하다는 것을 의미한다.    
즉 null이 아닌 모든 참조 값 x에 대해, x.equals(x)는 true를 반환해야한다.


### **대칭성**
두 객체나 요소가 동등하다면, 그 역도 성립한다는것을 의미한다.   
x.equals(y)가 true라면 y.equals(x)도 true다.


### **추이성**
세 개의 객체나 요소 x, y, z가 동등한 상황에서 x와 y, y와 z가 동등하다면, x와 z도 동등하다는 것을 의미한다.   
 x.equals(y)가 true이고, y.equals(z)도 true라면 x.equals(z)도 true


### **일관성**
만약 객체의 상태가 변경되지 않았다면, 동등성 비교의 결과는 변하지 않아야한다.   
equals() 메서드는 객체의 상태에 영향을 받지 않고 항상 일관된 결과를 반환해야한다.

두 객체 x와 y가 동일한 상태를 가지고 있다고 가정해보면, 이때, x.equals(y)를 호출하면 true를 반환해야 한다. 이후에도 x와 y의 상태가 변경되지 않는다면, x.equals(y)를 다시 호출해도 여전히 true를 반환해야한다.   
<br>
 일관성 원칙은 equals() 메서드뿐만 아니라 hashCode() 메서드와의 관련성도 갖습니다. equals() 메서드가 두 객체를 동일하다고 판단하는 경우, 두 객체는 동일한 해시 코드를 반환하는 것이 일관성을 유지하기 위한 권장 사항

### **equals(null)의 반환**
x.equals(null)은 항상 false를 반환해야 한다.  즉, null과는 동등하지 않은 객체는 equals() 메서드를 통해 확인해야 한다.   
null은 유효한 객체가 아니므로 null과 동등한 객체는 존재하지 않는다.



