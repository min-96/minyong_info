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

또한 finalizer메소드 내에서 발생하는 예외는 무시되며, 해당예외를 처리할수있는 방법이 제한적


## **cleaner 메소드**

finalizer메소드를 대체하기 위한 목적으로 제공된 cleaner메소드   

1. 객체에서 정리 작업이 필요한 경우 , 해당 객체에 대한 cleaner 인스턴스를 생성
2. register() 메소드를 사용하여 정리 작업을 수행할 Runnable객체를 등록
3. 객체가 더이상 사용되지 않을때 cleaner가 등록된 runnable객체를 실행하여 자원을 정리

**하지만 이또한 가비지 컬렉션에 의해 함께 실행되기때문에 바로 실행되지 않는다.   
cleaner쓰레드는 정리할 대상인 인스턴스를 참조하면 안된다 순환 참조가 생겨 GC대상이 되지못함그래서 cleaner쓰레드를 만들 클래스는 반드시 static 클래스여야함**
finalizer보다 안전한것은 객체가 더이상 접근되지 않을때 적절한 시점에 clean()메소드를 호출하여 자원을 해제할수있다.   

**finalizer와 다른점은 개발자가 직접 제어하여 자원을 해제하냐안하냐 이 차이인가 ?**
**그리고 자원을 해제하는 쓰레드도 다른거고?**

## **finalizer() vs cleaner()**

### **실행시점**
> **Finalizer**: Finalizer 메서드는 가비지 컬렉션 시스템에 의해 객체가 수집되기 직전에 호출되어 호출시점을 정확하게 예측하기 어려움 
<br>
**Cleaner**: Cleaner는 명시적으로 등록된 정리 작업이 필요한 객체에 대해 사용자가 직접 정의한 시점에 실행될 수 있도록 제어할 수 있음, 일반적으로 객체가 더 이상 접근되지 않을 때 실행된다
<br>

### **사용방법**
> **Finalizer**: Finalizer 메서드는 객체의 클래스 내에 선언되어야 한다. JVM은 객체의 finalizer 메서드를 호출하여 객체의 자원을 정리
<br>
**Cleaner**: java.lang.ref.Cleaner 클래스를 사용하여 객체에 대한 정리 작업을 등록. 객체와 관련된 정리 작업을 수행하는 Runnable 객체를 Cleaner에 등록하고, Cleaner가 관리하는 스레드에서 실행
<br>

### **예외처리**
> **Finalizer**: Finalizer 메서드 내에서 발생하는 예외는 무시된다  예외를 처리하거나 전파할 수 있는 방법이 제한적.
<br>
**Cleaner**: Cleaner는 Runnable 객체를 등록하고, 해당 객체가 실행될 때 발생하는 예외를 정상적인 방법으로 처리할 수 있다
<br>

### **성능**
> **Finalizer**: Finalizer 메서드는 가비지 컬렉션의 수행 속도를 저하시킬 수 있음, 가비지 컬렉션 시스템은 finalizer 메서드의 실행을 위해 객체를 더 오랫동안 유지해야함
<br>
**Cleaner**: Cleaner는 finalizer 메서드보다 더 예측 가능하고 성능이 좋다 , Cleaner는 객체의 명시적인 종료 메서드를 호출하여 자원을 정리할 수 있다.
 
