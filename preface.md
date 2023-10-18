---
title: 《算法导论》前言
---




> Cormen 在 Quora 上的回答
>
> - 为什么算导中没有 xx 主题？会有第二卷吗？https://qr.ae/pyof1A
>
> - 算导会出一些免费的在线内容吗？https://qr.ae/pyPXxo

以算法导论为主，其他作为补充。笔记中包含补充内容。

- [算法](https://book.douban.com/subject/19952400/) 仅有四章：排序、查找、图、字符串。深度小于算导。
- [数据结构与算法分析](https://book.douban.com/subject/33419792/) 内容更多，深度最浅。

学习路线：第一部分是其他部分的基础，其他部分之间没什么依赖关系。

如何看待 Thomas Cormen 所说看完《算法导论》需要的时间？ - 胡津铭的回答 - 知乎 https://www.zhihu.com/question/27129103/answer/652059486

## 第三版

本书在 2012 年出了第三版，当时第3版的主要变化为：

- 新增了van Emde Boas树和多线程算法，并且将矩阵基础移至附录。
- 修订了递归式（现在称为“分治策略”）那一章的内容，更广泛地覆盖分治法。
- 移除两章很少讲授的内容：二项堆和排序网络。
- 修订了动态规划和贪心算法相关内容。
- 流网络相关材料现在基于边上的全部流。
- 由于关于矩阵基础和 Strassen 算法的材料移到了其他章，矩阵运算这一章的内容所占篇幅更小。
- 修改了对 Knuth-Morris-Pratt 字符串匹配算法的讨论。
- 新增 100 道练习和 28 道思考题，还更新并补充了参考文献。

## 第四版

2022年4月发布：第四版的主要变化为：https://mitpress.mit.edu/9780262046305/introduction-to-algorithms/

- 斐波那契堆、van Emde Boas树、计算几何学已移至线上
- 140 个新 exercises、22 个新 problems 以及其他 exercises 和 problems 的改进版本
- 全文添加了颜色以增强可读性、突出显示定义的术语和伪代码注释
- 将代码从 Java 更新为 Python

## 3e 题目统计

1排序、2数据结构、3 DP贪心B树并查集、4图论、5傅里叶数论字符串

题目统计：

1. 8、1
2. 15、4
3. 16、6
4. 38、6
5. 22、2
6. 30、3
7. 18、6
8. 18、7
9. 15、4
10. 26、3
11. 22、4
12. 25、4
13. 25、4
14. 19、2
15. 27、12
16. 28、5
17. 16、5
18. 14、2
19. 5、4
20. 18、2
21. 20、3
22. 42、4
23. 19、4
24. 40、6
25. 25、2
26. 40、6
27. 21、6
28. 20、2
29. 39、5
30. 19，6
31. 48，4
32. 25，1
33. 29、5
34. 40、4
35. 24、7

Exercises：856

Problems：151

共 1007

跳过的部分

1. 贪心算法1
2. 斐波那契堆1
3. van Emde Boas树1
4. 最大流
5. 多线程算法
6. 矩阵运算1
7. 线性规划
8. 计算几何学1
9. NP 完全性1
10. 近似算法

Exercises：264

Problems：46

共 310



## 其他信息

- 第一版刊行于 1990 年。
- 第二版刊行于 2001 年。
- 第三版刊行于 2009 年。
- 第四版刊行于 2022 年。

《算法导论》第二版和第三版的区别大吗？有中文版的吗?https://developer.aliyun.com/ask/124998

算法导论，第三版https://mitpress.mit.edu/9780262533058/introduction-to-algorithms/

如何评价2022新版算法导论内容？ - 猎户座的回答 - 知乎 https://www.zhihu.com/question/527135069/answer/2543323043

《算法导论》第四版(Introduction to Algorithms, Fourth Edition/CLRS, 4e)修订内容 - Sefank的文章 - 知乎 https://zhuanlan.zhihu.com/p/466819939

## 4e

Chapter 3 Growth of Functions: Renamed “Characterizing Running Times” and added a section giving an overview of asymptotic notation before delving into the formal definitions

Chapter 4 Divide and Conquer: Substantially changed to improve its mathematical foundation and make it more robust and intuitive
• Algorithmic recurrence introduced, and topic of ignoring floors and ceilings in recurrences addressed more rigorously
• Second case of the master theorem incorporates polylogarithmic factors, and a rigorous proof of a “continuous” version of the master theorem now provided
• Now presents the powerful and general Akra-Bazzi method (without proof)

Chapter 9 Medians and Order Statistics: Deterministic order-statistic algorithm is different, and analyses of randomized and deterministic orderstatistic algorithms are revamped

Chapter 10 Elementary Data Structures: Section 10.1 discusses ways to store arrays and matrices

Chapter 11 Hash Tables: Includes modern treatment of hash functions and emphasizes linear probing as an efficient method for resolving collisions when the underlying hardware implements caching to favor local searches

Chapter 15 Greedy Algorithms: Replaced sections on matroids, converted a problem in the third edition about offline caching into a full section

Chapter 16 Amortized Analysis: Section 16.4 now contains a more intuitive explanation of the potential functions to analyze table doubling and halving

Chapter 17 Augmenting Data Structures: Relocated from Part III to Part V, reflecting the authors’ view that this technique goes beyond basic material

NEW Chapter 25 Matchings in Bipartite Graphs: Presents algorithms to find a matching of maximum cardinality, to solve the stable-marriage problem, and to find a maximum-weight matching (known as the “assignment problem”)

Chapter 26 Parallel Algorithms: Updated with modern terminology, including the name of the chapter

NEW Chapter 27 Online Algorithms: Describes several examples of online algorithms, including determining how long to wait for an elevator before taking the stairs, maintaining a linked list via the move-to-front heuristic, and evaluating replacement policies for caches

Chapter 29 Linear Programming: Removed the detailed presentation of the simplex algorithm. The chapter now focuses on the key aspect of how to model problems as linear programs, along with the essential duality property of linear programming

Chapter 32 String Matching: Section 32.5 adds to the chapter on string matching the simple, yet powerful, structure of suffix arrays

NEW Chapter 33 Machine-Learning Algorithms: Introduces several basic methods used in machine learning: clustering to group similar items together, weighted-majority algorithms, and gradient descent to find the minimizer of a function

Chapter 34 NP-Completeness: Section 34.5.6 summarizes strategies for polynomial-time reductions to show that problems are NP-hard