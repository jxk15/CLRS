---
title: 第 4 章 分治策略 Exercises & Problems
---



## Exercises

## 4-1 最大子数组问题

1. 返回数组中的最大值。

2. 暴力求解最大子数组问题。

    $\Theta(n^3)$：枚举所有子数组，$\Theta(n)$ 求每个子数组的和并尝试更新最大值。

    $\Theta(n^2)$：两种方法

       - 枚举所有的元素，求以被选中元素为左端点的子数组的和，并尝试更新最大值。由于左端点固定，可以使用 $A[i..j]+A[j+1]=A[i..j+1]$ 来优化求和。
       - 利用书中的股价模型，先 $\Theta(n)$ 遍历数组求前缀和，再 $\Theta(n^2)$ 枚举「买入」和「卖出」的时间，$O(1)$ 求差并尝试更新最大差值。

    === "Θ(n³)"

        ```cpp
        int MaxSubarray(int a[], int n)
        {
            // [0..n-1]
            int tmp, ans = 0x80000000;
            for (int i = 0; i < n; i++)       // 子数组起始位置
            {
                for (int j = i; j < n; j++)   // 子数组终止位置
                {
                    tmp = 0;
                    for (int k = i; k < j; k++)   // 子数组遍历求和
                        tmp += a[k];
                    if (tmp > ans)       // 更新最大和值
                        ans = tmp;
                }
            }
            return ans;
        }
        ```

    === "Θ(n²)"

        ```cpp
        int MaxSubarray(int a[],int n)
        {
            // [0..n-1]
            int tmp, ans = 0x80000000;
            for (int i = 0; i < n; i++)
            {
                tmp = 0;
                for (int j = i; j < n; j++)
                {
                    // 优化求和
                    tmp += a[j];
                    if (tmp > ans)
                        ans = tmp;
                }
            }
            return ans;
        }
        ```

3. 暴力与分治的性能交叉点。
4. 先 $\Theta(n)$ 扫描数组，若全为负数，返回空数组，和为 0；否则调用解决最大子数组问题的算法。
   或者，检查算法的返回值，若和为负数，则返回空数组。
5. DP 解决最大子数组问题。

    ??? 一个翻译错误

        算法导论 第三版 中文版

        题目的最后一句是「在已知 $A[1..j]$ 的最大子数组的情况下，可以在线性时间内找出形如 $A[i..j+1]$ 的最大子数组。」

        原文为：「Determine a maximum subarray of the form $A[i..j+1]$ in constant time based on knowing a maximum subarray ending at index $j$.」

        其意应为：「在已知以 $A[j]$ 结尾的最大子数组的情况下，可以在常数时间内找出形如 $A[i..j+1]$ 的最大子数组（即以 $A[j+1]$ 结尾的最大子数组）。」


    令 $f(j)$ 为数组 $A[1..j]$ 的最大子数组的和，$g(j)$ 为以 $A[j]$ 结尾的最大子数组的和。

    根据 $f(j+1)$ 是否含 $A[j+1]$，得 $f(j+1)=\max\{f(j),g(j+1)\}$。

    若已知 $g(j)$，则

    $$
        g(j+1) =
        \begin{cases}
        A[j+1] &\text{if\quad} g(j)\le 0 \\
        g(j)+A[j+1] &\text{if\quad} g(j) > 0
        \end{cases}
        \\	\Downarrow \\
        g(j+1) = \max\{A[j+1], A[j+1] + g(j)\}
    $$

    简证：设 $S(j)$ 为以 $A[j]$ 结尾的任意子数组的和

       - 当 $g(j)\le 0$，有 $A[j+1]+S(j)\le A[j+1]+g(j)\le A[j+1]$
       - 当 $g(j)>0$，有 $A[j+1]+S(j)\le A[j+1]+g(j)$

    显然，$f(1)=g(1)=A[1]$。

    ```c
    int MaxSubarray(int a[], int n)
    {
        // [0..n-1]
        int ans = a[0], tmp = a[0];
        for(int i = 1; i < n; i++)
        {
            tmp = max(a[i], a[i] + tmp);
            ans = max(tmp, ans);
        }
        return ans;
    }
    ```

## 4-2 矩阵乘法的 Strassen 算法

1. $S_1=6,S_2=4,S_3=12,S_4=-2,S_5=6,S_6=8,S_7=-2,S_8=6,S_9=-6,S_{10}=14$

    $P_1=6,P_2=8,P_3=72,P_4=-10,P_5=48,P_6=-12,P_7=-84$

    $C_{11}=18,C_{12}=14,C_{21}=62,C_{22}=66$

    结果为

    $$
    \begin{pmatrix}
        18 & 14 \\
        62 & 66
    \end{pmatrix}
    $$


2. 代码：适用于 $n$ 是 2 的幂。

    === "伪代码"
        ```
        Starssen(A, B)
            n = A.rows
            Let C be a new n * n matrix
            if n == 1
                return c11 = a11 * b11
            else
                partition A, B, C

                S1 = B12 - B22
                S2 = A11 + A12
                S3 = A21 + A22
                S4 = B21 - B11
                S5 = A11 + A22
                S6 = B11 + B22
                S7 = A12 - A22
                S8 = B21 + B22
                S9 = A11 - A21
                S10 = B11 + B12

                P1 = A11 * S1
                P2 = S2 * B22
                P3 = S3 * B11
                P4 = A22 * S4
                P5 = S5 * S6
                P6 = S7 * S8
                P7 = S9 * S10

                C11 = P5 + P4 - P2 + P6
                C12 = P1 + P2
                C21 = P3 + P4
                C22 = P5 + P1 - P3 - P7
            return C
        ```

    === "C 代码"

        以下代码在分解矩阵时使用了复制的方式。声明矩阵变量时需要使用指针，不能使用局部变量。因为该算法是递归算法，使用局部变量容易爆栈。

        如果不使用复制，需要在加减乘时指定参与运算的区间，即行和列的起始。

        ![](https://img.buxizhou.top/img/image-20230626131635133.png)

        ```C
        #include <stdio.h>
        #include <stdlib.h>
        #include <time.h>
        typedef struct matrix
        {
            int rows, cols;
            int **data;
        } matrix;

        // 申请空间, m->data
        void MatrixInit(matrix *m, int rows, int cols);
        // 释放空间, m->data
        void FreeMatrix(matrix *m);
        // 输入矩阵
        void InputMatrix(matrix *m);
        // 打印矩阵
        void PrintMatrix(matrix *m);
        // C = AB
        void NaiveMatrixMultiplication(matrix *A, matrix *B, matrix *C);
        // C = A + B
        void MatrixAddition(matrix *A, matrix *B, matrix *C);
        // C = A - B
        void MatrixSubtraction(matrix *A, matrix *B, matrix *C);
        // C = AB, 要求 n 为 2 的幂
        void Strassen(matrix *A, matrix *B, matrix *C);


        int main()
        {
            matrix *A, *B, *C, *D;
            A = (matrix *)malloc(sizeof(matrix));
            B = (matrix *)malloc(sizeof(matrix));
            C = (matrix *)malloc(sizeof(matrix));
            D = (matrix *)malloc(sizeof(matrix));
            int n = 2;
            MatrixInit(A, n, n);
            MatrixInit(B, n, n);
            MatrixInit(C, n, n);
            MatrixInit(D, n, n);

            InputMatrix(A);
            InputMatrix(B);

            // 书上的例子
            A->data[0][0] = 1;
            A->data[0][1] = 3;
            A->data[1][0] = 7;
            A->data[1][1] = 5;

            B->data[0][0] = 6;
            B->data[0][1] = 8;
            B->data[1][0] = 4;
            B->data[1][1] = 2;

            // Strassen & 朴素乘法
            Strassen(A, B, C);
            NaiveMatrixMultiplication(A, B, D);

            for (int i = 0; i < n; i++)
                for (int j = 0; j < n; j++)
                {
                    if(C->data[i][j] != D->data[i][j])
                    {
                        printf("错误\n");
                        // exit(0);
                    }
                }
            printf("算法正确\n");
            // 输出矩阵
            PrintMatrix(C);

            // 释放空间
            // 先释放 data，再释放 struct
            FreeMatrix(A);
            FreeMatrix(B);
            FreeMatrix(C);
            FreeMatrix(D);
            free(A);
            free(B);
            free(C);
            free(D);
            return 0;
        }

        // 申请空间
        void MatrixInit(matrix *m, int rows, int cols)
        {
            m->rows = rows;
            m->cols = cols;
            m->data = (int **)malloc(rows * sizeof(int *));
            for (int i = 0; i < rows; i++)
                m->data[i] = (int *)malloc(cols * sizeof(int));
        }
        // 释放空间
        void FreeMatrix(matrix *m)
        {
            // 先释放每一行的
            for (int i = 0; i < m->rows; i++)
                free(m->data[i]);
            free(m->data);
        }
        // 输入矩阵
        void InputMatrix(matrix *m)
        {
            srand((unsigned)time(NULL));
            for (int i = 0; i < m->rows; i++)
                for (int j = 0; j < m->cols; j++)
                    // scanf("%d", &(m->data[i][j]));
                    m->data[i][j] = rand() % 500 + 1;
        }
        // 打印矩阵
        void PrintMatrix(matrix *m)
        {
            for (int i = 0; i < m->rows; i++)
                for (int j = 0; j < m->cols; j++)
                {
                    printf("%d ", m->data[i][j]);
                    if (j == m->cols - 1)
                        printf("\n");
                }
        }

        // C = AB
        void NaiveMatrixMultiplication(matrix *A, matrix *B, matrix *C)
        {
            for (int i = 0; i < A->rows; i++)
                for (int j = 0; j < B->cols; j++)
                {
                    int sum = 0;
                    for (int k = 0; k < A->cols; k++)
                        sum += A->data[i][k] * B->data[k][j];
                    C->data[i][j] = sum;
                }
        }

        // C = A + B
        void MatrixAddition(matrix *A, matrix *B, matrix *C)
        {
            if ((A->rows != B->rows) || (A->cols != B->cols))
                exit(0);
            for (int i = 0; i < A->rows; i++)
                for (int j = 0; j < A->cols; j++)
                    C->data[i][j] = A->data[i][j] + B->data[i][j];
        }

        // C = A - B
        void MatrixSubtraction(matrix *A, matrix *B, matrix *C)
        {
            if ((A->rows != B->rows) || (A->cols != B->cols))
                exit(0);
            for (int i = 0; i < A->rows; i++)
                for (int j = 0; j < A->cols; j++)
                    C->data[i][j] = A->data[i][j] - B->data[i][j];
        }

        // 要求 n 为 2 的幂
        void Strassen(matrix *A, matrix *B, matrix *C)
        {
            if (A->rows == 1)
            {
                C->data[0][0] = A->data[0][0] * B->data[0][0];
                return;
            }

            // partition，使用复制矩阵的方法
            int n = A->rows / 2;
            matrix *subA[3][3], *subB[3][3], *subC[3][3];
            for (int i = 1; i < 3; i++)
                for (int j = 1; j < 3; j++)
                {
                    subA[i][j] = (matrix *)malloc(sizeof(matrix));
                    subB[i][j] = (matrix *)malloc(sizeof(matrix));
                    subC[i][j] = (matrix *)malloc(sizeof(matrix));
                    // 为 A11,A12,A21,A22 等的矩阵申请空间
                    MatrixInit(subA[i][j], n, n);
                    MatrixInit(subB[i][j], n, n);
                    MatrixInit(subC[i][j], n, n);
                }
            // 复制
            for (int i = 0; i < n; i++)
                for (int j = 0; j < n; j++)
                {
                    subA[1][1]->data[i][j] = A->data[i][j];
                    subA[1][2]->data[i][j] = A->data[i][j + n];
                    subA[2][1]->data[i][j] = A->data[i + n][j];
                    subA[2][2]->data[i][j] = A->data[i + n][j + n];

                    subB[1][1]->data[i][j] = B->data[i][j];
                    subB[1][2]->data[i][j] = B->data[i][j + n];
                    subB[2][1]->data[i][j] = B->data[i + n][j];
                    subB[2][2]->data[i][j] = B->data[i + n][j + n];
                }

            // S1 ~ S10，用于计算 P1 ~ P7
            // 计算 P_i 时，最多使用两个 S
            matrix *S1, *S2;
            S1 = (matrix *)malloc(sizeof(matrix));
            S2 = (matrix *)malloc(sizeof(matrix));
            MatrixInit(S1, n, n);
            MatrixInit(S2, n, n);
            // P1 ~ P7
            matrix *P[8];
            for (int i = 1; i < 8; i++)
            {
                P[i] = (matrix *)malloc(sizeof(matrix));
                MatrixInit(P[i], n, n);
            }

            // 计算 P1 ~ P7
            // P1 = A11 * S1, S1 = B12 - B22
            MatrixSubtraction(subB[1][2], subB[2][2], S1);
            Strassen(subA[1][1], S1, P[1]);
            // P2 = S2 * B22, S2 = A11 + A12
            MatrixAddition(subA[1][1], subA[1][2], S1);
            Strassen(S1, subB[2][2], P[2]);
            // P3 = S3 * B11, S3 = A21 + A22
            MatrixAddition(subA[2][1], subA[2][2], S1);
            Strassen(S1, subB[1][1], P[3]);
            // P4 = A22 * S4, S4 = B21 - B11
            MatrixSubtraction(subB[2][1], subB[1][1], S1);
            Strassen(subA[2][2], S1, P[4]);
            // P5 = S5 * S6, S5 = A11 + A22, S6 = B11 + B22
            MatrixAddition(subA[1][1], subA[2][2], S1);
            MatrixAddition(subB[1][1], subB[2][2], S2);
            Strassen(S1, S2, P[5]);
            // P6 = S7 * S8, S7 = A12 - A22, S8 = B21 + B22
            MatrixSubtraction(subA[1][2], subA[2][2], S1);
            MatrixAddition(subB[2][1], subB[2][2], S2);
            Strassen(S1, S2, P[6]);
            // P7 = S9 * S10, S9 = A11 - A21, S10 = B11 + B12
            MatrixSubtraction(subA[1][1], subA[2][1], S1);
            MatrixAddition(subB[1][1], subB[1][2], S2);
            Strassen(S1, S2, P[7]);

            // 重复利用 S1, S2 保存中间值
            // C11 = P5 + P4 - P2 + P6
            MatrixAddition(P[5], P[4], S1);
            MatrixSubtraction(S1, P[2], S2);
            MatrixAddition(S2, P[6], subC[1][1]);
            // C12 = P1 + P2
            MatrixAddition(P[1], P[2], subC[1][2]);
            // C21 = P3 + P4
            MatrixAddition(P[3], P[4], subC[2][1]);
            // C22 = P5 + P1 - P3 - P7
            MatrixAddition(P[5], P[1], S1);
            MatrixSubtraction(S1, P[3], S2);
            MatrixSubtraction(S2, P[7], subC[2][2]);

            // 合并
            for (int i = 0; i < n; i++)
                for (int j = 0; j < n; j++)
                {
                    C->data[i][j] = subC[1][1]->data[i][j];
                    C->data[i][j + n] = subC[1][2]->data[i][j];
                    C->data[i + n][j] = subC[2][1]->data[i][j];
                    C->data[i + n][j + n] = subC[2][2]->data[i][j];
                }

            // 释放空间，此处代码省略
            // matrix *subA[3][3], *subB[3][3], *subC[3][3];
            // matrix *S1, *S2;
            // matrix *P[8];
        }
        ```

3. 当 n 不是 2 的幂

    === "法一"
        把 $n$ 扩展为 2 的幂，多出的部分用 0 填充。扩展后的矩阵规模不超过 $2n\times 2n$，运行时间仍为 $\Theta(n^{\lg 7})$。

    === "法二"
        链接：https://blog.csdn.net/yangtzhou/article/details/105042627


4. 由题意知 $T(n)=kT(n/3) + \Theta(n^2)$。
    - $k=9$：此时 $\log_3{k}=2$，应用主定理情况二，得 $T(n)=\Theta(n^2\lg n)=o(n^{\lg 7})$。
    - $k<9$：此时 $\log_3{k}<2$，（存在正常数 $\dfrac{8}{9} < c < 1$ 使得所有足够大的 $n$ 有 $kf(\dfrac{n}{3})\le cf(n)$）应用主定理情况三，得 $T(n)=\Theta(n^2)=o(n^{\lg 7})$。
    - $k>9$：此时 $\log_3{k}>2$，应用主定理情况一，得 $T(n)=\Theta(n^{\log_3{k}})$。若 $\Theta(n^{\log_3{k}})=o(n^{\lg 7})$，则 $\log_3{k}<\lg 7$，得 $k<3^{\log_2{7}}\approx 21.85$，$k$ 的最大值为 21。
    - 综上，$k$ 的最大值为 21。

5. 递归式分别为 $T(n)=132464T(n/68)+\Theta(n^2)$，$T(n)=143640T(n/70)+\Theta(n^2)$，$T(n)=155424T(n/72)+\Theta(n^2)$。

    时间复杂度分别为 $\Theta(n^{\log_{68}{132464}}),\Theta(n^{\log_{70}{143640}}),\Theta(n^{\log_{72}{155424}})$。

    $$
    \begin{aligned}
        \log_{68}{132464} &\approx 2.795128\\
        \log_{70}{143640} &\approx 2.795122\\
        \log_{72}{155424} &\approx 2.795147\\
        \end{aligned}
    $$

    所以 143640 次乘法操作完成 $70\times 70$ 矩阵相乘的方法会得到最佳的渐进运行时间。



6. $kn\times n$ 矩阵和 $n\times kn$ 矩阵可表示为

    $$
        A=
        \begin{pmatrix}
        A_1\\A_2\\ \vdots\\A_k
        \end{pmatrix}
        ,
        B=
        \begin{pmatrix}
        B_1 & B_2 & \cdots B_k
        \end{pmatrix}
    $$

    其中，$A_i,B_i$，$1\le i\le k$ 均为 $n\times n$ 矩阵。

    则 $AB$ 需要 $k^2$ 次 $n\times n$ 矩阵相乘，$BA$ 需要 $k$ 次 $n\times n$ 矩阵相乘。

    所以 $AB$ 的运行时间为 $\Theta(k^2n^{\lg 7})$，$BA$ 的运行时间为 $\Theta(kn^{\lg 7})$。

7. 三次乘法

    $$
    \begin{aligned}
    A &= ad\\
    B &= bc\\
    C &= (a+b)(c-d) = ac - ad + bc - bd
    \end{aligned}
    $$

    实部为 $C+A-B$，虚部为 $A+B$。

## 4-3 代入法

> 注意对向上取整、向下取整的处理
>
> 对于 $\lg(n-a)=O(\lg n)$，$(n+a)\lg(n+a)=O(n\lg n)$ 等的证明省略。
>
> $0\le \lg(n-a)\le \lg n$
>
> $0\le (n+a)\lg(n+a)\le 4n\lg n = 2n\lg n^2$，当 $2n\ge n+a \text{ 且 } n^2 \ge n + a$

1. 假设 $T(n)\le cn^2$，

    $$
    \begin{aligned}
    T(n) &\le c(n - 1)^2 + n \\
        &=   cn^2 - 2cn + c + n \\
        &=   cn^2 + n(1 - 2c) + c \\
        &\le cn^2                       & (c > \dfrac{1}{2})
    \end{aligned}
    $$

2. 假设 $T(n)\le c\lg{(n-a)}$，

    $$
    \begin{aligned}
    T(n) &\le c\lg(\left\lceil \dfrac{n}{2} \right\rceil - a) + 1 \\
        &\le c\lg(\dfrac{n+1}{2} - a) + 1 \\
        &= c\lg(\dfrac{n+1+2a}{2}) + 1 \\
        &= c\lg(n + 1 - 2a) - c + 1    \\
        &\le c\lg(n + 1 - 2a)             & (c \ge 1) \\
        &\le c\lg(n - a)                  & (a \ge 1) \\
    \end{aligned}
    $$

3. 假设 $T(n)\le c(n+a)\lg{(n+a)}$，

    $$
    \begin{aligned}
    T(n) &\ge 2c(\left\lfloor \dfrac{n}{2} \right\rfloor + a)\lg(\left\lfloor \dfrac{n}{2}\right\rfloor + a) + n \\
        &= 2c\frac{n - 1 + 2a}{2}\lg\frac{n - 1 + 2a}{2} + n \\
        &= c(n - 1 + 2a)\lg(n - 1 + 2a) - c(n - 1 + 2a) + n \\
        &= c(n - 1 + 2a)\lg(n - 1 + 2a) + (1 - c)n + (1 - 2a)c  \\
        &\ge c(n - 1 + 2a)\lg(n - 1 + 2a)  & (0 \le c < 1, n \ge \frac{(2a - 1)c}{1 - c})\\
        &\ge c(n + a)\lg(n + a) & (a \ge 1)
    \end{aligned}
    $$

4. 假设 $T(n)\le cn\lg{n}+n$，

    $$
    \begin{aligned}
    T(n)&\le 2(c\left\lfloor \dfrac{n}{2}\right\rfloor\lg\left\lfloor \dfrac{n}{2}\right\rfloor+\left\lfloor \dfrac{n}{2}\right\rfloor)+n           \\
        &\le 2(c\dfrac{n}{2}\lg\dfrac{n}{2}+\dfrac{n}{2})+n           \\
        &= cn\lg{\dfrac{n}{2}}+2n    \\
        &= cn\lg{n}-cn\lg{2}+2n      \\
        &= cn\lg{n}+(2-c)n           \\
        &\le cn\lg{n}+n      & (c \ge 1)
    \end{aligned}
    $$

    对于边界条件 $T(1)$，$T(1)=1\le cn\lg{n}+n=0+1=1$。

5. 归并排序的严格递归式

    - 先证 $T(n)=O(n\lg n)$。假设 $T(n)\le c(n-a)\lg(n-a)$，$a>0$

        $$
        \begin{aligned}
            T(n)
                &\le c(\left\lceil \dfrac{n}{2} \right\rceil - a)\lg{(\left\lceil \frac{n}{2} \right\rceil - a)} + c(\left\lfloor \dfrac{n}{2}\right\rfloor - a)\lg{(\left\lfloor \dfrac{n}{2}\right\rfloor - a)} + dn   \\
                &\le c(\dfrac{n+1}{2} - a)\lg{(\frac{n+1}{2} - a)} + c(\dfrac{n}{2} - a)\lg{( \dfrac{n}{2} - a)} + dn   \\
                &= c(\dfrac{n+1-2a}{2})\lg{(\frac{n+1-2a}{2})} + c(\dfrac{n-2a}{2})\lg{( \dfrac{n-2a}{2})} + dn   \\
                &\le c(\dfrac{n+1-2a}{2})\lg{(\frac{n+1-2a}{2})} + c(\dfrac{n-a}{2})\lg{( \dfrac{n-a}{2})} + dn \\  &(\text{为了合并前两项，令 }a=1)\\
                &= c({n-1})\lg{({n-1})} - c(n-1)\lg 2 + dn \\  &(\text{为了消去 }c,\text{在假设后面加上} -c)\\
                &\le c({n-1})\lg{({n-1})}-c   &(c\ge d)\\
        \end{aligned}
        $$

        经过推导，对刚开始的假设进行补充，得到 $T(n)\le c(n-1)\lg(n-1)-{c}$。由此假设，得 $T(n)=O(n\lg n)$。

    - 再证 $T(n)=\Omega(n\lg n)$，假设 $T(n)\ge c(n+1)\lg(n+1)+{c}$

        $$
        \begin{aligned}
            T(n)
                &\ge c(\left\lceil \dfrac{n}{2} \right\rceil + 1)\lg{(\left\lceil \frac{n}{2} \right\rceil + 1)} + c(\left\lfloor \dfrac{n}{2}\right\rfloor + 1)\lg{(\left\lfloor \dfrac{n}{2}\right\rfloor + 1)} + dn + 2c   \\
                &\ge c(\dfrac{n+2}{2})\lg{(\frac{n+2}{2})} + c(\dfrac{n+1}{2})\lg{( \dfrac{n+1}{2})} + dn + 2c  \\
                &\ge c(\dfrac{n+1}{2})\lg{(\frac{n+1}{2})} + c(\dfrac{n+1}{2})\lg{( \dfrac{n+1}{2})} + dn + 2c  \\
                &= c({n+1})\lg{({n+1})} - c(n+1) + dn + 2c  \\
                &\ge c({n+1})\lg{({n+1})} + c   &(c\le d)\\
        \end{aligned}
        $$

        得 $T(n)=\Omega(n\lg n)$。

6. 假设 $T(n)\le c(n+a)\lg{(n+a)}$，

    $$
    \begin{aligned}
            T(n)
            &\le 2c(\left\lfloor \dfrac{n}{2} \right\rfloor + 17 + a)\lg(\left\lfloor \dfrac{n}{2} \right\rfloor + 17 + a) + n \\
            &\le 2c(\dfrac{n}{2} + 17 + a)\lg(\dfrac{n}{2} + 17 + a) + n \\
            &= c(n+34+2a)\lg(\dfrac{n}{2} + 17 + a) + n \\
            &= c(n+34+2a)\lg(n+34+2a) - c(n+34+2a) + n \\
            &= c(n+34+2a)\lg(n+34+2a) + (1-c)n - c(34+2a)  &(c>1,a\le -34, n\ge \dfrac{c(34+2a)}{1-c}) \\
            &\le c(n+a)\lg(n+a)  \\
        \end{aligned}
    $$

7. 证明 $T(n)=\Theta(n^{\log_3 4})$

    - 上界。假设 $T(n)\le cn^{\log_3{4}}$，则 $T(n)\le 4c\left(\dfrac{n}{3}\right)^{\log_3{4}}+n=cn^{\log_3{4}}+n$，归纳失败。

        减去一个低阶项，假设 $T(n)\le cn^{\log_3{4}}-dn$，则 $T(n)\le 4\left(c\left(\dfrac{n}{3}\right)^{\log_34}-d\dfrac{n}{3}\right)+n=cn^{\log_34}-\left(\dfrac{4d}{3}-1\right)n\le cn^{\log_34}-dn$。最后一步在 $d\ge 3$ 时成立。于是 $T(n)=O(n^{\log_34})$。

    - 下界。假设 $T(n)\ge cn^{\log_3{4}}$，则 $T(n)\ge 4c\left(\dfrac{n}{3}\right)^{\log_34}+n=cn^{\log_34}+n\ge cn^{\log_34}$。于是 $T(n)=\Omega(n^{\log_34})$。

8. 证明 $T(n)=\Theta(n^2)$

    - 上界。假设 $T(n)\le cn^2$，则 $T(n)\le c{n^2}+n$，归纳失败。

        减去一个低阶项，假设 $T(n)\le cn^{2}-dn$，则 $T(n)\le 4(c\dfrac{n^2}{4}-d\dfrac{n}{2})+n=cn^{2}+(1-2d)n\le cn^{2}-dn$。最后一步在 $d\ge \dfrac{1}{2}$ 时成立。于是 $T(n)=O(n^{2})$。

    - 下界。假设 $T(n)\ge cn^{2}$，则 $T(n)\ge cn^2+n\ge cn^2$。于是 $T(n)=\Omega(n^{2})$。

9. 令 $m=\log{n}$，则 $T(2^m)=3T(2^{m/2})+m$。

    令 $S(m)=T(2^m)$，则 $S(m)=3S(m/2)+m$。

    假设 $S(m)\le cm^{\lg{3}}+dm$，

    假设 $S(m)\ge cm^{\lg{3}}$，

    综上，$S(m)=\Theta(m^{\lg{3}})$，$T(n)=T(2^m)=S(m)=\Theta(m^{\lg{3}})=\Theta(\lg^{\lg{3}}{3})$。


## 4-4 递归树法

1. 1
2. 2
3. 3
4. 4
5. 5
6. 6
7. 7
8. 8
9. 9



## 4-5 主定理

1. 应用主定理

    1. case 1，得 $T(n)=\Theta(n^{0.5})$。
    2. case 2，得 $T(n)=\Theta(n^{0.5}\lg n)$。
    3. case 3，$\dfrac{1}{2} \le c < 1$，得 $T(n)=\Theta(n)$。
    4. case 3，$\dfrac{1}{8} \le c < 1$，得 $T(n)=\Theta(n^2)$。

2. $T(n)=aT(n/4) + \Theta(n^2)$，求 $a$ 的最大值。

    - $a=16$：此时 $\log_4{a}=2$，应用主定理情况二，得 $T(n)=\Theta(n^2\lg n)=o(n^{\lg 7})$。
    - $a<16$：此时 $\log_4{a}<2$，（存在正常数 $\dfrac{a}{16} < c < 1$ 使得所有足够大的 $n$ 有 $af(\dfrac{n}{4})\le cf(n)$）应用主定理情况三，得 $T(n)=\Theta(n^2)=o(n^{\lg 7})$。
    - $a>16$：此时 $\log_4{a}>2$，应用主定理情况一，得 $T(n)=\Theta(n^{\log_4{a}})$。若 $\Theta(n^{\log_4{a}})=o(n^{\lg 7})$，则 $\log_4{a}<\lg 7$，得 $a<49$，$a$ 的最大值为 48。
    - 综上，$a$ 的最大值为 48。

3. case 2

4. 不能，该递归式不满足主定理的任一情况。

    尝试代入法，假设 $T(n)\le cn^2\lg^2 n$，

    $$
    \begin{aligned}
    T(n)&\le 4c\dfrac{n^2}{4}\lg^2 \dfrac{n}{2} + n^2\lg n \\
        &= cn^2(\lg^2 n - 2\lg n + 1) + n^2\lg n           \\
        &= cn^2\lg^2 n - 2cn^2\lg n +cn^2 + n^2\lg n       \\
        &\le cn^2\lg^2 n - 2cn^2\lg n +cn^2\lg n + n^2\lg n      \\
        &= cn^2\lg^2 n + n^2\lg n(c + 1 - 2c)              \\
        &\le cn^2\lg^2 n             &(c\ge 1)
    \end{aligned}
    $$

5. 构造分段函数，令 $f(n)$ 和 $f(\dfrac{n}{b})$ 具有不同的复杂度。

    例：$T(n)=2T(n/2) + f(n)$，其中

    $$
    f(n) =
        \begin{cases}
            n^3 &\text{if } n\ne 7^i \\
            n^2 &\text{if } n= 7^i
        \end{cases}
    $$

    显然满足 $f(n)=\Omega(n^{\log_b{a}+\varepsilon})$。当 $n=7^i$，

    $$
    \begin{aligned}
    2f(\dfrac{n}{2}) &\le cf(n) \\
    \dfrac{n^3}{4} &\le cn^2
    \end{aligned}
    $$

    显然，不满足正则条件。


## 4-6 主定理的证明

1. 简化的表达式：$n_j=\left\lceil \dfrac{n}{b^j} \right\rceil,j\ge 0$。

    当 $n=0$：显然 $n_0=\left\lceil \dfrac{n}{b^0} \right\rceil$。

    若对 $n=j-1,j\ge 1$ 成立，则 $n_{j}=\left\lceil \dfrac{n_{j-1}}{b} \right\rceil=\left\lceil \dfrac{\left\lceil \dfrac{n}{b^{j-1}} \right\rceil}{b} \right\rceil=\left\lceil \dfrac{n}{b^{j}} \right\rceil$。最后一步用到了第三章中的等式 3.4。

2. 由 $f(n)=\Theta(n^{\log_b{a}}\lg^k n)$，得

    $$
    \begin{aligned}
    g(n)
    &= \Theta\Big(\sum_{j = 0}^{\log_b{n - 1}}a^j\big(\frac{n}{b^j}\big)^{\log_b a}\lg^k\frac{n}{b^j}\Big) \\
    &= \Theta\Big( n^{\log_b a} \sum_{j = 0}^{\log_b{n - 1}}\Big(\frac{a}{b^{\log_b a}}\Big)^j\lg^k\frac{n}{b^j}             \Big) \\
    &= \Theta\Big( n^{\log_b a}\sum_{j = 0}^{\log_b{n - 1}}\lg^k\frac{n}{b^j} \Big) \\
    &= \Theta\Big( n^{\log_b a}\sum_{j = 0}^{\log_b{n - 1}}(\lg{n}-\lg b^j)^k \Big) \\
    &= \Theta\Big( n^{\log_b a}\sum_{j = 0}^{\log_b{n - 1}}\Theta(\lg^k{n}) \Big) &(\text{第三章 exercises 3.1-2})\\
    &= \Theta(n^{\log_b a}\lg^{k + 1}{n})
    \end{aligned}
    $$

3. 假设对 $n\ge n0$ 和某个常数 $c<1$，$af(\dfrac{n}{b})\le cf(n)$ 成立。经过迭代，有 $(\dfrac{a}{c})^jf(\dfrac{n}{b^{j}})\le f(n)$。当 $j=\lfloor\log_{b}{\dfrac{n}{n_0}}\rfloor$ 时，

    $$
    \begin{aligned}
    \left( \dfrac{a}{c}\right)^{\log_{b}{\frac{n}{n_0}}-1}f(\dfrac{n}{b^{\log_{b}{\frac{n}{n_0}}}})&\le f(n)\\
    \left(\dfrac{a}{c}\right)^{\log_{b}{\frac{n}{n_0}}-1}f(n_0)&\le f(n)\\
    \left(\dfrac{a}{c}\right)^{-1}\left(\frac{n}{n_0}\right)^{\log_{b}{\frac{a}{c}}}f(n_0)&\le f(n)\\
    \left(\dfrac{a}{c}\right)^{-1}\left(\frac{1}{n_0}\right)^{\log_{b}{\frac{a}{c}}}n^{\log_{b}{\frac{a}{c}}}f(n_0)&\le f(n)\\
    \left(\dfrac{a}{c}\right)^{-1}\left(\frac{1}{n_0}\right)^{\log_{b}{\frac{a}{c}}}n^{\log_{b}{a}-\log_{b}{c}}f(n_0)&\le f(n)\\
    \left(\dfrac{a}{c}\right)^{-1}\left(\frac{1}{n_0}\right)^{\log_{b}{\frac{a}{c}}}n^{\log_{b}{a}+\varepsilon}f(n_0)&\le f(n)\\
    \end{aligned}
    $$

    由于 $b>1$ 且 $c<1$，所以 $\log_{b}{c}$ 是负数。

    由于 $a,b,c,n_0$ 是常数，所以 $f(n)=\Omega(n^{\log_{b}{a}+\varepsilon})$。

## Problems

1. 前 6 题都可以使用主定理。

    g. $T(n)=\Theta(n^3)$。

        - n 为偶数时，$T(n)=O(1)+\displaystyle\sum_{j=0}^{n/2-1}n-2j=\Theta(n^3)$
        - n 为奇数时，$T(n)=O(1)+\displaystyle\sum_{j=0}^{(n-1)/2}n-2j=\Theta(n^3)$

2. 参数传递的代价

    1. 递归二分查找

        - 指针：$T(n)=T(n/2)+\Theta(1)$：由主定理得 $T(n)=\Theta(\lg n)$
        - 复制所有元素：$T(n)=T(n/2)+\Theta(N)$：画出递归树得 $T(n)=\Theta(n\lg n)$
        - 复制部分元素：$T(n)=T(n/2)+\Theta(n)$：由主定理得 $T(n)=\Theta(n)$

    2. 归并排序

        - $T(n)=2T(n/2)+\Theta(n)$：由主定理得 $T(n)=\Theta(n\lg n)$
        - $T(n)=2T(n/2)+\Theta(n)+\Theta(N)$：画出递归树得 $T(n)=\Theta(n^2)$
        - $T(n)=2T(n/2)+\Theta(n)$：由主定理得 $T(n)=\Theta(n\lg n)$

    > $\Theta(N)$：$N=n*2^i,0\le i\le \lg N-1$，所以 $N$ 不能写成 $\Theta(n)$。

3. 更多的递归式例子

    1. 由主定理得 $T(n)=\Theta(n^{\log_3{4}})$。

    2. 代入法

    3. 由主定理得 $T(n)=\Theta(n^{2.5})$。

    4. 代入法

    5. 代入法

    6. $T(n)=\Theta(n)$。

       - 假设 $T(n)\le cn$，得

         $$
         \begin{aligned}
         T(n)
         &\le c\dfrac{n}{2}+c\dfrac{n}{4}+c\dfrac{n}{8}+n\\
         &\le (\dfrac{7c}{8}+1)n\\
         &\le cn              &(c\ge 8)
         \end{aligned}
         $$

       - 假设 $T(n)\ge cn$，得

         $$
         \begin{aligned}
         T(n)
         &\ge c\dfrac{n}{2}+c\dfrac{n}{4}+c\dfrac{n}{8}+n  \\
         &\ge (\dfrac{7c}{8}+1)n\\
         &\ge cn              &(c\le 8)
         \end{aligned}
         $$

    7.  递归树

    8.  递归树

    9.  递归树

    10. 递归树。$T(n)=\Theta(n\lg\lg n)$。


    ![image-20230701232331934](https://img.buxizhou.top/img/image-20230701232331934.png)

4. 本题讨论递归式（3.22）定义的斐波那契数的性质。

    1. 证明：$\mathcal F(Z)=z + z\mathcal F(z) + z^2\mathcal F(Z)$

        $$
        \begin{aligned}
        z + z\mathcal F(z) + z^2\mathcal F(Z)
            &= z + \sum_{i = 1}^{\infty} F_{i - 1}z^i + \sum_{i = 2}^{\infty}F_{i - 2} z^i \\
            &= z + F_0z + \sum_{i = 2}^{\infty}(F_{i - 1} + F_{i - 2})z^i \\
            &= z + F_0z + \sum_{i = 2}^{\infty}F_iz^i \\
            & = \mathcal F(z)
        \end{aligned}
        $$

    2. 已知 $\phi - \hat\phi = \sqrt 5$, $\phi + \hat\phi = 1,\phi\hat\phi = - 1$。

        $$
        \begin{aligned}
        \mathcal F(Z) &= z + z\mathcal F(z) + z^2\mathcal F(Z) \\
        -\mathcal F(Z) &= -z - z\mathcal F(z) - z^2\mathcal F(Z) \\
        z &= \mathcal F(z) - z\mathcal F(z) - z^2\mathcal F(Z) \\
        z &= \mathcal F(z) (1- z - z^2) \\
        \dfrac{z}{1 - z - z^2} &= \mathcal F(z) \\
        \dfrac{z}{1 - (\phi + \hat\phi)z + (\phi\hat\phi)z^2} &= \mathcal F(z) \\
        \dfrac{z}{(1 - \phi z)(1 - \hat\phi z)} &= \mathcal F(z) \\
        \dfrac{z(\phi - \hat\phi)}{\sqrt 5(1 - \phi z)(1 - \hat\phi z)} &= \mathcal F(z) \\
        \dfrac{1}{\sqrt 5}\Big(\dfrac{1}{1-\phi z} - \dfrac{1}{1-\hat\phi z}\Big) &= \mathcal F(z) \\
        \end{aligned}
        $$

    3. 已知当 $|x|<1$ 时，有 $\displaystyle\sum_{i=0}^{\infty}x^i=\dfrac{1}{1-x}$

        $$
        \begin{aligned}
        \mathcal F(n)
        &= \frac{1}{\sqrt 5}\Big(\frac{1}{1 - \phi z} - \frac{1}{1 - \hat\phi z}\Big) \\
        &= \frac{1}{\sqrt 5}\Big(\sum_{i = 0}^{\infty}\phi^i z^i - \sum_{i = 0}^{\infty}\hat{\phi}^i z^i\Big) \\
        &= \sum_{i = 0}^{\infty}\frac{1}{\sqrt 5}(\phi^i - \hat{\phi}^i) z^i.
        \end{aligned}
        $$

    4. 由第 3 小题知 $F_i = \dfrac{\phi^i - \hat{\phi}^i}{\sqrt 5}=\dfrac{\phi^i}{\sqrt 5}-\dfrac{\hat{\phi}^i}{\sqrt 5}$。由 $\hat{\phi}=-0.61803\dots$ 知 $\Big|\dfrac{\hat{\phi}^i}{\sqrt 5}\Big|<0.2367$，所以对 $\dfrac{\phi^i}{\sqrt 5}$ 就近舍入即可得到 $F_i$。

5. 5

6. 6