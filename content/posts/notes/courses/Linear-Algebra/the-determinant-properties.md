# 从零推导的行列式性质

上一篇文章[《如何定义行列式？从逆序数到行列式的行展开！》](./the-determinant.md)中，我们从逆序数的角度出发，定义了行列式，并且推导出了行列式的行展开公式。

在本篇文章中，我们将继续推导行列式的性质。首先我们主要还是采用第三公理化（递归法）的视角来进行计算，逆序数的视角仅作为补充，我个人认为理由递归法的递推公式比较容易进行数学归纳法的证明。

## 定义回顾

### 递归法定义（第三公理化定义）

设$A$为$n\times n$矩阵，$a_{ij}$为$A$的第$i$行第$j$列元素，$M_{ij}$为去掉$A$的第$i$行第$j$列元素后得到的$(n-1)\times(n-1)$矩阵，则行列式定义如下：
$$
\begin{aligned}
\det(A) &= \sum_{j=1}^n a_{ij}(-1)^{i+j}\det(M_{ij})\\
|a_{ij}| &= a_{ij}
\end{aligned}
$$
其中$i$可以取任意整数$1\leq i\leq n$，通常取$i=1$。

### 逆序数定义

设$A$为$n\times n$矩阵，$a_{ij}$为$A$的第$i$行第$j$列元素，则行列式定义如下：
$$
\det(A) = \sum_{\sigma\in S_n} (-1)^{\tau(\sigma)}\prod_{i=1}^n a_{i\sigma(i)}
$$

## 先计算一些常用的行列式

### $n$ 阶下三角行列式

设$A$为$n$阶下三角矩阵（当$i<j$时，$a_{ij}=0$，即主对角线以上元素为 $0$），则$A$的行列式为：
$$
\det(A) = \prod_{i=1}^n a_{ii}
$$

**证明**：对 $n$ 作数学归纳法，当$n=1,2$时，结论显然成立。

假设结论对$n-1$阶下三角行列式成立，则由定义得
$$
\begin{aligned}
D_n &= (-1)^{1+1}a_{11}D_{n-1}\\
&= a_{11}\prod_{i=2}^n a_{ii}\\
&= \prod_{i=1}^n a_{ii}
\end{aligned}
$$
其中$D_n$为$n$阶下三角行列式，$D_{n-1}$为去掉$D_n$的第一行第一列元素后得到的$(n-1)$阶下三角行列式。

因此结论对$n$阶下三角行列式成立。

### $n$ 阶上三角行列式

设$A$为$n$阶上三角矩阵（当$i>j$时，$a_{ij}=0$，即主对角线以下元素为 $0$），则$A$的行列式为：
$$
\det(A) = \prod_{i=1}^n a_{ii}
$$

**证明**：对 $n$ 作数学归纳法，当$n=1,2$时，结论显然成立。

假设结论对$n-1$阶上三角行列式成立，则由定义得
$$
\begin{aligned}
D_n &= (-1)^{n+n}a_{nn}D_{n-1}\\
&= a_{nn}\prod_{i=1}^{n-1} a_{ii}\\
&= \prod_{i=1}^n a_{ii}
\end{aligned}
$$
其中$D_n$为$n$阶上三角行列式，$D_{n-1}$为去掉$D_n$的第$n$行第$n$列元素后得到的$(n-1)$阶上三角行列式。

因此结论对$n$阶上三角行列式成立。

### $n$ 阶对角行列式

设$A$为$n$阶对角矩阵（当$i\neq j$时，$a_{ij}=0$），则$A$的行列式为：
$$
\det(A) = \prod_{i=1}^n a_{ii}
$$

**证明**：对 $n$ 作数学归纳法，当$n=1,2$时，结论显然成立。

假设结论对$n-1$阶对角行列式成立，则由定义得
$$
\begin{aligned}
D_n &= (-1)^{1+1}a_{11}D_{n-1}\\
&= a_{11}\prod_{i=2}^n a_{ii}\\
&= \prod_{i=1}^n a_{ii}
\end{aligned}
$$
其中$D_n$为$n$阶对角行列式，$D_{n-1}$为去掉$D_n$的第一行第一列元素后得到的$(n-1)$阶对角行列式。

因此结论对$n$阶对角行列式成立。

### $n$ 阶反对角行列式

设$A$为$n$阶反对角矩阵（当$i+j\neq n+1$时，$a_{ij}=0$），则$A$的行列式为：
$$
\det(A) = (-1)^{\frac{n(n-1)}{2}}\prod_{i=1}^n a_{i(n-i+1)}
$$

**证明**：对 $n$ 作数学归纳法，当$n=1,2$时，结论显然成立。

假设结论对$n-1$阶反对角行列式成立，则由定义得
$$
\begin{aligned}
D_n &= (-1)^{1+n}a_{1n}D_{n-1}\\
&= (-1)^{1+n}a_{1n}(-1)^{\frac{(n-1)(n-2)}{2}}\prod_{i=2}^n a_{i(n-i+1)}\\
&= (-1)^{\frac{n(n-1)}{2}}\prod_{i=1}^n a_{i(n-i+1)}
\end{aligned}
$$
其中$D_n$为$n$阶反对角行列式，$D_{n-1}$为去掉$D_n$的第一行第$n$列元素后得到的$(n-1)$阶反对角行列式。

因此结论对$n$阶反对角行列式成立。

**也可以通过计算得到**：

$$
\begin{aligned}
D_n &= (-1)^{1+n}a_{1n}D_{n-1}\\
D_{n-1} &= (-1)^{1+(n-1)}a_{2(n-1)}D_{n-2}\\
\vdots\\
D_2 &= (-1)^{1+2}a_{(n-1)2}D_1\\
D_1 &= a_{nn}
\end{aligned}
$$

将上面各式相乘，得到
$$
\begin{aligned}
D_n &= (-1)^{1+n}(-1)^{1+(n-1)}\cdots(-1)^{1+2}a_{1n}a_{2(n-1)}\cdots a_{(n-1)2}a_{nn}\\
&= (-1)^{\frac{n(n-1)}{2}}\prod_{i=1}^n a_{i(n-i+1)}
\end{aligned}
$$

## $n$ 阶行列式的性质

### 性质 1：行列式与矩阵的转置相等

$$
\det(A) = \det(A^T)
$$

**证明**： 由定义得

$$
\begin{aligned}
\det(A^T) &\xlongequal{\mathrm{define}} \sum_{\sigma\in S_n} (-1)^{\tau(\sigma)}\prod_{i=1}^n b_{i\sigma(i)}\\
&\xlongequal{b_{ij}=a_{ji}} \sum_{\sigma\in S_n} (-1)^{\tau(\sigma)}\prod_{i=1}^n a_{\sigma(i)i}\\
\end{aligned}
$$

$$
\begin{aligned}
\det(A) &= \sum_{\sigma\in S_n} (-1)^{\tau(\sigma)}\prod_{i=1}^n a_{i\sigma(i)}\\
&= \sum_{\sigma\in S_n} (-1)^{\tau(\sigma)}\prod_{i=1}^n a_{\sigma^{-1}(i)i}\\
&= \sum_{\sigma\in S_n} (-1)^{\tau(\sigma^{-1})}\prod_{i=1}^n a_{\sigma^{-1}(i)i}\\
&= \sum_{\rho\in S_n} (-1)^{\tau(\rho)}\prod_{i=1}^n a_{\rho(i)i}\\
\end{aligned}
$$

因此 $\det(A) = \det(A^T)$。

> 作者的话：你猜我为啥不用递归法定义来证明？

### 性质 2：行列式可以按任意一行（列）展开

$$
\det(A) = \sum_{j=1}^n a_{ij}(-1)^{i+j}\det(M_{ij}) = \sum_{i=1}^n a_{ij}(-1)^{i+j}\det(M_{ij})
$$

**证明**： 由定义得
$$
\begin{aligned}
\det{A} &=\sum_{\sigma \in S_n} (-1)^{\tau(\sigma)}\prod_{i=1}^n a_{i\sigma(i)}\\
&=\sum_{j=1}^n a_{ij}\sum_{\substack{\sigma \in S_n \\ \sigma(i)=j}} (-1)^{\tau(\sigma)}\prod_{\substack{k=1 \\ k\neq i}}^n a_{k\sigma(k)}\\
&=\sum_{j=1}^n a_{ij}(-1)^{i+j}\sum_{\rho \in S_{n-1}} (-1)^{\tau(\rho)}\prod_{k=1}^{n-1} a_{k'\rho(k')}\\
&=\sum_{j=1}^n a_{ij}(-1)^{i+j}\det(M_{ij})
\end{aligned}
$$
其中$k'=\begin{cases}k & k<i\\ k+1 & k\geq i\end{cases}$

同理，由性质 1 可得
$$
\det(A) = \sum_{i=1}^n a_{ij}(-1)^{i+j}\det(M_{ij})
$$

### 性质 3：行列式的线性性质

1. 若将矩阵$A$的某一行（列）乘以常数$k$，则行列式也乘以$k$，即
   $$
   \det(B) = k\det(A)
   $$
   其中$B$为将矩阵$A$的第$i$行（列）乘以常数$k$后得到的矩阵。
2. 若将矩阵$A$的某一行（列）拆成两行（列）的和，则行列式也拆成两项的和，即
   $$
   \det(B) = \det(A) + \det(C)
   $$
   其中$B$为将矩阵$A$的第$i$行（列）拆成两行（列）的和后得到的矩阵，$C$为将矩阵$A$的第$i$行（列）替换成另一行（列）后得到的矩阵。

**证明**： 由定义得
$$
\begin{aligned}
\det(B) &= \sum_{j=1}^n b_{ij}(-1)^{i+j}\det(M_{ij})\\
&= \sum_{j=1}^n k a_{ij}(-1)^{i+j}\det(M_{ij})\\
&= k\det(A)
\end{aligned}
$$ 
其中$B$为将矩阵$A$的第$i$行（列）乘以常数$k$后得到的矩阵。

同理，若将矩阵$A$的第$i$行（列）拆成两行（列）的和，则
$$
\begin{aligned}
\det(B) &= \sum_{j=1}^n b_{ij}(-1)^{i+j}\det(M_{ij})\\
&= \sum_{j=1}^n (a_{ij}+c_{ij})(-1)^{i+j}\det(M_{ij})\\
&= \sum_{j=1}^n a_{ij}(-1)^{i+j}\det(M_{ij}) + \sum_{j=1}^n c_{ij}(-1)^{i+j}\det(M_{ij})\\
&= \det(A) + \det(C)
\end{aligned}
$$
其中$B$为将矩阵$A$的第$i$行（列）拆成两行（列）的和后得到的矩阵，$C$为将矩阵$A$的第$i$行（列）替换成另一行（列）后得到的矩阵。

上述性质使用逆序数定义也可以证明，读者可自行尝试。

### 性质 4：行列式中两行元素全相等，则行列式为零

$$
\det(A) = 0
$$
其中$A$为$n\times n$矩阵，且$A$的第$i$行与第$j$行元素全相等，$i\neq j$。

**证明1**： 假设已经证明行列式交换两行（列）符号取反
$$
\det(A) = -\det(A)
$$
则可得$\det(A) = 0$。

**证明2**： 
用数学归纳法证明，结果对$n=2$时显然成立，假设结论对$n-1$阶矩阵成立，则对$n$阶矩阵$A$的第$k$行展开（$k\neq i,j$），则
$$
\begin{aligned}
\det(A) &= \sum_{l=1}^n a_{kl}(-1)^{k+l}\det(M_{kl})\\
\end{aligned}
$$
其中$M_{kl}$为去掉$A$的第$k$行第$l$列元素后得到的$(n-1)\times(n-1)$矩阵，且$M_{kl}$的第$i$行与第$j$行元素全相等，因此$\det(M_{kl})=0$，从而$\det(A)=0$。结论对$n$阶矩阵成立。

### 推论

1. 行列式中两行（列）成比例，则行列式为零。
2. 在行列式中，将某一行（列）的各元素加上另一行（列）对应元素的常数倍，行列式的值不变。

### 性质 5：行列式交换两行（列）符号取反

$$
\det(B) = -\det(A)
$$
其中$B$为将矩阵$A$的第$i$行（列）与第$j$行（列）交换后得到的矩阵，$i\neq j$。

**证明：**

设矩阵 $A = (a_{ij})$，$B$ 是将 $A$ 的第 $r$ 行与第 $s$ 行交换后得到的矩阵。
设 $p = (p_1 \cdots p_n)$ 是一个排列，$q$ 是将 $p$ 中的第 $r$ 个元素 $p_r$ 与第 $s$ 个元素 $p_s$ 交换后得到的排列。

根据行列式定义：
$$
\det(B) = \sum_{p \in S_n} (-1)^{\tau(p)} \prod_{i=1}^{n} b_{i, p_i}
$$

由 $B$ 的构造可知 $b_{r, p_r} = a_{s, p_r}$, $b_{s, p_s} = a_{r, p_s}$，且对 $i \neq r, s$ 有 $b_{i, p_i} = a_{i, p_i}$。代入上式：
$$
\det(B) = \sum_{p \in S_n} (-1)^{\tau(p)} \left( a_{s, p_r} a_{r, p_s} \prod_{i \neq r, s} a_{i, p_i} \right)
$$

根据排列 $q$ 的定义，上式右边的乘积项可以写作：
$$
a_{s, p_r} a_{r, p_s} \prod_{i \neq r, s} a_{i, p_i} = \prod_{i=1}^{n} a_{i, q_i}
$$

根据已知排列性质，交换排列中两个元素会改变逆序数的奇偶性，故 $(-1)^{\tau(p)} = -(-1)^{\tau(q)}$。

代入得：
$$
\det(B) = \sum_{p \in S_n} -(-1)^{\tau(q)} \prod_{i=1}^{n} a_{i, q_i}
$$

当 $p$ 遍历所有排列 $S_n$ 时，$q$ 也同样遍历 $S_n$。因此，我们可以将求和变量从 $p$ 换成 $q$：
$$
\det(B) = - \sum_{q \in S_n} (-1)^{\tau(q)} \prod_{i=1}^{n} a_{i, q_i} = -\det(A)
$$

**证毕。**

**利用推论的证明：**

> 思考一下，如果只有两个变量$a$和$b$，如何仅通过加减法运算来交换它们的值？（不得使用临时变量）
>
> 1. $a\ \text{+=}\ b = a + b$
> 2. $b\ \text{+=}\ -a = b - (a + b) = -a$
> 3. $a\ \text{+=}\ b = a + b - a = b$

设矩阵 $A = (a_{ij})$，$B$ 是将 $A$ 的第 $r$ 行与第 $s$ 行交换后得到的矩阵。

$$
\begin{aligned}
\det{A} &\xlongequal{r+s} \det(A_1)\\
&\xlongequal{s-r} \det(A_2)\\
&\xlongequal{r+s} \det(A_3)\\
&\xlongequal{-s} -\det(B)
\end{aligned}
$$

### 证明如下式子：
$$
\sum_{k=1}^n a_{ik}A_{jk} = 0 \quad (i \neq j)
$$
其中$A_{jk} = (-1)^{j+k}\det(M_{jk})$为$a_{jk}$的代数余子式。

**证明**：
设矩阵 $A = (a_{ij})$，构造矩阵 $B$，其第 $j$ 行与第 $i$ 行相同，其余行与 $A$ 相同。则显然其第 $j$ 行元素对应的代数余子式为 $A_{jk}$。
$$
\det(B) = \sum_{k=1}^n a_{ik}A_{jk} = 0
$$

**证毕。**