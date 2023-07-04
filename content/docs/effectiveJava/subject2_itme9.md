---
title: "Itme9"
weight: 9
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---


# Creating and Destroying Objects Item9 
* * *

## **Item 9 - try-finally보다는 try-with-resources를 사용하라**

> **item9번에서는 꼭 회수해야되는 자원을 닫을때 예외처리로 자원이 제대로 해제되도록 보장하게 만드는 방법을 제시**


## **try-finally 블록을 사용**

자원을 사용하는 코드에서 예외처리로 자원해제하기위해 try-finally블록을 사용하면 예외가 발생하더라도 항상 자원이 제대로 해제되도록 보장할 수 있다

```java
public void processFile(String fileName) {
    FileInputStream fileInputStream = null;
    try {
        fileInputStream = new FileInputStream(fileName);
        // 파일 처리 로직
        // ...
    } catch (IOException e) {
        // 예외 처리
        // ...
    } finally {
        if (fileInputStream != null) {
            try {
                fileInputStream.close();
            } catch (IOException e) {
                // 자원 해제 예외 처리
                // ...
            }
        }
    }
}

```

try 블록에서 파일을 열고 처리한 후 finally블록에서 자원인 fileInputStream 을 해제하고있다   
하지만 finally 블록은 try블록 내에서 발생한 예외와 생관없이 항상 실행되어야 하므로 코드가 복잡해지고 가독성이 떨어짐
**여러개의 자원을 처리하는 경우 finally 블록은 더욱 복잡해질가능성이 큼**

```java
public void processResource() {
    Resource resource1 = null;
    Resource resource2 = null;
    Resource resource3 = null;

    try {
        resource1 = acquireResource1();
        resource2 = acquireResource2();
        resource3 = acquireResource3();

        // 자원 처리 로직
        // ...
    } finally {
        if (resource3 != null) {
            try {
                resource3.close();
            } catch (IOException e) {
                // 자원 해제 예외 처리
                // ...
            }
        }

        if (resource2 != null) {
            try {
                resource2.release();
            } catch (Exception e) {
                // 자원 해제 예외 처리
                // ...
            }
        }

        if (resource1 != null) {
            try {
                resource1.cleanup();
            } catch (Exception e) {
                // 자원 해제 예외 처리
                // ...
            }
        }
    }
}
```

## **try-with-resorces 사용**

try-with-resources 는 try 블록에 자원을 할당하면, 해당 자원이 사용이 끝나면 자동으로 해제됨   
자원 클래스는 AutoCloseable 인터페이스를 구현해야됨 AutoCloseable 인터페이스에는 close() 메서드가 정의되어 있으므로 자원해제 작업을 수행함

```java
public void processFile(String fileName) {
    try (FileInputStream fileInputStream = new FileInputStream(fileName)) {
        // 파일 처리 로직
        // ...
    } catch (IOException e) {
        // 예외 처리
        // ...
    }
}
```
**try-finally보다 더욱 간결하고 가독성이 좋아짐** 여러개의 자원을 처리할때도 한번에 처리할수있음 예외가 발생하면 가장 마지막에 할당된 자원부터 역순으로 close()메서드가 호출되기 때문에 자원의 해제 순서에 대해서도 신경쓸 필요가 없음


## **AutoCloseable의 인터페이를 구현한 일부 클래스들**

* FileInputStream 및 FileOutputStream: 파일 입출력을 위한 클래스로, 파일을 자동으로 닫을 수 있다.

* Socket 및 ServerSocket: 네트워크 통신을 위한 클래스로, 소켓 연결을 자동으로 닫을 수 있다.

* Connection, Statement, ResultSet 등 JDBC 관련 클래스: 데이터베이스 연결 및 쿼리 실행을 위한 클래스들로, 자원을 자동으로 해제할 수 있다.

* BufferedReader 및 BufferedWriter: 버퍼링된 입출력을 제공하는 클래스로, 자원을 자동으로 닫을 수 있다.

* ZipInputStream 및 ZipOutputStream: 압축 파일 입출력을 위한 클래스로, 압축 파일을 자동으로 닫을 수 있다.

* Scanner: 입력 스트림을 파싱하기 위한 클래스로, 자원을 자동으로 해제할 수 있다.




