---
title: 第 7 章 快速排序 Exercises & Problems
---

## Exercises

## 7.1 快速排序的描述

1. 模拟过程：略。
2. 返回 $r$。使用一个变量：记录上一次 $A[j]=A[r]$ 时 $A[j]$ 被放在了哪个区间。
3. 循环之外的部分花费时间为 $\Theta(1)$，循环体执行了 $\Theta(n)$ 次，每次需要 $\Theta(1)$。所以总时间为 $\Theta(n)$。
4. 修改 $\text{PARTITION}$ 使小于等于主元的元素在右边，大于主元的元素在左边。

## 7.2 快速排序的性能

1. 代入法：略。

2. $T(n)=T(n-1)+T(0)+\Theta(n)$，所以 $T(n)=\Theta(n^2)$。

3. 此时子数组的大小仍分别为 0 和 $n-1$。

4. 几乎有序时插入排序的时间复杂度为 $\Theta(n)$，快速排序为 $\Theta(n^2)$。所以插入排序的性能更优。

    > 前面的都是废话，不用看。直接看「它实质上是一个对几乎有序的。。。」

5. 对任意深度为 $k=i+j$ 的结点，该结点所含元素数量约为 $n\times\alpha^i\times(1-\alpha)^j$。若该结点为叶结点（元素数量为 1），易知当 $j=0$ 时 $k$ 最小，当 $i=0$ 时 $k$ 最小。所以叶结点的最小深度约为 $\log_{\alpha}\dfrac{1}{n}=-\dfrac{\lg{n}}{\lg{\alpha}}$，最大深度约为 $\log_{1-\alpha}\dfrac{1}{n}=-\dfrac{\lg{n}}{\lg({1-\alpha})}$。

6. 当主元的排名为 $\alpha n$ 或 $(1-\alpha)n$ 时会导致 $1-\alpha:\alpha$ 的划分。所以主元的排名 $x$ 满足 $\alpha n< x <(1-\alpha)n$ 时会产生更好的划分。概率为 $\dfrac{(1-\alpha)n-\alpha n}{n}=1-2\alpha$。

## 7.3 快速排序的随机化版本

1. 因为随机化算法会做出一些随机的选择，使得难以针对算法流程生成导致最坏情况的输入，大大降低了遇到最坏情况的几率。

2. 最坏情况：$T(n)=T(n-1)+1$。$T(n)=\Theta(n)$。

    最好情况：$T(n)=2T(\dfrac{n}{2})+1$。$T(n)=\Theta(n)$。

    或者：每执行一次 RANDOM，被选到的元素将不会参与剩下的排序过程，所以选择次数为 $\Theta(n)$。

## 7.4 快速排序分析

1. 假设 $T(n)\ge cn^2$，代入得

    $$
    \begin{aligned}
    T(n) &\ge \max_{0\le q \le n-1}(cq^2+c(n-q-1)^2) + dn  \\
    &= c\times \max_{0\le q \le n-1}(q^2+(n-q-1)^2) + dn  \\
    &= cn^2 + (d-2c)n + c  &(0<c\le d/2) \\
    &\ge cn^2
    \end{aligned}
    $$

    所以 $T(n)=\Omega(n^2)$。

2. 最好情况下递归式为 $T(n)=\displaystyle\min_{0\le q \le n-1}(T(q)+T(n-q-1)) + \Theta(n)$。

    假设 $T(n)\ge cn\lg n$，代入得

    $$
    \begin{aligned}
    T(n) &\ge \min_{0\le q \le n-1} (cq\lg{q}+c(n-q-1)\lg{(n-q-1)}) + \Theta(n)  \\
    &\ge c(n-1)\lg{(\dfrac{n-1}{2})} + \Theta(n)             &(q=\dfrac{n-1}{2})       \\
    &= c(n-1)\lg(n-1) - c(n-1)  + \Theta(n) \\
    &= cn\lg(n-1)  - c\lg(n-1) - c(n-1) + \Theta(n) \\
    &\ge cn\lg{\dfrac{n}{2}} - c\lg(n-1) - c(n-1) + \Theta(n) \\
    &= cn\lg{n} - cn - c\lg(n-1) - c(n-1) + \Theta(n) \\
    &\ge cn\lg n
    \end{aligned}
    $$

    所以最好情况下 $T(n)=\Omega(n\lg n)$。

3. 略。

4. 对书中的证明稍作修改：

    $$
    \begin{aligned}
    E[X] &= \sum_{i=1}^{n-1}\sum_{j=i+1}^{n}\dfrac{2}{j-i+1} \\
    &= \sum_{i=1}^{n-1}\sum_{k=1}^{n-i}\dfrac{2}{k+1}  &(k=j-i) \\
    &\ge \sum_{i=1}^{n-1}\sum_{k=1}^{n-i}\dfrac{2}{2k}  \\
    &= \sum_{i=1}^{n-1}\Omega(\lg(n-i))  \\
    &= \sum_{l=1}^{n-1}\Omega(\lg l) \\
    &= \Omega(n\lg n)
    \end{aligned}
    $$

5. 排序结束后对整个数组运用插入排序：对 $\dfrac{n}{k}$ 个长度为 $k$ 的子数组排序，时间复杂度为 $O(\dfrac{n}{k}k^2)=O(nk)$。

    排序产生的递归树期望高度为 $O(\lg\dfrac{n}{k})$，每层的代价为 $O(n)$，所以时间复杂度为 $O(n\lg\dfrac{n}{k})$。

    综上，该算法的期望时间复杂度为 $O(nk+n\lg\dfrac{n}{k})$。

    理论角度：为了使 $c_1 nk + c_2n\lg\dfrac{n}{k} \le c_2n\lg n$，得 $k \le \dfrac{c_2}{c_1}\lg k$。

    实践角度：多次实验。

6. 该问题的补问题更容易解决。设 $0<\alpha\le\dfrac{1}{2}$，且能重复取某元素。获得比 $\alpha:1-\alpha$ 更差的划分时，中位数的排名 $x$ 满足 $1\le x\le\alpha n$ 或 $(1-\alpha)n\le x\le n$，即至少有两个元素的排名在 $[1,\alpha n]$ 或 $[(1-\alpha)n, n]$，概率为 $2\times\Big(\dbinom{3}{1}(\dfrac{\alpha n}{n})^2\times\dfrac{(1-\alpha)n}{n}+(\dfrac{\alpha n}{n})^3\Big)=6\alpha^2-4\alpha^3$。

    所以最坏划分为 $\alpha:1-\alpha$ 的概率为 $1-(6\alpha^2-4\alpha^3)$。

## Problems

1. Hoare 划分的正确性

    1. 模拟过程：略。

    2. 第一次迭代中的两个 $\text{repeat-until}$ 结束后，$p\le j \le r,i=p$。若 $j=p$，循环结束，未发生越界。否则发生交换，记 $A[p]$ 被交换到 $A[k]$，则 $A[p]\le x,A[k]=x$。继续迭代，

        - 对于 $i$：如果 $i$ 能移动到 $k$，一定会停下来。因为第一次迭代之后 $j$ 一定会递减 1，所以此时循环一定结束，$i$ 未越界。
        - 对于 $j$：同上。如果 $j$ 能移动到 $p$，一定会停下来。因为后续迭代中 $i$ 一定会递加 1，所以此时循环一定结束，$j$ 未越界。

    3. 由第 2 问的证明过程知循环结束时 $p \le j < r$。

        ??? 多说点
            首先由第 2 问知 $j$ 不会越界。

            若第一次迭代中 $j=p$，取到最小值然后循环结束。

            否则 $p<j\le r$，然后再迭代，$j$ 递减后一定小于 $r$。

            所以 $p\le j<r$。


    4. **循环不变量**：每次迭代开始时，$A[p..i]$ 中的元素小于等于 $x$，$A[j..r]$ 中的元素大于等于 $x$。

        **初始**：显然成立。

        **保持**：$j$ 递减直到 $A[j]\le x$，此时 $A[j+1..r]$ 中的元素大于等于 $x$。$i$ 递增直到 $A[i]\ge x$，此时 $A[p..i-1]$ 中的元素小于等于 $x$。若 $i<j$，发生交换，循环不变量保持成立。

        **终止**：若 $i=j$，则 $A[i]=A[j]=x$。由循环不变量知 $A[p..j]$ 中的元素均小于等于 $A[j+1..r]$ 中的元素。若 $j<i$，最后一次迭代没有发生交换，**虽然循环不变量没有保持**，但仍能提供一些有用的信息。由循环不变量知 $i=j+1$，所以 $A[p..j]$ 中的元素均小于等于 $A[j+1..r]$ 中的元素。

    5. 伪代码如下。由于主元会参与后续的排序，所以要保证子数组的大小一定更小。

        ```pseudocode
        QUICKSORT(A, p, r)
            if p < r
            q = HOARE-PARTITION(A, p, r)
            QUICKSORT(A, p, q)
            QUICKSORT(A, q + 1, r)
        ```




2. 针对相同元素值的快速排序

    1. 此时随机操作是无效的，运行时间为 $\Theta(n^2)$。

    2. 伪代码如下：

        ```
        PARTITION'(A, p, r)
            x = A[p]
            q = p
            t = p
            k = r + 1
            // 若 [t+1, k-1] 非空，执行循环体
            while k - t > 1
                if A[t + 1] = x
                    t = t + 1
                if A[t + 1] < x
                    exchange A[t + 1] with A[q]
                    q = q + 1
                    t = t + 1
                if A[t + 1] > x
                    k = k - 1
                    exchange A[t] with A[k]
            return (q, t)
        ```

        ??? 正确性简证

            **循环不变量**：在每次迭代前，$[p,q-1]$ 中的元素小于 $x$，$[q,t]$ 中的元素等于 $x$，$[t+1,k-1]$ 中的元素无限制，$[k,r]$ 中的元素大于 $x$。

            每次迭代，把 $A[t+1]$ 上的元素放在正确的区间，并修改相关区间的端点。

            每次把一个元素放到正确的区间，所以 $[t+1,k-1]$ 中元素个数为 0 时满足题目要求。

    3. 使用三路划分的快排。伪代码如下。

        ```
        RANDOMIZED-PARTITION'(A, p, r)
            i = RANDOM(p, r)
            exchange A[p] with A[i]
            return PARTITION'(A, p, r)

        QUICKSORT'(A, p, r)
            if p < r
                (q, t) = RANDOMIZED-PARTITION'(A, p, r)
                QUICKSORT'(A, p, q-1)
                QUICKSORT'(A, t+1, r)
        ```

    4. 互异假设不成立时，如何套用分析方法：由于存在重复元素，所以 $P\{z_i\text{ compare with } z_j\}$ 的概率下降。

3. 另一种快速排序的分析方法

    1. $E[X_i]=\dfrac{1}{n}$

    2. 当 $X_q=1$ 时，$T(n)=E[T(q-1)+T(n-q)+\Theta(n)]$。所以

        $$
        \begin{aligned}
        E[T(n)]
            &= \sum_{q=1}^{n} P\{X_q=1\}E[T(q-1)+T(n-q)+\Theta(n)] \\
            &= \sum_{q=1}^{n} E[X_q]\times E[T(q-1)+T(n-q)+\Theta(n)]  \\
            &= \sum_{q=1}^{n} E[X_q(T(q-1)+T(n-q)+\Theta(n))] &(这两个事件独立)\\
            &= E\Big[\sum_{q=1}^{n} X_q(T(q-1)+T(n-q)+\Theta(n))\Big] \\
        \end{aligned}
        $$

    3. 如下：

        $$
        \begin{aligned}
        E[T(n)]
        &= \sum_{q=1}^{n} E[X_q]\times E[T(q-1) + T(n-q) + \Theta(n)]\\
        &= \sum_{q=1}^{n} \frac{1}{n}E[T(q-1) + T(n-q) + \Theta(n)]\\
        &= \Theta(n) + \dfrac{1}{n} \sum_{q=1}^{n} E[T(q-1) + T(n-q)]\\
        &= \Theta(n) + \dfrac{2}{n}\sum_{q=1}^{n} E[T(q-1)]\\
        &= \Theta(n) + \dfrac{2}{n}\sum_{q=2}^{n-1} E[T(q)]
        \end{aligned}
        $$

    4. 略

    5. 假设 $E[T(n)]\le n\lg n + \Theta(n)$，代入得

4. 快速排序的栈深度

    1. 因为该算法等价于 $\text{QUICKSORT}$。

    2. 当数组已升序排序时，每次向下递归的子数组大小为 $n-1$，所以栈深度为 $\Theta(n)$。

    3. 修改：每次对较小的子数组向下递归。

        最坏情况下最大深度 $k$ 满足 $(1/2)^{k}n\approx1$。即每次划分产生的两个子数组大小接近。因为向下递归时子数组最大约为 $n/2$。此时 $k=\Theta(\lg n)$。

        由于栈深度为 $O(\lg n)$，每层代价为 $O(n)$，所以时间复杂度为 $O(n\lg n)$。

5. 三数取中划分

    1. 若 $x=A'[i]$，则选到的三个数中有一个在 $A[1..i-1]$ 内，一个在 $A[i+1..n]$ 内，还有一个是 $A[i]$。

        所以概率为 $p_i=3!\dfrac{1\times (i-1)\times(n-i)}{n(n-1)(n-2)}$。

    2. $n-\lfloor(n+1)/2\rfloor=\lfloor n/2\rfloor,\lfloor(n+1)/2\rfloor-1=\lfloor(n-1)/2\rfloor$。设增加的概率为 $x$，则

        $$
        \begin{aligned}
        \lim_{n\to\infty}x &= \lim_{n\to\infty}\dfrac{6(\lfloor n/2\rfloor)(\lfloor(n-1)/2\rfloor)}{n(n-1)(n-2)}-\dfrac{1}{n} \\
        &= \dfrac{1}{2n}
        \end{aligned}
        $$

        > 区别不大

    3. 平凡实现中的概率为 $\dfrac{1}{3}$。设三数取中划分中的概率为 $x$，则

        $$
        \begin{aligned}
        x &= \int_{n/3}^{2n/3}\dfrac{6(x-1)(n-x)}{n(n-1)(n-2)}dx \\
        &= \dfrac{13}{27}
        \end{aligned}
        $$

        > 与练习 7.4-6 的结果相等

    4. 三数取中划分的最好情况是每次都取到数组的中位数，所以不能使时间复杂度突破 $\Omega(n\lg n)$ 的下界。选取中位数的时间是常量的，所以只影响下界的常数因子。

6. 对区间的模糊排序

    1. 略



