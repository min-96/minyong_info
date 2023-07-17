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

 **사용자 정보비교**    
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

