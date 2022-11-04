# 高级数据结构课程笔记 

数据结构 ： 1. 存什么数据 2. 操作

## L2 BST

S. : a set of n real values 

* one-one mapping bettwen element(key) and note
* key(u)
* level 
* height : level  + 1

1. Insert / delete 
2. predecessor :  最大的 <= Q的元素  predecessor(Q)  类似于 STL 的 lower_bound 和 upper_bound  （log(n)

3. successor : 最小的 >= Q的元素 
4. range reporting。  I = [x, y]  返回 S 并 I  ![image-20221019203134966](my_note.assets/image-20221019203134966.png)

5. 最大/最小。(log(n) （最左边和最右边) 

6. **split** x 从 x 这个位置把 tree 切开分成两棵树 (logn)

   ![image-20221019203413728](my_note.assets/image-20221019203413728.png)

7. 把两颗满足要求的子树组合成一棵子树 (logn)

   ![image-20221019203746186](my_note.assets/image-20221019203746186.png)

**一棵树有 x 个节点那么一定会有 x+1个虚拟节点。每一个虚拟节点代表了一段 interval. （9个节点将一个无线的线段划分为了10份)** 

slap(x) 可以简单理解为 x 为根的子树对原有的树来说的范围。比如 slap(40) = [20, 50) 也就是这颗子树的所有节点的值都在这个范围里 

 虚拟节点也可以计算 slap , 虚拟节点的 slap 也就是物理节点划分得到的 interval 。（感觉区间树)

![image-20221019210021052](my_note.assets/image-20221019210021052.png)



很关键没有祖先关系的节点 slap 是 不相交的

count (log(n))

range counting.  返回符合范围的元素个数  log(n) (使用 slap来证明)

![image-20221019213104898](my_note.assets/image-20221019213104898.png)

split可以转化为join的操作

![image-20221020211305926](my_note.assets/image-20221020211305926.png)e

2和3看成一棵树

把 split转化为join 

![image-20221020211953308](my_note.assets/image-20221020211953308.png)

两个最小的高度的树开始 join 按照最差的情况

![image-20221020212148273](my_note.assets/image-20221020212148273.png)

![image-20221020212228498](my_note.assets/image-20221020212228498.png)

## L3 interval tree

 ![image-20221024200959202](my_note.assets/image-20221024200959202.png)

![image-20221024201108526](my_note.assets/image-20221024201108526.png)

How to deal with classical problems 

提出这个方法的原始的idea 

![image-20221024205256631](my_note.assets/image-20221024205256631.png)

对于一个值， 有 3 种interval : 

1. interval 都在 q 右边
2. 都在 q 的左边 
3. 可能包含q 

![image-20221024205746740](my_note.assets/image-20221024205746740.png)

可以忽略蓝色的 

黑色的 查找 stabbling list  O(1+ku)

![image-20221024210136762](my_note.assets/image-20221024210136762.png)

只需要查找 leftend point 即可 因为 一定是包含 节点的 节点  > q 

![image-20221024210438089](my_note.assets/image-20221024210438089.png)

处理红色的部分 ： 递归地查找下一个节点。 

递归看看你的处理是否符合所有的场景 (senario )

时间复杂度： 

![image-20221024211122714](my_note.assets/image-20221024211122714.png)

no interval will be reported twice 

![image-20221024211208112](my_note.assets/image-20221024211208112.png)

![image-20221024211221554](my_note.assets/image-20221024211221554.png)

## SEG-TREE 

space O(nlgn)

qry  O(lgn + k) 

[x, y)

![image-20221103200943040](my_note.assets/image-20221103200943040.png)

每一个节点挂着 分割后的 intervals (log(n)) 标准化分割引理

![image-20221103201133647](my_note.assets/image-20221103201133647.png)

直接把路径上 节点的 set report  S(u)  

1. 路径上的每一个节点 u slap(u) 都包含 q (查询的值)

对于节点 u, S(u) 里的每一个 interval 都包含  slab(u) ， 并且 slab(u) 一定包含 q。所以只需要把搜索路径(log(n))节点上的 S(u ) 打印出来即可。因此搜索路径一定包含q

interval 只会被report 一次。因为 interval 被划分为不相邻的多个 slap, 如果在同一个路径上出现了两次，那么说明这个interval 被划分的这两个slap 重叠了，这不符合lema的要求。（一条路径上，祖先节点的slab 包含孙子节点的 slab） 

却别，上一个 interval tree 是吧 interval 挂在节点上，每一个interval 只能挂一次。而 segment tree 把 interval 挂在 partition 后的节点集上可以出现多次。（这两个idea可能需要再仔细体会一下)

## kd-tree 

BST可以解决一维的查找

![image-20221103205031547](my_note.assets/image-20221103205031547.png)

时间和空间的tradeoff。 point-based structure ，这种 tradeoff 已经是最好的，不可能达到 log(n)的时间复杂度

![image-20221103205501145](my_note.assets/image-20221103205501145.png)

(右边是 d 维)

general posistioon assumption (没有两个点有相同的 x 或者 y)

n个点可以找到一条线 l. 把这 n 个点 尽量均匀地分成两份。

垂直分->水平分->垂直分...

(第一刀是垂直分)

![image-20221103210605703](my_note.assets/image-20221103210605703.png)

![image-20221103210901239](my_note.assets/image-20221103210901239.png)

minimum bounding rectangle (MBR) 最小的平行正方形能够覆盖这颗树上的**所有点**。 （感觉和 slab的概念优点像但是不完全一致) 

给定 范围 q 

搜索策略，只搜索MBR和q有交集的节点。

![image-20221103211911738](my_note.assets/image-20221103211911738.png)

这种搜索策略的时间复杂度是  $O(\sqrt{n} + k)$

证明 : 

![image-20221103213043267](my_note.assets/image-20221103213043267.png)

有个很有意思的思路，先考虑极端情况 + 递归 然后就可以用 master 定理证明了 。

![image-20221103213626670](my_note.assets/image-20221103213626670.png)

如果 q 是水平线和垂直线类似

每条线 intersects  $O(\sqrt(n))$个点

![image-20221103214613108](my_note.assets/image-20221103214613108.png)

type1 ，MBR至少和q其中的一条边代表的直线一定会相交

type2 很简单因为已经包含了，只需要把这棵树下的所有的叶子节点 report 就行了。O(k)