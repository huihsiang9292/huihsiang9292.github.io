---
title: Week 2
date: 2024-09-18
categories:
  - 網路視窗程式設計
---

3.

```
public class Main {
  public static void main(String[] args) {
    int i = 123;
    int j = -234;
    int k = 0256;
    int l = 0xccf;
    long m = 350000L;

    System.out.println("int i= " + i);
    System.out.println("int j= " + j);
    System.out.println("int k= " + k);
    System.out.println("int l= " + l);
    System.out.println("long m= " + m);
  }
}
```

```
public class Main {
  public static void main(String[] args) {
    char a = '\u0058';
    char b = '\u0046';
    char c = '\u0039';
    char d = '\u0065';

    System.out.println("a= " + a);
    System.out.println("b= " + b);
    System.out.println("c= " + c);
    System.out.println("d= " + d);
  }
}
```

4.

```
public class Main {
  public static void main(String[] args) {
    double f, c;
    java.util.Scanner sc = new java.util.Scanner(System.in);
    System.out.print("請輸入攝氏溫度=> ");
    c = sc.nextDouble();

    f = (9.0 * c) / 5.0 + 32.0;
    System.out.println("攝氏" + c + "=華氏" + f + "度");
    System.out.print("請輸入華氏溫度=> ");
    f = sc.nextDouble();

    c = (5.0 / 9.0) * (f - 32);
    System.out.println("華氏" + f + "=攝氏" + c + "度");
  }
}
```

```
public class Main {
  public static void main(String[] args) {
    int a = 14;
    int b = 6;
    int c = a & b;
    int d = a | b;
    int e = a << 2;
    System.out.println("c:" + c);
    System.out.println("d:" + d);
    System.out.println("e:" + e);
  }
}
```
