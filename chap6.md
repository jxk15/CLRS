---
title: 第 6 章 堆排序 Exercises & Problems
---

## Exercises

## 6.1 堆

1. 最少：在深度 $h$ 只有 1 个孩子，有 $2^h$ 个元素。最多：深度 $h$ 是充满的，有 $2^{h+1}-1$ 个元素。

2. 显然有 $2^{\lfloor\lg n\rfloor }\le 2^{\lg n}=n < 2^{\lfloor\lg n\rfloor + 1}$，含 $2^{\lfloor\lg n\rfloor }$ 和 $2^{\lfloor\lg n\rfloor + 1 }-1$ 个元素的堆的高度均为 $\lfloor\lg n\rfloor$，所以 $n$ 个元素形成的堆的高度也为 $\lfloor\lg n\rfloor$。

    法二：假设高度为 $h$。

    - $n\le 2^{h+1}-1$：$h\ge \lg(n+1)-1> \lg {n} - 1$
    - $2^h\le n$：$h\le \lg n$。
    - 所以 $\lg{n}-1 < h \le \lg{n}$，即 $h=\lfloor\lg n\rfloor$。

3. 若最大值只有 1 个不且在子树的根上，则说明该值大于其父结点，与最大堆的性质矛盾。

    若最大值有多个且不在子树的根上，则对于下标最小的最大值结点，一定大于其父结点，与最大堆的性质矛盾。

4. 可以在任何一个叶子。

5. 是。由于已排序，所以对任意的 $i$，有 $A[i]\le A[2i],A[i]\le A[2i+1]$。

6. 不是，6 的右孩子是 7，不满足条件。

7. 首先，由堆的性质，知叶结点的下标是连续的，我们只需证明叶结点下标最小是 $\lfloor\dfrac{n}{2}\rfloor + 1$。

    $n$ 个元素的堆可能是一个完全二叉树，或近似的完全二叉树。若是一个近似的完全二叉树，则下标最大的非叶结点只有左孩子时，$n$ 为偶数，有右孩子时 $n$ 为奇数。根据这些信息进行分类讨论即可完成证明。

    下面给出更好的证明。设下标最小的叶结点的下标为 $i$，则下标最大的非叶结点的下标为 $i-1$。

    易知 $n+1 \le 2i$，$2(i-1)\le n$，联立得 $\dfrac{n+1}{2}\le i \le \dfrac{n+2}{2}$。

    对 $n$ 的奇偶分类讨论得 $i=\lfloor\dfrac{n}{2}\rfloor + 1$。

    > 所以叶结点有 $\lceil\dfrac{n}{2}\rceil$ 个。

## 6.2 维护堆的性质

1. 略。（不会画动态图

2. 将求最大值改成求最小值即可，运行时间显然相同。

3. 比较完成后函数结束，不再向下递归。

4. 此时下标为 $i$ 的结点是叶结点。经过比较后 $largest$ 等于 $i$，不再向下递归。

5. 尾递归改成循环

    ```
    MAX-HEAPIFY(A, i)
        while true
        l = LEFT(i)
        r = RIGHT(i)
        if l ≤ A.heap-size and A[l] > A[i]
            largest = l
        else largest = i
        if r ≤ A.heap-size and A[r] > A[largest]
            largest = r
        if largest ≠ i
            exchange A[i] with A[largest]
            i = largest
        else return
    ```

6. 最坏情况时，根结点被交换到最底层的叶结点。由于树的高度为 $\lfloor\lg n\rfloor$，所以最坏情况运行时间为 $\Omega(\lg n)$。

## 6.3 建堆

1. 略

2. 从 1 递增的话不能保证子结点是最大堆，则 对 MAX-HEAPIFY 的调用是无意义的。

3. 证明题

    > 首先明确三个概念，附录 B 中有。
    >
    > 结点的深度、结点的高度、树的高度
    >
    > 对于非完全二叉树的堆，从下到上第二层中，有些结点的高度为 1，有些为 0。

    令 $n_h$ 为高度为 $h$ 的结点的数目。

    **假设**：$n_h\le \lceil\dfrac{n}{2^{h+1}}\rceil$。

    **基本情况**：对于高度为 0 的结点，即叶结点，由练习题 6.1-7 得 $n_0=\lceil\dfrac{n}{2}\rceil=\lceil\dfrac{n}{2^{0+1}}\rceil$。

    **归纳**：已知 $n_h\le \lceil\dfrac{n}{2^{h+1}}\rceil$。若 $n_h$ 是偶数，则 $n_{h+1}=\dfrac{n_h}{2}\le \lceil\dfrac{n}{2^{h+2}}\rceil$。若 $n_h$ 是奇数，则 $n_{h+1}=\lfloor\dfrac{n_h}{2}\rfloor + 1=\lceil\dfrac{n_h}{2}\rceil\le \lceil\dfrac{n}{2^{h+2}}\rceil$。

## 6.4 堆排序算法

1. 略

2. **初始**：$i = n$，显然成立。

    **保持**：在每次迭代中，首先交换 $A[1..i]$ 中的最大元素和最小元素 $A[1],A[i]$，所以子数组 $A[1..i-1]$ 是一个包含了数组 $A[1..n]$ 中前 $i-1$ 小元素的最大堆。因为 $A[i]$ 比 $A[i+1..n]$ 中的元素都小且 $A[i+1..n]$ 中的元素已有序，所以 $A[i..n]$  包含前 $n-i$ 个最大元素且有序。然后 $i$ 递减 1，循环不变量保持成立。

    **终止**：$i=0$，子数组 $A[1..n]$ 包含前 $n$ 个最大元素且已排序。

3. 均为 $\Theta(n\lg n)$。第 $i$ 次 MAX-HEAPIFY 的时间为 $\Theta(\lg i)$，$\displaystyle\sum_{i=2}^{\lfloor n/2\rfloor}\Theta(\lg i)=\Theta(\lg \dfrac{n}{2}!)=\Theta(n\lg n)$。

4. 同上。

5. 太难了，打上CLRSHard标记。

## 6.5 优先队列

1. 略

2. 略

3. 最小堆实现最小优先队列。伪代码如下。

    ```
    HEAP-MINIMUM(A)
        return A[1]
    //------------------------------------------------------------
    HEAP-EXTRACT-MIN(A)
        if A.heapsize < 1
            error"heap underflow"
        min = A[1]
        A[1] = A[A.heapsize]
        A.heapsize = A.heapsize - 1
        MIN-HEAPIFY(A, 1)
        return min
    //------------------------------------------------------------
    HEAP-DECREASE-KEY(A, i, key)
        if key > A[i]
            error"new key is bigger than current key"
        A[i] = key
        while i > 1 and A[PARENT(i)] > A[i]
            exchange A[i] with A[PARENT(i)]
            i = PARENT(i)
    //------------------------------------------------------------
    MIN-HEAP-INSERT(A, key)
        A.heapsize = A.heapsize + 1
        A[A.heapsize] = ∞
        HEAP-DECREASE-KEY(A, A.heapsize, key)
    ```

4. 因为 HEAP-INCREASE-KEY 要求 $A[i]$ 的新值必须大于等于旧值。但是在插入时应该使这一要求不可见。

5. **初始**：由于 $key$ 大于等于 $A[i]$ 旧值，所以第一次赋值后以 $A[i]$ 为根的子树仍为最大堆。除去以 $A[i]$ 为根的子树的部分没有改动，所以也仍是最大堆。所以只有 $A[i]> A[\text{PARENT}(i)]$ 会使 $A[1..A.heap-size]$ 不满足最大堆性质。

    **保持**：在每次迭代中，若 $i$ 不是根结点且 $A[i]> A[\text{PARENT}(i)]$ 则发生交换，由于 $A[\text{PARENT}(i)]$ 大于等于 $A[i]$ 旧值，所以交换后以 $A[i]$ 为根的子树仍为最大堆。$A[\text{PARENT}(i)]$ 的新值大于旧值，所以以 $A[\text{PARENT}(i)]$ 为根的子树仍为最大堆。
    除去以 $A[\text{PARENT}(i)]$ 为根的子树的部分没有改动，所以也仍是最大堆。然后更新 $i$，循环不变量保持成立。

    **终止**：终止时有两种情况。

    - 若 $i=1$，由循环不变量知迭代结束后，以 $A[i]$ 为根的子树是一个最大堆，所以 $A[1..A.heap-size]$ 满足最大堆性质。

    - 若 $A[i]\le A[\text{PARENT}(i)]$，由循环不变量知：以 $A[i]$ 为根的子树是一个最大堆；除去以 $A[i]$ 为根的子树的部分也是最大堆。所以 $A[1..A.heap-size]$ 满足所有的子结点小于等于父结点，所以 $A[1..A.heap-size]$ 满足最大堆性质。

6. 三次赋值优化为一次赋值。伪代码如下

    ```
    HEAP-INCREASE-KEY(A, i, key)
        if key < A[i]
            error"new key is smaller than current key"
        while i > 1 and A[PARENT(i)] < key
            A[i] = A[PARENT(i)]
            i = PARENT(i)
        A[i] = key
    ```

7. 假设使用最大堆。

    - 先进先出队列：添加元素时为递减地分配优先级。以优先级作为关键字。
    - 栈：添加元素时为递增地分配优先级。以优先级作为关键字。

8. 最大堆删除操作。伪代码如下。

    ```
    HEAP-DELETE(A, i)
        A[i] = A[A.heap-size]
        A.heap-size = A.heap-size - 1
        if A[i] > A[PARENT(i)]
        HEAP-INCREASE-KEY(A, i, A[i])
        else MAX-HEAPIFY(A, i)
    ```

9. $k$ 路归并，每次从 $k$ 个链表各自的最小值中取一个最小的，即动态地在 $k$ 个元素中取得最小值。显然可以用最小堆优化。

    在向堆中加入一个元素时，要备注一下该元素来自哪个链表（C 语言中可以通过结构体实现），当该元素取出后，需要从该元素所属链表中取最小值加入堆中。

    堆中元素数量不超过 $k$ 个，所以每次添加、取出的时间为 $O(\lg k)$，次数为 $O(n)$。

    所以该算法的时间复杂度为 $O(n\lg k)$。

    > 注意：是 $O(n\lg k)$，不是 $O(n\lg n)$。朴素合并方式的运行时间为 $O(nk)$。

## Problems

1. 用插入的方法建堆

    1. 例：输入为 $\{1,2,3\}$。
    2. 在最坏情况下，第 $i$ 次插入的时间为 $\Theta(\lg i)$，$\displaystyle\sum_{i=2}^{n}\Theta(\lg i)=\Theta(\lg n!)=\Theta(n\lg n)$。

2. 对 $d$ 叉堆的分析

    1. $A[1]$ 仍为根结点。结点 $i$ 的父结点 $\text{D-ARY-PARENT}(i)=\Big\lfloor\dfrac{i-2}{d+1} \Big\rfloor$。结点 $i$ 的第 $j$ 个子结点（$1\le j\le d$）$\text{D-ARY-CHILD}(i,j)=d(i-1)+j+1$。

    2. $\Theta(\log_{d}n)$

    3. 去掉并返回具有最大关键字的元素。伪代码如下。$\text{D-ARY-MAX-HEAPIFY}$ 每次执行时需要进行 $\Theta(d)$ 次比较，执行次数为 $O(\log_{d} n)$，所以运行时间为 $O(d\log_{d}n)$。

        ```pseudocode
        D-ARY-EXTRACT-MAX(A)
            if A.heap-size < 1
                error"heap underflow"
            max = A[1]
            A[1] = A[A.heap-size]
            A.heap-size = A.heap-size - 1
            D-ARY-MAX-HEAPIFY(A, 1)
            return max
        //------------------------------------------------------------
        D-ARY-MAX-HEAPIFY(A, i)
            largest = i
            for j = 1 to d
                child = D-ARY-CHILD(i, j)
                if child <= A.heap-size and A[child] > A[largest]
                    largest = child
            if largest ≠ i
                exchange A[i] with A[largest]
                D-ARY-MAX-HEAPIFY(A, largest)
        ```

    4. 插入元素。伪代码如下。时间复杂度为 $O(\log_{d}n)$。

        ```
        D-ARY-MAX-HEAP-INSERT(A, key)
            A.heap-size = A.heap-size + 1
            A[A.heap-size] = key
            i = A.heap-size
            while i > 1 and A[D-ARY-PARENT(i)] < A[i]
                exchange A[i] with A[D-ARY-PARENT(i)]
                i = D-ARY-PARENT(i)
        ```

    5. 增加元素的值。伪代码如下。时间复杂度为 $O(\log_{d}n)$。

        ```pseudocode
        D-ARY-INCREASE-KEY(A, i, key)
            if key < A[i]
                error"new key is smaller than current key"
            A[i] = key
            while i > 1 and A[D-ARY-PARENT(i)] < A[i]
                exchange A[i] with A[D-ARY-PARENT(i)]
                i = D-ARY-PARENT(i)
        ```

3. Young 氏矩阵

    1. 不唯一。

        $$
        \begin{pmatrix}
            2 & 4 & 9 & \infty \\
            3 & 8 & 16 & \infty \\
            5 & 14 & \infty & \infty \\
            12 & \infty & \infty & \infty
        \end{pmatrix}
        $$

    2. 若 $Y[1,1]=\infty$ 且存在 $Y[x,y]\ne \infty$，则有 $Y[x,y] > Y[x,1] > Y[1,1] = \infty$。产生矛盾。

        若 $Y[m,n]<\infty$ 且存在 $Y[x,y] = \infty$，则有 $\infty = Y[x,y]<Y[x,n]<Y[m,n]<\infty$。产生矛盾。

    3. 首先保存 $Y[1,1]$ 的值作为返回值。然后将 $Y[1,1]$ 赋值为无穷。

        为了维护杨氏矩阵的性质，把无穷与右邻居、下邻居中较小的交换。直到右邻居、下邻居均为无穷或已移至 $(m,n)$。

        时间复杂度：$T(p)=T(p-1)+\Theta(1)$，得 $T(p)=O(m+n)$。

        > 循环不变量与练习 6.5-5 类似

    4. 同上，从 $(m,n)$ 向 $(1,1)$ 移动。

        > 循环不变量与练习 6.5-5 类似

    5. 利用前两题的算法，先插入再取出。

    6. 从右上角出发。设当前位置为 $(i,j)$，若 $Y[i,j]>key$ ，则说明 $Y[i..m, j]$ 的所有元素均大于 $key$，那么我们向左走一步。若 $Y[i,j]<key$ ，则说明 $Y[i, 1..j]$ 的所有元素均小于 $key$，那么我们向下走一步。

        所以通过 1 次比较，可以排除一行或一列的数据，所以该算法时间复杂度为 $O(m+n)$。

        从左下角出发同理。