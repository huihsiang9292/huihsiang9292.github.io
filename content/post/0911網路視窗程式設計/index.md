---
title: Week 1
date: 2024-09-11
categories:
  - 網路視窗程式設計
---
- [課程平台](140.137.41.143/chenexam.php)
`B2402985` `java`

## 課堂練習

### 一

```java
public class Main {
    static public void main(String[] args) {
        System.out.println("Hello World !");
    }
}
```

### 二

```java
public class Main {
    static public void main(String[] args) {
        System.out.println("What your name ?");
        java.util.Scanner sc = new java.util.Scanner(System.in);
        String str = sc.nextLine();
        System.out.println(str +", how are you ?");
    }
}
```
