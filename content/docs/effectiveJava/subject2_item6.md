---
title: "Item6"
weight: 6
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---

# Creating and Destroying Objects Item6   

* * *

## **Item 6 - 불필요한 객체 생성을 피하자**

> **item6번에서는 불필요하게 많은 객체를 생성하면 메모리 사용이 증가하고, GC에의한 성능저하 발생할수있다. 따라서 객체를 재사용하거나 필요할때 직접 생성하자**

## **불필요한 객체를 생성한 문제**

```java
String s = new String("stringett");
```
위 코드는 String객체를 매번 새롭게 생성하고있다.   


## **객체 재사용**
```java
String s = "stringett";
```
문자열 리터럴을 사용하면 동일한 문자열에 대해 한번만 객체를 생성하고 재사용하므로 더 효율적이다.   
String 리터럴은 내부적으로 String 풀 이라는 공간에 저장되고 관리된다   
같은 문자열 리터럴이 존재하는 경우, 새로운 String 객체를생성하는 대신 이미 존재하는 객체의 참조를 반환하다.
```java
String str1 = new String("jinui");
String str2 = new String("jinui");
System.out.println(str1 == str2); // false

String str3 = "jinui";
String str4 = "jinui";
System.out.println(str3 == str4); // true
```

**또 다른 예시**
```java
Boolean b = new Boolean(true);
```
```java
Boolean b = Boolean.valueOf(true);
```
정적 메소드를 사용하여 객체 재사용을 함 


## **값비싼 객체 재사용**
* **객체 풀**: 객체 풀(object pool)은 생성 비용이 큰 객체를 미리 생성해두고 필요할 때 재사용하는 방식입니다. 풀에 객체가 없을 때는 새로운 객체를 생성하고, 사용 후에는 다시 풀에 반환합니다. 데이터베이스 연결 풀이 이러한 예입니다.

* **싱글턴 패턴**: 싱글턴 패턴은 클래스의 인스턴스가 하나만 생성되도록 보장하는 디자인 패턴입니다. 값비싼 객체를 매번 새로 생성하는 대신 싱글턴 객체를 한 번만 생성하고 이를 재사용합니다.

* **정적 멤버**: 클래스의 정적 멤버는 클래스 레벨에서 공유되므로, 객체 생성 비용이 크지만 한 번만 생성하면 되는 객체를 정적 멤버로 두는 것이 좋을 수 있습니다.

* **불변 객체**: 불변 객체(Immutable Object)는 생성 후 상태가 변경되지 않는 객체를 의미합니다. 불변 객체는 안전하게 공유할 수 있으므로, 한 번 생성한 뒤 필요할 때마다 재사용할 수 있습니다.
