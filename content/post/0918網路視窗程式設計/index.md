---
title: Week 2
date: 2024-09-18
categories:
  - 網路視窗程式設計
---
## 課堂練習

### 三之一

```java
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

### 三之二

```java
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

### 四之一

```java
public class Main {
  public static void main(String[] args) {
    double f = 0;
    double c = 0;

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

### 四之二

```java
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

## 作業

### 輸出 0x3FC + 0456

```java
public class Main {
  public static void main(String[] args) {
    int ans = 0x3FC + 456;

    System.out.println(ans);
  }
}
```

### 輸入攝氏溫度，輸出華氏溫度

```java
public class Main {
  public static void main(String[] args) {
    double f = 0;
    double c = 0;

    java.util.Scanner sc = new java.util.Scanner(System.in);
    c = sc.nextDouble();
    f = (9.0 * c) / 5.0 + 32.0;
    System.out.println(f);
  }
}
```

### 輸入球的半徑，輸出球的體積

```java
public class Main {
  public static void main(String[] args) {
    double v = 0;
    double r = 0;
    double pi = 3.14159;

    java.util.Scanner sc = new java.util.Scanner(System.in);
    r = sc.nextDouble();
    v = 1.33333 * pi * (r * r * r);
    System.out.print(v);
  }
}
```

### 設 `int a = 34`，輸入 `n`，輸出 `a << n` 的結果

```java
public class Main {
  public static void main(String[] args) {
    int a = 34;
    int n = 0;

    java.util.Scanner sc = new java.util.Scanner(System.in);
    n = sc.nextInt();
    System.out.print(a << n);
  }
}
```

### 輸入 `n`，計算並輸出 `n^5`

```java
public class Main {
  public static void main(String[] args) {
    int n = 0;

    java.util.Scanner sc = new java.util.Scanner(System.in);
    n = sc.nextInt();
    System.out.print(n * n * n * n * n);
  }
}
```
