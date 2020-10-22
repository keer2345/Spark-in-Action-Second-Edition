**第二章 结构体系和流程**

> [ryvkxlyt <> sharklasers.com 123456](https://livebook.manning.com/book/spark-in-action-second-edition/chapter-2/1)

本章讲述：
- 构件 Spark 典型应用的心智模型（mental model）
- 了解相关 Java 代码
- 浏览 Spark 应用的大致结构
- 理解数据流程


*心智模型*（mental model）是用你的思维过程和下列图表来解释事物在现实世界中是如何工作的。这一章的目标是帮助你定义你自己的想法，关于我将带你走过的思考过程。我会使用很多图表和一些代码。建立一个独一无二的Spark心智模型是极其自命不凡的;这个模型将描述一个典型的场景，包括加载、处理和保存数据。您还将遍历这些操作的 Java 代码。

接下来的场景涉及到分布式加载 CSV 文件，执行一个小操作，并将结果保存在 PostgreSQL 数据库（以及 Apache Derby）中。理解这个示例并不需要了解或安装 PostgreSQL 。如果您熟悉使用其他 rdbms 和 Java，那么您将很容易适应这个示例。[附录F](appendix/F.md)提供了有关关系数据库的额外帮助（提示、安装、链接等）。

本章代码在[这里](https://github.com/jgperrin/net.jgp.books.spark.ch02)。

# 建立你的心智模型

在本节中，您将构建Spark的心理模型。就软件而言，心智模型是一种概念图，您可以使用它来计划、预测、诊断和调试应用程序。要开始构建心智模型，您将处理一个大数据场景。在学习场景的同时，您将探索Spark的总体架构、流程和术语，并将很好地理解Spark的总体框架。

设想一下以下大数据场景:您是一个书商，在一个文件中有一个作者列表，您想对该文件执行一个基本操作，然后将其保存到一个数据库中。从技术上来说，这一过程如下:
1. 接收CSV文件，正如您在第1章中看到的那样。
1. 通过连接姓和名来转换数据。
1. 将结果保存在关系数据库中。

图2.1说明了您和Spark将要做的事情。

我在整本书中使用特定的图标； 您的应用程序（也称为驱动程序）连接到Apache Spark（主服务器），要求它加载CSV文件，进行转换并将其保存到数据库。 在图的左侧，您可以看到时间线（此处为t0，t1和t7）。 您可能已经猜到了，这些代表了步骤。 您的应用程序是此流程的第一个也是最后一个元素。

![](https://drek4537l1klr.cloudfront.net/perrin/HighResolutionFigures/figure_2-1.png)

您的应用程序或驱动程序连接到Spark集群。 应用程序从那里告诉集群该怎么做：应用程序驱动集群。 在这种情况下，主服务器通过加载CSV文件开始，并通过保存在数据库中结束。

# 使用Java代码构建心智模型

在深入研究构建心理模型的每个步骤之前，让我们分析整个应用程序。 在本节中，您将在解构并深入研究每一行代码及其结果之前，设置Java的“礼节”。

图2.2是该过程的基本表示：Spark读取CSV文件； 连接姓氏，逗号和名字； 然后将整个数据集保存在数据库中。这是一个包含三个步骤的简单过程:读取CSV文件，执行一个简单的连接操作，并将结果数据保存在数据库中。

![](https://drek4537l1klr.cloudfront.net/perrin/HighResolutionFigures/figure_2-2.png)


运行应用时，将获取到简单的流程完成消息（Process complete message）。图 2.3 描述了过程的结果，表格 `ch02` 中有三列 —— `fname`, `lname` 和你想要的新的列 `name`。

![](https://drek4537l1klr.cloudfront.net/perrin/HighResolutionFigures/figure_2-3.png)

PostgreSQL中CSV文件中的数据以及附加列。 该图使用SQLPro，但是您可以使用数据库附带的标准工具（pgAdmin版本3或4）。

我试图尽可能完整地显示代码，包括import语句，以防止您使用错误的程序包或名称不赞成的类：
