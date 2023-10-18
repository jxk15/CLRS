---
title: 第 2 章 算法基础 Exercises & Problems
---

## Exercises

## 2.1 插入排序

1. 模拟插入排序

    <img src="https://img.buxizhou.top/img/QQImg20230617160646.png" style="zoom:50%;" />


2. 非升序排序

    ``` C
    // sort for [l, r)
    void insertionSortDecrease(int a[], int l, int r)
    {
        for (int i = l; i < r; i++)
        {
            int key = a[i];
            int j = i - 1;
            while (j >= l && a[j] < key) // a[j] < key: 与非降序排序的唯一区别
            {
                a[j + 1] = a[j];
                j--;
            }
            a[j + 1] = key;
        }
    }
    ```

3. 线性查找

    ```C
    int find(int v, int a[], int n)
    {
        int idx = NIL;
        for(int i = 1; i <= n; i++)
            if(a[i] == v)
            {
                return i;
            }
        return idx;
    }
    ```

    **循环不变量**：每次迭代前，[1, i) 中不存在 v。
    **初始**：i = 1，[1, i) 为空集，循环不变量成立。
    **保持**：若 a[i] = v，返回正确的下标。若不相等，i 递增 1，循环不变量成立。
    **终止**：i = n+1，[1, n+1) 中不存在 v，返回 NIL。

4. 二进制大整数加法

    ```C
    void add(int a[], int b[], int n, int ans[])
    {
        int carry = 0;
        for(int i = 1; i <= n; i++)
        {
            int sum = a[i] + b[i] + carry;
            ans[i] = sum % 2;
            carry = sum / 2;
        }
        ans[n + 1] = carry;
    }
    ```

    **循环不变量**：每次迭代前，ans 中 [1, i-1) 为 a 和 b 的前 i-1 位相加的和的低 i-1 位，carry 为 a 和 b 的前 i-1 位相加的和的第 i 位。

    **初始**：i=1，，[1, i-1) 为空集，carry=0，循环不变量成立。

    **保持**：在每次迭代中，令 sum 的值为 a[i]+b[i]+carry，sum%2 为 ans[i] 的值，sum/2 为 carry。此时 ans 中 [1, i] 为 a 和 b 的前 i 位相加的和的低 i 位，carry 为 a 和 b 的前 i 位相加的和的第 i+1 位。然后 i 递增 1，循环不变量成立。
    **终止**：i = n+1，ans 中 [1, n] 为 a 和 b 的前 n 位相加的和的低 n 位，carry 为 a 和 b 的前 n 位相加的和的第 n+1 位。。

    $$
    \begin{aligned}
    A[1,i] + B[1,i]&=A[1,i-1] + B[1,i-1] + A[i]\times 2^{i-1}+B[i]\times 2^{i-1}\\
    &=C[1,i-1] + carry\times 2^{i-1} + A[i]\times 2^{i-1}+B[i]\times 2^{i-1}\\
    &=C[1,i-1] + 2^{i-1}\times (carry+A[i]+B[i])\\
    &=C[1,i-1] + 2^{i-1}\times (sum/2 \times 2 + sum \bmod 2)\\
    &=C[1,i-1] + 2^{i-1}\times sum \bmod 2 + 2^i \times sum/2
    \end{aligned}
    $$


<!--

<iframe src="https://img.buxizhou.top/code/binary-addition.html" frameborder="no" marginwidth="0" width="100%" height="300px" marginheight="0" scrolling="auto"></iframe>

-->

## 2.2 分析算法

1. $\Theta(n^3)$

2. 选择排序

    ```c
    void selectionSort(int a[], int l, int r)
    {
        for(int i = l; i < r; i++)
        {
            int min = i;
            for(int j = i; j <= r; j++)
                if(a[j] < a[min])
                    min = j;
            swap(a[min], a[i]);
        }
    }
    ```

    **循环不变量**：每次迭代前，数组中 [l, i) 已按非降序排序，[i, r] 中的任意元素大于等于 [l, i) 的最大值 a[i-1]。

    **初始**：i = l，循环不变量成立。

    **保持**：在每次循环中，从 [i, r] 中选出最小值，与 a[i] 交换，然后 i 递增 1。因为 a[i-1] 大于等于 a[i-2]，所以 [l, i) 保持有序。因为 a[i-1] 为 [i-1, r] 中的最小值，所以 [i, r] 中的任意元素大于等于 [l, i) 的最大值 a[i-1]。

    **终止**：i = r，此时数组中 [l, r) 已按非降序排序，a[r] 大于 [l, r) 的最大值 a[r-1]，故整个数组已有序。

    最好情况和最坏情况的运行时间均为 $\Theta(n^2)$。

    设 if 语句块的运行时间为 $a_1$，swap 语句的运行时间为 $a_2$，则对任意输入，运行时间为 $\displaystyle\sum_{i=l}^{r-1}a_1(r-i+1)+\sum_{i=l}^{r-1}a_2=\Theta(n^2)$。

3. 线性查找

    由题意知：$P\{A_i=v\}=\dfrac{1}{n}$，设检查次数为 $X$，则 $EX=\displaystyle\sum_{i=1}^{n}\dfrac{i}{n}=\dfrac{n+1}{2}$。

    所以平均情况下，需要检查 $\dfrac{n+1}{2}$ 个。最坏情况，即只有 A[n]=v 或数组中不含 v，需要检查 n 个。

    所以最好情况和最坏情况下均为 $\Theta(n)$。

4. 特判：若输入具有某些特征，则直接返回结果（打表）或执行一个时间复杂度更低的算法。

## 2.3 设计算法

1. 模拟归并排序

    <img src="https://img.buxizhou.top/img/QQImg20230617162345.png" style="zoom:50%;" />

2. 重写 Merge

    ```c
    void Merge(int a[], int p, int q, int r)
    {
        // merge [p, q] [q+1, r]
        int *tmp = (int *)malloc((r - p + 1) * sizeof(int));
        int i = p, j = q + 1, k = 0;
        while (i <= q && j <= r)
        {
            if(a[i] <= a[j]) {
                tmp[k] = a[i];
                i++;
            }
            else {
                tmp[k] = a[j];
                j++;
            }
            k++;
        }
        while(i <= q) {
            tmp[k] = a[i];
            k++, i++;
        }
        while(j <= r) {
            tmp[k] = a[j];
            k++, j++;
        }
        for (int i = 0, j = p; i < (r - p + 1); i++, j++)
            a[j] = tmp[i];
        free(tmp);
    }
    ```

3. 递归式求解

    当 $n=2$ 时，$T(n)=2$，$n\lg n = 2$，基本情况成立。

    当 $n=2^i$ 成立，对于 $n=2^{i+1}$，有 $T(2^{i+1})=2T(2^i)+2^{i+1}=2\times 2^i\times i + 2^{i+1}=2^{i+1}(i+1)$。

    所以，当 n 等于 2 的幂时，递归式的解是 $T(n)=n\lg n$。

4. 插入排序递归版的递归式

    $$
    T(n) = \begin{cases}
            \Theta(1) &\text{if\quad} n=1 \\
            T(n-1)+\Theta(n) &\text{if\quad} n > 1
        \end{cases}
    $$

5. 二分查找
    <!-- https://www.cnblogs.com/kyoner/p/11080078.html -->

    在线评测链接：https://leetcode.cn/problems/binary-search/

    假设需要查找 x 的位置，对于一个已排序的数列，其中可能包含多个 x，有时候需要左边界，有时需要右边界。当只含有一个 x 时，左右边界相同。

    下列代码是查找左边界的，若不存在` target`，则返回第一个大于 target 的元素的下标。（`nums` 从小到大排列）

    ```c
    int binarySearch(int nums[], int l, int r, int target)
    {
        // search in [l, r]
        while(l < r)
        {
            int mid = l + r >> 1;
            if(nums[mid] == target)
                r = mid;
            else if(nums[mid] < target)
                l = mid + 1;
            else r = mid;
        }
        return l;
    }
    ```

6. 二分查找优化插入排序？

    不能，即使在 $\Theta(\lg{n})$ 内找到需要插入的位置，将一个元素插入数组仍需要 $\Theta(n)$ 的时间。

7. 两数之和

    在线评测链接：https://leetcode.cn/problems/two-sum/

    设 a + b = x，因为 a、b均为整数，所以必有 $a\le \lfloor\dfrac{x}{2}\rfloor, \lceil\dfrac{x}{2}\rceil\le b$。

    - $\Theta(n^2)$：遍历集合 $S$，对于每个元素 $y$，在集合 $S-\{x-y\}$ 中查找 $x-y$。遍历与查找均为 $\Theta(n)$。

    - $\Theta(n\lg n)$：先对集合 $S$ 排序，$\Theta(n\lg n)$。

        1. **法一**：遍历 $S$ 中小于等于 $\lfloor\dfrac{x}{2}\rfloor$ 的元素，对于每个元素 $y$，在大于等于 $\lceil\dfrac{x}{2}\rceil$ 的部分应用二分查找（或其他 $\lg n$ 的查找）。遍历为 $\Theta(n)$，查找为 $\Theta(\lg n)$。

        1. **法二**：令指针 $i$ 指向最小元素，指针 $j$ 指向最大元素，若 $S_i+S_j > x$，则令 $j$ 递减 1，若小于，则令 $i$ 递增 1，直至 $i \ge j$。遍历为 $\Theta(n)$。（证明：淘汰一个数时，含该数的所有组合不符合要求）


    - $\Theta(n)$：$\Theta(n)$ 建立哈希表，对于每个元素 $y$，在集合 $S-\{y\}$ 中查找 $x-y$。遍历为 $\Theta(n)$，查找为 $\Theta(1)$。

    如果还要求输出 a、b 的下标，可先用结构体保存元素的值及其下标，再排序或建哈希表。

## Problems

1. 在归并排序中对小数组采用插入排序

    - 对一个子表排序的时间是 $\Theta(k^2)$，对 $\dfrac{n}{k}$ 个子表排序的时间是 $\dfrac{n}{k}\Theta(k^2)=\Theta(nk)$。
    - 递归树有 $\lg \dfrac{n}{k}$ 层，每层贡献总代价 $cn$，所以最坏情况时间复杂度为 $\Theta(n\lg\dfrac{n}{k})$。
    - $\Theta(nk+n\lg\dfrac{n}{k})=\Theta(n(\lg n-\lg k+k))=\Theta(n(\lg n+k))$，所以当 $k=\Theta(\lg n)$ 时，修改后的算法与标准的归并排序具有相同的运行时间。
    - 根据实际运行情况，选择插排快于归并的最大值。

2. 冒泡排序的正确性

    - $A'$ 中的元素与 $A$ 中的元素完全相同

    - **循环不变量**：每次迭代前，A[j] 是子数组 A[j..n] 中的最小元素。

        **初始**：j = n，显然成立。

        **保持**：在每次循环中，将 A[j] 与 A[j-1] 作比较，若 A[j-1] 大于 A[j]，则交换 A[j]、A[j-1]，所以 A[j-1] 一定小于 A[j..n] 中的任意元素。然后 j 递减 1，循环不变量保持成立。

        **终止**：最后一次迭代结束后，j = i，所以 A[i] 是 A[i..n] 中的最小元素。

    - **循环不变量**：每次迭代前，子数组 A[1..i-1] 已按非降序排序且包含数组 A 的前 i-1 小的元素。

        **初始**：i = 1，显然成立。

        **保持**：在每次迭代中，内层循环使得 A[i] 是 A[i..n] 中的最小元素。此时 A[i] 一定大于 A[i-1]。然后 i 递增 1，循环不变式保持成立。

        **终止**：最后一次迭代结束后，i = n，子数组 A[1..n-1] 已按非降序排序且包含数组 A 的前 n-1 小的元素。不等式 2.3 得证。

    - 最坏情况运行时间是：$\Theta(n^2)$。冒泡排序的最好情况和最坏情况的运行时间均为 $\Theta(n^2)$，因此，在最坏情况下，二者性能差不多。在最好情况下，插排性能更优。

3. 霍纳（Horner）规则的正确性

    - $\Theta(n)$
    - 朴素的多项式求值算法：$\Theta(n^2)$

        ```c
        void f(int a[], int len, int x)
        {
            int ans = 0;
            for(int i = 0; i <= len; i++)
            {
                int y = 1;
                for(int j = 1; j <= i; j++) // 由于 x 固定，此处可优化
                    y *= x;
                ans *= a[i] * y;
            }
        }
        ```

    - 初始：y = 0，i = n，循环不变量成立。
     保持：在每一次迭代前，$y=\displaystyle\sum_{k=0}^{n-(i+1)}a_{k+i+1}x^k$。在迭代中 $y=a_i+x\displaystyle\sum_{k=0}^{n-(i+1)}a_{k+i+1}x^k=a_ix^0+\displaystyle\sum_{k=0}^{n-(i+1)}a_{k+i+1}x^{k+1}=\sum_{k=0}^{n-i}a_{k+i}x^{k}$，然后 i 递减 1，循环不变量保持成立。
     终止：最后一次迭代结束后，i = -1，$y=\displaystyle\sum_{k=0}^{n}a_{k}x^k$。
    - 由 c 的结论知可以正确求出。

4. 逆序对

    - (2, 1)、(3, 1)、(8, 6)、(8, 1)、(6, 1)

    - 构成的降序数组 $\{n, \cdots,3,2,1\}$ 具有最多的逆序对。有 $\frac{n(n-1)}{2}$ 个逆序对。

    - 成正比关系。

        设 $S_i$ 为以 i 为右端点的逆序对集合，集合 $S=\{S_i|2\le i \le n\}$ 包含所有的逆序对。
        在插入排序的每次迭代前，子数组 A[1..j-1] 已有序。对于 $A[j],2\le j\le n$，需要移动的元素个数为大于 A[j] 的元素个数，即 $S_j$ 中逆序对的左端点。所以需要移动的元素个数与 $S_j$ 中元素个数相同。所以需要移动的元素总个数与逆序对个数相同。

    - 归并排序求逆序对数量

        算法思想：在合并两个有序子数组 $L,R$ 时，若 $L[i]>R[j]$，则大于等于 $L[i]$ 的元素与 $R[j]$ 构成逆序对。（$L[i]$ 与小于等于 $R[j]$ 的元素也构成逆序对，但这种计数方式会导致重复计数）

        ```c
        int ans = 0;
        void Merge(int a[], int p, int q, int r)
        {
            // merge [p, q] [q+1, r]
            int *tmp = (int *)malloc((r - p + 1) * sizeof(int));
            int i = p, j = q + 1, k = 0;
            while (i <= q && j <= r)
            {
                else if(a[i] <= a[j]) {
                    tmp[k] = a[i];
                    i++;
                }
                else {
                    ans += q - i + 1; // !!!!!!!!!!!!!!
                    tmp[k] = a[j];
                    j++;
                }
                k++;
            }
            while(i <= q) {
                tmp[k] = a[i];
                k++, i++;
            }
            while(j <= r) {
                tmp[k] = a[j];
                k++, j++;
            }
            for (int i = 0, j = p; i < (r - p + 1); i++, j++)
                a[j] = tmp[i];
            free(tmp);
        }
        void MergeSort(int a[], int l, int r)
        {
            if(l == r) return;
            int mid = l + r >> 1;
            ans += MergeSort(a, l, mid) + MergeSort(a, mid + 1, r);
            ans += Merge(a, l, mid, r);
        }
        ```