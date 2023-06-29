---
title: "Item4"
weight: 4
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---

# Creating and Destroying Objects Item4

* * *

## **Item 4 - 인스턴스화를 막으려거든 private 생성자를 사용**

>유틸리티 클래스와 같이, 객체 지향 프로그래밍에서 상태를 갖지 않는 클래스, 즉 인스턴스를 생성할 필요가 없는 클래스를 설계할 때 사용하는 패턴


```java
public class UtilityClass {
    // 기본 생성자가 만들어지는 것을 막는다 (인스턴스화 방지용)
    private UtilityClass() {
        throw new AssertionError();
    }
}
```

생성자는 명시적으로 private 선언되어 클래스 외부에서 접근할수없다.   
내부 클래스에서 실수라도 생성자를 호출하려 할때에도 예외를 던져 인스턴스 방지   
단 , 이러한 방식은 상속도 불가능하게 만든다 이 클래스의 모든 생성자는 상위 클래스의 생성자를 명시적이나
묵시적으로 호출해야 되는데 이 클래스 생성자가 private 이므로 상위 클래스 외부에서 호출할수없다. 따라서 이 클래스를 상속하는것은 불가능   

이 방법은 클래스가 주로 정적 메서드와 정적 필드만 담고있는 **유틸리티클래스**를 만들때 주로 사용하자
