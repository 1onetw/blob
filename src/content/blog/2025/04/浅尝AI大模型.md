---
title: "浅尝AI大模型"
categories: code
tags: ['AI', '大模型', '机器学习', '深度学习']
id: "92058e28b4cd66e7"
date: 2025-04-29 15:17:36
cover: "https://wp-cdn.4ce.cn/v2/wE7HFp5.jpeg"
---

:::note
浅尝AI大模型：人工智能 + 机器学习 + 深度学习 + 大模型架构
:::

# 大模型

## 1人工智能基础概念全景介绍

### 1.1人工智能

- 提出：**1956年的达特茅斯会议**正式确立"人工智能"这一领域
- 也就是AI

### 1.2机器学习

- 概念：让计算机通过大量数据，自行识别模式总结规律

    ![image-20240327163339953](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271633264.png)

- 类别：

    - （有）监督学习：计算机通过已知的输入和对应输出的样本，训练出一个模型，使得模型能够预测未知输入的输出（学习原始数据与对应标签的关系）

        - 分类：输入是一组特征，输出是离散的类别
        - 回归：输入是一组特征，输出是连续的数值

        ![image-20240327163750944](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271637404.png)

    - 无监督学习：计算机通过数据本身，训练出一个模型，使得模型能够 发现数据中的 模式、规律、结构等

        - 聚类
        - 降维
        - 异值检测
        - 自编码器
        - 自监督学习

        ![image-20240327164107074](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271641210.png)

    - 强化学习：计算机通过与环境的互动，训练出一个模型，使得模型能够 在不断的试错中，找到最优的策略。（让模型能够在环境中获得最大奖励的手段）如：Alpha-go

    ![image-20240327164307138](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271643150.png)

### 1.3深度学习

- 概念：深度学习（英语：deep learning）是机器学习的分支，是**一种以 人工神经网络 为架构，对资料进行表征学习的算法。**

- 特点："深度"是指在网络中使用多层

    ![image-20240327164702831](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271647017.png)

- 深度学习全过程：

    - i. 数据准备

    - ii. 模型构建、损失函数(loss function)定义、优化器(optimizer)选择

    - iii. 模型训练：

        - a. 前向传播
        - b. 计算损失
        - c. 反向传播
        - d. 优化器更新参数
        - e. 迭代训练

    - iv. 模型评估与调优

    - v. 模型应用与部署

        ![image-20240327165028358](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271650696.png)

#### 思考1：深度学习 属于监督学习、无监督学习、和强化学习这三类中的哪一种呢？

答案：每一类都有深度学习的体现，故而都可以包含

#### 思考2：生成式AI 与 大语言模型的关系？

答案：生成式AI 与 大语言模型 都属于深度学习的范畴，但两者的方向不同，属于交集的关系

![image-20240327165214089](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271652210.png)

### 1.4概念全景图

![image-20240327165234902](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271652806.png)

## 2语言模型的发展及核心算法

Larger Language Models（LLM）是指具有**海量训练数据**和**参数量**的语言模型，是深度学习的重要研究之一。

### 2.1发展格局

![image-20240327165719647](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271657966.png)

![image-20240327165740764](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271657729.png)

### 2.2LLM之"大"——数据&参数

1. **数据量巨大**：大模型的数据量通常是以 TB 计，而普通模型的训练数据量通常是以 GB 计。
2. **参数量巨大**：大模型的参数量通常是以 亿 计，而普通模型的参数量通常是以 千万 计。

![image-20240327170013781](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271700816.png)

![image-20240327170054071](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271700824.png)

### 2.3LLM之"通用"——集中化

![image-20240327170136023](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271701445.png)

### 2.4LLM之"最大功臣"——Transformer

![image-20240327170259349](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271703167.png)

- 提出：2017年，Google AI团队提出了 Transformer 架构，并在NLP领域取得了巨大成功
- Transformer 架构是一种基于 Attention 的 Seq2Seq 模型，其特点是 **通过学习输入序列的全局依赖关系，来实现序列到序列的转换**
- 优势：
    - 自注意力机制 ->上下文记忆增强
    - 位置编码 -> 可以实现并行计算，不需等待上文计算完毕再根据目前输入做输出

#### 2.4.1自注意力机制

![image-20240327170849409](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271708257.png)

#### 2.4.2位置编码

- 并行计算优势

    ![image-20240327170928446](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271709333.png)

### 本质

![image-20240327170951738](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271709643.png)

## 3Transformer内部发生着什么

### 3.1数据预处理

1. **Tokenization** 将文本数据转化成 token序列
2. **Embedding** 将token序列映射到 embedding向量空间
3. **Position Encoding** 给每个token 添加位置编码

![image-20240327171259893](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271713751.png)

### 3.2编码器

1. **关联词与其他所有词**：计算每个词与其他词的 **相关性得分**，确定哪些词在当前上下文更加重要
2. **权重赋予**：根据相关性得分，给每个词赋予**权重**
3. **权重加权**：使用这些 **权重** 对每个词的 **embedding向量** 进行加权求和，得到当前上下文的 **表示向量**
4. **多头注意力机制(Multi-Head Attention)**：多头注意力机制是指【多个头（多个注意力机制，从不同角度出发，互不影响）】共同计算当前上下文的 **注意力权重**，并将其**加权求和**，得到最终的 **表示向量**。

![image-20240327171849456](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271718291.png)

![image-20240327171943151](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271719350.png)

### 3.3解码器

- 解码器接受：编码器的输出 + 上一轮文本的输入
- 与编码器的不同之处：
    - 编码器中，在处理各个词时，会关注【输入序列里所有其他词】
    - 解码器中，只会关注【当前词和前面的其他词（确保生成过程的顺序性和正确性）】

![image-20240327172250815](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271722732.png)

### 3.4Linear层和 Softmax层

1. **Linear层**：将 解码器的输出 映射到一个 更大logits向量（长度通常与词汇表的大小一致，预测各个词的概率值）
2. **Softmax层**：将logis向量 转换成 各个词的概率分布（0-1之间的概率值，归一化）

![image-20240327172549136](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271725135.png)

![image-20240327172807789](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271728555.png)

### 拓展：Transformer的变形

![image-20240327172825130](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271728880.png)

## 4 大语言模型的诞生之路

![image-20240327173020964](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271730795.png)

### 4.1无监督学习 -> 基座大模型

1. 数据准备和预处理
    - 海量文本
    - **tokenizer**将文本数据转化成token序列
2. 任务构建
    - **掩码语言建模(Masked Language Modeling,MLM)**：随机替换输入序列的一部分，并预测被替换的部分
3. 模型训练

![image-20240327173318151](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271733365.png)

### 4.2有监督微调 -> 可对话

- 监督微调(supervised fine-tuning,SFT)：使其能更好地适应特定任务
- 加入高质量的问答集合

![image-20240327173638742](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271736517.png)

### 4.3有监督学习 -> 奖励模型

- 能够评估回答的 奖励模型(Reward Model)

![image-20240327173730352](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271737155.png)

![image-20240327173831557](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271738024.png)

![image-20240327173921238](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271739672.png)

### 4.4强化学习 -> 提升回复质量

- 大模型权重调整：权重更新过程中 **朝着增加这些行为或回答发生概率 的方向调整**，不断提升模型的回复质量

![image-20240327174149145](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271741924.png)
![image-20240327174323855](https://cdn.jsdelivr.net/gh/1onetw/BlogImage@main/pic/202403271743279.png)

## The End

