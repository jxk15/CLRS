---
title: 第 3 章 函数的增长 Exercises & Problems
---


## Exercises

## 3.1 渐进记号

1. 因为 $f(n),g(n)$ 都是渐进非负函数，所以 $\exist n_1,n_2$，当 $n\ge n_1$，有 $f(n)\ge 0$，当 $n\ge n_2$，有 $g(n)\ge 0$。令 $n_0=\max\{n_1,n_2\}$。当 $n\ge n_0$，得到 $0\le \dfrac{f(n)+g(n)}{2}\le\max\{f(n),g(n)\}\le f(n)+g(n)$。所以 $\max\{f(n),g(n)\}=\Theta(f(n)+g(n))$。

2. $(n+a)^b=\displaystyle\sum_{i=0}^{b}C_b^{i}n^{b-i}a^i$。
   $n^b\le\displaystyle\sum_{i=0}^{b}C_b^{i}n^{b-i}a^i\le n^{b}\displaystyle\sum_{i=0}^{b}C_b^{i}a^i$，所以 $(n+a)^b=\Theta(n^b)$。

3. $O(n^2)$ 包含 $\Theta(n^2),o(n^2)$。所以算法 A 的下界是不清晰的。又因为「至少」，所以上界也未知。

4. $2^{n+1}=O(2^n)$ 成立。
   $2^{2n}=O(2^n)$ 不成立。若成立，则存在正常数 $c,n_0$ 使得当 $n\ge n_0$，有 $2^{2n}\le c\times2^{n}$，两边同时除以 $2^n$ 得 $2^n\le c$，因为 $c$ 为常量，所以对任意大的 $n$，该不等式不成立。

5. 证明定理 3.1
    - 充分性：
      因为 $f(n)=\Theta(g(n))$，所以存在正常量 $c_1,c_2,n_0$，使得对所有 $n\ge n_0$，有 $0\le c_1g(n)\le f(n)\le c_2g(n)$。
      所以存在正常量 $c_1,n_0$，使得对所有 $n\ge n_0$，有 $0\le c_1g(n)\le f(n)$，所以 $f(n)=\Omega(g(n))$。
      存在正常量 $c_2,n_0$，使得对所有 $n\ge n_0$，有 $0\le f(n)\le c_2g(n)$，所以 $f(n)=O(g(n))$。

    - 必要性：
     因为 $f(n)=\Omega(g(n))$。所以存在正常量 $c_1,n_1$，使得对所有 $n\ge n_1$，有 $0\le c_1g(n)\le f(n)$。
     因为 $f(n)=O(g(n))$。所以存在正常量 $c_2,n_2$，使得对所有 $n\ge n_2$，有 $0\le f(n)\le c_2g(n)$。
     令 $n_0=\max\{n_1,n_2\}$， 则存在正常量 $c_1,c_2,n_0$，使得对所有 $n\ge n_0$，有 $0\le c_1g(n)\le f(n)\le c_2g(n)$，所以 $f(n)=\Theta(g(n))$。

6. 最坏情况运行时间为 $O(g(n))$，即算法运行时间为 $O(g(n))$。最好情况运行时间为 $\Omega(g(n))$，即算法运行时间为 $\Omega(g(n))$。由定理 3.1 知待证问题成立。

7. 设 $f(n)\in o(g(n))\cap w(g(n))$

    则 $\forall c>0$，c 是常数，$\exist n_1,n_2$，当 $n\ge n_1$，有 $0\le f(n)< cg(n)$；当 $n\ge n_2$，有 $0\le cg(n)<f(n)$。令 $n_0=\max\{n_1,n_2\}$，当 $n\ge n_0$，有 $0 \le f(n)<cg(n)<f(n)$，矛盾。

8. $\Omega(g(n,m))=\{f(n,m):\text{存在正常量} c,n_0,m_0,\text{使得对所有} n\ge n_0\text{或} m\ge m_0,\text{有}0\le cg(n,m)\le f(n,m) \}$

    $\Theta(g(n,m))=\{f(n,m):\text{存在正常量} c_1,c_2,n_0,m_0,\text{使得对所有} n\ge n_0\text{或} m\ge m_0,\text{有}0\le c_1g(n,m)\le f(n,m)\le c_2g(n,m) \}$

## 3.2 标准记号与常用函数

1. 略
2. 证明 $a^{\log_b{c}}=c^{\log_b{a}}$

    $$
    \begin{aligned}
    \ln{a^{\log_b{c}}}&=\log_b{c}\ln{a}\\
    &=\dfrac{\ln{c}}{\ln{b}}\ln{a}\\
    &=\dfrac{\ln{a}}{\ln{b}}\ln{c}\\
    &=\ln{c^{\log_b{a}}}
    \end{aligned}
    $$

3. 证明

    由斯特林近似公式，得

    $$
    \begin{aligned}
          \lg{(n!)}&=\lg{(\sqrt{2\pi n}(\dfrac{n}{e})^n(1+\Theta(\dfrac{1}{n})))}\\
          &=\dfrac{1}{2}\lg{(2\pi n)}+n\lg(\dfrac{n}{e})+\lg(1+\Theta(\dfrac{1}{n}))
    \end{aligned}
    $$

    所以 $\lg(n!)=\Theta(n\lg n)$。

    对于 $n!=w(2^n)$，

    $$
    \begin{aligned}
    \lim_{n \to \infty} \frac{2^n}{n!}
    &= \lim_{n \to \infty} \frac{2^n}{\sqrt{2\pi n} \left(\frac{n}{e}\right)^n \left (1 + \Theta\left(\frac{1}{n}\right)\right)} \\
    &= \lim_{n \to \infty} \frac{1}{\sqrt{2\pi n} \left(1 + \Theta\left(\frac{1}{n} \right)\right)} \left(\frac{2e}{n}\right)^n \\
    &\le \lim_{n \to \infty} \left(\frac{2e}{n}\right)^n \\
    &\le \lim_{n \to \infty} \frac{1}{2^n} = 0  &(n\ge 4e)
    \end{aligned}
    $$

    对于 $n!=o(n^n)$，

    $$
    \begin{aligned}
    \lim_{n \to \infty} \frac{n^n}{n!}
    &= \lim_{n \to \infty} \frac{n^n}{\sqrt{2\pi n} \left(\frac{n}{e}\right)^n \left(1 + \Theta\left(\frac{1}{n}\right)\right)} \\
     &= \lim_{n \to \infty} \frac{e^n}{\sqrt{2\pi n} \left(1 + \Theta\left(\frac{1}{n}\right)\right)} \\
    &\ge \lim_{n \to \infty} \frac{e^n}{2\pi\sqrt n} = \infty
    \end{aligned}
    $$

4. 函数 $f(n)$ 多项式有界：存在正常数 $c,k,n_0$，使得当 $n\le n_0$ 时 $f(n)\le cn^k$。进一步地，$\lg(f(n))\le k\lg cn$，即$\lg(f(n))=O(\lg n)$。

    $\lceil \lg n \rceil!$ 不是多项式有界。

    $$
    \begin{aligned}
    \lg(\lceil \lg n \rceil!)
       &= \Theta(\lceil \lg n \rceil \lg \lceil \lg n \rceil) \\
       &= \Theta(\lg n\lg\lg n) \\
       &= \omega(\lg n)
    \end{aligned}
    $$

    $\lceil \lg\lg n \rceil!$ 是多项式有界。

    $$
    \begin{aligned}
    \lg(\lceil \lg\lg n \rceil!)
       &= \Theta(\lceil \lg\lg n \rceil \lg \lceil \lg\lg n \rceil) \\
       &= \Theta(\lg\lg n\lg\lg\lg n) \\
       &= o((\lg\lg n)^2) \\
       &= o(\lg n) \\
       &= O(\lg n)
    \end{aligned}
    $$


5. $\lg(\lg^*{n})\text{与}\lg^*{(\lg n)}$

    $\lg^*{(\lg n)}=\lg^*{n}-1$，所以 $\lg^*{(\lg n)}$ 渐进更大。

6. 证明

    $$
    \begin{aligned}
    \phi^2 &= \Big(\dfrac{1 + \sqrt 5}{2}\Big)^2 = \dfrac{6 + 2\sqrt 5}{4} = \dfrac{1 + \sqrt 5}{2} + 1 = \phi + 1\\
    \hat\phi^2 & = \Big(\dfrac{1 - \sqrt 5}{2}\Big)^2 = \dfrac{6 - 2\sqrt 5}{4} = \dfrac{1 - \sqrt 5}{2} + 1 = \hat\phi + 1
    \end{aligned}
    $$

7. 归纳法。基本情况有两个，$i=0,i=1$。
    当 $i=0$，$\dfrac{\phi^0 - \hat\phi^0}{\sqrt 5} = \dfrac{1 - 1}{\sqrt 5} = 0 = F_0$。

    当 $i=1$，$\dfrac{\phi^1 - \hat\phi^1}{\sqrt 5} = \dfrac{(1 + \sqrt 5) - (1 - \sqrt 5)}{2 \sqrt 5} = 1 = F_1$。

    假设 $F_{i-1},F_{i-2},i\ge 2$ 满足等式，则

    $$
    \begin{aligned}
       F_{i - 1} + F_{i - 2} &= \dfrac{\phi^{i - 1} - \hat\phi^{i-1}}{\sqrt{5}} + \dfrac{\phi^{i - 2} - \hat\phi^{i - 2}}{\sqrt{5}} \\
          &= \dfrac{\phi^{i - 2}+\phi^{i - 1} - (\hat\phi^{i - 2}+\hat\phi^{i - 1})}{\sqrt{5}}  \\
          &= \dfrac{\phi^{i - 2}(\phi + 1) - \hat\phi^{i - 2}(\hat\phi + 1)}{\sqrt{5}}  \\
          &= \dfrac{\phi^{i - 2}\phi^2 - \hat\phi^{i - 2}\hat\phi^2}{\sqrt{5}}& (x^2=x+1)\\
          &= \dfrac{\phi^i - \hat\phi^i}{\sqrt{5}}\\
          &= F_i
       \end{aligned}
    $$

8. 由对称性知 $n=\Theta(k\ln k)$。所以存在正常数 $c_1,c_2,n_1$，使得当 $n\ge n_1$ 有 $0\le c_1k\ln k\le n\le c_2k\ln k$。

    两边取对数得 $\ln n=\Theta(\ln(k\ln k))=\Theta(\ln k+\ln\ln k)=\Theta(\ln k)$。所以存在正常数 $c_3,c_4,n_2$，使得当 $n\ge n_2$ 有 $0\le c_3\ln k\le \ln n\le c_4\ln k$。

    取 $n_0=\max\{n_1,n_2\}$，由上述两个不等式得，$0\le \dfrac{c_1k\ln k}{c_4\ln k}\le\dfrac{n}{\ln n}\le \dfrac{c_2k\ln k}{c_3\ln k}$。所以 $\dfrac{n}{\ln n}=\Theta(k)$，由对称性知 $k=\Theta({\dfrac{n}{\ln n}})$。


## Problems

1.
2.
3.
4.
5.
6.