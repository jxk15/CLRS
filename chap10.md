---
title: 第 10 章 基本数据结构 Exercises & Problems
---

## 第 10 章 基本数据结构
## Exercises

## 10.1 栈和队列

1. 略。

2. 一个栈底为 1，向 $n$ 生长；另一个栈底为 $n$，向 1 生长。两个栈顶均指向所属栈中最新插入的元素，若栈顶之和小于 $n$，则可插入。

3. 略。

4. 下溢：队列为空时。上溢：队列满时。

    ```
    // 判空
    QUEUE-EMPTY(Q)
        if Q.head == Q.tail
            return TRUE
        else return FALSE

    // 判满, 最多容纳 n - 1 个元素
    QUEUE-FULL(Q)
        if (Q.head == Q.tail + 1) or (Q.head == 1 and Q.tail == Q.length)
            return TRUE
        else return FALSE

    // overflow
    ENQUEUE(Q, x)
        if QUEUE-FULL(Q)
            error "overflow"
        else
            Q[Q.tail] = x
        if Q.tail == Q.length
            Q.tail = 1
        else Q.tail = Q.tail + 1

    // underflow
    DEQUEUE(Q)
        if QUEUE-EMPTY(Q)
            error "underflow"
        else
            x = Q[Q.head]
        if Q.head == Q.length
            Q.head = 1
        else Q.head = Q.head + 1
        return x
    ```

5. deque：在 head 插入、**在 head 删除**、**在 tail 插入**、在 tail 删除。在上一题中已经实现了两个。

    ```
    // 在 head 插入
    HEAD-ENDEQUE(D, x)
        if QUEUE-FULL(D)
            error "overflow"
        else
            if D.head == 1
                D.head = D.length
            else D.head = D.head - 1
            D.[D.head] = x

    // 在 tail 删除
    HEAD-DEDEQUE(D)
        if QUEUE-EMPTY(D)
            error "underflow"
        else
            if D.tail == 1
                D.tail = D.length
            else D.tail = D.tail - 1
            return D[D.tail]
    ```

6. 设两个栈分别为 $A,B$。

    $\rm{ENQUEUE}$：把元素 push 到 $B$ 中。时间为 $O(1)$。

    $\rm{DEQUEUE}$：若 $A$ 为空，先将 $B$ 中所有元素 pop 并 push 到 $A$ 中。此时 $A$ 的顶部元素即为存在时间最长的元素，把该元素 pop 即完成 $\rm{DEQUEUE}$。每次出队，当 $A$ 为空时，时间复杂度为 $O(B.size)$，否则为 $O(1)$。总体来看，每出队一个元素，该元素一定先从 $B$ 中 pop，再 push 到 $A$，再从 $A$ 中 pop，所以平均时间为 $O(1)$。

7. 设两个队列分别为 $A,B$。

    1. 设初始时 $A$ 为入栈队列，$B$ 为出栈辅助队列。
        1. 入栈：把元素加入入栈队列 $A$，时间为 $O(1)$。
        2.  出栈：把入栈队列 $A$ 中的元素出队并入队到出栈辅助队列 $B$，入栈队列中的队尾即为待返回的栈顶。然后令 $B$ 为入栈队列，$A$ 为出栈辅助队 列。时间为 $O(n)$。
    2.  设初始时 $A$ 为入栈辅助队列，$B$ 为出栈队列。
        1.  出栈：出栈队列的队首即为栈顶。
        2.  入栈：把元素加入到入栈辅助队列 $A$，然后把出栈队列 $B$ 中的元素出队并入队到入栈辅助队列 $A$。然后令 $B$ 为入栈辅助队列，$A$ 为出栈队列。时间为 $O(n)$。

## 10.2 链表

1. 单链表可以 $O(1)$ 插入；不能 $O(1)$ 删除，因为必须先 $O(n)$ 找到前驱结点。

2. 单链表实现栈：在表头插入、删除

    ```
    STACK-EMPTY(L)
        if L.head == NIL
            return TRUE
        else return FALSE
    ```

    ```
    PUSH(L, x)
        x.next = L.head
        L.head = x
    ```

    ```
    POP(L)
        if LIST-EMPTY(L)
            error "underflow"
        else
            x = L.head
            L.head = x.next
            return x
    ```

3. 单链表实现队列：在表头删除、在表尾插入（增加一个 tail 指针，指向表尾元素）

    ```
    QUEUE-EMPTY(L)
        if L.head == NIL
            return TRUE
        else return FALSE
    ```

    ```
    // if-else 中的共同部分可以放在 if-else 之外
    ENQUEUE(L, x)
        if QUEUE-EMPTY(L)
            x.next = NIL
            L.head = x
            L.tail = x
        else
            x.next = NIL
            L.tail.next = x
            L.tail = x
    ```

    ```
    DEQUEUE(L)
        if QUEUE-EMPTY(L)
            error "underflow"
        else
            x = L.head
            if L.head == L.tail
                L.tail = NIL
            L.head = L.head.next
            return x
    ```

4. 在循环开始前，把 $k$ 赋值给 $L.nil.key$。

5. $\rm{INSERT}$：$O(1)$

    ```
    LIST-INSERT(L, x)
        x.next = L.nil.next
        L.nil.next = x
    ```

    $\rm{DELETE}$：$O(n)$

    ```
    LIST-DELETE(L, x)
        prev = L.nil
        while prev.next ≠ x
            if prev.next == L.nil
                error "the element not exist"
            prev = prev.next
        prev.next = x.next
    ```

    $\rm{SEARCH}$：$O(n)$

    ```
    LIST-SEARCH(L, k)
        x = L.nil.next
        L.nil.key = k
        while x.key ≠ k
            x = x.next
        if x == L.nil
            error "the element not exist"
        else return x
    ```

6. 连接链表的端点处。

7. 单链表逆转：时间 $O(n)$，空间 $O(1)$。在线评测：https://leetcode.cn/problems/reverse-linked-list

    1. 头插法：先创建一个元素指针指向新链表的表头。依次删除输入链表的表头，并使用头插法插入到新链表中。最后把新链表的表头指针赋给输入链表的表头指针。

        ```cpp
        /**
        * Definition for singly-linked list.
        * struct ListNode {
        *     int val;
        *     ListNode *next;
        * };
        */
        class Solution {
        public:
            ListNode* reverseList(ListNode* head) {
                // tmp: 输入链表的表头
                ListNode* newList = nullptr, *tmp = nullptr;
                while(head != nullptr)
                {
                    // 删除输入链表的表头
                    tmp = head;
                    head = head->next;
                    // 头插法：插入新链表
                    tmp->next = newList;
                    newList = tmp;
                }
                return newList;
            }
        };
        ```

    2. 迭代：创建元素指针 $previous,current,next$。以 prev 为表头的链表为已逆序，以 curr 为表头的链表未逆序。

        初始：显然成立

        保持：通过 while 循环体可以保持

        终止：curr 指向输入链表的表尾，prev 指向最后一个元素。于是反转成功。

        ```cpp
        class Solution {
        public:
            ListNode* reverseList(ListNode* head) {
                ListNode* prev = nullptr, *curr = head, *nextNode = nullptr;
                while(curr != nullptr)
                {
                    // 为下一次迭代做准备
                    nextNode = curr->next;
                    // 反转
                    curr->next = prev;
                    // 更新 prev, curr
                    prev = curr;
                    curr = nextNode;
                }
                return prev;
            }
        };
        ```

    3. 递归：从表尾开始逆转。时间空间均为 $O(n)$。

        ```cpp
        class Solution {
        public:
            ListNode* reverseList(ListNode* head) {
                // head == nullptr: 空表
                // head->next == nullptr: 到达表尾
                if (head == nullptr || head->next == nullptr)
                    return head;

                // 只是为了得到表尾指针，不参与反转
                ListNode* newHead = reverseList(head->next);

                // 反转
                head->next->next = head;
                head->next = nullptr;

                return newHead;
            }

        };
        ```


8. 略。

## 10.3 指针和对象的实现

1. 略。

2. 假设给定一个指针 $i$，$i.next$ 的值为 $A[i]$。

    $\text{ALLOCATE-OBJECT}$

    ```
    ALLOCATE-OBJECT()
        if free == NIL
            error "out of space"
        else
            x = free
            free = A[free]
            return x
    ```

    $\text{FREE-OBJECT}$

    ```
    FREE-OBJECT(x)
        A[x] = free
        free = x
    ```

3. 因为在设置 next 指针后，就可以 $O(1)$ 完成对象分配和释放了。

4. 假设数组 $A$ 长度为 $n$，以 $A[n]$ 为栈底，向 $A[1]$ 生长，栈底和栈顶之间（$A[stackTop..n]$）为未分配的。

    1. 当需要分配对象时，取出栈顶即可。
    2. 当释放对象时，先把和栈顶相邻的已分配对象移动到待释放对象的位置，并修改相关指针（prev 的 next、next 的 prev），然后栈顶减 1。

5. 遍历 $L$，把 $L$ 的第 $i$ 个对象与数组中第 $i$ 个对象的指针交换并修改相应指针，使 $L,F$ 的关键字顺序不变。遍历结束后，$L$ 的第 $i$ 个对象的指针即为 $i$。

    核心就在于交换两个对象的指针并保证两个链表的关键字顺序不变，我写的这个比较丑陋，但能用。

    注意：如果遍历了 $F$，时间复杂度就是 $O(\max(m-n, n))$，不符合题目要求。

    ```
    // a 属于 L
    // b 属于 L 或 F
    // 交换两个对象的指针
    swapObject(a, b, L, F)
        // 先记录下 b 的 prev, next, 防止修改 a 的邻居时被修改
        oldprev = b.prev, oldnext = b.next

        // 处理 a.prev.next, a.next.prev
        if a.prev == NIL
            // 那么 a 是 L 的表头
            L.head = b
        else a.prev.next = b
        if a.next == NIL
            // 那么 a 是 L 的表尾
            do nothing
        else a.next.prev = b

        // 类似地,处理 b.prev.next, b.next.prev


        // 最后交换对象属性
        swap(a.key, b.key)
        swap(a.prev, b.prev)
        swap(a.next, b.next)
    ```

    ```
    COMPACTIFY-LIST(L, F)
        tmp = L.head
        i = 1
        while tmp != NIL
            // 若相等，不交换，
            // 在交换时，若该对象为表头，需要确定属于哪个表，以此来修改表头指针
            // 在 swapObject 中，我假设若 b 为表头，那么 b 一定是 F 的表头
            // 因为第一次交换后，L 的表头为 1，后续不会再和 L 的表头发生交换
            // 所以只需对初始时 L 的表头指针为 1 做特判
            if tmp == 1 && i== 1
                tmp = tmp.next
                i = i + 1
            // 若 i 为表头，一定是 F 的表头，因为 L 的表头的指针为 1
            swapObject(tmp, i, L, F)
            tmp = i.next
            i = i + 1
    ```

    ??? C代码
        === "运行结果"
            `success: n` 表示第 n 次测试成功

            `n: x m: x` 表示本次测试中 `n、m` 的值

            <img src="https://img.buxizhou.top/img/image-20230819151218440.png" style="zoom:45%;" />
        === "C代码"
            ```c hl_lines="63"
            #include<stdio.h>
            #include<stdlib.h>
            #include<time.h>
            #include <unistd.h>
            #define N (10000 + 10)
            #define NIL (0)
            // 测试次数
            int times = 100;
            // 保存 1~n 的排列
            int numbers[N];
            // 存储 L、F
            int key[N], prev[N], next[N];
            // 保存初始的 L、F
            int checkL[N], checkF[N];

            // 交换两个 int 型变量
            void swap(int *a, int *b);
            // 随机生成两个 List
            void getList(int *lhead, int *fhead, int n, int m);
            // 记录生成的两个 List
            void recordList(int lhead, int fhead);
            // 与 COMPACTTIFY 之后的链表比较，验证关键字顺序、指针值是否正确
            void checkList(int lhead, int fhead);
            // 打印 List
            void printList(int head);
            // 交换两个对象的指针
            void swapObject(int a, int b, int *lhead, int *fhead);

            int main()
            {
                times = 300;
                while (times--)
                {
                    srand((unsigned)time(NULL));
                    int n, m, tmp;
                    m = rand() % 1000 + 20;
                    srand((unsigned)m);
                    n = rand() % m + 1;

                    // n = 6, m = 13; // 小数据测试
                    for (int i = 1; i <= m; i++)
                        key[i] = rand() % 10000 + 1;

                    int lhead, fhead;
                    // 随机生成两个 List, L 长度为 n，F 长度为 m
                    getList(&lhead, &fhead, n, m);
                    // printList(lhead);
                    // printList(fhead);
                    recordList(lhead, fhead);

                    // 核心，显然是 O(n)
                    tmp = lhead;
                    int i = 1;
                    while (tmp != NIL)
                    {
                        if(tmp == 1 && i == 1)
                        {
                            // 如果位置正确，不再操作
                            tmp = next[tmp];
                            i++;
                            continue;
                        }
                        swapObject(tmp, i, &lhead, &fhead);
                        tmp = next[i];
                        i++;
                    }

                    // printf("after compactify---------------------------\n");
                    // printList(lhead);
                    // printList(fhead);
                    checkList(lhead, fhead);
                    printf("n: %d  m: %d\n", n, m);
                    sleep(1); // 防止srand()不刷新
                }
                return 0;
            }

            // int 型交换
            void swap(int *a, int *b)
            {
                int tmp = *a;
                *a = *b;
                *b = tmp;
            }
            void getList(int *lhead, int *fhead, int n, int m)
            {
                // 获取 1~n 的排列
                for (int i = 1; i <= m; i++)
                    numbers[i] = i;
                for (int i = 1; i <= m; i++)
                    swap(numbers + i, numbers + i + rand() % (m - i + 1));
                // 生成 L
                *lhead = numbers[1];
                prev[*lhead] = NIL, next[*lhead] = numbers[2];
                prev[numbers[n]] = numbers[n - 1], next[numbers[n]] = NIL;
                for (int i = 2; i <= n - 1; i++)
                {
                    prev[numbers[i]] = numbers[i - 1];
                    next[numbers[i]] = numbers[i + 1];
                }
                // 生成 F
                *fhead = numbers[n + 1];
                prev[*fhead] = NIL, next[*fhead] = numbers[n + 2];
                prev[numbers[m]] = numbers[m - 1], next[numbers[m]] = NIL;
                for (int i = n + 2; i <= m - 1; i++)
                {
                    prev[numbers[i]] = numbers[i - 1];
                    next[numbers[i]] = numbers[i + 1];
                }
            }

            void recordList(int lhead, int fhead)
            {
                for (int i = 1; lhead != NIL; i++, lhead = next[lhead])
                    checkL[i] = key[lhead];

                for (int i = 1; fhead != NIL; i++, fhead = next[fhead])
                    checkF[i] = key[fhead];
            }
            void checkList(int lhead, int fhead)
            {
                for (int i = 1; lhead != NIL; i++, lhead = next[lhead])
                    // 第 i 个元素的指针为 i
                    if (checkL[i] != key[lhead] || lhead != i)
                    {
                        printf("fail!!!!!!!!!!!!!!!!!\n");
                        exit(0);
                    }

                for (int i = 1; fhead != NIL; i++, fhead = next[fhead])
                    if (checkF[i] != key[fhead])
                    {
                        printf("fail\n");
                        exit(0);
                    }
                printf("success: %d\n", times);
            }

            // 交换两个对象的指针，并保持关键字顺序不变
            void swapObject(int a, int b, int *lhead, int *fhead)
            {
                // 由于 L 表头指针为 1 时不调用 swapObject，所以若 b 为表头，一定是 F 的表头
                // 防止 b 指向的元素被 a 指向的元素被修改
                int oldprev = prev[b], oldnext = next[b];

                // a 的前后，要搬家了，通知一下邻居
                if (prev[a] == NIL)
                    *lhead = b;
                else
                    next[prev[a]] = b;
                if (next[a] == NIL)
                    ;
                else
                    prev[next[a]] = b;
                // b 的前后，要搬家了，通知一下邻居
                if (oldprev == NIL)
                    *fhead = a;
                else
                    next[oldprev] = a;
                if (oldnext == NIL)
                    ;
                else
                    prev[oldnext] = a;

                // 交换属性
                swap(key + a, key + b);
                swap(prev + a, prev + b);
                swap(next + a, next + b);
            }

            void printList(int head)
            {
                int tmp = head;
                while (tmp != NIL)
                {
                    // 打印指针值和 key
                    printf("(%d, %d) ", tmp, key[tmp]);
                    tmp = next[tmp];
                }
                printf("\n");
            }
            ```

## 10.4 有根树的表示

1. 略。注意下标为 2 和 8 的结点不在二叉树中。

2. $O(n)$ 递归遍历二叉树：先中后序遍历

3. $O(n)$ 非递归遍历二叉树

    1. 层次遍历：使用队列
    2. 递归转非递归：使用栈
    3. Morris 遍历：https://ghh3809.github.io/2018/08/06/morris-traversal/。

4. 递归：类似于二叉树的先序遍历、后序遍历。

    非递归：层次遍历或使用栈

5. 详见邓俊辉《数据结构》第三版。

    > Morris 遍历临时修改了树，不符合要求。

6. 想法：只有最右子结点指向父结点。

    结点的 $p_1$ 指针总指向最左子节点。

    若布尔值为 TRUE，表示该结点还有右兄弟，则 $p_2$ 指针指向右兄弟；为 FALSE，表示该结点为最右兄弟，则 $p_2$ 指针指向父结点。

    找给定结点的父结点：先找到最右兄弟结点，其 $p_2$ 指针即为父结点。

    找给定结点的所有子结点：从最左子节点开始，到最右子结点。

## Problems

1. 链表间的比较

    |                           | 未排序的单链表 | 已排序的单链表 | 未排序的双向链表 | 已排序的单链表 |
    | :------------------------ | :------------: | :------------: | :--------------: | :------------: |
    | $\text{SEARCH}(L,k)$      |  $\Theta(n)$   |  $\Theta(n)$   |   $\Theta(n)$    |  $\Theta(n)$   |
    | $\text{INSERT}(L,x)$      |  $\Theta(1)$   |  $\Theta(n)$   |   $\Theta(1)$    |  $\Theta(n)$   |
    | $\text{DELETE}(L,x)$      |  $\Theta(n)$   |  $\Theta(n)$   |   $\Theta(1)$    |  $\Theta(1)$   |
    | $\text{SUCCESSOR}(L,x)$   |  $\Theta(n)$   |  $\Theta(1)$   |   $\Theta(n)$    |  $\Theta(1)$   |
    | $\text{PREDECESSOR}(L,x)$ |  $\Theta(n)$   |  $\Theta(n)$   |   $\Theta(n)$    |  $\Theta(1)$   |
    | $\text{MINIMUM}(L)$       |  $\Theta(n)$   |  $\Theta(1)$   |   $\Theta(n)$    |  $\Theta(1)$   |
    | $\text{MAXIMUM}(L)$       |  $\Theta(n)$   |  $\Theta(1)$   |   $\Theta(n)$    |  $\Theta(1)$   |

2. 利用链表实现可合并堆


3. 搜索已排序的紧凑链表

