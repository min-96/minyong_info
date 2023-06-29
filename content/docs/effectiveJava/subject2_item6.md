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

# Creating and Destroying Objects Item5   

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


