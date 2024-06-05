---
title: Flink技术架构及原理
date: 2024-05-27 14:00:13
author: okeeper
top: true
toc: true
categories: 后端&架构
tags:
  - 后端&架构
  - Flink
  - 大数据
---

# Flink简介

Flink是一个用于分布式流处理和批处理的大数据处理框架。它由Apache基金会维护，旨在提供高吞吐量、低延迟的数据处理能力，并且支持复杂的事件处理和状态管理。它的重点能力就是**同时支持批处理和流处理的有状态计算**

![](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/watermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9saXhpbmt1YW4uYmxvZy5jc2RuLm5ldA%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70-20240605144243247.png)

Flink起源于Stratosphere项目，Stratosphere是在2010~2014年由3所地处柏林的大学和欧洲的一些其他的大学共同进行的研究项目，2014年4月Stratosphere的代码被复制并捐赠给了Apache软件基金会，参加这个孵化项目的初始成员是Stratosphere系统的核心开发人员，2014年12月，Flink一跃成为Apache软件基金会的顶级项目。

### 应用场景

1. 实时智能推荐
2. 复杂时间流处理
3. 实时反欺诈检测
4. 实时数仓与ETL
5. 数据流分析和报表统计

### Flink的特点

- **高吞吐、低延迟、高性能**：Flink能够同时支持高吞吐、低延迟、高性能的流处理，使其成为处理大规模、高吞吐量的实时数据流和批量数据的首选

- **事件时间支持**：Flink支持事件时间(event time)概念，结合watermark处理乱序数据，这使得Flink在处理乱序事件流时能够提供一致且准确的结果

- **有状态计算**：Flink支持有状态计算，并且支持多种状态存储方式，如内存、文件、RocksDB，这使得Flink能够维护复杂的计算状态，提高计算效率

- **精确一次的状态一致性保证**：基于轻量级分布式快照(checkpoint)实现的容错保证exactly- once语义，确保了数据处理的准确性和一致性

- **灵活的窗口操作**：支持高度灵活的窗口操作，如time、count、session等，使得Flink能够适应各种复杂的流数据处理需求

- **支持多种编程语言和API**：Flink提供了丰富的API，包括Java和Scala的API，以及SQL和Table API，使得开发者可以根据自己的需求选择合适的编程语言进行开发

### Flink与Storm、Spark Streaming的比较

![](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/webp)

- **实时性**：Flink提供了比Storm和Spark Streaming更低的延迟，能够实现毫秒级的实时处理，而Storm和Spark Streaming虽然也能处理实时数据，但在某些场景下可能无法满足低延迟的要求

- **吞吐量**：Flink在吞吐量方面表现出色，能够处理大规模的数据流，而Storm和Spark Streaming虽然也能处理大规模数据，但在某些场景下可能无法达到Flink的吞吐量

- **容错性和状态管理**：Flink提供了精确一次的状态一致性保证，以及基于轻量级分布式快照的容错机制，这使得Flink在容错性和状态管理方面表现更加优秀

- **编程模型和API**：Flink提供了丰富的API和灵活的编程模型，支持多种编程语言，使得开发者可以更加方便地构建复杂的流处理应用

# Flink架构

![](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/1717556697669.png)

*Client* 不是运行时和程序执行的一部分，而是用于准备数据流并将其发送给 JobManager。之后，客户端可以断开连接（*分离模式*），或保持连接来接收进程报告（*附加模式*）。客户端可以作为触发执行 Java/Scala 程序的一部分运行，也可以在命令行进程`./bin/flink run ...`中运行。

可以通过多种方式启动 JobManager 和 TaskManager：直接在机器上作为[standalone 集群](https://nightlies.apache.org/flink/flink-docs-release-1.15/zh/docs/deployment/resource-providers/standalone/overview/)启动、在容器中启动、或者通过[YARN](https://nightlies.apache.org/flink/flink-docs-release-1.15/zh/docs/deployment/resource-providers/yarn/)等资源框架管理并启动。TaskManager 连接到 JobManagers，宣布自己可用，并被分配工作。

**Flink的架构包括以下几个关键组件：**

#### **JobManager**

   *JobManager* 具有许多与协调 Flink 应用程序的分布式执行有关的职责：它决定何时调度下一个 task（或一组 task）、对完成的 task 或执行失败做出反应、协调 checkpoint、并且协调从失败中恢复等等。这个进程由三个不同的组件组成：

- **ResourceManager**负责 Flink 集群中的资源提供、回收、分配 - 它管理 **task slots**，这是 Flink 集群中资源调度的单位（请参考[TaskManagers](https://nightlies.apache.org/flink/flink-docs-release-1.15/zh/docs/concepts/flink-architecture/#taskmanagers)）。Flink 为不同的环境和资源提供者（例如 YARN、Kubernetes 和 standalone 部署）实现了对应的 ResourceManager。在 standalone 设置中，ResourceManager 只能分配可用 TaskManager 的 slots，而不能自行启动新的 TaskManager。
- **Dispatcher**提供了一个 REST 接口，用来提交 Flink 应用程序执行，并为每个提交的作业启动一个新的 JobMaster。它还运行 Flink WebUI 用来提供作业执行信息。
- **JobMaster**负责管理单个[JobGraph](https://nightlies.apache.org/flink/flink-docs-release-1.15/zh/docs/concepts/glossary/#logical-graph)的执行。Flink 集群中可以同时运行多个作业，每个作业都有自己的 JobMaster。
  始终至少有一个 JobManager。高可用（HA）设置中可能有多个 JobManager，其中一个始终是 *leader*，其他的则是 *standby*（请参考 [高可用（HA）](https://nightlies.apache.org/flink/flink-docs-release-1.15/zh/docs/deployment/ha/overview/)）。

#### **TaskManager**

**TaskManager**负责实际执行任务，管理任务的状态和数据流动。*TaskManager*（也称为 *worker*）执行作业流的 task，并且缓存和交换数据流。必须始终至少有一个 TaskManager。在 TaskManager 中资源调度的最小单位是 task *slot*。TaskManager 中 task slot 的数量表示并发处理 task 的数量。请注意一个 task slot 中可以执行多个算子（请参考[Tasks 和算子链](https://nightlies.apache.org/flink/flink-docs-release-1.15/zh/docs/concepts/flink-architecture/#tasks-and-operator-chains)）。

## Tasks 和算子链

对于分布式执行，Flink 将算子的 subtasks *链接*成 *tasks*。每个 task 由一个线程执行。将算子链接成 task 是个有用的优化：它减少线程间切换、缓冲的开销，并且减少延迟的同时增加整体吞吐量。

下图中，假设Source、map的并行度是2，keyBy()/window()/apply()的并行度也是2，而最终结果的输出处理sink只有1个并行。那么由于Source()和map()处理的相关性将优化成到一个slot由一个线程进行处理，而key()/window()/apply()等这种需要有状态计算的subtask可以优化到另外一个slot由一个线程进行处理，最终将结果汇集到一个subtask中进行汇总计算，最终整个job将有5个Solt进行计算

![](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/1717555608114.png)

![]()

# 窗口

窗口本质就是将无限数据集沿着时间（或者数量）的边界切分成有限数据集。在流处理系统中，窗口（Window）用于将无限的数据流划分为有限的部分进行处理。Flink支持多种类型的窗口：

1. **Time Window**：基于时间的，分为Tumbling Window（无数据重叠）和Sliding Window（有数据重叠） 。

2. **Count Window**：基于数量的，分为Tumbling Window（无数据重叠）和Sliding Window（有数据重叠）。 

3. **Session Window**：基于会话的，一个session window关闭通常是由于一段时间没有收到元素。

4. **Global Window**：全局窗口。

# 如何保证消息的可靠性

实时任务不同于批处理任务，除非用户主动停止，一般会一直运行，运行的过程中可能存在机器故障、网络问题、外界存储问题等等，要想实时任务一直能够稳定运行，实时任务要有自动容错恢复的功能。而批处理任务在遇到异常情况时，在重新计算一遍即可。实时任务因为会一直运行的特性，如果在从头开始计算，成本会很大，尤其是对于那种运行时间很久的实时任务来说。

实时任务开启 Checkpoint 功能，也能够减少容错恢复的时间。因为每次都是从最新的 Chekpoint 点位开始状态恢复，而不是从程序启动的状态开始恢复。举个列子，如果你有一个运行一年的实时任务，如果容错恢复是从一年前启动时的状态恢复，实时任务可能需要运行很久才能恢复到现在状态，这一般是业务方所不允许的。

## Checkpoint机制

Flink Checkpoint 是一种容错恢复机制。这种机制保证了实时程序运行时，即使突然遇到异常或者机器问题时也能够进行自我恢复。Flink Checkpoint 对于用户层面来说，是透明的，用户会感觉实时任务一直在运行。

Flink Checkpoint 是 Flink 自身的系统行为，用户无法对其进行交互，用户可以在程序启动之前，设置好实时任务 Checkpoint 相关的参数，当任务启动之后，剩下的就全交给 Flink 自行管理。

## 什么是Flink任务的状态State

Flink 任务状态可以理解为实时任务计算过程中，中间产生的数据结果，同时这些计算结果会在后续实时任务处理时，能够继续进行使用。实时任务的状态可以是一个聚合结果值，比如 WordCount 统计的每个单词的数量，也可以是消息流中的明细数据。

Flink 任务状态整体可以划分两种：Operator 状态和 KeyedState。常见的 Operator 状态，比如 Kafka Topic 每个分区的偏移量。KeyedState 是基于 KeyedStream 来使用的，所以在使用前，你需要对你的流通过 keyby 来进行分区，常见的状态比如有 MapState、ListState、ValueState 等等。

![](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/--1-4.png)

## Checkpoint流程和原理

要开启任务的额Checkpoint，要进行配置。一种是在Job代码中设置，如下,设置了开启checkpoint功能，并设置CheckpointMode为**EXACTLY_ONCE**, 使用RocksDB进行存储：

```java
StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
/** 开启 checkpoint 功能 */
env. enableCheckpointing ( interval: 3000, CheckpointingMode.EXACTLY_ONCE);
/** 使用RocksDB 进行状态存储 */
env.setStateBackend(new RocksDBStateBackend ( checkpointDataUri: "hdfsPath" )) ;
```

由于 Flink 管理的 keyed state 是一种分片的键/值存储，每个 keyed state 的工作副本都保存在负责该键的 taskmanager 本地中。另外，Operator state 也保存在机器节点本地。Flink 定期获取所有状态的快照，并将这些快照复制到持久化的位置，例如分布式文件系统。

如果发生故障，Flink 可以恢复应用程序的完整状态并继续处理，就如同没有出现过异常。这个过程就是Checkpoint的容错和恢复的机制。

接下来先解释下两个概念

### StateBackend

Flink 管理的状态存储在 *state backend* 中，实现有三种：MemoryStateBackend、FsStateBackend、RocksDBStateBackend。三种状态存储方式与使用场景各不相同，对比如下:

<table class="table table-bordered">
  <thead>
    <tr class="book-hint info">
      <th class="text-left">名称</th>
      <th class="text-left">Working State</th>
      <th class="text-left">状态备份</th>
      <th class="text-left">快照</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th class="text-left">RocksDBStateBackend</th>
      <td class="text-left">本地磁盘（tmp dir）</td>
      <td class="text-left">分布式文件系统</td>
      <td class="text-left">全量/增量</td>
    </tr>
    <tr>
      <td colspan="4" class="text-left">
        <ul>
          <li>支持大于内存大小的状态</li>
          <li>经验法则：比基于堆的后端慢10倍</li>
        </ul>
      </td>
    </tr>
    <tr>
      <th class="text-left">FsStateBackend</th>
      <td class="text-left">JVM Heap</td>
      <td class="text-left">分布式文件系统</td>
      <td class="text-left">全量</td>
    </tr>
    <tr>
      <td colspan="4" class="text-left">
        <ul>
          <li>快速，需要大的堆内存</li>
          <li>受限制于 GC</li>
        </ul>
      </td>
    </tr>
    <tr>
      <th class="text-left">MemoryStateBackend</th>
      <td class="text-left">JVM Heap</td>
      <td class="text-left">JobManager JVM Heap</td>
      <td class="text-left">全量</td>
    </tr>
    <tr>
      <td colspan="4" class="text-left">
        <ul>
          <li>适用于小状态（本地）的测试和实验</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

可以看到只有**RocksDBStateBackend**方式的working state是保存到磁盘中的，这就意味着这种存储方式最为可靠且支持大状态存储，但同时也比基于堆内存的存储更慢。

**所有这些 state backends 都能够异步执行快照，这意味着它们可以在不妨碍正在进行的流处理的情况下执行快照。**

### CheckpointMode

当流处理应用程序发生错误的时候，结果可能会产生丢失或者重复。Flink 根据你为应用程序和集群的CheckpointMode配置，可以产生以下结果：

- Flink 不会从快照中进行恢复（*``CheckpointingMode.AT_MOST_ONCE``*）
- 没有任何丢失，但是你可能会得到重复冗余的结果（*CheckpointingMode.AT_LEAST_ONCE*）
- 没有丢失或冗余重复（*CheckpointingMode.EXACTLY_ONCE*）

其中精准不重复消费(*CheckpointingMode.EXACTLY_ONCE*),只是通过屏障对齐(Barrier alignment)保证了流处理内部的状态一致性，如果要确保严格的端到端精准只消费一次，还必须额外满足一下两个条件：

1. 你的 sources 必须是可重放的

2. 你的 sinks 必须是事务性的（或幂等的）

Barrier 只有在需要提供精确一次的语义保证时需要进行对齐（Barrier alignment）。如果不需要这种语义，可以通过配置 `CheckpointingMode.AT_LEAST_ONCE` 关闭 Barrier 对齐来提高性能。

### 基于EXACTLY_ONCE的Checkpoint过程

一次 Flink Checkpoint 的流程是从 `CheckpointCoordinator` 的 `triggerCheckpoint `方法开始，下面来看看一次 Flink Checkpoint 涉及到的主要内容：

1. Checkpoint 开始之前先进行预检查，比如检查最大并发的 Checkpoint 数，最小的 Checkpoint 之间的时间间隔。默认情况下，最大并发的 Checkpoint 数为 1，最小的 Checkpoint 之间的时间间隔为 0.
2. 判断所有 Source 算子的 Subtask (Execution) 是否都处于运行状态，有则直接报错。同时检查所有待确认的算子的 SubTask(Execution)是否是运行状态，有则直接报错。
3. 创建 `PendingCheckpoint`，同时为该次 Checkpoint 创建一个 `Runnable`，即超时取消线程，默认 Checkpoint 十分钟超时。
4. 循环遍历所有 Source 算子的 `Subtask(Execution)`,最底层调用 Task 的`triggerCheckpointBarrier`, 广播 CheckBarrier 到下游 ，同时 Checkpoint 其状态。
5. 下游的输入中有 `CheckpointBarrierHandler` 类来处理 `CheckpoinBarrier`，然后会调用 `notifyCheckpoint` 方法，通知 Operator SubTask 进行 Checkpoint。
6. 每当 Operator SubTask 完成 Checkpoint 时，都会向 `CheckpointCoordoritor` 发送确认消息。`CheckpointCoordinator` 的 `receiveAcknowledgeMessage` 方法会进行处理。
7. 在一次 Checkpoint 过程中，当所有从 Source 端到 Sink 端的算子 SubTask 都完成之后，`CheckpointCoordoritor` 会通知算子进行 `notifyCheckpointCompleted` 方法，前提是算子的函数实现 `CheckpointListener` 接口。

Flink 会定时在任务的 Source 算子的 SubTask 触发 `CheckpointBarrier`，`CheckpointBarrier` 是一种特殊的消息事件，会随着消息通道流入到下游的算子中。只有当最后 Sink 端的算子接收到 `CheckpointBarrier` 并确认该次 Checkpoint 完成时，该次 `Checkpoint` 才算完成。所以在某些算子的 Task 有多个输入时，会存在 Barrier 对齐时间，我们可以在 Flink Web UI上面看到各个 Task 的 `CheckpointBarrier` 对齐时间。

下图是一次 Flink Checkpoint 实例流程示意图：

![](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/flink----.png)

# 如何保证消息乱序的正确计算

在分布式系统中的并发处理过程中，数据流在subtask间传输可能会乱序到达。这会导致我们在依赖时事件时间进行计算时出现错误结果。在Flink中是采用watermark机制进行解决。

在了解watermark之前先来理解下时间语义的几个概念。

1. **事件时间（Event Time）**：事件在源系统中生成的时间。Flink支持使用事件时间进行处理，使得处理更加准确。
2. **进入Flink时间（Ingestion Time）**：事件进入Flink系统的时间。
3. **处理时间（Processing Time）**：Flink处理事件的当前系统时间。

那么一般情况下，我们所说的消息乱序是指基于时间时间的乱序，需要基于事件时间进行计算时，由于网络传输等原因，事件时间在到达处理节点是并不一定是顺序的，如下图所示的一个事件流：

![在这里插入图片描述](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/4f662ef225d04c03b0e65f6e62b11aa4.png)

它的原理就相当于当数据流到达进行窗口计算时，不严格按照时间窗口定义的结束时间触发窗口计算，而是根据watermark设定的延迟时间适当地进行延迟计算，等一等迟到的数据。

watermark就是一个简单的周期性标记，上图中设置watermark的late time=4, 触发计算的事件窗口长度为20，当source的数据事件时间7达到之后，立刻生成watermark=3(7-4)特殊数据流插入到数据流后下发给下游，当11数据到达之后生成w(7)的watermark,以此类推...直到24数据到达时，watermark变成20，触发窗口结束计算，此后如果19这个数据再到达则直接丢弃，因为它小于watermark。

下游收到一个接收到watermark具体值时，代表这这个wartermark对于的事件时间前的事件数据已经达到，当时间窗口结束时间与watermark时间一致时，将触发窗口的结束计算，即使可能真的还有小于watermark时间的数据还没来，也不管了，先结束当前的事件窗口触发计算。

watermark有几个特点如下：

1. 当数据流到达后根据设置的watermark延迟时间计算出watermark,如果计算出的watermark大于之前收到的watermark值，则覆盖为最新的watermark，否则维持原有水位不变

2. watermark只能单调递增或者持平，不能递减

3. 只有Source算子才会产生watermark
   
   **如果真的有比watermark更晚的数据如何处理**

flink有三种处理方式：

1. 直接丢弃（默认方式）

2. 通过allowedLateness 指定允许数据延迟的时间
   
   即使真的有数据达到watermark时间后还是迟到，可以在延迟回收计算好的窗口数据状态，等它来了之后再更新一下,如下代码：
   
   ```java
   waterMarkStream
   .keyBy(0)
   .window(TumblingEventTimeWindows.of(Time.seconds(3))).allowedLateness(Time.seconds(2))//允许数据迟到2S
   //function: (K, W, Iterable[T], Collector[R]) => Unit
   .apply(new MyWindowFunction).print()
   ```

3. 通过sideOutputLateData 收集迟到的数据
   
   可以通过侧输出流将异常数据保存下来进行后续的手工处理或者告警。

通过这种机制，可以通过短暂的等待延迟来处理大部分的乱序数据，应为实际情况中的乱序数据也是在短时间（几时毫秒到几秒之间）产生的。

# 总结

Apache Flink因其独特的特性和强大的性能：其高吞吐、低延迟的流处理能力让其成为处理实时数据流和批量数据的首选，而且其支持复杂的事件处理和状态管理，使得处理实时数据流变得更加简单、高效。

Flink的窗口操作提供了多种灵活的窗口类型，可以根据业务需求选择合适的窗口类型，从而实现对数据流的精确控制和高效计算。而Checkpoint机制则保证了数据处理的可靠性和一致性，即使在发生故障或异常情况下，Flink也能够自动进行容错恢复，保障数据处理的稳定性。此外，通过Watermark机制处理消息乱序问题，Flink能够在实时数据流中准确地确定事件的发生顺序，从而确保了计算结果的准确性和一致性。

### 参考文献：

[Flink Checkpoint 原理流程以及常见失败原因分析](https://tech.youzan.com/flink_checkpoint_mechanism/)

[Flink 源码阅读笔记（11）- Checkpoint 机制和状态恢复 ](https://blog.jrwang.me/2019/flink-source-code-checkpoint/)
