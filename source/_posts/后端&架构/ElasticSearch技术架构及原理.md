---
title: Elasticsearch技术架构及原理
date: 2024-05-12 15:54:13
author: okeeper
top: false
toc: true
categories: 后端&架构
tags:
  - 后端&架构
  - Elasticsearch
  - 数据库
---

# Elasticsearch介绍

Elasticsearch 是一个基于 Apache Lucene 的开源搜索和分析引擎，设计用于云计算中，能够快速地处理大量数据。它可以近实时地进行复杂的查询，并且可以用于全文搜索、结构化搜索以及分析。

特性：

- 分布式搜索引擎，可以扩展到上百台服务器，处理PB级的数据。

- RESTful API，使用JSON进行数据交换。

- 实时分析，可以对数据进行实时分析。

- 高可用性，节点失败时可以自动重分配。

- 近实时，数据被索引后立即可搜索。1s内返回

- 支持各种编程语言。

# Elasticsearch的原理

每一种存储引擎都有自己特定的应用场景，例如Mysql，它更擅长的是事务的操作，事务里面有原子性、持久性、一致性、隔离性这些，因此可以保证数据的安全性、持久化存储、数据一致，但它不适合大量数据的查询和搜索(亿级海量数据)，它底层的存储引擎虽然使用B+ tree索引优化了检索速度，但随着数据量的增大，多次的磁盘IO读写依然会比较慢。此时IO读写将是不可逾越的一大瓶颈。

Elasticsearch搜索引擎就是为了解决海量数据的搜索和数据检索。那么之所以它适合海量数据的检索，一定有它独特的设计，优化了检索磁盘IO读写过程。

## 倒排索引

我们知道数据是放磁盘中的，要对磁盘中海量数据的搜索，一定要为这些数据建立索引数据结构，降低磁盘IO次数。那么ES的索引方式就不是MySQL的B+ tree方式，而是倒排索引。

**倒排索引中有两个非常重要的概念：**

- 文档（`Document`）：用来搜索的数据，其中的每一条数据就是一个文档。例如一个网页、一个商品信息
- 词条（`Term`）：对文档数据或用户搜索数据，利用某种算法分词，得到的具备含义的词语就是词条。例如：我是中国人，就可以分为：我、是、中国人、中国、国人这样的几个词条

**倒排索引**简单来说就是通过doc(数据行)的某个字段的词条（Term）对应起doc的id及出现的位置信息等的一种数据结构，例如将下图中的title字段进行分词后映射了每个词条对应的文档id.

![](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/2729274-20230205171751933-576636800.png)

最终将一个所有表数据维护成了词条和文档id集合的一个映射，这个就是倒排索引。而每个词条对应的文档id集合是按顺序进行存储的，称为posting list. 词条这一列称为Term Dictionary（词项字典）。

这里有两个问题：

1. 如果一个词条对应的文档id特别多，岂不是要大量的磁盘空间进行存储

2. 一般一个文档不单只有一个字段，如果每个字段分别建立了倒排索引，当需要取各个字段的搜索交集时，如何快速实现。

> 一般来讲，一个文档id时long类型的，8byte存储，如果一个词条包含的文档id特别多，成千上万个，例如要所示英文文档中包含"of"单词的所有文档，这是英文中最常见的单词，显然会很多。如果是1千万个id * 8byte/1024 = 78125KB = 76M。这还只是一个词条，实际上词条会很多，每个词条包含的文档数量可能远不止1千万个id，如果直接存储id对搜索来讲会大大增加磁盘空间的占用和后续搜索性能的降低。

为了解决这两个问题，ES想到的是将Posting list进行压缩，ES实现了两种节省空间的压缩算法，一个是**Frame Of Reference（FOR）**，另一个是 **Roaring Bitmaps(RBP) **

针对不同的文档ID序列会选择不同的压缩算法

### Frame Of Reference（FOR）算法

它的核心思想是对于一个有序的doc id列表，只记录从第一个元素依次开始的增量数组，进而达到压缩空间的目的。

例如有以下一个数组：

```
[73, 300, 302, 332, 343, 372]
```

进行增量编码之后：

```
[73, 227, 2, 30, 11, 29]
```

那么id的所需的存储空间就由这个增量编码的最大值决定，例如这里的最大值是227，如果采用无符号二进制位表示需要1byte，那么6个数总共压缩之后=6\*1byte=6byte

而原数组的最大值是372，需要2byte, 原数组就需要12byte表示。压缩了一半的空间。那么看起来有一定的压缩率了，能不能进一步压缩呢？

答案是肯定的，ES在实际实现过程中，会将数组中每255个id进行分组，并在每一个分组中取最大的数值所需的空间大小当成整个分组中每个元素的空间大小。这样每个分组都能尽可能地使用最低存储空间进行存储。例上面这个增量数组，假设如果是按3个元素进行分组。

```
[73, 227, 2], [30, 11, 29]
```

对于第一个块，[73, 227, 2]，最大元素是227，需要 8 bits，所以就给每个元素都分配 8 bits的空间。

但是对于第二个块，[30, 11, 29]，最大的元素才30，只需要 5 bits，所有给每个元素只分配 5 bits 的空间足矣。

为了在解码的时候知道这个分组用了多少位，用一个bit来存储所用的bit位数即可。

![es FOR编码技术](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/es%20FOR%E7%BC%96%E7%A0%81%E6%8A%80%E6%9C%AF.jpg)

这个就是Frame Of Reference（FOR）编码算法，在Posting list中的文档id步长相差不大的情况下，能够极大地压缩所需的磁盘空间存储。

当有多个字段的倒排索引需要聚合条件取交集搜索时，从磁盘中加载到内存中进行计算，从最短的数组的最小元素开始遍历，通过有序数组进行跳表搜索(Skip table)其他数组中是否存在即可取得最终的交集。

![Lucene 跳表例子](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/Lucene%E8%B7%B3%E8%A1%A8%E8%AE%A1%E7%AE%97%E4%BE%8B%E5%AD%90.jpg)

从上面例子可以看出，使用FOR进行压缩后能够极大降低磁盘所需空间，便于一次性加载到内存中进行聚合计算，但当文档ID数据依然很多，压缩之后依然暂用很多空间时，加载到内存肯定也吃不消。

这就引出了我们另外一种压缩编码算法，**Roaring Bitmaps**

### Roaring Bitmaps(RBM)——咆哮位图

Bitmap我们知道，它通过一个bit数组，来表示一个有序数组是否在当前bit位中存在元素的一种数据结构，假设有这样一个数组：

```
[3,6,7,10]
```

它的最大值是10，就需要10位的bit数组表示，bitmap （位图）来表示为：

```
[0,0,1,0,0,1,1,0,0,1]
```

我们用 0 表示角标对应的数字不存在，用 1 表示存在。例如从第一个数组元素1开始，第3个元素为1表示这里有个值是3。

Roaring Bitmaps(RBM)的原理是，将原始的posting list的Integer Id，32位表示，划分为低16位和高16位两部分，通过计算得知这两部分最大的数值都是65535。

其效果和ID / 65535 的到**一个商**合**一个余数**效果是一样的。那么将一个有序不重复的ID数组必定能够转换成商合余数都是65535之内的数。

将商当做key，将余数当做value进行映射起来，就可以将有相同商的聚合到一个value钟，在通过bitmap分别表示key和value。这样可以通过恒定的65535的bit数组表示一个32位的Integer集合，大概是43亿。最终效果如下：

![roaring array](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/74bf2380c19d4a039ca3ce3a2dd09661.png)

用65535位的bitmap标识 keys，用ArrayContrainer或者Bitmap Contrainer来表示每个key对应的values，由于values不管有多少个元素，都用65535位的bitmap标识，当元素较少时，固然也是比较浪费的，计算Bitmap和直接存Integer数组与元素个数的存储空间关系，得到以下临界关系，当元素个数<4096时，用ArrayContrainer直接存Integer原始数值比较省空间，当元素个数>=4096时，使用Bitmap Contraine来存储，存储空间很定在65536 个 bit

![](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/85a4ddbf2f2845b89818f64fc00d97b3.png)

通过Roaring Bitmaps(RBM)这种数据编码的优化，即使极端情况Posting List由43(2^32)亿个无符号Integer组成的Id数组，其最大所需的存储空间为计算如下：

65536 个 bit，也就是 65536/8 = 8192 bytes

8192 bytes \* 8192 bytes=65535KB = 64M

而43亿个整形所需的空间为 2^32 \* 4byte = 16384M = 16G，压缩了不是一点半点了。

而且Bitmap有个好处就是在取多个数组交集的时候，只需要&位运算即可，相当方便，这样就可以将一个极大的有序Posting list一下加载到内存中进行快速计算得出搜索结果。

到目前为止，我们值将了ES如何优化和压缩Posting list。还有Term Dictionary词项字典是怎么处理的呢，对应上亿级的文档数据，其词项也是相当巨大的，如何在搜索时通过所示关键字(词项)快速找到对应的post list呢？接下来引出Term Index.

## Term Index（词项索引）

为了加快通过关键字快速找到PostingList, ES的思想是在词项字典中建立索引，并将索引缓存在内存中进行极速计算搜索。最终的结构应该如下图所示：

![Lucene倒排索引内部结构](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/Lucene%E5%80%92%E6%8E%92%E7%B4%A2%E5%BC%95%E5%86%85%E9%83%A8%E7%BB%93%E6%9E%84.jpg)

那么问题来了，对于一个巨量的词项字典，其词项索引必然也是巨大的，直接加载到内存中肯定不是它的作风。

对的ES使用的是一种有限状态转换器（Finite StateTransducers 简称 FST）数据结构，将词项字典的前缀和后缀及对应的索引Block块位置构建成一个FST树。

它是从Trie前缀树演变而来的，其满足前缀树的特性：

![](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/b33b1595505ec3a32774f79eae3dd8b8.png)

1、根节点不包含字符，除根节点外每一个节点都只包含一个字符  
2、从根节点到某一节点，路径上经过的字符连接起来，为该节点对应的字符串  
3、每个节点的所有子节点包含的字符都不相同

 而ES实现数据结构是FST（Finite State Transducer）有的不一样，它不但可以表示相同前缀的词典，相同后缀也能表示。它有以下两个优点：

1. 空间占用小。通过对词典中单词前缀和后缀的重复利用，压缩了存储空间；
2. 查询速度快。O(len(str))的查询时间复杂度。

### 构建过程

例如：

我们对“cat”、 “deep”、 “do”、 “dog” 、“dogs”这5个单词进行插入构建FST（注：必须已排序）。

1. 插入“cat”,每个字母形成一条边，其中t边指向终点。
   
   ![](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/e2a43283b890ee417d1680b1e2d15d28.png)

2. 插入“deep”,与前一个单词“cat”进行最大前缀匹配，发现没有匹配则直接插入，P边指向终点。
   
   ![](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/c11a3bc0faf2d7d5c21f29ce29458c37.png)

3. 插入“do”,与前一个单词“deep”进行最大前缀匹配，发现是d，则在d边后增加新边o，o边指向终点。
   
   ![](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/166c463a70a4e724a788930bfbfe428e.png)

4. 插入“dog”,与前一个单词“do”进行最大前缀匹配，发现是do，则在o边后增加新边g，g边指向终点。
   
   ![](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/ee27cb44f5171a4936f9f101a17c31ff.png)

5. 插入“dogs”, 与前一个单词“dog”进行最大前缀匹配，发现是dog，则在g后增加新边s，s边指向终点。
   
   ![](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/075af4c74246952f99fa0876c583b7d1.png)
   
   有了这个索引结构，就能很方便地在内存中快速查找posting list，进行进一步的快速搜索。
   
   关于FST这里有个在线的算法演示网站，感兴趣的可以点开玩玩方便理解：http://examples.mikemccandless.com/fst.py?terms=&cmd=Build+it%21
   
   ![image-20240605145158758](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20240605145158758.png)

# 总结

## 为什么 Elasticsearch/Lucene 检索可以比 mysql 快

1. Mysql 只有 term dictionary 这一层，是以 b-tree 排序的方式存储在磁盘上的。检索一个 term 需要若干次随机 IO 的磁盘操作。

2. 而 ES 在 term dictionary 的基础上添加了term index来加速检索，term index 以树的形式缓存在内存中。从 term index 查到对应的 term dictionary 的 block 位置之后，再去磁盘上找 term，大大减少了磁盘的 random access （随机IO）次数。

3. ES在Posting List存储时，通过Frame Of Reference（FOR）和Roaring Bitmaps(RBM)的编码压缩，可以实现海量数据的存储而不再用太大的空间，大大节省了磁盘空间，进而检索了搜索时的随机IO次数。再者由于压缩的存在，使得可以一次性加载至内存中进行快速计算。

以上就是ES之所能在海量数据搜索中快的核心技术。
