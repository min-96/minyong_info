---
title: "Item8"
weight: 8
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---

# Creating and Destroying Objects Item8
* * *

## **Item 8 - finalizer와 cleaner 사용을 피하자**

> **Itme 8번은 명시적인 종료 메서드를 정의하여 메모리 누수와 성능저하 문제를 피하기 위한 방법제시**

## **finalizer 메소드**

가비지 컬렉션 시스템에 의해 수거되기 전에 호출되는 메서드
```java
protected void finalize() throws Throwable {
}
```
일반적으로 객체의 자원해제 또는 정리작업을 수행하는데 사용된다.   
이 메서드를 오버라이딩하여 자신이 소유한 리소스를 해제

**하지만   
가비지 컬렉터가 실행되는 시점을 정확히 할수 가 없음, 그래서 메모리 해제를 지연시킬 가능성이 크다
또한 finalizer메서드는 가비지 컬렉션 시스템은 finalizer메서드의 실행을 위해 객체를 더 오랫동안 유지해야되므로  가비지 컬렉션의 수행속도를 저하시킬수도 있다**   
   
