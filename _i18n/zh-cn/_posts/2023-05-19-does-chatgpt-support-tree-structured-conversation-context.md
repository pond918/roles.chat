---
layout: post
title: ChatGPT是否支持树形分支上下文
date: 2023-05-19 01:47:09+0800
description: 随着聊天的深入，会出现越来越多分支子话题，ChatGPT是否支持分支上下文呢？实验揭晓。
tags: tag-prompt-skills
giscus_comments: true
related_posts: true
---

漫长的一个月。。

值得庆祝的是，https://repo.roles.chat 服务系统快要beta了~

今天分享一个小实验，《ChatGPT是否支持分支上下文》

什么意思？看例子。
> 备注：上下文是通过`parentMessageId`来维持的，表示本句话是接话之前哪一句。

**以下问答，都是按照时间顺序呈现的**

-----

- Q1: 高斯是做什么的？
- A1: 高斯（Carl Friedrich Gauss）是18-19世纪德国著名的数学家、天文学家、物理学家和地理学家，被誉为“现代数学之父”。他对数学、统计学、几何学、物理学、天文学等领域做出了众多开创性的贡献，其中最著名的是高斯函数及其在各领域的应用，以及高斯消元法和正态分布等。此外，他还发明了高斯镜头并进行了许多地理测量工作，被认为是现代科学的开拓者之一。

- Q2: 他的老婆是谁？
- A2: 高斯的妻子名叫约翰娜·玛丽亚·沃尔夫（Johanna Maria Walpurga），她和高斯于1805年结婚。约翰娜是一个很有才华的女性，也是一位出色的数学家，她对高斯的学术事业产生了很大的帮助。
  > 这是瞎说的，不必在意。。

- Q2.1: 她哪年出生？
- A2.1: 约翰娜·玛丽亚·沃尔夫出生于1777年10月16日。
  > 此处的“她”理解正确。

- Q1.1: 他几个孩子？
- A1.1: 高斯一共有六个孩子，分别是三个儿子和三个女儿。他的两个儿子成为了著名的数学家，其中最年长的儿子Joseph Fourier Gauss是著名的解析数论学家，次子Eugene Gauss则是对热力学和电磁学做出重要贡献的物理学家。

- Q2.2: 她何时死的？
- A2.2: 约翰娜·玛丽亚·沃尔夫于1854年8月9日去世，享年76岁。
  > 接话很棒。

- Q1.2: 做物理那个何时生的？
- A1.2: 高斯的次子Eugene Gauss于1811年出生，他是一位著名的物理学家，对热力学和电磁学做出了重要贡献。
  > 穿插问话毫无压力。

- Q1.3: 做数学那个何时生的？
- A1.3: 高斯的三个儿子中，最年长的Joseph Fourier Gauss是1792年生的，他也是高斯唯一一个活到成人的儿子，并且成为了著名的数学家。

- Q1.4.1: 他何时死的？
- A1.4.1: 高斯于1855年2月23日逝世，享年77岁。
  > 这个“他”有歧义，GPT不会询问，自选了一个理解来回答。

- Q1.4.2: 这个儿子何时死的？
- A1.4.2: Eugene Gauss于1889年去世，享年78岁。

- Q2.3: 高斯何时认识她的？
- A2.3: 高斯和约翰娜·玛丽亚·沃尔夫是在1800年相识的，当时高斯正在研究数学和天文学，并担任哥廷根大学的教授。两人于1805年结婚。
  > 没问题。

## 小结

基于`parentMessageId`，GPT支持树形分支上下文。为嵌套式任务提供了基础支撑。