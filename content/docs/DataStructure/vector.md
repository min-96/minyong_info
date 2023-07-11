---
title: "Vector"
weight: 3
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---

# Vector

## **Vector란?**

{{< hint info >}}
**Vector는 컬렉션 프레임워크(Collection Framework)가 존재하기 전에 추가된 레거시 클래스 중 하나
Vector는 ArrayList와 동일한 내부구조를 가지고있음 하지만 vector와 arrayList의 차이점은 vector는 동기화된 메소드로 구성되어있어 멀티스레드가 동시에 메소드를 실행할수없음, 멀티스레드 환경에서 안전하게 객체를 추가하고 삭제할수있다.**
{{< /hint >}}


## **Vector와 ArrayList**
vector는 멀티스레드 환경에서 동기화되어있기에 안전하지만, 스레드가 1개일때도 동기화를 하기 때문에 ArrayList보다 성능이 떨어진다.   
ArrayList와 동일하나 더 속도가 빠르기때문에 ArrayList를 주로 사용한다




