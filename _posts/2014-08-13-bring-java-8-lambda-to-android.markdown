---
layout: post
title: 在 Android 中使用 Java 8 lambda 表达式
tags: Android Java
summary: Java 的语法实在是蛋疼，Java 8 发布后终于给 Java 带来了 lambda，但是在 Android 中却无法使用 Java 8
categories: develop
---

每次写 Ruby 的时候，用 Ruby 的 Block 语法都写的我身心舒畅。lambda 表达式和闭包，已经是现代语言的标配了，
甚至很多基于 JVM 的语言都有了这些特性，比如 Groovy，Scala，Clojure，但是 Java 这位老大哥却迟迟不上。
虽然在 Java 中用匿名类可以实现这些特性，但是这代码量，实在是惨不忍睹，太不美了。
值得庆幸的是，随着 Java 8 的发布，终于给 Java 这个笨重的家伙带来了一谢谢灵动。

## Java 8 lambda

Java 8 lambda 的语法如下：

{% highlight java %}
public void runThread() {
  new Thread(new Runnable() {
    public void run() {
      System.out.println("Run!");
    }
  }).start();
}

public void runThreadUseLambda() {
  new Thread(() -> {
    System.out.println("Run!");
  }).start();
}

// 还可以简化成这样
public void runThreadUseLambdaMore() {
  new Thread(() -> System.out.println("Run!")).start();
}
{% endhighlight %}

有了 lambda 表达式，就可以把 List 的操作写得跟 Ruby 的 Array 操作一样优美

{% highlight ruby %}
# ruby version
[1, 2, 3].map { |i| i * 2 } # => [2, 4, 6]
{% endhighlight %}

{% highlight java %}
// Java version
Lists.newArrayList(1, 2, 3).map((i) -> i * 2) // => [2, 4, 6]

// or 更简化

class Person {
  private String name;

  public Person(String name) {
    this.name = name;
  }

  public String getName() {
    return name;
  }
}

List<Person> persons = Lists.newArrayList();

for (int i = 0; i < 3; i++) {
  persons.add(new Person("Person " + i));
}

persons.map(Person::getName) // => ["Person0", "Person1", "Person2"]
{% endhighlight %}

Lambda 大法好，退匿名类保平安！！！

**但是！！**

Android 不支持 Java 8...

Android 开发的同胞们也不用灰心，这位大神 [Esko Luontola](https://github.com/orfjackal) 写出这个 [Retrolambda](https://github.com/orfjackal/retrolambda)
神器，神器在手，天下我有，Android 可以使用 Java 8 的 lambda 表达式啦。

## Retrolambda

想在项目中使用 Retrolambda，得先配置一下。下面以 Android Studio 为例。

Android Studio 使用 Gradle 作为
构建工具，所以我们还需要 [retrolambda-gradle](https://github.com/evant/gradle-retrolambda) 来帮助我们在 Gradle 中加入 Retrolambda。

当然，要用 Java 8 的特性，肯定要先安装 Java 8， 下载安装 [jdk8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)。
然后配置好环境变量 `JAVA8_HOME`。原来的 Java 版本也不能丢，使用 Java 6 的配置好 `JAVA6_HOME`，使用 Java 7 的配置好 `JAVA7_HOME`。

然后在 `build.gradle` 中加入下面的内容：

{% highlight groovy %}
buildscript {
  repositories {
     mavenCentral()
  }

  dependencies {
     classpath 'me.tatarka:gradle-retrolambda:2.2.2'
  }
}

// Required because retrolambda is on maven central
repositories {
  mavenCentral()
}

apply plugin: 'com.android.application' //or apply plugin: 'java'
apply plugin: 'retrolambda'

android {
  compileOptions {
    sourceCompatibility JavaVersion.VERSION_1_8
    targetCompatibility JavaVersion.VERSION_1_8
  }
}

{% endhighlight %}

OK，Sync Project with Gradle File，搞定，打完收工。
赶紧在自己的代码里体验 lambda 之美吧。。。
