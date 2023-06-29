---
title: "Item5"
weight: 5
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---


# Creating and Destroying Objects Item5

* * *

## **Item 5 - 자원을 직접 명시하지 말고 의존객체 주입을 사용**

> item5 에서는  클래스가 하나 이상의 자원에 의존하고 있고 , 클래스가 이런 자원을 사용하는 방법을 클래스 사용자가 설정하게 하려면   
의존성주입 패턴을 사용하는것이 좋다


## **의존 객체를 사용하기 전**

**클래스가 필요로 하는 자원을 클래스 내부에서 직접 생성**   

```java
public class SpellChecker {
    private final Dictionary dictionary = new EnglishDictionary();
    // EnglishDictionary는 Dictionary의 한 구현체라고 가정
}
```

**싱글톤 패턴을 사용하는 경우**
```java
public class SpellChecker {
    private final Dictionary dictionary = Dictionary.getInstance();
    // Dictionary는 싱글톤 인스턴스를 제공
}

```


Dictionary 를 직접 생성하고있음   
다음과 같은 문제점   

* 강한 결합도 : SpellChecker가 EnglishDictionary에 강하게 결합되어 있음 따라서 다른 Dictionary를 사용하려면    
SpellChecker의 코드를 직접 수정해야됨. 코드 유연성이 저해되고 코드 수정 시 부작용을 초래할 가능성이 높음

* 코드 재사용성 :  SpellChecker는 EnglishDictionary에 의존적임 만약 다른 언어의 사전을 사용해야 하는 SpellChecker가 필요하다면, 이 코드는 재사용할 수 없음
