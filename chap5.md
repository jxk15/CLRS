---
title: 第 5 章 概率分析和随机算法 Exercises & Problems
---

## Exercises

## 5.1 雇佣问题

1. 设应聘者集合 $S$ 上的关系 $R=\{(a,b):a,b\in S \text{ 且 }a\text{ 与 } b \text{ 同样好或 }a \text{ 比 }b\text{ 更好}\}$。由「我们总能决定哪一个应聘者最佳」，得 $R$ 满足以下三个性质：

    1. 自反性：$aRa$
    2. 反对称性：由 $aRb \land bRa$ 可得 $a=b$
    3. 传递性：由 $aRb \land bRc$ 可得 $aRc$

    所以 $R$ 既是一个偏序关系，也是一个全关系。所以可得到应聘者排名的全部次序。

2. 伪代码如下：

    ```
    RANDOM(a, b)
       n = ceil(lg(b - a + 1))
       while true
          ans = 0
          for i = 0 to n-1
          ans = ans + RANDOM(0, 1) << i
          if ans <= b-a
          return a + ans
    ```

    生成区间 $[a,b]$ 中任一整数的概率均为 $\dfrac{1}{b-a+1}$。

    对每次 while 循环，算法结束的概率为 $\dfrac{b-a+1}{2^n}$，因此 while 循环的次数 $X\sim G(\dfrac{b-a+1}{2^n})$。所以 $EX=\dfrac{2^n}{b-a+1}$。

    所以该算法的期望运行时间为 $\Theta(n\dfrac{2^n}{b-a+1})=\Theta(\lg(b-a))$。

    > 证明：生成区间 $[a,b]$ 中任一整数的概率均为 $\dfrac{1}{b-a+1}$
    >
    > 令 $x$ 是 $[0,b-a]$ 中的某一整数，事件 $A$ 为 $ans=x$ ，事件 $B$ 为算法结束。
    >
    > 显然 $P\{A\}=\dfrac{1}{2^n},P\{B\}=\dfrac{b-a+1}{2^n},P\{B|A\}=1$。
    >
    > 由贝叶斯公式得：$P\{A|B\}=\dfrac{ P\{A\}P\{B|A\} }{ P\{B\} }=\dfrac{1}{b-a+1}$。

3. 伪代码如下：

    ```
    RANDOM(0, 1)
        while true
            x = BIASED-RANDOM()
            y = BIASED-RANDOM()
            if(y != x)
            return x
    ```

    因为 $P\{00 | 11\}=p^2 + (1-p)^2,P\{01\}=(1-p)p, P\{10\}=p(1-p)$。所以结果无偏。

    对每次 while 循环，算法结束的概率为 ${2p(1-p)}$，因此 while 循环的次数 $X\sim G(2p(1-p))$。所以 $EX=\dfrac{1}{2p(1-p)}$。

    假设 BIASED-RANDOM 的运行时间为 $x$，该算法的期望运行时间为 $\Theta(\dfrac{x}{2p(1-p)})$。

## 5.2 指示器随机变量

1. 古典概型

    1. 正好雇佣一次：第一个人即为最优选择，概率为 $\dfrac{(n-1)!}{n!}=\dfrac{1}{n}$。

    2. 正好雇佣 $n$ 次：每个应聘者都优于上个应聘者，概率为 $\dfrac{1}{n!}$。

2. **定义** 应聘者的排名：对 $n$ 位应聘者按优秀程度升序排序，则应聘者的排名为该应聘者从低到高的位数。（最优应聘者的排名为 $n$）

    观察到以下事实：

    - 第一位应聘者和排名为 $n$ 的应聘者都一定会被雇佣。
    - 雇佣排名为 $n$ 的应聘者后，不会再雇佣其他应聘者。

    所以，为了正好雇佣两次，第一个应聘者的排名 $i$ 一定是 $1\le i\le n-1$。而且排名为 $i+1\sim n-1$ 的应聘者一定在最优应聘者之后出现。

    令事件 $E_i$ 为第一个应聘者的排名是 $i$，事件 $F_i$ 为最优应聘者在排名为 $i+1\sim n-1$ 的应聘者之前出现。则 $P\{E_i\}=\dfrac{1}{n}, P\{ F_i|E_i \}=\dfrac{1}{n-i}$。

    令事件 $A$ 为正好雇佣两次。$A=\displaystyle\bigcup_{i=1}^{n-1}F_i\cap E_i$。则

    $$
    \begin{aligned}
    P\{ A \}
    &= \sum_{i=1}^{n-1}P\{ F_i|E_i \}P\{E_i\} \\
    &= \dfrac{1}{n}\sum_{i=1}^{n-1}\dfrac{1}{n-i}
    \end{aligned}
    $$

3. 令随机变量 $X_i$ 为第 $i$ 个骰子的值，随机变量 $X$ 为 $n$ 个骰子之和。

    $$
    \begin{aligned}
    E[X] &= E\Big[\sum_{i=1}^{n} X_i\Big] \\
    &= \sum_{i=1}^{n}E[X_i] \\
    &= \sum_{i=1}^{n}\sum_{j=1}^{6}j\times P\{ X_i= j\} \\
    &= \sum_{i=1}^{n}\sum_{j=1}^{6}\dfrac{j}{6} \\
    &= 3.5n
    \end{aligned}
    $$

4. 设随机变量 $X_i$ 为当顾客 $i$ 拿到自己的帽子时为 1，否则为 0。随机变量 $X$ 为拿到自己帽子的顾客数量。

    $$
    \begin{aligned}
    E[X] &= E\Big[ \sum_{i=1}^{n}X_i \Big] \\
    &= \sum_{i=1}^{n}E[X_i]  \\
    &= \sum_{i=1}^{n}\dfrac{1}{n} \\
    &= 1
    \end{aligned}
    $$

5. 设 $X_{i,j}$ 是一个随机变量，当 $i<j$ 且 $A[i]>A[j]$ 时 $X_{i,j}=1$，否则为 0。逆序对数目的期望值为

    $$
    \begin{aligned}
    E\Big[ \sum_{i=1}^{n-1}\sum_{j=i+1}^{n} X_{i,j}  \Big] &= \sum_{i=1}^{n-1}\sum_{j=i+1}^{n} E[X_{i,j}]\\
    &= \sum_{i=1}^{n-1}\sum_{j=i+1}^{n} P\{A[i]>A[j] \}\\
    &= \sum_{i=1}^{n-1}\sum_{j=i+1}^{n} \dfrac{1}{2} \\
    &= \dfrac{n(n-1)}{4}
    \end{aligned}
    $$


## 5.3 随机算法

1. 将循环中的 i 修改为从 2 开始。

    ```
    swap A[1] with A[RANDOM(1, n)]
    for i = 2 to n
        swap A[i] with A[RANDOM(i, n)]
    ```

2. 没有。排列 $<2,1,3,4,\dots,n>$ 出现的概率为 0。

3. 不会。

    易知该算法会等概率产生 $n^n$ 种交换方式，并对应到 $n!$ 个排列。假设对应到某排列的交换方式有 $m$ 种，则该排列出现的概率为 $\dfrac{m}{n^n}$。

    若可以产生均匀随机排列，则必有 $\dfrac{m}{n^n}=\dfrac{1}{n!}$，即 $\dfrac{n^n}{n!}$ 为整数。当 $n$ 为奇数，不符合条件。

    所以不会。

4. $A[i]$ 出现在 $B$ 中任何特定位置所需的 $offset$ 唯一，而产生任何特定 $offset$ 的概率为 $\dfrac{1}{n}$。将数组 $A$ 首尾相连，观察到该算法不改变 $A$ 中元素的相对位置，也就是说有些排列产生的概率为 0，因此排列结果不是均匀随机排列。

5. 假设事件 $E_i$ 为第 $i$ 个数和 $[1,i-1]$ 中的数不同。则

    $$
    \begin{aligned}
    P\Big\{ \bigcap_{i=1}^{n} E_i \Big\} &= P\{ E_1 \}\times P\{ E_2|E_1 \}\times P\{ E_3|E_1\cap E_2 \} \times\cdots\times P\Big\{ E_n\big | \bigcap_{i=1}^{n-1} E_i \Big\}              \\
    &= 1\times \dfrac{n^3-1}{n^3}\times\dfrac{n^3-2}{n^3}\times\cdots \times\dfrac{n^3-(n-1)}{n^3}\\
    &\ge  \dfrac{n^3-n}{n^3}\times\dfrac{n^3-n}{n^3}\times\dfrac{n^3-n}{n^3}\times\cdots \times\dfrac{n^3-n}{n^3}\\
    &= (\dfrac{n^3-n}{n^3})^n = (1-\dfrac{1}{n^2})^n\\
    &\ge 1-\dfrac{1}{n}                 &(\text{伯努利不等式})
    \end{aligned}
    $$


6. 存在相同的优先级时，从 $m$ 个数中取出一个数的概率不再是 $\dfrac{1}{m}$。

    - 法一：重新生成。
    - 法二：先对优先级排列，对于相同的优先级再应用 `RANDOMIZE-IN-PLACE`。解释：假设对优先级 9 生成了 $m$ 个，8 生成了 $n-m$ 个。则对于 $A[i]$，优先级为 9 的概率为 $\dfrac{m}{n}$。然后对 $m$ 个 9 排序，则排第 1 的概率为 $\dfrac{1}{m}$，所以，$A[i]$ 的最终位置为 $n-m+1$ （从左往右）的概率为 $\dfrac{m}{n}\times\dfrac{1}{m}=\dfrac{1}{n}$。

    > 立即推，放弃 `RANDOMIZE-BY-SORTING`。

7. **理解题意**：输入 $n,m$，在 $\dbinom{n}{m}$ 个 $m$ 集合中等概率（$\dfrac{m!(n-m)!}{n!}$）地选择一个。或者理解为：对 $\{1,2,3,\cdots,n\}$ 中的每个数，出现在生成的 $m$ 集合中的概率为 $\dfrac{m}{n}$。（$\dfrac{m}{n}=\dbinom{n-1}{m-1}\Big/\dbinom{n}{m}$，包含某个数的 $m$ 集合数与所有 $m$ 集合数之比为 $\dfrac{m}{n}$）

    题目中提供了一个易于从古典概型角度理解的算法，该算法需要使用 $n$ 次 `RANDOM`。
    但是如果 $n$ 比 $m$ 大很多，我们希望使用 $m$ 次 `RANDOM` 而不是 $n$ 次。

    算法 `RANDOM-SAMPLE` 显然是调用了 $m$ 次 `RANDOM` 并生成了一个 $m$ 集合（因为第 6 行一定可以加入一个新元素）。
    接下来使用数学归纳法说明对 $\{1,2,3,\cdots,n\}$ 中的每个数，出现在生成的 $m$ 集合中的概率为 $\dfrac{m}{n}$。

    **假设**：`RANDOM-SAMPLE(m,n)` 生成一个 $m$ 集合且对 $\{1,2,3,\cdots,n\}$ 中的每个数，出现在生成的 $m$ 集合中的概率为 $\dfrac{m}{n}$。

    **基本情况**：当 $m=0,1$，显然成立。

    **归纳**：对于 `RANDOM-SAMPLE(m,n)`，已知 `RANDOM-SAMPLE(m-1,n-1)` 会返回一个 $m-1$ 集合 $S'$，其中 $\{1,2,3,\cdots,n-1\}$ 的每个数出现在 $S'$ 的概率为 $\dfrac{m-1}{n-1}$。
    在算法的 4~7 行，向 $S'$ 中加入一个 $i$，$i$ 小于或等于 $n$，于是得到一个 $m$ 集合 $S$。
    令 $E_i$ 为第 4 行返回的数为 $i$。
    若 $1\le i\le n-1$，有
    $$
    \begin{aligned}
    P\{ i\in S \} &= P\{ i\in S' \} + P\{ i\notin S' \land E_i \} \\
    &= \dfrac{m-1}{n-1}+(1-\dfrac{m-1}{n-1})\dfrac{1}{n} \\
    &= \dfrac{m}{n}
    \end{aligned}
    $$

    若 $i=n$，有

    $$
    \begin{aligned}
    P\{ n\in S\} &= P\{ E_n \} + P\{ E_i \land i\in S'(i< n) \} \\
    &= \dfrac{1}{n} + \dfrac{n-1}{n}\dfrac{m-1}{n-1} \\
    &= \dfrac{m}{n}
    \end{aligned}
    $$

    ??? 另一个证明
        **假设**：`RANDOM-SAMPLE(m,n)` 等可能（$1\Big/\dbinom{n}{m}=\dfrac{m!(n-m)!}{n!}$）生成一个 $m$ 集合。

        **基本情况**：当 $m=0,1$，显然成立。

        **归纳**：令 `RANDOM-SAMPLE(m,n)` 生成的 $m$ 集合为 $S$，令 $E_i$ 为第 4 行返回的数为 $i$。分两种情况，

        - 若 $S$ 含元素 $n$，则 $S$ 由集合 $S'=S-\{n\}$ 转移得到。若已有 $S'$，当 $E_n$ 或 $E_i\land i\in S'$ 得到 $S$，概率为 $\dfrac{1+(m-1)}{n}$。所以 $P\{S\}=P\{S'\}\times \dfrac{m}{n}=1\Big/\dbinom{n-1}{m-1}\times \dfrac{m}{n}=1\Big/\dbinom{n}{m}$。
        - 若 $S$ 不含元素 $n$，则 $S$ 由 $m$ 种 $m-1$ 集合（从 $S$ 中去除一个元素）转移得到。对于每个 $m-1$ 集合 $S_i$，当 $E_{S-S_i}$ 时得到 $S$，概率为 $\dfrac{1}{n}$。所以 $P\{S\}=\sum_{i=1}^{m}P\{S_i\}\times \dfrac{1}{n}=m\times 1\Big/\dbinom{n-1}{m-1}\times \dfrac{1}{n}=1\Big/\dbinom{n}{m}$。


## 5.4 概率分析和指示器随机变量的进一步使用

1. **问题 1**：设一年的天数 $n=365$，当房间里有 $k$ 个人，这 $k$ 个人的生日都和我不同的概率为 $\Big(\dfrac{364}{365}\Big)^k$，所以 $1-\Big(\dfrac{364}{365}\Big)^k$ 是存在某人与我生日相同的概率。所以 $k$ 至少为 253。

    **问题 2**：设一年的天数 $n=365$，当房间里有 $k$ 个人，

    $$
    \begin{aligned}
        &P\{\text{至少两个人生日为 7 月 4 号}\} \\\\
        = &1-P\{\text{没有人生日为 7 月 4 号}\}-P\{\text{只有一个人生日为 7 月 4 号}\}  \\\\
        = &1 - \Big(\dfrac{n-1}{n}\Big)^k - \binom{k}{1}\dfrac{1}{n}\Big(\dfrac{n-1}{n}\Big)^{k-1}
    \end{aligned}
    $$

    所以 $k$ 至少为 613。

2. 该问题等同于生日悖论。

3. 由等式 5.6 知两两独立即可。

4. 设一年的天数 $n=365$，$i$ 的生日为 $B_i$，设随机变量 $X_{ijk}$，当 $i,j,k$ 的生日相同时为 1，否则为 0。则 $P\{B_i=B_j=B_k\}=\sum_{r=1}^nP\{B_i=B_j=B_k=r\}=\dfrac{1}{n^2}$。

    当房间内有 $m$ 个人时，

    $$
    \begin{aligned}
    E\Big[\sum_{i=1}^m\sum_{j=i+1}^m\sum_{k=j+1}^mX_{ijk}\Big]
    &= \sum_{i=1}^m\sum_{j=i+1}^m\sum_{k=j+1}^mE[X_{ijk}] \\
    &= \sum_{i=1}^m\sum_{j=i+1}^m\sum_{k=j+1}^m\dfrac{1}{n^2} \\
    &= \binom{m}{3}\dfrac{1}{n^2}
    \end{aligned}
    $$

    所以 $m$ 至少为 94。

5. 构成一个 $k$ 排列：$k$ 个字符互不相同。该问题是生日悖论的补，等式 5.7 即为该问题的解。

6. 设随机变量 $X_i$，当第 $i$ 个箱子为空时为 1，否则为 0。则 $P\{X_i=1\}=\Big(\dfrac{n-1}{n}\Big)^n$，随机变量 $X$ 为空箱子数目，

    $$
    \begin{aligned}
    E[X] &= E\Big[\sum_{i=1}^n X_i \Big ] \\
    &= \sum_{i=1}^n E[X_i] \\
    &= \sum_{i=1}^n \Big(\dfrac{n-1}{n}\Big)^n \\
    &= \dfrac{n}{e} &(由等式 3.14 得)
    \end{aligned}
    $$

    设随机变量 $X_i$，当第 $i$ 个箱子中正好有一个球时为 1，否则为 0。则 $P\{X_i=1\}=\dbinom{n}{1}\dfrac{1}{n}\Big(\dfrac{n-1}{n}\Big)^{n-1}=\Big(\dfrac{n-1}{n}\Big)^{n-1}$，随机变量 $X$ 为正好有一个球的箱子的数目，

    $$
    \begin{aligned}
    E[X] &= E\Big[\sum_{i=1}^n X_i \Big ] \\
    &= \sum_{i=1}^n E[X_i] \\
    &= \sum_{i=1}^n \Big(\dfrac{n-1}{n}\Big)^{n-1} \\
    &= \dfrac{n(\frac{n-1}{n}^{n})}{\frac{n-1}{n}} \\
    &= \dfrac{n^2}{e(n-1)}  &(由等式 3.14 得)
    \end{aligned}
    $$

7. 7


## Problems

1. 概率计数  http://home.ustc.edu.cn/~skiper/BigData/2.html

    - 设随机变量 $X_i$ 为第 $i$ 次 INCREMENT 操作使计数器增加的值，随机变量 $V_n$ 为 $n$ 次 INCREMENT 操作后计数器的值。

        得 $E[V_n]=E\Big[\displaystyle\sum_{i=1}^n X_i\Big]=\sum_{i=1}^nE[X_i]$。

        由题意知 $E[X_i]=0\times (1-\dfrac{1}{n_{i+1}-n_i}) + (n_{i+1}-n_i)\times \dfrac{1}{n_{i+1}-n_i}=1$。

        所以 $E[V_n]=n$。

    - 沿用 $X_i,V_n$ 的定义。

        $X_i$ 两两独立，由等式 C.29 得 $Var[V_n]=\displaystyle\sum_{i=1}^nVar[X_i]$。

        由 $n_i=100i$，得 $n_{i+1}-n_i=100$。

        由等式 C.27 得

        $$
        \begin{aligned}
        Var[X_i] &= E[X_i^2] - E^2[X_i] \\
        &= 0^2\times \dfrac{99}{100} + 100^2\times\dfrac{1}{100} - 1^2 \\
        &= 99
        \end{aligned}
        $$

        所以 $Var[V_n]=99n$。

2. 查找一个无序数组

    1. 伪代码如下

        ```
        // 数组 A 中有 n 个元素，待查找值为 x
        RANDOM-SEARCH(A, n, x)
            bool B[n]  // 初始均为0，B[i] = 0 表示 A[i] 未检查过
            int m = 0  // A 中检查过的元素个数
            while m < n
            i = RANDOM(1, n)
            if A[i] == x
                return i
            else
                if B[i] == 0
                B[i] = 1, m = m + 1
            return NIL
        ```

    2. 每次随机找到 $x$ 的概率为 $\dfrac{1}{n}$，符合几何分布，期望为 $n$ 次。

    3. 同 2，期望为 $\dfrac{n}{k}$ 次。

    4. 题意：每个元素都至少选到一次的期望。同 5.4.2 球与箱子，需要 $n(\ln n + O(1))$ 次。

    5. $x = A[i],1\le i\le n$ 的概率为 $\dfrac{1}{n}$。设随机变量 $X$ 为查找次数，则 $E[X]=\displaystyle\sum_{i=1}^{n}\dfrac{1}{n}\times i=\dfrac{1+n}{2}$。最坏情况即为 $x=A[n]$，需要查找 $n$ 次。

    6. 最坏情况即 $A[(n-k+1)..n]$ 内均为 $x$，需要查找 $n-k+1$ 次。

        平均情况：

        $\displaystyle\sum_{i=1}^{n-k+1}i\times \dfrac{\dbinom{n-i}{k-1}}{\dbinom{n}{k}}=\dfrac{\dbinom{n+1}{k+1}}{\dbinom{n}{k}}=\dfrac{n+1}{k+1}$

    7. 显然，平均情况与最坏情况均需查找 $n$ 次。

    8.  为了随机重排，需要 $\Theta(n)$ 的运行时间。

        $k = 0$，最坏运行时间与运行时间期望均为 $O(n)$。

        $k = 1$，最坏运行时间与运行时间期望均为 $O(n)$。

        $k \ge 1$，最坏运行时间为 $O(n-k+1)+\Theta(n)$，运行时间期望为 $\dfrac{n+1}{k+1}+\Theta(n)$。

    9.  综合最坏情况和平均情况的运行时间，我选择第二种算法，即 DETERMINISTIC-SEARCH。

