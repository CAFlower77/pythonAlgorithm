### Chapter 2	算法基础

#### 一、插入排序

```python
def INSERTION_SORT(A):
    for j in range(1, len(A)):
        key = A[j]
        i = j - 1
        while i >= 0 and A[i] > key:
            A[i + 1] = A[i]
            i = i - 1
        A[i + 1] = key
    return A
```

排序在没有特殊说明的情况下将为

- 循环不变式：在一个循环中恒为真的命题，算法的正确性来自于循环不变式的证明
  - 初始化：在循环第一次迭代之前，循环不变式为真
  - 保持：对于循环内的每次迭代，能够通过上一次迭代的循环不变式推出当前迭代的循环不变式
  - 终止：循环终止时，循环不变式在计数器最终迭代的值时成立

根据数学归纳法可以看出，循环不变式的证明就是证明循环的初始化、保持、终止。

> 排序算法的证明：
>
> - *循环不变式：*$A[0..j-1]=[A[0]..A[j-1]]$ 对 $j\in[1..A.length-1]$ 已排序
>
>   - *初始化：*$j=1$ 时循环不变式的数组为 $[A[0]]$，满足循环不变式
>
>   - *保持：*这里需要给出 `while` 函数的*循环不变式：*
>
>     $A[0..i]$ 已排序对$i\in[j-1..0]$成立，且 $A[i]=A[i+1]$ 对 $i\in[j-2..0]$ 成立
>
>     首先考虑 $A[i]>key=A[j]$ 的情况
>
>     - *初始化：*$i=j-2$ 时循环前 $A[0..i]=A[0..j-2]$ 作为$A[0..j-1]$的子数组必然已排序，且此时 $A[i]=A[j-2],A[i+1]=A[j-1]$，而在 $i=j-1$ 循环中 $A[i+1]=A[i]$ 即 $A[j]=A[j-1]$，此时应该有 $A[j-2]=A[j-1]$。
>
>     - *保持：*若 $A[0]<A[1]<A[2]<\cdots<A[i-1]<A[i]=A[i+1]$，执行代码后数组变成$A[0]<A[1]<A[2]<\cdots<A[i-1]=A[i]=A[i+1]$ 
>
>       更新指标 $i$ 后为 $A[0]<A[1]<A[2]<\cdots<A[i-2]=A[i-1]=A[i]$。
>
>     - *结束：*$i=-1$ 时跳出循环，此时数组为空串。
>
>     可以看出，为了证明保持，我们必须证明 `while` 循环的循环不变式的上述结论在 $j$ 变成 $j+1$ 时已然成立，循环和结束与 $j$ 无关，我们只要证明初始化中 $A[j-2]=A[j-1]$ 这个等式能够在 $j$ 变成 $j+1$ 时依然成立即可，在 `for` 循环的 $j$ 迭代时我们执行了代码 `A[j]=key` ，这说明在进入 $j+1$ 循环前就有 $A[j]=A[j-1]=key$ ，此时按照 `while` 循环的初始化应该有 $A[j-2]=key$ ，现在进入 `for` 循环的 $j+1$ 迭代，此时 $A[j+1]=key$ ，此时的 $A[j-2]$ 应该是 $j$ 迭代时的 $A[j-1]=key$ ，那么我们就证明了 $A[j-2]=key$，故 `while` 循环的证明到此结束，而第8行代码把 $key$ 的值赋给跳出循环的 $A[i]$，此时 $i$ 应该为 $A[i]\leq key=A[j]$ 的最大 $i$ 指标，这已经说明 $A[0..j]$ 是已经排好序的了，这就证明了保持。
>
>     对 $A[i]<key$ 的情况，无非就是最后少一步赋值而已。
>
>   - *终止：*终止循环时 $j=A.length$，此时 $A[j]$ 无定义 `(list index out of range)`，而 $A[0..j-1]=A[0..A.length]=A$ 已经排好序了。
>
> 由此可知，排序算法 `INSERTION_SORT` 是正确的，QED 。
>
>     

---

下面是插入排序对数组 $A=\langle 5,2,4,6,1,3\rangle$ 作用过程中 `for` 循环的 $j=4$ 迭代的详细情况：

| index                 |         =         |  <   |         0          |         1          |         2          |         3          |        4         |  5   |  >   |               |
| --------------------- | :---------------: | :--: | :----------------: | :----------------: | :----------------: | :----------------: | :--------------: | :--: | :--: | ------------: |
| A                     |         =         |  <   |         5          |         2          |         4          |         6          |       $1$        |  3   |  >   |       $key=1$ |
| 初始化 $i=3$          | $\longrightarrow$ |  <   |      ***2***       |      ***4***       |      ***5***       |      ***6***       |        1         |  3   |  >   | $j=4\ ,\ i=3$ |
|                       |                   |      |                    |                    |                    |  $\uparrow\\ (i)$  | $\uparrow \\(j)$ |      |      |               |
| 循环 $i=3$            |                   |  <   |         2          |         4          |         5          |         6          |        6         |  3   |  >   |         $i=2$ |
|                       |                   |      |                    |                    |  $\uparrow\\ (i)$  | $\uparrow\\ (i+1)$ | $\uparrow \\(j)$ |      |      |               |
| 循环 $i=2$            |                   |  <   |         2          |         4          |         5          |         5          |        6         |  3   |  >   |         $i=1$ |
|                       |                   |      |                    |  $\uparrow\\ (i)$  | $\uparrow\\ (i+1)$ |                    | $\uparrow \\(j)$ |      |      |               |
| 循环 $i=1$            |                   |  <   |         2          |         4          |         4          |         5          |        6         |  3   |  >   |         $i=0$ |
|                       |                   |      |  $\uparrow\\ (i)$  | $\uparrow\\ (i+1)$ |                    |                    | $\uparrow \\(j)$ |      |      |               |
| 循环 $i=0$            |                   |  <   |         2          |         2          |         4          |         5          |        6         |  3   |  >   |        $i=-1$ |
|                       | $\uparrow\\ (i)$  |      | $\uparrow\\ (i+1)$ |                    |                    |                    | $\uparrow \\(j)$ |      |      |               |
| 结束 `A[i + 1] = key` |                   |  <   |         1          |         2          |         4          |         5          |        6         |  3   |  >   |       $key=1$ |

作业答案：

- 2.1.2

  ```python
  def INSERTION_SORT_DOWN(Z):
      for j in range(1, len(Z)):
          key = Z[j]
          i = j - 1
          while i >= 0 and Z[i] < key:
              Z[i + 1] = Z[i]
              i = i - 1
          Z[i + 1] = key
      return Z
  ```

- 2.1.3

  ```python
  def LINEAR_SEARCH(A, v):
      result = []
      for count in range(0, len(A)):
          if A[count] == v:
              result.append(count)
      return result
  ```

- 2.1.4

  ```python
  def BINADD(A, B):
      ans = []
      A.reverse()
      B.reverse()
      for i in range(len(A)):
          if (A[i] != 0 and A[i] != 1) or A[len(A) - 1] == 0:
              return False
          else:
              break
      for i in range(len(B)):
          if (B[i] != 0 and B[i] != 1) or A[len(A) - 1] == 0:
              return False
          else:
              break
      topIndex = max(len(A), len(B)) + 1
      for i in range(topIndex + 1):
          ans.append(0)
      for i in range(len(A), topIndex + 1):
          A.append(0)
      for i in range(len(B), topIndex + 1):
          B.append(0)
      A.reverse()
      B.reverse()
      ifsign = 0
      for i in range(topIndex, -1, -1):
          ans[i] = (A[i] + B[i] + ifsign) % 2
          if (A[i] + B[i] + ifsign) >= 2:
              ifsign = 1
          else:
              ifsign = 0
      ans.reverse()
      i = len(ans) - 1
      while ans[i] == 0:
          ans.pop()
          i = i - 1
      ans.reverse()
      return ans
  ```

#### 二、分析算法

1. 分析算法时用到的基本概念和术语
   1. RAM: Random-Access Machine 随机访问机，一种通用的单处理器计算模型，没有并发操作(比如 Go 语言)
      1. 指令集：只包含常数指令，包括算术指令、数据移动指令和控制指令
      2. 不考虑灰色区域算法(如通过移位计算指数使指数运算时间降到常数时间)
      3. 不对 Cache 和虚拟内存建模
   2. 输入规模 $n$ ：例如 `INSERTION_SORT` 的输入规模为 $n=A.length$
   3. 总运行时间 $T(n)$ ：常数 $c$ 时间指令对 $T(n)$ 的贡献为 $ck$，$k$ 为该指令运行(如循环)次数
   4. 最坏情况：考虑使 $T(n)$ 尽可能大的输入，得到的 $T(n)$ 为最坏情况时间
   5. 增长量级：若 $T(n)$ 为 $p$ 次多项式，则 $T(n)=\Theta(n^p)$ 

2. 分析 `INSERTION_SORT` 算法

   设第 $k\in [1..9]$ 行代码的常数时间为 $c_k$ ，数组 $c=[c_1..c_9]$ 叫做算法 `INSERTION_SORT` 的常数时间表。注意 $c_1=c_9=0$，那么
   $$
   T(n)=c_2n+c_3(n-1)+c_4(n-1)+c_5\sum_{j=1}^{n-1}t_j+c_6\sum_{j=1}^{n-1}(t_j-1)+c_7\sum_{j=1}^{n-1}(t_j-1)+c_8(n-1)\tag 1
   $$
   这里 $t_j$ 为 `while` 循环的执行次数
   $$
   t_j=\begin{cases}1&\text{if}\ A[j-1]\leq key\\j&\text{if}\ A[j-1]>key\end{cases}\qquad j\in[1..n-1]\tag 2
   $$
   那么明显最好情况为 $t_j\equiv1$，此时
   $$
   T(n)=c_2n+c_3(n-1)+c_4(n-1)+c_5(n-1)+c_8(n-1)=\Theta(n)
   $$
   最坏情况为$t_j\equiv j$，此时
   $$
   \begin{split}
   	T(n)&=c_2n+c_3(n-1)+c_4(n-1)+c_5\frac{n(n-1)}{2}+c_6\frac{(n-1)(n-2)}{2}+c_7\frac{(n-1)(n-2)}{2}+c_8(n-1)\\
   	{}&=\Theta(n^2)
   \end{split}
   $$

作业答案：

- 2.2-1
  $$
  \frac{n^3}{1000}-100n^2-100n+3=\Theta(n^3)
  $$
  
- 2.2-2

  ```python
  def min(A):
      minValue = A[0]
      for i in range(len(A)):
          if A[i] <= minValue:
              minValue = A[i]
      return minValue
  # 运行时间：Θ(n)
  
  def find_min(A):
      minIndex = 0
      for i in range(len(A)):
          if A[i] <= A[minIndex]:
              minIndex = i
      return minIndex
  # 运行时间：最坏Θ(n)、最好Θ(1)
  
  def SELECTION_SORT(A):
      for i in range(len(A) - 1):
          B = []
          for j in range(i, len(A)):
              B.append(A[j])
          temp = A[i]
          A[i] = A[i + find_min(B)]
          A[i + find_min(B)] = temp
      return A
  ```

  循环不变式：$A[0..i-1]$ 已排序

  输入规模：$n=A.length=\mathop{\rm len}(A)$

  最坏情况运行时间：$T(n)=(n-1)\cdot\Big(\Theta(1)+(n-i-1)\Theta(1)+\Theta(1)+2\Theta(n)\Big)=\Theta(n^2)$

  最好、最坏、平均运行时间都是 $\Theta(n^2)$

- 2.2-3

  最好：$\Theta(1)$，最坏：$\Theta(n)$，平均：$\Theta(n)$。

#### 三、设计算法

1. 分治算法：分解 $\to$ 解决 $\to$ 合并

2. 归并排序算法 `MERGE_SORT()` ：参数为 $A,p,r$ ，用来排序数组 $A[p..r]$，在 $p\geq r$ 时直接返回原数组

   1. 分解：均分数组 $A[p..r]$ 为$A_1,A_2$
   2. 归并排序 $A_1,A_2$
   3. 合并数组

3. 合并的算法 `MERGE`：

   ```python
   def MERGE(A, p, q, r):
       n1 = q - p + 1
       n2 = r - q
       L = []
       R = []
       for i in range(n1 + 1):
           L.append(None)
       for j in range(n2 + 1):
           R.append(None)
       for i in range(n1):
           L[i] = A[p + i]
       for j in range(n2):
           R[j] = A[q + j + 1]
       L[n1] = float("inf")
       R[n2] = float("inf")
       i = 0
       j = 0
       for k in range(p, r + 1):
           if L[i] <= R[j]:
               A[k] = L[i]
               i = i + 1
           else:
               A[k] = R[j]
               j = j + 1
       return A
   ```

   当然我们也可以通过判断列表是否为空而不是用哨兵

   ```python
   def MERGE(A, p, q, r):
       n1 = q - p + 1
       n2 = r - q
       L = []
       R = []
       for i in range(n1):
           L.append(None)
       for j in range(n2):
           R.append(None)
       for i in range(n1):
           L[i] = A[p + i]
       for j in range(n2):
           R[j] = A[q + j + 1]
       for k in range(p, r + 1):
           if len(L) != 0 and len(R) != 0:
               A[k] = min(L[0], R[0])
           elif len(L) == 0 and len(R) == 0:
               break
           else:
               if len(R) == 0 and len(L) > len(R):
                   A[k] = L[0]
                   L.pop(0)
               elif len(L) == 0 and len(R) > len(L):
                   A[k] = R[0]
                   R.pop(0)
           if len(L) != 0 and len(R) != 0 and L[0] <= R[0]:
               L.pop(0)
   
           elif len(L) != 0 and len(R) != 0 and L[0] > R[0]:
               R.pop(0)
       return A
   ```

   循环不变式(第 $18\sim 24$ 行)：数组 $A[p..k-1]$ 已排序且其前 $k-p$ 个成员为数组 $L[0..n_1]+R[0..n_2]$ 的最小 $k-p$ 个数。

   > - 初始化：$(k=p)\Longrightarrow(A[p..k-1]=\varnothing)$，命题成立
   >
   > - 保持：若 $L[i]\leq R[j]$ ，说明 $L[i]=\min(L+R)$，则 $A[p..k-1]$ 在进入下一次循环时加入 $A[k]=L[i]$ ，对 $L[i]>R[j]$ 同理我们总有 $A[k]=\min(L+R)$，这就证明了保持。
   >
   > - 终止：此时 $k=r+1$，数组 $A[p..k-1]=A[p..r]$ 就是 $L+R$ 的最小 $k-p=r-p+1=n_1+n_2$ 个成员的重排，此时 $L,R$ 只剩哨兵牌 $\infty=\texttt{float("int")}$，终止成立。
   >
   >   QED。

   运行时间的细分表为：

   |    line     |            time            |
   | :---------: | :------------------------: |
   |  $1\sim 1$  |             0              |
   |  $2\sim 5$  |        $\Theta(1)$         |
   |  $6\sim 7$  |     $(n_1+1)\Theta(1)$     |
   |  $8\sim 9$  |     $(n_2+1)\Theta(1)$     |
   | $10\sim 11$ |       $n_1\Theta(1)$       |
   | $12\sim 13$ |       $n_2\Theta(1)$       |
   | $14\sim 17$ |        $\Theta(1)$         |
   | $18\sim 24$ | $(r-p+1)\times 2\Theta(1)$ |

   $$
   T(n)=\Big(2(n_1+n_2)+(r-p+1)+4\Big)\Theta(1)=(3n+4)\Theta(1)=\Theta(n)
   $$

   这里用到了 $n=n_1+n_2=r-p+1$ 。

4. 分治排序算法 `MERGE_SORT()`

   ```python
   def MERGE_SORT(A, p, r):
       if p < r:
           q = floor((p + r) / 2)
           MERGE_SORT(A, p, q)
           MERGE_SORT(A, q + 1, r)
           MERGE(A, p, q, r)
       return A
   ```

5. 算法 `MERGE_SORT()` 的分析

   1. 分治算法的分析策略

      设某个分治算法的输入规模为 $n$ (很大，不然 $T(n)=\Theta(1)$ 为常数时间，这没有意义)，运行时间为 $T(n)$ 。我们分析分治算法的每个步骤：

      - 分解（用时 $D(n)$）：将原问题分解为 $a$ 个子问题使得每个子问题的输入规模为 $1/b$
      - 解决（用时$S(n)$）：函数 $T$ 对原算法及其每个子命题以及子命题的递归子命题 (当然在这里不存在) 都成立，即某个子问题或多级子问题 (不存在) 的输入规模为 $m$ ，这个子问题的运行时间就是 $T(m)$，这足以说明 $S(n)=aT(n/b)$
      - 合并（用时$C(n)$）：最终合并的用时

      可以看到，总用时 $T(n)$ 应该为递归方程
      $$
      T(n)=D(n)+S(n)+C(n)=aT(n/b)+D(n)+C(n)\tag 3
      $$
      如果分解时对子问题 (1级子问题) 重复使用分治算法，使得每个 $k$ 级子问题都被分解为 $a_k$ 个 $k+1$ 级子问题，一直分解到 $N$ 级，每个 $k$ 级子问题的输入规模为其父问题输入规模的 $1/b_k$，那么此时
      $$
      S(n)=a_1a_2\cdots a_NT\left(\frac{n}{b_1b_2\cdots b_N}\right)=\prod_{k=1}^N a_kT\left(\left.n\middle/\prod_{k=1}^N b_k\right.\right)
      $$
      而 $D(n),C(n)$ 此时应该依赖于 $N$，注意到当 $N$ 可以任意指定时应该也作为输入规模的衡量指标，由此
      $$
      T(n,N)=\prod_{k=1}^N a_kT\left(\left.n\middle/\prod_{k=1}^N b_k\right.,1\right)+D(n,N)+C(n,N)\tag 4
      $$
      明显如果令 $a=a_1,b=b_1,T(n)=T(n,1)$，则 $(4)$ 式在 $n=1$ 时退化到 $(3)$ 式。

   2. 分析 `MERGE_SORT()` 算法

      假定输入规模 $n$ 为 $2$ 的幂

      - 分解：明显 $N=1$，用式 $(3)$ 即可。只计算 $\displaystyle{q=\left\lfloor\frac{p+r}{2}\right\rfloor}$，故 $D(n)=\Theta(1)$ 。
      - 解决：平均二分可知 $a=b=2$ ，故 $S(n)=2T(n/2)$ 。
      - 合并：我们已经算出 $C(n)=\Theta(n)$ 。

      由此：$T(n)=2T(n/2)+\Theta(n)$，设 $\Theta(n)=\lambda n+\mu$，带入初值条件 $T(1)=1$ 得到递归问题
      $$
      T(n)=2T(n/2)+\lambda n+\mu\qquad T(1)=1\tag 5
      $$
      通过某些特殊的手段可以解出
      $$
      T(n)=n(\lambda\log n+\mu+1)-\mu=\Theta(n\log n)
      $$

   3. 递归树

      我们可以猜解，一种手段为设 $n=2^p,T(2^p)=a_p$，则 $(5)$ 式变成
      $$
      a_p=2a_{p-1}+\lambda\log p+\mu\tag 6
      $$
      对每个表达式 $2x+\lambda\log x+\mu$，我们构造一颗树
      $$
      (2x+\lambda\log x+\mu)\leftrightarrow\langle x,\lambda\log x+\mu\rangle\longrightarrow\begin{array}{c}{}&{}&\lambda\log x+\mu&{}&{}\\{}&{\swarrow}&{}&{\searrow}&{}\\{x}&{}&{}&{}&{x}\end{array}
      $$
      那么式 $(6)$ 可以按照这个方式构造一个层层递归的树
      $$
      \left(a_p\leftrightarrow\langle a_{p-1},\lambda\log p+\mu\rangle\longrightarrow\begin{array}{c}{}&{}&\lambda\log p+\mu&{}&{}\\{}&{\swarrow}&{}&{\searrow}&{}\\{a_{p-1}}&{}&{}&{}&{a_{p-1}}\end{array}\right)\qquad a_{p-1}\leftrightarrow\langle a_{p-2},\lambda\log(p-1)+\mu\rangle
      $$
      这种树叫做递归方程 $(6)$ 的递归树。我们把上树用 $n$ 改写为
      $$
      T(n)\leftrightarrow\langle T(n-1),\lambda n+\mu\rangle\longrightarrow\begin{array}{c}{}&{}&\lambda n+\mu&{}&{}\\{}&{\swarrow}&{}&{\searrow}&{}\\{T(n/2)}&{}&{}&{}&{T(n/2)}\end{array}
      $$
      那么可以看出递归树共有 $p+1=\log n+1$ 层，第$k\in[0..\log n]$层带入后的成员之和为
      $$
      2^k\left(\lambda\frac{n}{2^k}+\mu\right)=\lambda n+2^k\mu
      $$
      我们把上式求和即得到 $T(n)$ 的值
      $$
      \begin{split}
      	T(n)&=\sum_{k=0}^{\log n}(\lambda n+2^k\mu)\\
      	{}&=\lambda n\log n+\mu(1+2+2^2+\cdots +2^{p})\\
      	{}&=\lambda n\log n+\mu\frac{1-2^{p-1}}{1-2}\\
      	{}&=\lambda n\log n+\mu(n/2-1)=\Theta(n\log n)
      \end{split}
      $$

作业答案：

- 2.3-2

  ```python
  def MERGE_NOINF(A, p, q, r):
      n1 = q - p + 1
      n2 = r - q
      L = []
      R = []
      for i in range(n1):
          L.append(None)
      for j in range(n2):
          R.append(None)
      for i in range(n1):
          L[i] = A[p + i]
      for j in range(n2):
          R[j] = A[q + j + 1]
      i = 0
      j = 0
      k = p
      while k < r + 1 and i != 4 and j != 4:
          if L[i] <= R[j]:
              A[k] = L[i]
              i = i + 1
          else:
              A[k] = R[j]
              j = j + 1
          k = k + 1
      if i < 4 and j == 4:
          for count in range(i, len(L)):
              A[k] = L[count]
              k = k + 1
              if k == r + 1:
                  break
      elif i == 4 and j < 4:
          for count in range(j, len(R)):
              A[k] = R[count]
              k = k + 1
              if k == r + 1:
                  break
      return A
  ```

- 2.3-3

  $k=1$ 时 $T(2)=2=2\log 2$

  若 $k=p>1$ 时 $T(2^p)=p2^p$

  则 $k=p+1$ 时 $T(2^{p+1})=2T(2^{p+1}/2)+2^p=2T(2^p)+2^{p+1}=2p2^p+2^{p+1}=(p+1)2^{p+1}$

  QED。

- 2.3-4

  设插入排序的运行时间为 $T(n)$，则 $T(n)=T(n-1)+\Theta(n)\ (n>1)$，故
  $$
  T(n)=\begin{cases}\Theta(1)&\text{if }n=1\\ T(n-1)+\Theta(n)&\text{if }n>1\end{cases}
  $$

- 2.3-5

  ```python
  from math import floor
  
  def BINARY_SEARCH(A, v):
      for count in range(1, len(A)):
          if A[count - 1] <= A[count]:
              continue
          else:
              return False
      p = floor(len(A) / 2)
      if len(A) > 1:
          B = []
          if A[p] > v:
              for i in range(p):
                  B.append(A[i])
              if BINARY_SEARCH(B, v) != None:
                  return BINARY_SEARCH(B, v)
          elif A[p] < v:
              for i in range(p + 1, len(A)):
                  B.append(A[i])
              if BINARY_SEARCH(B, v) != None:
                  return BINARY_SEARCH(B, v) + p + 1
          else:
              return p
      else:
          if len(A) == 0:
              return False
          else:
              if A[0] == v:
                  return 0
              else:
                  return None
  ```

  最坏情况时每次循环都执行到底（不计检查和数组构造的运行时间），有
  $$
  T(n)=\Theta(1)+T(n/2)+\Theta(1)=T(n/2)+\Theta(1)
  $$
  利用递归树可以明显看出每层的代价都是 $\Theta(1)$ ，总层数为 $\log n+1$ 层，由此
  $$
  T(n)=(\log n+1)\Theta(1)=\Theta(\log n)
  $$

- 2.3-6（答案来自CLRS）

  Each time the `while` loop of lines 5-7 of `INSERTION_SORT` scans backward through the sorted array $A[1..j-1]$ . The loop not only searches for the proper place for $A[j]$ , but it also moves each of the array elements that are bigger than $A[j]$ one position to the right (line 6). These movements takes $\Theta(j)$ time, which occurs when all the $j-1$ elements preceding $A[j]$ are larger than $A[j]$ . The running time of using binary search to search is $\Theta(j)$ , which is still dominated by the running time of moving element $\Theta(j)$ .

​	Therefore, we can't improve the overall worst-case running time of insertion sort to $\Theta(n\log n)$ .

- 2.3-7（答案来自CLRS）

  First, sort $S$ , which takes $\Theta(n\log n)$ . Then, for each element $s_i\in S_i,i=1,2,\cdots,n$ ,search $A[i+1..n]$ for $s'_i=x-s_i$ by binary search, which takes $\Theta(\log n)$ .

  - If $s'_i$ is found, return its position;
  - otherwise, continue for next iteration.

  The time complexity of the algorithm is $\Theta(n\log n)+n\cdot\Theta(\log n)=\Theta(n\log n)$ .

