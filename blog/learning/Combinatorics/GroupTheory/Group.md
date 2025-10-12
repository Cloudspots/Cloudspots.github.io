# 什么是群

群的标准定义：

> 定义一个群 $G=(S,\star)$ 满足：
>
> 封闭性：$\forall x,y\in S, x\star y\in S$。
>
> 运算 $\star$ 在 $S$ 上满足结合律——$\forall a,b,c\in S,(a\star b)\star c=a\star (b\star c)$。
>
> 存在单位元：$\exist e\in S \text{s.t.} \forall a\in S, e\star a=a\star e=a$。
>
> 存在逆元：$\forall x\in S,\exist x^{-1}\in S, x\star x^{-1}=x^{-1}\star x=e$。

单位元 $e$ 通常记作 $1$。$\star$ 可以省略不写。注意可能不满足交换律。

我们在组合数学中一般研究**置换群**。什么是置换？

> 定义 $1,2,3,\dots,n$ 的一个置换 $\begin{pmatrix}1&2&3&\dots&n\\a_1&a_2&a_3&\dots&n\end{pmatrix}$ 代表：
>
> - $1$ 变为 $a_1$。
> - $2$ 变为 $a_2$。
> - $3$ 变为 $a_3$。
> - ...。
> - $n$ 变为 $a_n$。
>
> 需要满足 $a$ 为 $1\dots n$ 的排列。

运算是什么？置换可以看做一个排列的函数，那么运算就是函数的复合。如何计算？也很简单，$AB$ 代表先进行 $B$ 置换再进行 $A$ 置换，也就是 $i$ 变为 $b_i$ 然后变为 $a_{b_i}$。
