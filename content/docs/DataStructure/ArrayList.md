---
title: "ArrayList"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---


# ArrayList 
* * *

## **ArrayList란?**

{{< hint info >}}
**ArrayList는 List 인터페이스를 상속받은 클래스로 크기가 가변적으로 변하는 선형리스트다.
 일반적인 배열과 같은 순차 리스트이며 인덱스로 내부의 객체를 관리한다는점등이 유사하지만 한번 생성되면 크기가 변하지 않>는 배열과는 달리 ArrayList는 객체들이 추가되어 저장 용량(capacity)을 초과한다면 자동으로 부족한 크기만큼 저장 용량(capacity)이 늘어난다는 특징을 가지고 있다**
{{< /hint >}}



![image](/DataStructure/ArrayList1)
![image](/DataStructure/ArrayList)


* **순서가 있는 데이터 저장**: List는 요소들의 순서를 유지한다. 따라서 데이터를 저장할 때 입력한 순서대로 요소들이 유지되어 관리된다. 

* **중복 요소 저장**: List는 중복된 요소를 허용한다. 동일한 요소를 여러 번 저장할 수 있으며, 각 요소는 별도의 인덱스를 가지게 된다

* **인덱스 기반 접근**: List는 인덱스를 사용하여 요소에 빠르게 접근할 수 있다.  인덱스를 통해 특정 위치의 요소를 읽거나 수정하는 것이 가능하며, 이는 데이터 검색 및 수정 작업에 효율적이다.


## **ArrayList 구현**

{{< expand "ArrayList 구현보기" >}}

```java

public class ArrayList<E> {
    private int size;
    private Object[] elementData;

    private static final int DEFAULT_CAPACITY = 10;

    public ArrayList() {
        this.size =0;
        this.elementData = new Object[DEFAULT_CAPACITY];
    }

    public void add(E ele){
        if(size == elementData.length){
            int newArr = elementData.length * 2;
            elementData = Arrays.copyOf(elementData, newArr);
        }
        elementData[size] = ele;
        size++;
    }

    public E get(int index){
        if(index <0 || index >= size){
            throw new IndexOutOfBoundsException("Index: " + index + ", Size " + index);
        }else{
            return (E)elementData[index];
        }
    }

    public void remove(int index){
        if(index <0 || index > elementData.length){
            throw new IndexOutOfBoundsException("Index: " + index + ", Size " + index);
        }
        for(int i=0; i<size-1; i++){
            elementData[i] = elementData[i+1];
        }
        size--;
        elementData[size-1] = null;
    }

    public int size(){
        return size;
    }


    public boolean contains(E ele){
        for(int i=0; i<size; i++){
            if(ele.equals(elementData[i])){
                return true;
            }
        }
        return false;
    }
}
```
{{< /expand >}}


## **ArrayList 메소드 종류**
```java
        List<Integer> list = new ArrayList<>(Arrays.asList(1,3,2,4,3));

        list.remove(1);
        // 1,2,4,3
        list.size();
        // 4
        list.contains(3);
        // true
        list.get(1);
        // 2
        list.add(1, 3);
        // 1,3,2,4,3  -> 인덱스가 뒤로 밀리게 됨
        list.set(1, 5);
        // 1,5,2,4,3 // 인덱스에 있는 요소가 대체됨
        list.sort((o1, o2) -> o1 - o2);
        // 1,2,3,4,5
        list.sort((o1, o2) -> o2 - o1);
        // 5,4,3,2,1
        list.indexOf(5);
        // 0
        list.add(3);
        // 5,4,3,2,1,3
        list.lastIndexOf(3);
        // 5
        list.addAll(Arrays.asList(6,7,8));
        // 5,4,3,2,1,3,6,7,8
        list.addAll(2, Arrays.asList(9,10));
        // 5,4,9,10,3,2,1,3,6,7,8
       // list.stream().forEach(System.out::println);
        list.containsAll(Arrays.asList(1,2,3));
        // true
        list.equals(Arrays.asList(5,4,9,10,3,2,1,3,6,7,8));
        // true
        list.subList(2, 5);
        // 9,10,3
        list.replaceAll(e -> e * 2);
        // 10,8,18,20,6,4,2,6,12,14,16
        list.removeAll(Arrays.asList(10, 8, 18));
        // 20,6,4,2,6,12,14,16
        list.retainAll(Arrays.asList(20, 6, 4)); // 교집합만 남게됨


	 ListIterator<Integer> iterator = list.listIterator();
        	while (iterator.hasNext()) {
            	int element = iterator.next();
            // 20,6,4,6
        } 
        // ==
        iterator.forEachRemaining(System.out::println);
        // 20,6,4,6


	list.toArray(new Integer[0]);
        // 20,6,4,6 -> Integer[] 배열로 변환


	Spliterator<Integer> spliterator =list.spliterator();     
	//리스트의 요소들을 병렬로 처리하기 위한 Spliterator를 반환
        spliterator.forEachRemaining(System.out::println);



```
