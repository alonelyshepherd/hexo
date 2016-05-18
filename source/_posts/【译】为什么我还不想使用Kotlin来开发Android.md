---
title: 【译】为什么我还不想使用Kotlin来开发Android
date: 2016-03-22 22:07:06
tags:
---
原文链接[在此](http://gold.xitu.io/entry/56ea7118731956005d037ef1)。

尽管Kotlin在许多方面都优于Java，但是还是有重大缺陷的。
>请把它当成个人意见，如果你有这些问题的解决方案请在文后列出

### 1）编译缓慢
小项目（一共约100个class，大部分是Kotlin）需要约1分钟的时间来编译。这是无法接受的。
[https://youtrack.jetbrains.com/issue/KT-6246](https://youtrack.jetbrains.com/issue/KT-6246)

### 2) Kotlin IDEA插件的性能
IDEA(Android Studio)中Kotlin的语法分析和高亮常常在输入的时候造成开发机卡顿，不可接受。

### 3) 注解处理
有时候会随机的出错然后就必须`clean`。几乎每天我都会从不同地方看到抱怨，我不是一个人。

### 4) 用Mockito来模拟Kotlin类很痛苦
在Kotlin中几乎所有东西默认都是`final`的：类，方法，等等。我喜欢这一点因为它强制要求不变性->更少bug。但是它同时使得通过`Mockito`（JVM世界的一种标准）模拟变的很痛苦而且背离了语言设计的方向。

是的，PowerMock是一种可能的解决方案，但是它跟一些Robolectric这类的工具关联而且总得来说有一个很好的原则就是你不应该模拟final类和final方法。

我理解Java的问题：所有的东西从设计上就不是final的，但是同时**我不希望仅仅为了测试去修改代码**

### 5) 目前还没有Kotlin的静态分析工具
是的，`kotlinc`为代码增加比`javac`更多安全性，但是如果你想使编译器达到更高的性能的话，你不会想让它变成静态分析器的。

静态代码分析工具对CI来说很好，但是你也许不想在本地开发时每次点击IDE上的`run`按钮都运行它

Java有：FindBugs，PMD，Checkstyle，Sonarqube，Error Prone，FB infer。

Kotlin有：`kotlinc`。

> 上面的观点是客观的，我希望。下面的观点更加主观。

### 6) `==`是`equals()`而不是引用比较
如果Kotlin是“更好的”Java或是“Java吃了兴奋剂”那么它就应该更好，而不是相反。

想象一下你正在用Kotlin重写Java工程，你的工程中会同时有Java和Kotlin代码。

你将会读写工作起来不同的同一套代码。这也是我不喜欢Groovy的一个原因。

### 7) 如果使用不当运算符重载会造成严重后果
说明1：将来你会需要去处理老旧的Kotlin代码基线
说明2：通过扩展功能你可以为**已有**的Java类添加运算符重载

现在想象一下你看到一些诸如`val person3 = person1 + person2`这样已写好的代码需要处理。

你工作的每个工程对于相同的类都可能有自己的运算符意义😿

运算符重载是有争议的，这些链接也许能帮助你决定（不是所有的都指向同一个结论）：

- [Operator Overloading Considered Harmful](http://cafe.elharo.com/programming/operator-overloading-considered-harmful)
- [Operator Overloading Ad Absurdum](http://james-iry.blogspot.ru/2009/03/operator-overloading-ad-absurdum.html)
- [Why Everyone Hates Operator Overloading](http://blog.jooq.org/2014/02/10/why-everyone-hates-operator-overloading)
