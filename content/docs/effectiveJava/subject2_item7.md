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

> 개발자가 명시적으로 객체 참조를 제거하지 않으면, 가비지 컬렉션이 이루어지지 않을 수 있다   
 이로 인해 메모리 누수(memory leak)가 발생하고, 이는 애플리케이션의 성능 저하를 일으킬수있다.
이를 방지하기 위해 더 이상 사용하지 않는 객체 참조를 null로 설정하자

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

