---
title: 從零開始的JAVA學習歷程（一）
date: 2019-09-22 21:29:32
tags: 
  - Java
---

## JAVA 安裝

### Ubuntu

```bash
sudo apt install default-jdk default-jre
```

## Hello World

所有程式的根本都是由Hello World開始的，所以我們現在要讓我們的Java幫我們印出 `Hello World!!!`

### hello_world.java

```java
public class hello_world {
public static void main(String[] args) {
    System.out.println("Hello World!!!");
}
}
```

接著，在終端機上輸入 `java hello_world.java`

你就會看到熟悉的`Hello World!!!`出現我們面前

## 測試驅動開發(Test-driven development, TDD)

筆者在大一下學期學到了一種開發方法叫做TDD，在未來的文章中，都會使用到TDD。在這種開發模式下，單元測試是很重要，詳細自己上維基百科查吧！

## Maven

在C/C++我們可以用makefile來減少輸入指令的數量，在Java，我們用的叫做Maven的套件管理工具

### 安裝Maven (Ubuntu)

```bash
sudo apt install maven
```

使用Maven，我們需要建立Maven的資料夾結構，如下：

```bash
└───maven-project
    ├───pom.xml
    └───src
        ├───main
        │   └───java
        └───test
            └───java
```

## 製作結合TDD的Maven專案

依照上一段的資料結構我們需要三個檔案：
`pom.xml`,`src/main/java/main_file.java`,`src/test/java/mainTest.java`

其中，`pom.xml`是maven專案最重要的檔案，它說明了這個專案的所有細節包含此專案所需要的Dependency

### pom.xml

```xml
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.mycompany.app</groupId>
  <artifactId>my-app</artifactId>
  <packaging>jar</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>Java Practice Unittest</name>
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>11</maven.compiler.source>
    <maven.compiler.target>11</maven.compiler.target>
    <maven.compiler.release>11</maven.compiler.release>
  </properties>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
```

### src/main/java/Tdd.java

```java
public class Tdd {
  public int cal(int a, int b) {
    return a+b;
  }
}
```

### src/test/java/mainTest.java

```java
import static org.junit.Assert.assertEquals;
import org.junit.Test;

public class mainTest {
  @Test
  public void evaluatesExpression() {
    Tdd a = new Tdd();
    assertEquals(7, a.cal(2,5));
  }
}
```

## 初步試驗

準備好上面的檔案之後，在終端機上輸入 `mvn test`

當你看到

```bash
-------------------------------------------------------
 T E S T S
-------------------------------------------------------
Running mainTest
Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.058 sec

Results :

Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
```

代表你成功使用Java製作出結合TDD的Maven專案
