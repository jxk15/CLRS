---
title: 第 9 章 中位数和顺序统计量 Exercises & Problems
---

## Exercises

## 9.1 最小值和最大值

1. 类似于归并排序，自底向上得出最小值，由练习 B.5-3 知需要比较 $n-1$ 次。然后在所有输给最小值的元素中选出最小的，即为第二小的元素，因为第二小的元素只小于最小值，最多比较 $\lceil\lg n\rceil-1$ 次。

2. 若 $n$ 为奇数，先从三个数中得到当前最小值和当前最大值，最多比较 3 次。然后对剩下的元素先两两比较，然后与当前最小值和当前最大值比较。次数为 $3 + \dfrac{3(n-3)}{2}=\lceil\dfrac{3n}{2}\rceil-2$。

    若 $n$ 为偶数，先从两个数中得到当前最小值和当前最大值，比较 1 次。然后对剩下的元素先两两比较，然后与当前最小值和当前最大值比较。次数为 $1 + \dfrac{3(n-2)}{2}=\lceil\dfrac{3n}{2}\rceil-2$。

## 9.2 期望为线性时间的选择算法

1. 易知 $i$ 一定满足 $1\le i\le r-p+1$。若递归调用了含 0 个元素的子数组，则

    - $i<k$ 且 $p=q$，此时 $k=1$，矛盾。

    - $i>k$ 且 $q=r$，此时 $k=r-p+1$，矛盾。

2. 因为当前的 $\text{RANDOMIZED-PARTITION}$ 和后面的独立。

3. 尾递归改循环。伪代码如下：

    ```
    RANDOMIZED-SELECT(A, p, r, i)
        while true
        if p == r
            return A[p]
        q = RANDOMIZED-PARTITION(A, p, r)
        k = q - p + 1
        if i == k              // the pivot value is the answer
            return A[q]
        else if i < k
            r = q - 1
        else i = i - k
            p = q + 1
    ```

4. 每次选到的主元都是子数组中的最大值。

## 9.3 最坏情况为线性时间的选择算法

1. 分为每组 7 个元素，仍是线性的。分析方法同书中的。

    分为每组 3 个元素，大于等于 $x$ 的元素个数至少为 $2(\lceil\dfrac{1}{2}\lceil\dfrac{n}{3}\rceil\rceil-2)\ge \dfrac{n}{3}-4$。所以 $T(n)\le T(\lceil\dfrac{n}{3}\rceil) + T(\dfrac{2n}{3}+4)+O(n)$。由练习 4.4-9 知 $T(n)=\Theta(n\lg n)$。

2. 由 $\dfrac{3n}{10}-6\ge \lceil\dfrac{n}{4}\rceil$ 得 $n\ge 140$。

3. 先用 $\rm{SELECT}$ 选出中位数。

4. 由于仅通过比较来确定第 $i$ 小的元素，也就是仅通过比较来确定两个元素的大小关系。

    所以对另外的 $n-1$ 个元素中的任意一个，一定存在 1 个或多个不等式能够直接或间接证明该元素比第 $i$ 小的元素更大或更小。

    即：我们已经有了足够的信息来确定某数比第 $i$ 小的元素更小还是更大。

5. $T(n)=T(\dfrac{n}{2}) + O(n)$，所以 $T(n)=O(n)$。

6. 首先计算待求的 $k-1$ 个顺序统计量的下标，求出第 $\lfloor(k-1)/2\rfloor$ 个顺序统计量的值 $x$，然后使用 $x$ 把数组划分为比  $x$ 更大或更小的两部分。在比 $x$ 小的部分递归计算第 $1\sim\lfloor(k-1)/2\rfloor-1$ 个顺序统计量的值，在比 $x$ 大的部分递归计算 $\lfloor(k-1)/2\rfloor+1\sim k-1$ 顺序统计量的值。直到子数组中不含需要求的顺序统计量。

    显然递归层数为 $\Theta(\lg k)$，每层代价为 $O(n)$，所以时间复杂度为 $O(n\lg k)$。

    ??? "C 代码"

        运行结果截图：17个元素分为 4 份。

        ![](https://img.buxizhou.top/img/image-20230811181212054.png)

        ```c
        #include<stdio.h>
        #include<stdlib.h>
        #include<time.h>
        // 仅调用了其中的一个排序算法
        #include"./include/mySort.h"
        #define N (1000 + 5)
        int ans[N + 10];

        typedef struct {
            // 顺序统计量的值、返回时的下标
            // 因为要根据值来划分数组
            int index, value;
        } ansOfSelect;

        void swap(int *a, int *b)
        {
            int tmp = *a;
            *a = *b;
            *b = tmp;
        }

        int Partition(int A[], int p, int r)
        {
            int x = A[r];
            int i = p - 1;
            for (int j = p; j < r; j++)
            {
                if(A[j] <= x)
                {
                    i++;
                    swap(A + i, A + j);
                }
            }
            swap(A + r, A + i + 1);
            return i + 1;
        }

        int randomizedPartition(int A[], int p, int r)
        {
            int x = p + (rand() % (r - p + 1));
            swap(A + r, A + x);
            return Partition(A, p, r);
        }

        ansOfSelect randomizedSelect(int A[], int p, int r, int i)
        {
            ansOfSelect tmp;
            if(p == r) {
                tmp.index = p;
                tmp.value = A[p];
                return tmp;
            }
            int q = randomizedPartition(A, p, r);
            int k = q - p + 1;
            if (i == k) {
                tmp.index = q;
                tmp.value = A[q];
                return tmp;
            }
            else if(i < k)
                return randomizedSelect(A, p, q-1, i);
            else
                return randomizedSelect(A, q+1, r, i - k);
        }

        void kQuantitles(int A[], int L, int R, int B[], int p, int r)
        {
            if(R - L + 1 <= 0)
                return;
            int mid = (L + R) / 2;
            // 第 A[mid] 个顺序统计量
            ansOfSelect tmp = randomizedSelect(B, p, r, A[mid]);
            // 记录顺序统计量
            ans[mid] = tmp.value;
            // 根据值来划分数组
            swap(B + r, B + tmp.index);
            int q = Partition(B, p, r);

            int k = q - p + 1;
            for (int i = mid + 1; i <= R; i++)
                A[i] -= k;
            // 显然，递归层数为 O(lg k)
            kQuantitles(A, L, mid - 1, B, p, q - 1);
            kQuantitles(A, mid + 1, R, B, q + 1, r);
        }

        int main()
        {
            // n 个元素，分为 k 份
            int n = 17, k = 4;
            int A[N], B[N];
            // 计算顺序统计量的下标
            for (int i = 1; i < k; i++)
                A[i] = n / k;
            // 把多出来的分到前 k-1 份中，保证了大小相差不超过 1
            for (int i = 1; i <= n % k; i++)
                A[i]++;
            // 计算前缀和
            for (int i = 2; i < k; i++)
                A[i] += A[i - 1];
            // 查看下标计算是否正确
            for (int i = 1; i < k; i++)
                printf("%d ", A[i]);
            printf("\n");
            // 生成 n 个元素
            srand((unsigned)time(NULL));
            for (int i = 1; i <= n; i++)
                B[i] = rand() % 1000;
            // 打印出来看一下
            printf("n 个元素：");
            for (int i = 1; i <= n; i++)
                printf("%d ", B[i]);
            printf("\n");

            // 求 k-1 个顺序统计量
            kQuantitles(A, 1, k - 1, B, 1, n);

            // 排序
            insertionSortIncrease(B, 1, n+1);
            // 输出有序序列
            printf("排序后：");
            for (int i = 1; i <= n; i++)
                printf("%d ", B[i]);
            printf("\n");
            // 输出 k-1 个值，把有序集合分成 k 个等大小的集合（大小相差不超过 1）
            printf("k - 1 个数，把有序集合分成 k 个等大小的集合: ");
            for (int i = 1; i < k; i++)
                printf("%d ", ans[i]);
            printf("\n");

            return 0;
        }
        ```


7. 首先 $O(n)$ 求出中位数 $x$。然后把数组中的每个数减去 $x$ 并取绝对值，再求第 $k$ 个顺序统计量。

8. 加强版在线评测：https://leetcode.cn/problems/median-of-two-sorted-arrays/

    我们要求的中位数即第 $n$ 大的数。假设中位数在数组 $X$，那么对于 $X[k]$，当 $Y[n-k]\le X[k] \le Y[n-k+1]$ 时，$X[k]$ 是中位数，特别地，若 $k==n$，需要 $X[k]\le Y[1]$。若 $X[k]<Y[n-k]$ 则说明中位数下标大于 $k$；若 $X[k]>Y[n-k+1]$ 则说明中位数下标小于 $k$。我们可以使用二分来优化 $k$ 的查找。时间复杂度为 $O(\lg n)$。



    力扣上的题目为在两个长度不等的有序数组中找下中位数和上中位数。其实对下述代码稍加改动，就能求得第 $k$ 大的数（$1\le k\le n+m$），**注意边界问题**。

    ```c
    int findMedian(int A[], int B[], int n, int l, int r)
    {
        if(l > r)
            return -1;
        int k = (l + r) / 2;
        if(k == n && A[k] <= B[1])
            return A[k];
        else if(k < n && B[n-k] <= A[k] && A[k] <= B[n-k+1])
            return A[k];
        else if(A[k] > B[n-k+1])
            return findMedian(A, B, n, l, k - 1);
        else return findMedian(A, B, n, k+1, r);
    }

    int twoArrayMedian(int A[], int B[], int n)
    {
        int median = findMedian(A, B, n, 1, n);
        if(median == -1)
            median = findMedian(B, A, n, 1, n);
        return median;
    }
    ```

9. 由题意知，我们只需要确定管道的 $y$ 坐标。

    - 当 $n$ 为偶数时，管道的 $y$ 坐标与油井 $y$ 坐标的下中位数或上中位数相同，或它们之间的任意位置。
    - 当 $n$ 为奇数时，管道的 $y$ 坐标为油井 $y$ 坐标的中位数。



## Problems

1. 有序序列中的 $i$ 个最大数
2. 带权中位数
3. 小顺序统计量
4. 随机选择的另一种分析方法

