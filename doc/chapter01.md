**第一章 什么是Spark**


> [iooffqqx <> sharklasers.com 123456](https://livebook.manning.com/book/spark-in-action-second-edition/chapter-1/48)



本章内容：
- 什么是 Spark 及其使用场景
- 分布式技术的基础
- Spark的四大支柱
- 存储及 APIs：爱上数据框架

上世纪80年代，当我还是个孩子的时候，通过 Basic 和我的雅达利（Atari）编程，我无法理解为什么我们不能实现基本执法活动的自动化，比如速度控制、闯红灯和停车计时器。一切似乎都很简单:我在书中说过，要成为一名优秀的程序员，应该避免 `GOTO` 语句。这就是我所做的，从 12 岁开始，我就试图构造我的代码。但是，在开发类似《大富翁》的游戏时，我无法想象数据量（以及蓬勃发展的物联网或物联网）。 当我的游戏适合64 KB的内存时，我绝对不知道数据集会变得更大（由一个巨大因素决定），或者数据会具有速度或速率，因为我耐心地等待着保存游戏到我的 Atari 1010 录音机。


短短35年之后，所有我想象中的用例似乎都是可访问的(而我的游戏则是无用的)。数据的增长速度已经超过了支持数据的硬件技术。一组小型计算机的成本比一台大型计算机还要低。内存比2005年便宜了一半，而2005年的内存比2000年便宜了五倍。网络速度要快很多倍，现代数据中心提供高达100Gbps的速度，比五年前的家庭Wi-Fi快近2000倍。这就是促使人们问这个问题的一些因素:我如何使用分布式内存计算来分析大量数据。

当您阅读文献或在web上搜索关于Apache Spark的信息时，您可能会发现它是一个用于大数据的工具、Hadoop的继承者、分析平台、集群计算机框架等等。


本章试验可以在这里找到：https://github.com/jgperrin/net.jgp.books.spark.ch01

<!-- markdown-toc start - Don't edit this section. Run M-x markdown-toc-refresh-toc -->
**Table of Contents**

- [Spark是什么以及能做些什么](#spark是什么以及能做些什么)
    - [Spark是什么](#spark是什么)
    - [四大支柱](#四大支柱)
- [如何使用Spark](#如何使用spark)
    - [数据处理/工程场景中的Spark](#数据处理工程场景中的spark)
    - [数据科学场景中的Spark](#数据科学场景中的spark)
- [可以用Spark做什么](#可以用spark做什么)
    - [Spark预测NC餐馆的餐厅质量](#spark预测nc餐馆的餐厅质量)
    - [Spark允许Lumeris快速传输数据](#spark允许lumeris快速传输数据)
    - [Spark分析CERN的设备日志](#spark分析cern的设备日志)
    - [其他用例](#其他用例)
- [为什么您会喜欢dataframe](#为什么您会喜欢dataframe)
    - [Java角度的dataframe](#java角度的dataframe)
    - [数据格式的图形表示](#数据格式的图形表示)
- [你的第一个例子](#你的第一个例子)
    - [推荐软件](#推荐软件)
    - [下载代码](#下载代码)
    - [运行第一个应用](#运行第一个应用)
    - [第一个代码](#第一个代码)
- [总结](#总结)

<!-- markdown-toc end -->


# Spark是什么以及能做些什么
## Spark是什么
Spark不仅仅是一个数据科学家的软件栈。当您构建应用程序时，您是在操作系统之上构建它们的。操作系统提供的服务使您的应用程序开发更加轻松； 换句话说，您并不是在为开发的每个应用程序构建文件系统或网络驱动程序。

![](https://drek4537l1klr.cloudfront.net/perrin/HighResolutionFigures/figure_1-1.png)

随着对更多计算能力的需求，对分布式计算的需求也越来越大。 随着分布式计算的出现，分布式应用程序必须合并那些分布功能。下图展示了向你的应用添加组件所增加的复杂性。

![](https://drek4537l1klr.cloudfront.net/perrin/HighResolutionFigures/figure_1-2.png)


综上所述，Apache Spark可能看起来像一个复杂的系统，需要您具有很多先验知识。 我坚信您只需要Java和关系数据库管理系统（RDBMS）技能即可理解，使用，构建带有Spark的应用程序以及扩展Spark。

应用程序也变得更加智能，可以生成报告并执行数据分析（包括数据聚合，线性回归或仅显示甜甜圈图）。 因此，当您想向应用程序中添加此类分析功能时，必须链接库或构建自己的库。 所有这些使您的应用程序变得更大（或在胖客户端中变得更胖），更难维护，更复杂，因此对企业而言更加昂贵。

那么为什么不把这些功能放在操作系统级别呢?你可能会问。将这些特性放在较低的级别(如操作系统)有很多好处，包括以下方面：
- 提供一种处理数据的标准方法（有点像关系数据库的结构化查询语言或SQL）。
- 降低应用程序的开发（和维护）成本。
- 使您可以专注于理解如何使用该工具，而不是该工具的工作方式。 （例如，Spark执行分布式提取，您可以学习如何从中受益，而不必完全掌握Spark完成任务的方式。）

对我来说，Spark已经变成了一个分析操作系统。图1.3显示了这个简化的堆栈。

**图1.3 Apache Spark通过向应用程序提供服务，简化了面向分析的应用程序的开发，就像操作系统一样。**

![](https://drek4537l1klr.cloudfront.net/perrin/HighResolutionFigures/figure_1-3.png)

在本章中，您将发现针对不同行业和不同项目规模的Apache Spark的一些用例。 这些示例将简要概述您可以实现的目标。

我坚信，为了更好地了解我们的位置，我们应该回顾历史。 这也适用于信息技术（IT）：如果需要，请阅读[附录E](appendix/E.md)。

现在已经设置好场景，您将深入研究Spark。 我们将从全局概述开始，看一下存储和API，最后，完成第一个示例。

## 四大支柱
根据波利尼西亚人的说法，法力是体现在物体或人体内的自然基本力量的力量。 此定义适合您在所有Spark文档中都能找到的经典图表，显示了将这些基本要素带入Spark的四个支柱：Spark SQL，Spark Streaming，Spark MLlib（用于机器学习）和位于Spark Core上方的GraphX。 尽管这是Spark堆栈的精确表示，但我发现它是有限制的。 需要扩展堆栈以显示硬件，操作系统和您的应用程序，如图1.4所示。

**图1.4您的应用程序以及其他应用程序正在通过统一的API与Spark的四个支柱（SQL，流技术，机器学习和图形）进行对话。 Spark使您免受操作系统和硬件限制：您不必担心应用程序在何处运行或它是否具有正确的数据。 Spark会解决这个问题。 但是，如果需要，您的应用程序仍然可以访问操作系统或硬件。**

![](https://drek4537l1klr.cloudfront.net/perrin/HighResolutionFigures/figure_1-4.png)

当然，运行Spark的群集可能不会被您的应用程序独占使用，但是您的工作将使用以下内容：
- *Spark SQL* 运行数据操作，如RDBMS中的传统SQL作业。 Spark SQL提供API和SQL来处理您的数据。 您将在第11章中发现Spark SQL，并在随后的大多数章节中阅读有关它的更多信息。 Spark SQL是Spark的基石。
- *Spark Streaming*（特别是Spark结构化的流）来分析流数据。 Spark的统一API将帮助您以类似的方式处理数据，无论是流数据还是批处理数据。 您将在第10章中了解有关流式传输的详细信息。
- *Spark MLlib* 用于机器学习的Spark MLlib和深度学习的最新扩展。 机器学习，深度学习和人工智能值得一读。
- *GraphX* 利用图数据结构。 要了解有关GraphX的更多信息，您可以阅读Michael Malak和Robin East（Manning，2016）编写的Spark GraphX in Action。


# 如何使用Spark
在本部分中，您将通过关注典型的数据处理方案以及数据科学方案来详细了解如何使用Apache Spark。 无论您是数据工程师还是数据科学家，您都可以在工作中使用Apache Spark。

## 数据处理/工程场景中的Spark

Spark可以多种方式处理您的数据。 但是，当在大数据场景中播放数据，清理，转换和重新发布数据时，它会表现出色。

我喜欢将数据工程师视为数据准备者和数据后勤人员。 他们确保数据可用，数据质量规则已成功应用，转换成功执行，并且数据可用于其他系统或部门，包括业务分析师和数据科学家。 数据工程师也可以是承担数据科学家工作并将其产业化的人。

Spark是数据工程师的理想工具。 数据工程执行的典型Spark（大数据）场景的四个步骤如下：
- 摄取
- 改善数据质量（DQ）
- 转型
- 发布


图1.5典型数据处理场景中的Spark。 第一步是摄取数据。 在这个阶段，数据是原始的； 您可能接下来要应用一些数据质量（DQ）。 现在您可以转换数据了。 转换数据后，数据将变得更加丰富。 现在是发布或共享该文件的时候，以便组织中的人员可以对其执行操作并根据该文件做出决策。

![](https://drek4537l1klr.cloudfront.net/perrin/HighResolutionFigures/figure_1-5.png)

该过程包括四个步骤，并且在每个步骤之后，数据都会进入一个区域：

1. *Ingesting data* —— Spark可以从各种来源摄取数据（请参阅有关摄取的第7、8和9章）。 如果找不到受支持的格式，则可以构建自己的数据源。 我现阶段将数据称为原始数据。 您还可以找到此区域，称为 staging, landing, bronze, swamp zone。
1. *Inproving data quality (QD)* —— 在处理数据之前，您可能需要检查数据本身的质量。 DQ 的一个示例是确保所有出生日期都在过去。 作为此过程的一部分，您还可以选择混淆一些数据：如果您在医疗保健环境中处理社会安全号码（SSN），则可以确保开发人员或未经授权的人员无法访问SSN。之后 您的数据已经过优化，我将此阶段称为纯数据区（*pure data zone*）。 您可能还会发现此区域，称为 *refinery*, *silver*, *sandbox*, 或 *exploration zone*。
1. *Transforming data* —— 下一步是处理您的数据。 您可以将其与其他数据集连接，应用自定义功能，执行聚合，实施机器学习等。 此步骤的目标是获取丰富的数据，这是分析工作的成果。 大多数章节都讨论了转换。 该区域也可以称为 *production*, *gold*, *refined*, *lagoon* 或 *operationlization zone*。
1. *Loading and publishing* —— 与 ETL 流程一样，您可以使用商业智能（BI）工具将数据加载到数据仓库中，调用API或将数据保存到文件中来完成。 结果是您企业的可操作数据。

## 数据科学场景中的Spark

数据科学家与软件工程师或数据工程师的方法略有不同，因为数据科学家以交互方式关注转换部分。 为此，数据科学家使用不同的工具，例如笔记本。 笔记本的名称包括 Jupyter，Zeppelin，IBM Watson Studio 和 Databricks Runtime。

数据科学家的工作方式绝对对您很重要，因为数据科学项目将消耗企业数据，因此您可能最终将数据交付给数据科学家，将他们的工作（例如机器学习模型）卸载到企业数据存储中，或者 工业化他们的发现。

因此，如图1.6所示的类似UML的序列图将更好地解释数据科学家如何使用Spark。使用Spark的数据科学家的序列图：用户与笔记本“交谈”，笔记本在需要时调用Spark。 Spark直接处理摄取。 每个正方形代表一个步骤，每个箭头代表一个序列。 该图表应从顶部开始按时间顺序阅读。

![](https://drek4537l1klr.cloudfront.net/perrin/HighResolutionFigures/figure_1-6.png)

> 如果您想进一步了解Spark和数据科学，可以看看这些书：
>
> - *[PySpark in Action](www.manning.com/ books/pyspark-in-action?a_aid=jgp)* by Jonathan Rioux (Manning, 2020)
> - *[Mastering Large Datasets with Python](www.manning.com/books/mastering-large-datasets-with-python?a_aid=jgp)* by John T. Wolohan (Manning, 2020)

在图1.6中描述的用例中，数据被加载到Spark中，然后用户将使用它进行播放，应用转换并显示部分数据。 显示数据并不是过程的结束。 用户将能够以交互方式继续操作，例如在物理笔记本中，您可以在其中写食谱，做笔记等。 最后，笔记本电脑用户可以将数据保存到文件或数据库中，或生成（交互式）报告。

# 可以用Spark做什么

Spark 用于各种项目，因此让我们探索其中的几个。 所有用例都涉及无法在单台计算机上容纳或处理的数据（又名大数据），因此需要计算机集群，因此需要专门用于分析的分布式操作系统。

大数据的定义已经随着时间的推移从具有五个 Vs 特征的数据演变为“无法容纳在一台计算机上的数据”。 我不喜欢这个定义； 您可能知道，许多 RDBMS 将数据拆分到多个服务器上。 与许多概念一样，您可能必须做出自己的定义。 这本书希望会对您有所帮助。


对我而言，大数据是数据集的集合，可在企业中的任何地方使用，并且聚集在一个位置，您可以在其上运行基本分析到更高级的分析，例如机器和深度学习。 这些更大的数据集可以成为人工智能（AI）的基础。 技术，大小或计算机数量与该概念无关。

通过其分析功能和本地分布式体系结构，Spark可以处理大数据，无论您认为大数据还是适合一台或多台计算机。 只需记住，在132列点矩阵打印机上的传统报告输出并不是Spark的典型用例。 让我们发现一些真实的例子。

## Spark预测NC餐馆的餐厅质量
在美国大部分地区，饭店需要当地卫生部门进行检查才能运营，并根据这些检查进行分级。 较高的分数并不代表更好的食物，但是它可能表明您在前往南方的某个小屋中烧烤后是否会丧命。 等级用于衡量厨房的清洁度，食物存储的安全性以及（希望）避免食源性疾病的更多标准。

在北卡罗来纳州，餐厅的等级评分范围为0到100。每个县都可以访问餐厅的等级，但是没有中央位置可以访问全州的信息。

NCEatery.com 是一个面向消费者的网站，列出了随时间推移其检查等级的餐馆。 NCEatery.com 的目标是集中这些信息，并对餐厅进行预测分析，以了解我们是否可以发现餐厅质量的模式。 我两年前爱过的这个地方下坡了吗？

在网站的后端，Apache Spark 接收来自不同国家的餐馆、检查和违规数据集，对数据进行处理，并在网站上发布摘要。在运算阶段，会应用一些数据质量规则，以及机器学习来尝试投影检查和评分。Spark 使用一个小型集群处理 1.6×1021 个数据点，并每 18 小时发布约 2500 个页面。这个正在进行的项目正在向更多的NC县推广。

## Spark允许Lumeris快速传输数据

Lumeris 是一家基于信息的医疗保健服务公司，位于密苏里州圣路易斯。 传统上，它帮助医疗保健提供者从他们的数据中获得更多见解。 公司最先进的 IT 系统需要增强，以容纳更多的客户并从其数据中获得更强大的见解。

在 Lumeris，作为数据工程流程的一部分，Apache Spark 提取存储在 Amazon Simple Storage Service（S3）上的数千个逗号分隔值（CSV）文件，构建符合医疗保健要求的 HL7 FHIR 资源，7并将其保存在 专用文档存储，现有应用程序和新一代客户端应用程序都可以使用它们。

这种技术堆栈使Lumeris在处理数据和应用程序方面都可以继续增长。 借助这种技术，Lumeris 致力于拯救生命。

## Spark分析CERN的设备日志

欧洲核子研究组织（CERN）或欧洲核研究组织成立于1954年。它是大型强子对撞机（LHC）的所在地，该环行长27公里，位于法国和瑞士之间的边界下100米，位于日内瓦。

在那里进行的巨型物理实验每秒生成 1PB 数据。 经过大量过滤后，数据每天减少为 900GB。

在 与Oracle，Impala 和 Spark 进行实验之后，CERN 团队在运行多达 250,000 个内核的 OpenStack 本地云上围绕 Spark 设计了 Next CERN 加速器日志记录服务（NXCALS）。 这种令人印象深刻的体系结构的消费者是科学家（通过自定义应用程序和 Jupyter 笔记本），开发人员和应用程序。 CERN 的志向是装载更多数据并提高数据处理的整体速度。

## 其他用例

Spark 参与了许多其他用例，其中包括：
- 构建交互式数据整理工具，例如 IBM 的 Watson Studio 和 Databricks 的笔记本
- 监视 MTV 或 Nickelodeon 等电视频道的视频馈送质量
- 通过 Riot Games 公司监控在线视频游戏玩家的不良行为，并准实时调整玩家互动，以最大化所有玩家的积极体验。

# 为什么您会喜欢dataframe

在本部分中，我的目标是使您喜欢该 dataframe。 您将学到足够多的知识，想发现更多，这将在您深入学习第3章和整本书时进行。 dataframe 既是数据容器又是 API。

dataframe 的概念对于 Spark 至关重要。 但是，这个概念并不难理解。 您将一直使用 dataframe。 在本节中，您将从 Java（软件工程师）和 RDBMS（数据工程师）的角度看一下数据框是什么。 一旦您熟悉了一些类比，我将总结一个图表。

## Java角度的dataframe

如果您的背景是 Java，并且具有 Java 数据库连接（JDBC）经验，则数据框将看起来像 `ResultSet`。 它包含数据； 它有一个 API ...

ResultSet 和 dataframe 的相似性如下:
- 数据可以通过一个简单的 API 访问。
- 您可以访问 Schema。

以下是一些不同之处:
- 数据可以嵌套，比如在 JSON 或 XML 文档中。第7章描述了这些文档的获取，您将在第13章中使用这些嵌套结构。
- 你不需要更新或删除整行;您创建了新的数据仓库。
- 您可以轻松地添加或删除列。
- 在dataframe上没有约束、索引、主键或外键或触发器。

## 数据格式的图形表示

dataframe 是一个强大的工具，您将在整本书和您与 Spark 的旅程中使用它。它强大的 API 和存储能力使它成为辐射一切的关键元素。图1.7显示了一种想象 API、实现和存储的方法。

![](https://drek4537l1klr.cloudfront.net/perrin/HighResolutionFigures/figure_1-7.png)

# 你的第一个例子

现在是看第一个例子的时候了。您的目标是通过一个简单的应用程序运行 Spark ，该应用程序将读取文件，将其内容存储在一个 dataframe 中，并显示结果。您将学习如何设置您的工作环境，您将在本书中使用这些环境。您还将学习如何与 Spark 交互并执行基本操作。

执行以下操作：
- 安装您可能已经拥有的基本软件：*Git*，*Maven*，*Eclipse*。
- 通过从 *GitHub* 克隆代码来下载代码。
- 执行示例，该示例将加载基本CSV文件并显示一些行。

## 推荐软件
详细的安装请看[附录A](appendix/A.md)和[附录B](appendix/B.md)。

本书使用以下软件：
- Apache Spark 3.0.0
- MacOS Catalina 或 Ubuntu14~18 或 Windows10
- Java8。虽然 Java11 也适合，但大多数企业使用的版本仍然较低，到目前为止，只有 Spark v3 才通过 Java 11 认证。

示例将使用命令行或Eclipse。对于命令行，您可以使用以下命令:
- Maven 3.6.3
- Git 2.28.0

项目使用 Maven 的 pom.xml 结构，该结构可以导入或直接在许多集成开发环境（IDE）中使用。 但是，所有可视示例都将使用 Eclipse。 您可以使用 4.7.1a（Eclipse Oxygen）之前的任何早期版本的 Eclipse，但是 Eclipse 的 Oxygen 版本中增强了 Maven 和 Git 的集成。 我强烈建议您至少使用 Oxygen 发行版，到目前为止，这些发行版还很旧。

## 下载代码

https://github.com/jgperrin/net.jgp.books.spark.ch01

## 运行第一个应用
**在命令行运行**
```
cd net.jgp.books.spark.ch01
mvn clean install exec:exec
```

**在Eclipse运行**

参考[附录D](appendix/D.md)，点击 `src/main/java/net.jgp.books.spark.ch01.lab100_csv_to_dataframe/CsvToDataframeApp.java` 右键，`Run As` -> `2 Java Application`:

![](https://drek4537l1klr.cloudfront.net/perrin/HighResolutionFigures/figure_1-8.png)

用上面两种方式运行之后，都可以看到下面的结果：
```
+---+--------+--------------------+-----------+--------------------+
| id|authorId|               title|releaseDate|                link|
+---+--------+--------------------+-----------+--------------------+
|  1|       1|Fantastic Beasts ...|   11/18/16|http://amzn.to/2k...|
|  2|       1|Harry Potter and ...|    10/6/15|http://amzn.to/2l...|
|  3|       1|The Tales of Beed...|    12/4/08|http://amzn.to/2k...|
|  4|       1|Harry Potter and ...|    10/4/16|http://amzn.to/2k...|
|  5|       2|Informix 12.10 on...|    4/23/17|http://amzn.to/2i...|
+---+--------+--------------------+-----------+--------------------+
only showing top 5 rows
```
接下来，我们来了解发生了什么。

## 第一个代码
终于，您开始编码了!在上一节中，您看到了输出。现在运行您的第一个应用程序。它将获取一个会话，要求 Spark 加载一个 CSV 文件，然后显示数据集的5行(最多)。

说到显示代码，存在两种学派：一种学派是显示摘要，另一种学派是显示所有代码。我属于后一种类型：我喜欢完整的例子，而不是片面的，因为我不想让你去找出缺失的部分或需要的包，即使它们是明显的。
``` java
package net.jgp.books.spark.ch01.lab100_csv_to_dataframe;

import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.SparkSession;

/**
 * CSV ingestion in a dataframe.
 * 
 * @author jgp
 */
public class CsvToDataframeApp {

  /**
   * main() is your entry point to the application.
   * 
   * @param args
   */
  public static void main(String[] args) {
    CsvToDataframeApp app = new CsvToDataframeApp();
    app.start();
  }

  /**
   * The processing code.
   */
  private void start() {
    // Creates a session on a local master
    SparkSession spark = SparkSession.builder()
        .appName("CSV to Dataset")
        .master("local")
        .getOrCreate();

    // Reads a CSV file with header, called books.csv, stores it in a
    // dataframe
    Dataset<Row> df = spark.read().format("csv")
        .option("header", "true")
        .load("data/books.csv");

    // Shows at most 5 rows from the dataframe
    df.show(5);
  }
}
```
尽管该示例很简单，但是您已经完成了以下工作：
- 安装了使用Spark所需的所有组件。 （是的，就是这么简单！）
- 创建一个可以执行代码的会话。
- 加载了CSV数据文件。
- 显示此数据集的五行。

现在，您可以更深入地了解 Apache Spark，并进一步了解其幕后知识。

# 总结
- Spark 是一个分析操作系统。 您可以使用它以分布式方式处理工作量和算法。 它不仅对分析有好处：您可以使用 Spark 进行数据传输，海量数据转换，日志分析等。
- Spark 支持将 SQL，Java，Scala，R 和 Python 作为编程接口，但在本书中，我们重点介绍 Java（有时是 Python）。
- Spark 的内部主要数据存储是数据框。 该数据帧将存储容量与 API 结合在一起。
- 如果您具有 JDBC 开发的经验，那么您会发现与 JDBC `ResultSet` 有相似之处。
- 如果您有关系数据库开发的经验，则可以将数据框与具有较少元数据的表进行比较。
- 在 Java 中，数据帧被实现为 `Dataset<Row>`。
- 您可以使用 Maven 和 Eclipse 快速设置 Spark。 无需安装 Spark。
- Spark 不仅限于 MapReduce 算法：其 API 允许将许多算法应用于数据。
- 由于企业希望访问实时分析，因此流媒体在企业中的使用越来越频繁。 Spark 支持流式传输。
- 分析已从简单的联接和聚合演变而来。 企业希望计算机为我们思考。 因此，Spark 支持机器学习和深度学习。
- 图是分析的一种特殊用例，但是，Spark 支持它们。
