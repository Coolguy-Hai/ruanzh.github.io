---
title: 【数据结构与算法】哈夫曼编码
date: 2021-04-22 10:06:34
tags: 数据结构与算法
---

哈夫曼编码(Huffman Coding)，又称霍夫曼编码，是一种编码方式，可变字长编码(VLC)的一种。Huffman于1952年提出一种编码方法，该方法完全依据字符出现概率来构造异字头的平均长度最短的码字，有时称之为最佳编码，一般就叫做Huffman编码（有时也称为霍夫曼编码）。

哈夫曼编码，主要目的是根据使用频率来最大化节省字符（编码）的存储空间。

简易的理解就是，假如我有A,B,C,D,E五个字符，出现的频率（即权值）分别为5,4,3,2,1,那么我们第一步先取两个最小权值作为左右子树构造一个新树，即取1，2构成新树，其结点为1+2=3，如图：

![这里写图片描述](https://img-blog.csdn.net/20180828202812256?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NjUzNTA1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

虚线为新生成的结点，第二步再把新生成的权值为3的结点放到剩下的集合中，所以集合变成{5,4,3,3}，再根据第二步，取最小的两个权值构成新树，如图：

![这里写图片描述](https://img-blog.csdn.net/20180828202906929?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NjUzNTA1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

再依次建立哈夫曼树，如下图：

![这里写图片描述](https://img-blog.csdn.net/20180828203039475?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NjUzNTA1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

其中各个权值替换对应的字符即为下图：

![这里写图片描述](https://img-blog.csdn.net/20180828203113726?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NjUzNTA1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

所以各字符对应的编码为：A->11,B->10,C->00,D->011,E->010

## 扩展

>Fence Repair (POJ 3253 )
>农夫约翰为了修理栅栏，要将一块很长的木板切割成N块。准备切成的木板的长度为L1、L2、...、 LN，未切割前木板的长度恰好为切割后木板长度的总和。每次切断木板时，需要的开销为这块木板的长度。例如长度为21的木板要切成长度为5、8、8的三块木板。长21的木板切成长为13和8的板时，开销为21。 再将长度为13的板切成长度为5和8的板时，开销是13。于是合计开销是34。请求出按照目标要求将木板切割完最小的开销是
>多少。
>**限制条件**
>
>- 1≤N≤20000
>- 0≤Li≤50000

最终每块木板带来的开销应为**经历的切割次数*自身长度**，我们可以将切割过程看成一颗二叉树，叶子节点即为最终的每块木板，其深度即为经历的切割次数。因此很容易可以想到，越深的叶子节点应对应越短的木板，进而我们可以参考哈夫曼树的构建过程来进行木板的切割。

## 参考

[哈夫曼编码的理解(Huffman Coding)_Never-Giveup的博客-CSDN博客](https://blog.csdn.net/qq_36653505/article/details/81701181)

