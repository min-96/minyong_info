---
title: "Stack"
weight: 4
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---

# Stack

## **Stack이란?**

![image](/DataStructure/stack)


{{< hint info >}}
**후입선출(LIFO, Last-In-First-Out)의 원칙을 따른다, 새로운 데이터는 스택의 맨위에 쌓이게 되고, 가장 최근에 쌓인 데이터부터 차례로 꺼낸다.    
스택은 주로 함수 호출과 관련된 정보를 저장하는데 사용된다. 함수가 반환되면 스택에서 해당함수의 정보를 제거하여 이전 함수로 돌아간다.**
{{< /hint >}}


## **Stack은 vector를 상속받아 구현되어있음**
동적 크기조정이 가능하고 인덱스를 사용하여 요소에 접근할수도있다.
하지만 vector클래스를 상속받기에 동기화를 지원한다.
따라서 멀티스레드에서는 ArrayList나 LinkedList를 사용하여 스택을 구현하던지, Deque를 사용하는것이 좋다.


## **LinkedList로 Stack 구현**
{{< expand "Stack 구현보기" >}}
```java
import java.util.LinkedList;

public class Stack<E> {
    LinkedList<E> linkedList = new LinkedList<>();


    public void push(E data){
        linkedList.addFirst(data);
    }

    public void pop(){
        linkedList.removeFirst();
    }

    public E peek(){
        E data =linkedList.getFirst();
        return data;
    }

    public boolean contains(E data){
        return linkedList.contains(data);
    }

    public int search(E data){
        return linkedList.indexOf(data);
    }
}
```
{{< /expand >}}









