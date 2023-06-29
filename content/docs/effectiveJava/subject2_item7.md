---
title: "Item7"
weight: 7
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---

# Creating and Destroying Objects Item7
* * *

## **Item 7 - 다 쓴 객체 참조를 해제하자**

> **개발자가 명시적으로 객체 참조를 제거하지 않으면, 가비지 컬렉션이 이루어지지 않을 수 있다   
 이로 인해 메모리 누수(memory leak)가 발생하고, 이는 애플리케이션의 성능 저하를 일으킬수있다.
이를 방지하기 위해 더 이상 사용하지 않는 객체 참조를 null로 설정하자**

## **자기 메모리를 직접 관리하는 클래스에서만 객체 참조를 null로 설정**

```java
public class Stack {
    private Object[] elements;
    private int size = 0;

    public Stack(int initialCapacity) {
        this.elements = new Object[initialCapacity];
    }

    public void push(Object object) {
        ensureCapacity();
        elements[size++] = object;
    }

    public Object pop() {
        if (size == 0) {
            throw new EmptyStackException();
        }
        Object result = elements[--size];
        elements[size] = null; // 더 이상 필요 없는 참조 해제
        return result;
    }

    private void ensureCapacity() {
        if (elements.length == size) {
            elements = Arrays.copyOf(elements, 2 * size + 1);
        }
    }
}
```
스택에서 객체를 제거한 후에 해당 참조를 null로 설정하여 , 가비지 컬렉션 대상임을 명시적으로 나타낸다.

## **캐시 , 리스너, 콜백 등등 메모리 누수 원인이 될 수 있다.**

1. **캐시** : 캐시는 시스템 성능을 향상시키기 위해 사용되지만, 캐시에 저장된 객체들이 적절하게 제거되지 않으면 메모리 누수를 일으킬 수 있다.    
 예를 들어, 캐시에 데이터를 계속 추가하면서 오래된 데이터를 제거하지 않으면 메모리가 계속 증가하게 됩니다. 이를 해결하기 위해 캐시 항목의 유효성을 정기적으로 확인하고, 필요 없는 항목은 제거하는 등의 캐시 정책을 구현해야한다.

2. **리스너나 콜백**: 이벤트 리스너나 콜백 함수 등을 등록하지만 해제하지 않으면, 이들이 계속 메모리에 남아있게 되어 메모리 누수를 일으킬 수 있다.  이를 해결하기 위해 필요 없어진 리스너나 콜백은 적절하게 해제하는 것이 필요함.



## **WeakHashMap 을 이용한 메모리 관리**
 WeakHashMap의 작동 방식을 이해하려면 JVM의 GC와 관련하여 WeakReference  를 조금은 이해할 필요가 있다.  Java에서는 세 가지 주요 유형의 참조(Reference) 방식이 존재한다.   

**강한 참조 (Strong Reference)**   
– Integer prime = 1;   와 같은 가장 일반적인 참조 유형이다.    prime 변수 는 값이 1 인 Integer 객체에 대한 강한 참조 를가진다.  이 객체를 가리키는 강한 참조가 있는 객체는 GC대상이 되지않는다.
 

**부드러운 참조 (Soft Reference)**   
– SoftReference<Integer> soft = new SoftReference<Integer>(prime);   와 같이 SoftReference Class를 이용하여 생성이 가능하다.  만약 prime == null 상태가 되어 더이상 원본(최초 생성 시점에 이용 대상이 되었던 Strong Reference) 은 없고 대상을 참조하는 객체가 SoftReference만 존재할 경우 GC대상으로 들어가도록 JVM은 동작한다.   다만 WeakReference 와의 차이점은 메모리가 부족하지 않으면 굳이 GC하지 않는 점이다.  때문에 조금은 엄격하지 않은 Cache Library들에서 널리 사용되는 것으로 알려져있다.
 

**약한 참조 (Weak Reference)**   
– WeakReference<Integer> soft = new WeakReference<Integer>(prime);   와 같이 WeakReference Class를 이용하여 생성이 가능하다.  prime == null 되면 (해당 객체를 가리키는 참조가 WeakReference 뿐일 경우) GC 대상이 된다.  앞서 이야기 한 내용과 같이 SoftReference와 차이점은 메모리가 부족하지 않더라도 GC 대상이 된다는 것이다.    다음 GC가 발생하는 시점에 무조건 없어진다.

```java
import java.util.Map;
import java.util.WeakHashMap;

public class CacheManager {
    private Map<Object, Object> cache = new WeakHashMap<>();

    public void put(Object key, Object value) {
        cache.put(key, value);
    }

    public Object get(Object key) {
        return cache.get(key);
    }
}

```

하지만 캐시된 데이터를 장시간에 보관하려면 다른 방법을 사용해야됨   
 weakHashMap은 장시간 걸친 캐시보다는 짧은 시간 동안만 캐시해야하는 경우나 리스너와 같은 일시적인 참조를 저장하는 용도로 더 적합하다.
