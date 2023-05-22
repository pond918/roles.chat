---
layout: post
title: Does ChatGPT support tree-structured conversations context?
date: 2023-05-19 01:47:09+0800
description: As the chat deepens, there will be more and more branching subtopics. Does ChatGPT support branching context? The experiment reveals the answer.
tags: tag-prompt-skills
giscus_comments: true
related_posts: true
---

2023/5/19 01:47:09

Long time no see! What a busy month..

I’glad to come back to share, that the backend service for https://repo.roles.chat is almost ready for beta.

Today let’s do a small experiment, "Does ChatGPT support branching contexts?”

What does it mean? Let's take an example.
> Note: The context is maintained through `parentMessageId`, indicating which sentence this one follows.

**The following Q&A will be presented in chronological order.**

-----

- Q1:  Who was Gauss?
- A1: Carl Friedrich Gauss was a famous German mathematician, astronomer, physicist, and geographer of the 18th-19th centuries, known as the "father of modern mathematics". He made numerous pioneering contributions to fields such as mathematics, statistics, geometry, physics, and astronomy. His most famous works include Gaussian functions and their applications in various fields, Gaussian elimination method, and normal distribution, among others. In addition, he invented the Gauss lens and conducted many geographic survey works, and is considered one of the pioneers of modern science.

- Q2: Who was his wife?
- A2: Gauss's wife was named Johanna Maria Walpurga, and they were married in 1805. Johanna was a talented woman and also an outstanding mathematician who greatly assisted Gauss in his academic career.
  > This is just BS, don't sweat it.

- Q2.1: When was she born?
- A2.1: Johanna Maria Walpurga was born on October 16, 1777.
  > The "she" mentioned here is understood correctly.

- Q1.1: How many children did he have?
- A1.1: Gauss had a total of six children, three sons and three daughters. His two eldest sons became famous mathematicians, with the oldest son Joseph Fourier Gauss being a renowned analytic number theorist, and the second son Eugene Gauss making important contributions to thermodynamics and electromagnetism as a physicist.

- Q2.2: When did she die?
- A2.2: Johanna Maria Walpurga passed away on August 9, 1854, at the age of 76.
  > Well done！

- Q1.2: When was the person who did physics born?
- A1.2: Eugene Gauss, the second son of Gauss, was born in 1811. He was a famous physicist who made important contributions to thermodynamics and electromagnetism.
  > Interjecting questions is effortless.

- Q1.3: When was the person who did mathematics born?
- A1.3: Joseph Fourier Gauss, the eldest of Gauss's three sons, was born in 1792. He was also Gauss's only son who survived into adulthood and became a famous mathematician.

- Q1.4.1: When did he die?
- A1.4.1: Gauss passed away on February 23, 1855 at the age of 77.
  > This "he" is ambiguous, and GPT will not ask for clarification. It chooses one interpretation to provide an answer.

- Q1.4.2:  When did this son pass away?
- A1.4.2: Eugene Gauss passed away in 1889 at the age of 78.

- Q2.3: When did Gauss meet her?
- A2.3: Gauss met Johanna Maria Magdalena Waldeck in 1800 when he was studying mathematics and astronomy and serving as a professor at the University of Göttingen. The two married in 1805.
  > No problem.

## Summary

Based on `parentMessageId`, GPT supports tree-structured conversation context, providing a fundamental support for nested tasks.
