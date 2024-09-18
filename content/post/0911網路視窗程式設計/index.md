---
title: Week 1
date: 2024-09-11
categories:
  - 網路視窗程式設計
---

1.

```
    public class Main {
        static public void main(String[] args) {
            System.out.println("Hello World !");
        }
    }
```

2.

```
    public class Main {
        static public void main(String[] args) {
            System.out.println("What your name ?");
            java.util.Scanner sc = new java.util.Scanner(System.in);
            String str = sc.nextLine();
            System.out.println(str +", how are you ?");
        }
    }
```
