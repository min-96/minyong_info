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

## **cleaner 메소드**

finalizer메소드를 대체하기 위한 목적으로 제공된 cleaner메소드   

1. 객체에서 정리 작업이 필요한 경우 , 해당 객체에 대한 cleaner 인스턴스를 생성
2. register() 메소드를 사용하여 정리 작업을 수행할 Runnable객체를 등록
3. 객체가 더이상 사용되지 않을때 cleaner가 등록된 runnable객체를 실행하여 자원을 정리

## **finalizer() vs cleaner()**

 **실행시점**    
Finalizer: Finalizer 메서드는 가비지 컬렉션 시스템에 의해 객체가 수집되기 직전에 호출되어 호출시점을 정확하게 예측하기 어려움 
 
