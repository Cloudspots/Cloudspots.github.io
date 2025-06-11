# 浅谈中国剩余定理（Chinese Remainder Theorem，CRT）

问题描述：

我们要解如下形式的方程：

$$ \begin{cases}x\equiv a_1\pmod{m_1}\\x\equiv a_2\pmod{m_2}\\x\equiv a_3\pmod{m_3}\\\vdots\\x\equiv a_n\pmod{m_n}\end{cases} $$

## 答案表述

显然有无数组解或无解——把一组解加上 $\prod m$ 后仍然是一组合法解。

> 什么是 $\prod m$？
>
> $\displaystyle \prod_{\text{VAR}=\text{VAL1}}^{\text{VAL2}} \text{FUN}$ 代表当 $\text{VAR}$ 从 $\text{VAL1}$ 到 $\text{VAL2}$ 时（每次 $+1$，即 $\text{VAL1},\text{VAL1}+1,\text{VAL1}+2,\dots,\text{VAL2}$），$\text{FUN}$ 的所有值的乘积。如 $\displaystyle \prod_{i=1}^n i=n!$，当 $a=(1,2,3,2,1)$ 时，$\displaystyle \prod_{i=1}^{\lvert a \rvert}a_i=1\times 2\times 3\times 2\times 1=12$。
>
> 而 $\prod m$ 是 $m$ 中所有元素的乘积的意思。见过 $\sum$ 的可能更好理解。

我们先不看无解的情况。实际上，加上 $M=[m_1,m_2,\dots,m_n]$ 仍然是合法解，证明显然。

接下来我们证明一组合法解加上一个非 $M$ 的倍数的数就不是合法解。显然我们只需要证明加上一个在 $[1,M-1]$ 中的正整数 $K$ 时，不是一组合法解。

因为这个数不是 $M$ 的倍数，则必然存在一个 $m_i \nmid K$，此时 $x \equiv a_i \pmod{m_i}$ 就变成了 $x \equiv a_i+K \pmod{m_i}$，而因为 $m_i\nmid K$，则 $K\not \equiv 0\pmod {m_i}$，那么 $a_i\not\equiv a_i+K\pmod {m_i}$，矛盾。

所以我们需要找到一个 $A$ 满足 $x$ 满足条件等价于 $x \equiv A \pmod M$，或者报告无解。我们刚才证明了 $A$ 若存在则必然唯一。

> 给出一个无解的构造：$\begin{cases}x\equiv 1\pmod 2\\x\equiv 2\pmod 4\end{cases}$，第一条说明 $x$ 是奇数而第二条说明 $x$ 是偶数。所以存在无解的情况。

## 简化问题

重点探究 $n=2$ 如何解，即给出 $\begin{cases}x\equiv a\pmod u\\x\equiv b\pmod v\end{cases}$，求 $c$ 使得方程等价于 $x\equiv c\pmod{uv}$（$u\perp v$）。

第一条相当于 $x=ku+a$，代入第二条得 $ku+a\equiv b\pmod v$（求 $k$），也就是 $ku\equiv b-a\pmod v$。我们令 $c=(b-a)\bmod v$。

现在问题就相当于给定 $ku\equiv c\pmod v$，求 $k$ 满足的条件。

那么我们可以使用 exgcd（扩展欧几里得算法）解决这个问题。exgcd 可以看我其它的文章。

## 回到问题

核心思想是，一次一次合并条件。

首先合并前两项，然后合并第一次合并完的结果和第三项，这样下去，直到只有一次为止。

这就是中国剩余定理（Chinese Remainder Theorem，CRT）。

什么你问拓展中国剩余定理（extended-CRT，exCRT）是什么？我也不知道。我在自学 CRT 的时候意外发现我对 CRT 的推导（所有 $m$ 互质）似乎完全适用于 exCRT（因为唯一用到 $m$ 互质的条件的地方就是算最小公倍数时直接算乘积，改了这个），后面忘了。

[模版题](https://www.luogu.com.cn/problem/P1495)和[模版题](https://www.luogu.com.cn/problem/P4777)。参考代码：

```cpp
// 十分恶心需要开 i128，除非用神奇做法
#ifndef ONLINE_JUDGE
#include "__msvc_int128.hpp"
#define int128 _Signed128
#endif
#include <cstdio>

using namespace std;

// Calculate (a,b) and solve ax+by=(a,b)
__int128 exgcd(__int128 a, __int128 b, __int128 &x, __int128 &y)
{
    if(b == 0)
    {
        // ax=a
        x = 1;
        y = 0;
        return a;
    }
    __int128 res = exgcd(b, a % b, x, y);
    __int128 t = x;
    x = y;
    y = t - a / b * y;
    return res;
}

__int128 gcd(__int128 x, __int128 y) { return y == 0 ? x : gcd(y, x % y); }

int main()
{
    int n;
    scanf("%d", &n);
    __int128 x = 0, y = 1;
    for(int i=1;i<=n;i++)
    {
        long long a, b;
        scanf("%lld%lld", &a, &b);
        __int128 g = ((x - b) % y + y) % y;
        __int128 k = 114, r = 514;
        __int128 p = exgcd(a, y, k, r);
        y = y / gcd(y, a) * a;
        // 若 p 不是 g 的因数则根据裴蜀定理无解。这里略去无解判断。
        x = ((k * g / p * a + b) % y + y) % y;
    }
    printf("%lld\n", x);
    return 0;
}
```

[record(exCRT)](https://www.luogu.com.cn/record/219560961)。
