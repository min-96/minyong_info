---
title: "LinkedList"
weight: 2
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---

# LinkedList
* * *

## **LinkedList란?**
{{< hint info >}}
**LinkedList는 자바 컬렉션 프레임워크에서 제공되는 리스트(List) 인터페이스의 구현체 중 하나이다. 이는 이중 연결 리스트(doubly linked list)로 구현되어 있어, 리스트의 요소들이 이전 요소와 다음 요소와의 연결을 갖는다  각 요소는 데이터와 이전 요소에 대한 참조, 다음 요소에 대한 참조를 가지고 있다.**
{{< /hint >}} 


* **삽입과 삭제** : LinkedList는 삽입과 삭제에 대해 편함, 요소를 삽입하거나 삭제할 때에는 해당 위치에 대한 참조만 수정하면 되기 때문에, 배열과 달리 요소들을 이동시킬 필요가 없다. 따라서 삽입과 삭제 연산의 시간 복잡도는 O(1)입니다.

* **접근 시간**: LinkedList는 특정 위치의 요소에 접근하는 데에는 선형 시간이 소요된다. 특정 인덱스로 직접 접근하기 위해서는 연결 리스트를 처음부터 탐색해야 하므로, 접근 시간은 O(n)

* **메모리 사용량**: LinkedList는 각 요소마다 이전 요소와 다음 요소에 대한 참조를 유지해야 하므로, 메모리 사용량이 상대적으로 더 많다. 이는 각 요소에 추가적인 참조 필드가 필요하기 때문임


![image](/DataStructure/linkedList)


## **LinkedList 구현**
{{< expand "LinkedList 구현보기" >}}
```java
public class LinkedList<E> {
    private Node<E> head;
    private Node<E> tail;
    private int size;

    class Node<E> {
        private E data;
        private Node<E> next;
        private Node<E> prev;

        public Node(E data) {
            this.data = data;
            this.prev = null;
            this.next = null;
        }
    }

    public LinkedList() {
        this.head = null;
        this.tail = null;
        this.size = 0;
    }

    public void add(E value){
        Node<E> newNode = new Node<>(value);
        if(head == null) {
            head = newNode;
        }else {
            tail.next = newNode;
            newNode.prev = tail;
        }
        tail = newNode;
        size++;

    }

    public void add(int index, E value){
        Node<E> newNode = new Node<>(value);
        Node<E> node = head;
        for(int i=0; i<index-1; i++){
            node = node.next;
        }
        newNode.next = node.next;
        node.next.prev = newNode;
        node.next = newNode;
        newNode.prev = node;
        size++;
    }

    public E get(int index){
        Node<E> node = head;
        for(int i=0; i<index; i++){
            node = node.next;
        }
        return node.data;
    }

    public void remove(int index){
        Node<E> node = head;
        for(int i=0; i<index; i++){
            node = node.next;
        }
        node.prev.next = node.next;
        node.next.prev = node.prev;
        size--;
    }

}
```
{{< /expand >}}

