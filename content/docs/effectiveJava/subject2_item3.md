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


### **방법 1 :  private 생성자와 public static final 필드를 사용하는 것**
static final 필드 INSTANCE를 사용하여 하나의 인스턴스를 만들며,이는 클래스가 로드될 때 한 번만 생성된다   
생성자는 private로 선언되어 있어서 클래스 외부에서는 new 키워드를 통해 Singleton 클래스의 객체를 만들 수 없다   
 이로 인해 Singleton.INSTANCE를 통해서만 Singleton 클래스의 인스턴스에 접근할 수 있다.

