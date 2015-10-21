title: My Introduction to Algorithms(CLRS) Notes
date: 2015-10-20 21:39:43
categories:
- Algorithm
tags:
- Algorithm
---

##Overview
It takes me half year to finish the CLRS and learn a lot. This is my notes about the legendary book.

I have read the whole book and take the online [MIT open Course](http://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-046j-introduction-to-algorithms-sma-5503-fall-2005/). Thanks MIT for the online course. 
I've also get resource from Chinese and English forum.

I want to share my note to other guy who is interested in the algorithm and thinking.
Please feel free to check it out. Hope it's helpful to you. Thank you.

<!--more-->

##Math
###Master theorem
证明递归式的算法复杂度
![Alt text](http://i.imgur.com/CWyQle2.png)

###Geometric progression
可用于递归树
![Alt text](http://i.imgur.com/4f5Q6NB.png)

###Recursion tree
求出每层的操作代价，相加，一般可能是等比数列，求等比数列。

例子：$T(n) = T(n/4) + T(n/2) + n^2$
第一行  $ n^2$
第二行   $\dfrac{5}{16} n^2$
第三行  $\dfrac{25}{256} n^2$
......

利用等比数列求和公式算出大O为<$2n^2$
所以算法复杂度为$n^2$
用这个方法求出的解可以在**代数法**中验算

###代数法
* 猜测解的形式
* 用数学归纳法找出使真正有效的常数


如何做猜测：
* 递归树
* 做下界和上界，然后逼近

###Asymptotic notation

$\Theta$:函数的上界和下界
$O$:函数上界
$\Omega$:函数下界
$o$
$\omega$

![Alt text](http://i.imgur.com/v0oBeE9.png)

###阶乘
P:34

##Divide and Conquer

###$x^n$
1. 普通算法$O(n)$
2. 分治法递归式:$T(n) = T(n/2) + \Theta(1)$
复杂度:$O(lgn)$

###Fibonacci numbers
1. 递归求和，指数级
2. 线性:两个互相相加，复杂度$O(n)$,动规思想
3. 利用公式:  $$ a_n = \dfrac{\sqrt{5}}{5}\cdot\left [\left( \dfrac {1 + \sqrt{5}}{2} \right)^n - \left( \dfrac {1 - \sqrt{5}}{2} \right)^n \right]$$
**但是无法实现，因为计算机中float形会丢失精度**
4. 利用矩阵
$$\begin{pmatrix}F_{n+1} & F_n\\F_n & F_{n-1}\end{pmatrix}={\begin{pmatrix} 1 &1 \\ 1 & 0 \end{pmatrix}^n}$$
矩阵连乘，所以用分治法复杂度为$O(lgn)$

###Factorial 矩阵相乘

1. 普通算法直接相乘$O(n^3)$
2. [分块矩阵算法](http://zh.wikipedia.org/wiki/%E5%88%86%E5%A1%8A%E7%9F%A9%E9%99%A3)相乘，然后相加
递归式：$T(n)=8T(n/2)+\Theta(n^2)$
case1复杂度:$O(n^3)$没有进步
3. [Strassen algorithm](http://en.wikipedia.org/wiki/Strassen_algorithm)
思想:相乘的代价比较大，相加是常数量，所以尽可能减少子问题，也就是减少相乘代价，增加相加的代价，但还是常数量。算法运用了7次相乘的代价，因此只有7个子问题，比之前的8个少了一个，而相加次数略微变多，但是还是常数可以忽略
递归式:$T(n)=7T(n/2)+\Theta(n^2)$
复杂度case1:$O(n^{lg7})<O(n^{2.81})$
指数的优化有很大的作用，最优理论值为$O(n^{2.376})$

###VLIS layout
问题：一个**完全二叉树**铺放的面积最小，在集成电路中使用
假设有n个叶子节点，如何让使用的面积最小
高度:H(n)
宽度:W(n)

普通算法:按照树的形状进行摆放
面积:$H(lgn)\cdot W(n)=O(nlgn)$

如何将面积降低到线性，可以利用递归式**反推**，两个$\sqrt n$相乘可以得到线性的面积
递归式反推，通过主定理case1来推导如何形成$n^{1/2}$,可以猜测a=2，b=4，后面的常数小于case1中的大小，就能构造
将问题分成两个子问题，每个子问题是原来的4分之一。

算法:[H tree](http://zh.wikipedia.org/wiki/H%E6%A0%91)
递归式:$T(n)=2T(n/4)+\Theta(1)$



##Sorting and Order Statistics
* 內部排序：排序的資料量小，可以完全在主記憶體內進行排序
* 外部排序：排序的資料量大，無法直接在主記憶體內進行排序

外排序：merge sort
[External sorting](http://en.wikipedia.org/wiki/External_sorting)

堆排序：如果只要求排出前x个数，只要保留x个树的堆在内存中就行，其余的叶节点都舍弃

###排序时间
####比较排序算法
定理:最坏情况，要做$\Omega(nlgn)$次比较
证明:P98，斯特林近似公式
$n!\le l\le s^h$
$h\ge lg(n!)$=$\Omega(nlgn)$   

![Alt text](http://i.imgur.com/1CZRH4i.png)

###quicksort

####作用
分治法思想
原地排序
实践中非常有用
虚拟内从中缓存利用非常好

* Divide:partition array into 2 subarray around **pivot**
* conquer:recusive sort 2 subarray
* conbine:trivial

如果元素重复的比较多，性能会不好，使用Hoare

####复杂度
* Worst-case:already almost sorted/reverse sorted
$$T(n)=T(n-1)+\Theta(n)=\Theta (n^2)$$
like the insertion sort 等差数列

* Best-case:in the middle
$$T(n)=2T(n/2)+\Theta(n)=\Theta(nlgn)$$

* Aerage-case:lucky or unlucky
如果是1:10和9:10来划分，画出递归树
$$T(n)\le Cn \cdot \log _{10/9}{n}=\Theta(nlgn)$$
在luck和unluck变换，使用代换法证明就行

####优化
随机化的quicksort
* 随机化pivot
* 打乱输入顺序
证明:用到期望，不会，略。。。。

pivot选取
* 最左(对于已经排序的数组，复杂度太高)
* 三值取平均(会整数溢出问题)
* 随机

重复元素
* 如果数组中重复元素比较多，快排效果比较差，荷兰国旗问题
* 可以修改算法，等于pivot的算作已经排序，只要再排序大于和小于pivot的值
```
quicksort(A, lo, hi)
    if lo < hi
        p := pivot(A, lo, hi)
        left, right := partition(A, p, lo, hi)  // note: multiple return values
        quicksort(A, lo, left)
        quicksort(A, right, hi)
```

资料
* [怎样让快速排序更快](http://blog.csdn.net/nwpu_kexie/article/details/7538673)
* [wiki](http://en.wikipedia.org/wiki/Quicksort)
* [荷兰国旗问题](https://github.com/julycoding/The-Art-Of-Programming-By-July/blob/master/ebook/zh/02.07.md)

###timsort

###heapsort
使用最大堆
堆数据结构是一个数组对象，也是一棵完全二叉树
普遍情况下，快速排序更快。
堆排序常见应用：高效的优先级队列 priority queue,每次操作的时间复杂度都是$O(lgn)$
![Alt text](http://i.imgur.com/3HTrr0A.png)

####MAX-HEAPIFY
运行时间$O(lgn)$,保持最大堆的性质。
和子女比较谁大，如果小于子女，把子女中较大的提上来，依次往下

![Alt text](http://i.imgur.com/JuR4pNt.png)

####BUILD-MAX-HEAP
通过一个数组，进行最大堆的建立，从第一个不是叶子的节点开始依次调用MAX-HEAPIFY来满足最大堆的性质. 通过数学论证，这一步的复杂度一共是$O(n)$
![Alt text](http://i.imgur.com/JyLZIQH.png)

####HEAPSORT
首先调用BUILD-MAX-HEAP来建立一个最大堆。然后每次将第一个元素(最大)与堆中最后一个元素互换，最大的元素被排到后面，新的堆中第一项有可能不满足堆的性质，所以在此做HEAPIFY来满足

复杂度为$O(nlgn)$

![Alt text](http://i.imgur.com/8RtgXhK.png)






###bubble sort

###insertion sort
running time:depend on input and input size
best case:already almost sorted
worst case:reverse sorted

worst case:
T(n)=max time to run

Average case:
T(n)=expected time
need assumption of statics distribution

Best case:bogus
cheat

O(n^2)
small n:fast
big n:slow

###mergesort

```
if n=1 done
recursive sort
	1-n/2
	n/2-n
merge
```

T(n) = 2T(n/2) + O(n)

use recursive tree
tree high = lgn
each level = n
O(nlgn)

tips: when the element size > 30 ,the merge sort is better than insertion sort

###selection sort

###shell sort

###bucket sort
当输入符合均匀分布时，就可以得到线性期望时间
复杂度:$O(n+k)$
空间复杂度非常高
![Alt text](http://i.imgur.com/SlYm6SF.png)

[桶排序](http://hxraid.iteye.com/blog/647759)

###counting sort
假设n个输入元素中的每一个都介于0到k之间的整数,此处k为某个整数，总时间复杂度$\Theta(n+k)$，当$k=O(n)$时，计数排序为$\Theta(n)$
计算每一个值出现的次数，放入一个数组中，最后反向遍历数组进行排序
性质:稳定排序，相同值的相对次序不会改变

![Alt text](http://i.imgur.com/X0yGjXa.png)

###radix sort

利用counting sort的稳定排序来对数的最后一位开始进行排序
![Alt text](http://i.imgur.com/c7qIh9X.png)

定理：Given n d -digit numbers in which each digit can take on up to k possible values, RADIX-SORT correctly sorts these numbers in ‚.d.n C k// time if the stable sort it uses takes ‚.n C k/ time.

定理：Given n b-bit numbers and any positive integer r 􏰎 b, RADIX-SORT correctly sorts these numbers in ‚..b=r/.n C 2r // time if the stable sort it uses takes ‚.n C k/ time for inputs in the range 0 to k.

###Medians and Order Statistics
input:输入包含n个不同数的集合A和一个数i
output:元素x，恰好大于A中其他$i-1$个元素

####Minimum and maximum
单纯最大值或者最小值:$n-1$次比较

同时最大或者最小:$3\lfloor n/2 \rfloor$次比较
先将一对输入元素互相比较，然后较小者与当前最小值比较，较大者与当前最大值比较，因此每两个元素要3次比较

####Selection in expected linear time
利用快速排序，但是只做一边的递归，而不做另外一边的递归
平均复杂度:$\Theta(n)$
最坏复杂度:$\Theta(n^2)$

![Alt text](http://i.imgur.com/R4HUKYj.png)


####Selection in worst-case linear time
可以当情况最坏时，还是保证线性的时间复杂度，但是其中的线性的常量会非常高，所以一般不可取，理论上实现了，一般还是用普通的选择算法
主要思想:在快排中让子问题规模小于1，这样就有线性的时间复杂度了

![Alt text](http://i.imgur.com/xKTTQqN.png)
![Alt text](http://i.imgur.com/luCxdDe.png)
![Alt text](http://i.imgur.com/iFnmpkQ.png)                                                                                                                                                                                                                                                                                                                                       
           
##Hash Tables
###Direct-address tables
当关键字全域比较小时，直接寻址简单有效
![Alt text](./1427484381325.png)

U很大时，需要的容量太大，但是实际用到的缺比较小

###Hash Tables
直接寻址：k个关键字元素存放在槽 k
散列：通过哈希函数存放在[0.。m-1]的槽上
通过哈希把需要的下标从 U降低为 m 了

![Alt text](http://i.imgur.com/ZVWEetA.png)

但是还要处理碰撞问题，两个不同的 key 哈希一个相当的哈希值，就要解决

连接法：
 ![Alt text](http://i.imgur.com/8WwAR0F.png)

 n 个元素，m 个槽，装载因子$\alpha$为 n/m，即一个链中平均存储的元素数
 最坏情况：$\Theta (n)$
简单一致散列（simple uniform hashing）：假定任何元素散列到 m 个槽中每一个的可能性是相同的，且与其他元素被散列到什么位置是独立无关的

定理：对于一个用链接技术来解决碰撞的散列表，在简单一致散列的假设下，一次不成功查找的期望时间为$\Theta(1+\alpha )$
定理：对于一个用链接技术来解决碰撞的散列表，在简单一致散列的假设下，平均情况下一次成功的查找需要$\Theta(1+\alpha )$
于是，一次查找的时间为$\Theta(1+\alpha )$
结论：如果散列表中的槽数至少与表中的元素成正比，所有的字典操作都只要$O(1)$来完成

###Hash function
好的散列函数满足简单一致散列的假设
把关键字解释为自然数

除法散列法：
$h(k)=k\ mod\ m$
m 不应该是2的幂
m 常常是与2的整数幂不太接近的质数
除法在计算机中操作循环太多

乘法散列法:
$h(k)=\lfloor m(kA\ mod\ 1)\rfloor$
m 一般取2的某个幂($m=2^p$，p 为某个整数)
计算机字长为 w 位
![Alt text](http://i.imgur.com/sLsIli8.png)

$A\approx (\sqrt5-1)/2=0.618\ 033\ 988\ 7...$

###Open addressing
把所有元素都存放在散列表中，不用额外的指针节约空间，查找元素时，要检查所有的表项，知道找到所需的元素，或发现元素不在表内。这种方法散列表可能被填满，然后不能插入新的元素

装载因子$\alpha$不超过1

探查寻列
![Alt text](http://i.imgur.com/pMMiYLK.png)

开放寻址中，对于散列元素的删除很困难。当使用特殊的值 DELETED 时，查找时间就不再依赖装载因子$\alpha$了，所以在删除关键字的应用中，往往采用连接法来解决碰撞

三种技术
1. 线性探查
* 每次探查哈希之后的下一个
* 简单快速
* 有集群问题，当连续的槽被填满时，查找时间会增加

2. 二次探查
* 有二次集群问题

3. 双重散列
* 是开放寻址方的最好方式
* $h(k,i)=(h_1(k)+ih_2(k))mod\ m$
* 两个哈希函数，每次增加第二个哈希函数的值
* 为了能查找整个散列表，$h_2(k)$要与表的大小 m 互质
* 双重散列用了$\Theta(m^2)$中探查序列，线性探查或者二次探查用了$\Theta(m)$次，所以后者更好

分析
定理：给定一个装载因子为$\alpha =n/m<1$的开放寻址散列表，再一次不成功的查找中，期望的探查数至多为$1/(1-\alpha)$
推论：给定一个装载因子为$\alpha =n/m<1$的开放寻址散列表，再插入操作，需要为$1/(1-\alpha)$探查
定理：给定一个装载因子为$\alpha =n/m<1$的开放寻址散列表，再一次成功的查找中，探查数为
$$\dfrac {1}{\alpha}In\dfrac {1}{1-                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            \alpha}$$


##Tree

###Binary Search Trees
中序遍历：按顺序输出，复杂度$\Theta (n)$
定理：对于树高 h 的二叉查找树，动态集合操作 search,min,max,successor,predecessor,insert,delete 运行时间为$O(h)$
定理：一棵在 n 个关键字上随机构造二叉查找树的期望高度为$O(lgn)$

###Red-Black Trees
* [尾递归](http://zh.wikipedia.org/wiki/%E5%B0%BE%E8%B0%83%E7%94%A8):栈优化
* [july红黑树](https://github.com/julycoding/The-Art-Of-Programming-By-July/blob/master/ebook/zh/03.01.md)
* [动画](https://www.cs.usfca.edu/~galles/visualization/RedBlack.html)
红黑树，一种二叉查找树，但在每个结点上增加一个存储位表示结点的颜色，可以是Red或Black。
通过对任何一条从根到叶子的路径上各个结点着色方式的限制，`红黑树确保没有一条路径会比其他路径长出俩倍`，因而是接近平衡的。
红黑树虽然本质上是一棵二叉查找树，但它在二叉查找树的基础上增加了着色和相关的性质使得红黑树相对平衡，从而保证了红黑树的查找、插入、删除的时间复杂度最坏为`O(lgn)`。

####Properties of red-black trees

* 每个结点要么是红的要么是黑的。  
* 根结点是黑的。  
* 每个叶结点（叶结点即指树尾端NIL指针或NULL结点）都是黑的。  
* 如果一个结点是红的，那么它的两个儿子都是黑的。  
* 对于任意结点而言，其到叶结点树尾端NIL指针的每条路径都包含相同数目的黑结点。 


####Insert

* 如果插入的是根结点，因为原树是空树，此情况只会违反性质2，所以直接把此结点涂为黑色。
* 如果插入的结点的父结点是黑色，由于此不会违反性质2和性质4，红黑树没有被破坏，所以此时也是什么也不做。

但当遇到下述3种情况时：
* 插入修复情况1：如果当前结点的父结点是红色且祖父结点的另一个子结点（叔叔结点）是红色
* 插入修复情况2：当前结点的父结点是红色,叔叔结点是黑色，当前结点是其父结点的右子
* 插入修复情况3：当前结点的父结点是红色,叔叔结点是黑色，当前结点是其父结点的左子

####Delete

> 在删除结点后，原红黑树的性质可能被改变，如果删除的是红色结点，那么原红黑树的性质依旧保持，此时不用做修正操作，如果删除的结点是黑色结点，原红黑树的性质可能会被改变，我们要对其做修正操作。那么哪些树的性质会发生变化呢，如果删除结点不是树唯一结点，那么删除结点的那一个支的到各叶结点的黑色结点数会发生变化，此时性质5被破坏。如果被删结点的唯一非空子结点是红色，而被删结点的父结点也是红色，那么性质4被破坏。如果被删结点是根结点，而它的唯一非空子结点是红色，则删除后新根结点将变成红色，违背性质2。

六种情况：
如果是以下情况，恢复比较简单：
* 当前结点是红+黑色
解法，直接把当前结点染成黑色，结束此时红黑树性质全部恢复。
* 当前结点是黑+黑且是根结点， 解法：什么都不做，结束。
但如果是以下情况呢？：

* 删除修复情况1：当前结点是黑+黑且兄弟结点为红色(此时父结点和兄弟结点的子结点分为黑)
* 删除修复情况2：当前结点是黑加黑且兄弟是黑色且兄弟结点的两个子结点全为黑色
* 删除修复情况3：当前结点颜色是黑+黑，兄弟结点是黑色，兄弟的左子是红色，右子是黑色
* 删除修复情况4：当前结点颜色是黑-黑色，它的兄弟结点是黑色，但是兄弟结点的右子是红色，兄弟结点左子的颜色任意

###Augmenting Data Structures

在许多应用中，要求数据结构要有所创新，但是很少创造全新的数据结构，所以可以对已有的数据结构进行扩张

####RB Tree：Dynamic order statistics
第九章中，使用快排能再 O(n)时间内找到第 i 个顺序统计量
但是用红黑树可以在O(lgn)内找到，一个元素的排序也能再 O(lgn)内找到

扩展红黑树，增加一个 seiz 域，表明这个节点 子树个数，包括自己
![Alt text](http://i.imgur.com/ldyxvwe.png)
* 检索具有给定排序的元素，O(lgn)
* 确定一个元素的 rank，中序遍历思想 O(lgn)
* 对子树规模的维护，每次维护都是O(1) 

####How to augment a data structure
四个步骤
1. 选择基础的数据结构
2. 确定要在数据结构中添加哪些信息
3. 验证可用基础数据结构上的基础修改操作来维护这些新添加的信息
4. 设计新的操作

红黑树：
* 提供 size 域
* 插入和删除
* 新操作 OS-SELECT和 OS-RANK

####Interval trees
一个红黑数上存储的是一个区间，以最小额时间来排序，增加一个 max，显示子树中端的最大值
![Alt text](http://i.imgur.com/LQ7gOLz.png)

步骤
1. 基础数据结构为红黑树
2. 附加信息 max
3. 对信息维护，插入和删除操作
4. 添加新的操作，interval-search，找出 T中覆盖区间 i 的哪个节点，如果不存在就返回 nil

###B Tree

####Properties
降低磁盘I/O操作次数，数据库中经常使用，和红黑树类似
与红黑树不同：B树的结点有很多子女
B树算法在主存中只要一定量的页数，所以主存的大小不限制可被处理的B树的大小
一个结点的大小通常相当于一个完整的磁盘页，所以，一个B树结点可以拥有的子女数就有磁盘页的大小决定
选取一个大的分支因子，可以大大降低树高，降低了磁盘的存取次数

####Definition
![Alt text](http://i.imgur.com/yJSAYoc.png)
![Alt text](http://i.imgur.com/oRCERAb.png)

树高：![Alt text](http://i.imgur.com/61lq7iC.png)
和红黑树比较：高度都是(lgn)，但是B树要少lgt的因子
####Search
和二叉搜索树差不多
树高不一样更短，但是一个结点内的搜索长度更长

####Insert
要分裂来增加树高
碰见满的就要分裂

####Delete
对结点x递归调用B-TREE-DELETE后，x的关键个数都至少等于最小度数t，而不是t-1
3个类别，6种情况书中 P274

###skip list
在一个排序的链表中，做一次的搜索复杂度是 O(n),因为要一次一次的进行搜索，但是用了跳跃表后可以将删除，插入，搜索做到 log(n),

创建多个辅助的表，每一个表示下面一层表的一半
![Alt text](http://i.imgur.com/iqhvt3a.png)
每一次插入就有50%几率往上一层添加元素，这样能保持跳跃表

[跳跃表资料](http://blog.csdn.net/zy825316/article/details/22600003)

##Dynamic Programming

###装配线问题
比较简单，一共两个子问题，

###Matrix-chain multiplication
矩阵相乘中不同的排列组合得出的乘法量不同
[代码](https://github.com/liulin2012/algorithm/blob/master/15/matrix.cpp)

###LCS


###Four steps
* 描述最优解的结构
* 递归定义最优解的值
* 按自底向上的方式计算最优解的值
* 由计算的结果构造一个最优解

先自顶向下递归定义最优解，然后自底向上来寻找这个最优解。
第四步如果不需要路径可以省略。需要额外空间存储。

###Elements of dynamic programming
* 最优子结构
当一个问题拥有最优子结构，就提示可以用DP
寻找最有子结构：
1. 问题的一个解可以是做一个选择。做这种选择会的到一个或者多个有待解决的子问题
2. 假设对一个给定的问题，已知的时一个可以导致最优解的选择，不必关心如何确定这个选择，假定是已知 。
3. 选择后，确定哪些子问题会随之发生。
4. 利用剪贴技术证明
最优子结构
1. 有多少子问题被使用
2. 在决定一个最优解时又多少选择
3. 时间依赖于两个的乘积

* 重叠子问题
原本的递归解有很多重复的子问题，而不是产生新的子问题
* 备忘录（递归）
重复的子问题用备忘录来记录

###Greedy
动态规划的一种，要证明递归的任一阶段，最优的选择总是贪心选择，所以贪心选择是安全的，通过贪心选择，子问题（除了一个）都为空

例子：0-1背包和部分背包
赫夫曼编码：取出词频最小的组合
使用最小堆时间复杂度O(nlgn)

###Amortized Analysis

执行一系列数据结构操作所需要的时间是通过对执行的所有操作的平均而得出的。
即使单一的操作有较大代价，平均代价还是很小的

动态增长数组
单一操作：增加一个元素，增加一倍长
平均：增加一个元素，但是不用增加原数组一倍长

* 聚集分析(aggregate analysis)
对于所有 n，由 n 个操作所构成的序列的总时间在最坏情况下为 $T(n)$，因此这个操作的平均代价为$T(n)/n$
* 记账方法
赋予不同的操作不同的费用，某些操作的费用比他们的实际代价或多或少
* 势能方法(potential method)

MTF 自组织表，在链表中，总是把刚被访问的元素放到链表头上
[竞争性分析](http://blog.csdn.net/onyheart/article/details/16331219)

##Fibonacci number

![Alt text](http://i.imgur.com/YJV8kYb.png)

用两个值互相加就行。
O(1) 空间复杂度

斐波那契数列:
1. 递归求和，指数级
2. 线性:两个互相相加，复杂度$O(n)$
3. 利用公式:  $$ a_n = \dfrac{\sqrt{5}}{5}\cdot\left [\left( \dfrac {1 + \sqrt{5}}{2} \right)^n - \left( \dfrac {1 - \sqrt{5}}{2} \right)^n \right]$$
**但是无法实现，因为计算机中float形会丢失精度**
4. 利用矩阵
$$\begin{pmatrix}F_{n+1} & F_n\\F_n & F_{n-1}\end{pmatrix}={\begin{pmatrix} 1 &1 \\ 1 & 0 \end{pmatrix}^n}$$
矩阵连乘，所以用分治法复杂度为$O(lgn)$

###Fibonacci heap
很好的支持 union 操作，如果不需要 union，普通的二叉堆的性能就很好。
斐波那契堆的时间是平摊时间，不是最坏时间。
![](http://i.imgur.com/kP1zC0U.png)

所有的操作都延后到 EXTRACE-MIN 的时候进行，非常复杂。

##Data Structures for Disjoint Sets
两个重要操作：
找出给定的元素所属的集合
合并

![](http://i.imgur.com/HNRUJA2.png)

###Linked-list representation of disjoint sets
每个集合都用一个链表表示，每个链表的第一个对象作为所在集合的代表。链表中的每一个对象都包含一个集合成员，一个指向包含下一个集合成员的对象的指针，一级指向代表的指针。

![](http://i.imgur.com/39X3bNU.png)

用加权的方法
定理：利用不相交集合的链表表示和加权合并启发式，一个包含 m 个 MAKE-SET，UNION 和 FIND-SET 操作，哪个 MAKE-SET 操作，的序列 需要$O(m+nlgn)$

###Disjoint-set forests
目前已知最快的不相交集合的数据结构
每棵树都代表一个集合，每棵树的根包含了代表，并且它是自己的父节点

![](http://i.imgur.com/aDkYAUI.png)

MAKE-SET：创建一个结点的树
FIND-SET：沿着父节点一直找到树根
UNION：让一棵树的树根指向另一棵树的根

union by rank:包含较少结点的树的根指向包含较多结点的树的根
path compression:FIND-SET 操作中让查找路径上的每一个结点都直接指向根节点。路径压缩不改变 rank
![](http://i.imgur.com/Yidqkow.png)

UNION 时，看 rank 是不是相等，相等就加1，不相等就用rank 大的作为父节点，rank 不变。

最坏情况$O(ma(n))$，$a(n)$增长非常缓慢 <=4	

##Graph
有向图G是一对(V,E)
无向图不允许自身环

无向图度
有相图：出度+入度

简单路径：如果路径上各顶点不重复，那就是简单路径
回路和简单回路

无向图：连通图，连通分支
有相图：互相可达，强连通图，强连通分支

###Representations of graphs

邻接表：稀疏
$Storage:\theta (V+E)$

邻接矩阵：稠密
$Storage:\theta (v^2)$

![](http://i.imgur.com/Eoq0X5k.png)

###BFS
典型算法，prim最小生成树，Dijkstra单元最短路径

给定图G和一个特定的源项点s的情况下，广度优先搜索系统地搜索G中的边，以期发现可从s到达 所有顶点，并计算s到所有顶点可达顶点之间的距离。该算法同时生成一棵树，包括所有s的可达顶点的广度优先树。
该算法对有向图和无向图都有用。
无向图例子：prim
有向图例子：Dijkstra

广度优先：始终是将已发现和未发现顶点之间的边界，沿其广度方向向外扩展。
广度优先树可能会有所不同，但是计算出来的距离一定是相同的

![](http://i.imgur.com/h6dGhtH.png)

时间复杂度使用聚集分析，每个顶点入队列和进队列各一次，所以是$O(V)$，每一个邻接表都在顶点出队列时被扫描，所以是$O(E)$.
所以加上初始化，BFS总的运行时间为$O(V+E)$
tips，这和计算Dijkstra算法有点类似，但是Dijkstra用的时优先权队列，所以复杂度更高。

和Dijkstra的区别
1. 不是优先权队列，而是普通的FIFO队列，所以复杂度更低
2. relax的过程是+1，而不是加权。

广度优先树：
BFS过程中的$\pi$来构建
![](http://i.imgur.com/G7nvTGo.png)

###Single-Source Shortest Paths
边的权值可以代表各种，路径，时间等

单元最短路径的变种
* 单终点最短路径问题：找出每个顶点v到指定终点t的最短路径。把图中每条边反向，就是单源最短路径了
* 单对顶点最短路径问题：给定u和v一条最短路径。还是利用单源最短路径，没有更好的算法
* 每对顶点间的最短路径问题：不必对每个顶点都做单源最短路径

最优子结构：
一条两顶点间的最短路径包含路径上其他的最短路径，这事动态规划和贪心的标志
Dijkstra是贪心，Floyd-Warashall是动态规划

定理：最短路径的子路径也是最短路径

负权值边：
如果存在负权值**回路**，那就没有最短路径

Dijkstra算法假定所有的边都是非负权值。
Bellman-Ford允许负权值，只要不存在负权回路。存在的话，算法也可以检测并报告

最短路径的表示和BFS一样，但是用的不是边而是权值表示。

松弛技术：描述最短路径上权值的上界，松弛之后会让权值变小，最后的权值就是最短路径的权值
Dijkstra以及有向无回路的最短路径算法中，对每一条边执行一次松弛操作
Bellman-Ford执行多次
![](http://i.imgur.com/M2QuP5L.png)
![](http://i.imgur.com/4R784tj.png)

###Dijkstra

一种BFS
要求权值非负
用到最小优先权队列，初始化为所有顶点，每次取出一个顶点然后更新别的顶点权值。和prim还有BFS一样的思想
取出顶点为EXTRACT-MIN，更新别的顶点权值为DECRASE-KEY。这和数据结构的使用还有复杂度有关
![](http://i.imgur.com/b5hXibY.png)

算法复杂度分析：
一共需要V次EXTRACT-MIN和INSERT。最多E次DECRASE-KEY。Fib heap就是利用了这个性质，因为DECRASE-KEY的次数一般远大于EXTRACT-MIN。
数组：$O(V^2)$
二叉最小堆：$O((V+E)lgV)$
斐波那契堆：$O(E+VlgV)$

###DFS

DFS产生的先辈子图可以由几棵树组成。BFS就一棵树
形成的叫做深度优先森林
还为每个顶点加盖时间戳。每个顶点有两个时间戳，开始和结束的

![](http://i.imgur.com/W55oMAF.png)

时间复杂度：$\theta (V+E)$

性质
* 先辈子图形成的森林，准确反映了递归调用的顺序
* 括号定理
* 后裔区间的嵌套
* 白色路径定理

![](http://i.imgur.com/LgHqClm.png)

边的分类：
* 树边：白色
* 反向边：灰色
* 正向边：黑色
* 交叉边：黑色

无向图中，只有树边和反向边

###Topological sort

dag：有向无回路图
可以说明事物的发展顺序

![](http://i.imgur.com/UyHJiY8.png)
按照**完成的时间**进行**降序**排序
定理：一个有向图G是无回路的，当且仅当G进行深度优先搜索时没有得到反向边

[拓扑排序的原理及其实现](http://blog.csdn.net/dm_vincent/article/details/7714519)
[wiki](http://en.wikipedia.org/wiki/Topological_sorting)

###Single-source shortest paths in DAG
先对图进行拓扑排序，然后计算最短路径，复杂度线性$\theta (V+E)$
![](http://i.imgur.com/i7Ny2ZR.png)

###强连通分支

把一个有向图分解为各强连通分支。
分解之后，算法在每一个强连通分支上独立运行
最后根据各个分支之间的关系，把所有的解组合起来。

G和G的转制有同样的强连通分支
![](http://i.imgur.com/gouGGoe.png)
![](http://i.imgur.com/MTKrEiI.png)
第一次DFS之后，按照**完成**时间降序进行排序，转制G，然后执行DFS。其实就是通过一次拓扑排序的顺序来进行DFS。但是转制之后G，如果不是连通的话是无法到达，但是如果是强连通的话，还是依然可以到达。这样就可以形成不同的强连通分支。模拟拓扑排序来理解算法。

###Minimum Spanning Trees
生成树：无向图中，T无回路且连接所有的顶点，必然是一棵树。
![](http://i.imgur.com/frXembU.png)

循环不变式：
![](http://i.imgur.com/ntBSh87.png)

一个 MST 删去一条边，形成的两个子树还是 MST。
两个cut 之间选能保证无回路。
选出来的局部最优也是全局最优可以证明23.1
推论23.2为后面的算法用到
P345

两种实现的算法
In Kruskal’s algorithm, the set A is a forest whose vertices are all those of the given graph. The safe edge added to A is always a least-weight edge in the graph that connects two distinct components. 
In Prim’s algorithm, the set A forms a single tree. The safe edge added to A is always a least-weight edge connecting the tree to a vertex not in the tree.

####Kruskal

利用不相交森林，每一个顶点起初都是一个森林，边按照权值来进行非降序排列。
每次取出一个权值最小的边，看两个顶点是否为在一个森林中，不在的话就union在一起。
不相交森林的实现在之前的章节有，利用了rank和压缩路径来进行，能将算法的复杂度降低。
复杂度：$O(ElgV)$
![](http://i.imgur.com/8AK4lcF.png)

####Prim

每一步，把连接了树A和GA 中的某孤立顶点的轻边加入到树A
有效实现prim算法的关键是设法容易地选择一条新的边，将其添加到由Ade边所形成的树中。
不在树中的所有顶点都放在一个基于key域的最小优先权队列Q中。
复杂度取决于使用的数据结构
二叉堆:$O(ElgV)$
斐波那契堆:$O(E+VlgV)$
![](http://i.imgur.com/O0t98jp.png)·

###Bellman-Ford
可以有负值
会报告负值回路
![](http://i.imgur.com/sflETMi.png)
2到4行算权值
5到7行判断有没有负权回路
操作复杂度：$O(VE)$

检验边时可以以任意顺序，这样就不需要知道全局的情况。分布式系统中只要和相邻的端尿性交互信息就行，比如说网络中的寻找最短路径，每个机器不需要知道所有机器和路径的情况，只要知道最近的就行。但是网络中要解决路径权值增加或者减少的问题，因为网络会堵塞或者空闲

####Difference constraints and shortest paths
线性规划中，要寻找最大值，但是有时候只想知道可行解，或者线性规划有一些特殊情况，可以达到多项式复杂度的解法。

差分约束：线性规划A的每一行包含一个1和个-1，所有其他元素为0.找出满足要求的解或者知道无可行解
相似处有两点：
* 差分约束条件和最短路径中的松弛是一个道理
* 约束图中有负权回路，差分约束无解
![](http://i.imgur.com/ImDaow7.png)
![](http://i.imgur.com/MnZW1Rp.png)

###All-Pairs Shortest Paths
对于无权边，用V次BFS。$O(V^2+VE)$
对于没有负权值的有向图，用V次Dijkstra。$O(VE+V^2lgv)$
对于有负权值的，可以进行改进，V次Bellman-Ford太费时间。

####Shortest paths and matrix multiplication
复杂度：$\theta (n^3lgn)$
利用矩阵的n次方可以分治法来进行相乘。因为等式满足结合律

####Floyd-Warshall
复杂度：$\theta (V^3)$
利用另一种DP思想来进行求解
![](http://i.imgur.com/j4gW6FJ.png)

####Johnson
复杂度：$O(V^2 lgV + VE)$
结合了Dijkstra和Bellman-ford，复杂度和用Dijkstra一样。
主要思想是，利用重新赋值权值，而且不会改变最短路径的引力
这样将权值进行修改之后没有负数权值，这样就可以用Dijkstra,所以复杂度和V次Dijkstra一样
![](http://i.imgur.com/a7Bqjwg.png)

先用Bellman-Ford，然后通过计算出来的结果来重赋权值得到非负权值，执行V次Dijkstra来获得最后的结果

###Maximum Flow
定义：比如物流问题，每一个城市都有出货 能力，如何按照一定的速度生产产品能和运输的时间匹配上,最后的目的是让运输量最大。源点和汇点之间找出之间的最大流
流的定义：进入运输网络中间某个城市的速度，必须等于它们离开该城市的速度，否则会堆积，这就是流守恒
具有多个源点和多个汇点的网络和单源点，单汇点是一样的
![](http://i.imgur.com/B9rzBno.png)

####Ford-Fulkerson
总思想：起初都为0，每次迭代，寻找一条**增广路径**。增加路径可以看做是源点s到汇点t之间的一条路景，沿着这个路径可以压入更多的流。反复这个过程直到不存在增广路径了，说明压不进更多的流了。最大流最小割定理说明算法结束时，能计算出最大流。

残留网络：给定一个网络和一个流，残留网络由可以容纳更多网络流的边组成
![](http://i.imgur.com/aEoaTtI.png)
阴影是增广路径

增广路经：给定网络Ghetto流f，增广路径p是残留网络G中从s到t的一条简单路径

证明一个流是最大流，当且仅当它的残留网络不包含增广路径。
![Imgur](http://i.imgur.com/2IAO8zo.png)
初始化为0，每次找一条增光路经，然后更新。直至找不到增广路径。
例子书上有

如何找增广路径很重要，选不好算法可能不会停止。如果采用BFS来选择增广u，算法的运行时间是多项式复杂度的。
复杂度分析：while循环最多执行$|f*|$次，因为每次迭代，流的值最少增加1.$|f*|$是算法找出的最大流。
所以复杂度为$O(E|f*|)$
$|f*|$小时算法速度还不错，如果很大时，比如特例情况
![Imgur](http://i.imgur.com/Evmfu9F.png)

第四行用广度优先搜索来对增广路径进行计算。能够改进方法复杂度。这种实现叫做Edmonds-Karp算法
复杂度：$O(VE^2)$

####Maximum bipartite matching
定义：无向图中，左边 右边的匹配要最多
对于一个无向二分图。建立流网络，对它进行Ford-Fulkerson方法，根据求得的具有整数值的最大流f，可以直接获得最大的匹配M
![Imgur](http://i.imgur.com/gVvg6Rb.png)

##Matrix Operations

矩阵的基本操作加减乘，逆矩阵，解线性方程式
矩阵的操作具有数值稳定性，因为大了之后如果不稳定float型会不准。
向量vector就是数字的一维矩阵

strassen乘法算法的复杂度从$O(n^3)$到了$O(n^{lg7})$
最好的已知是$O(n^{2.376})$

###解线性方程式
最简单的方法就是[高斯消元](https://www.wikiwand.com/en/Gaussian_elimination)，但是只能做一次，所以基本的方法是LUP分解，数值稳定而且速度快，算出来之后可以做任何Ax=b

L是单位下三角矩阵
U是上三角矩阵
P是一个置换矩阵

优点就是当A进行了LU分解之后就可以通过公式来求解方程：正向替换和逆向替换
![](http://i.imgur.com/KVjWUDH.png)
![](http://i.imgur.com/DMoNCt8.png)

如何进行LUP分解，用到了高斯消元，之所以要用到P事为了选取更大的主元，这样就不会有精度问题，也不能让主元为0，full pivoting是row和column都换，partial是换row。
所以主要是进行lu分解
![](http://i.imgur.com/GPAzACk.png)

分解完了之后就能通过之前的正向替换和反向替换来求解**任何**Ax=b，包括求逆矩阵
求逆矩阵就是每次都求逆矩阵中的一列

复杂度分析：
LUP分解是：$O(n^3)$
分解完之后任何线性方程式都是:$O(n^2)$

对于特殊矩阵比如三对角矩阵tridiagonal matrix所有操作都是$n$

##Classic Topics

###Editor distance
http://wilbeibi.com/2015/05/2015-05-09-K_edit_distances/
https://gist.github.com/Wilbeibi/46afba947d929cc7a0c8
http://www.zhihu.com/question/29592463
##Tools

[可视化](https://www.cs.usfca.edu/~galles/visualization/)
[排序](http://www.sorting-algorithms.com/)
[latex:Mathematics](http://en.wikibooks.org/wiki/LaTeX/Mathematics)



