# exgcd（Extended Euclidean Algorithm，扩展欧几里得算法）学习笔记

> 下文中 $(a,b)$ 均指 $\gcd(a,b)$，即 $a,b$ 的最大公因数。

## 引入——欧几里得算法（Euclidean Algorithm）

试设计算法，给定正整数 $a,b$，计算 $a,b$ 的**最大公因数**（Greatest Common Divisor，GCD，两个数的共同因子中的最大数）$(a,b)$。

首先我们想到试除。这是一个 $\mathcal O(\min(a,b))$ 的做法，因为需要从大到小枚举所有因数。这个算法并不优秀。

那现在我问你，$(1145141919810, 1145141919809)$（前者比后者大 $1$）是多少？

显然是 $1$。你为什么能这么快地回答出来？

一种解释方法是，因为 $a\neq b$，所以 $a,b$ 必然是 $(a,b)$ 的不同倍（即，$c=(a,b),a=nc,b=mc$，则 $n\neq m$）。那么 $(a,b)$ 的两个相邻倍数之间相差 $(a,b)$，也就是两个不同倍数之间最少相差 $(a,b)$，但是 $a,b$ 相差 $1$，故 $(a,b) \mid 1$，所以 $(a,b)=1$。当然你可能根本想不到这些，但是你的数学直觉会十分笃定地告诉你 $(a,b)=1$。

那我问你，$(1145141919810,1145141919808)$（前者比后者大 $2$）是多少？

> 读者：你能不能别用这两个数字了。

同样的方法可以知道，$(1145141919810,1145141919808) \mid 2$，然后一看 $2$ 是公因子所以是 $2$。但如果是 $(1145141919811,1145141919809)$ 则不是 $2$ 而是 $1$。

我们现在就能够猜出来了：$(a,b) \mid (\lvert a-b \rvert)$。具体证明仿照上面显然。进一步地，因为若 $r \mid a$ 且 $r \mid a-b$，则 $r \mid b$。所以 $(a,b)=(a,a-b)=(b,a-b)$，进一步地可以得到 $(a,b)=(b,a\bmod b)$。但是当 $b=0$ 时有 $(a,b)=a$。

所以我们就可以写出这个算法的程序了。

```cpp
int gcd(int x, int y) { return y == 0 ? x : gcd(y, x % y); }
```

也叫辗转相除法。

关于时间复杂度：~~显然是优秀的。~~ 显然 $x \bmod y$ 相当于多次 $x-y$，而这样会让原本的 $x$ 更大的情况下时间复杂度不变，所以假设全是 $x-y$。那么就是 `y == 0 ? x : gcd(y, x-y)`，并且 $x>y$ 一定成立。容易发现（证明用脚做）递归次数最多的是 $(F_n,F_{n-1})$，其中 $F_n$ 是斐波那契数列第 $n$ 项，那么一次递归会从 $(F_n,F_{n-1})$ 变为 $(F_{n-1},F_{n-2})$。因为斐波那契数列是指数级增长的，所以时间复杂度是 $\mathcal O(\log x)$。当然如果不是这种特殊情况则是 $\mathcal O(\log \max(x,y))$。

因为这个算法是用来计算 $\gcd$ 的，所以（非正式地）又称为 gcd 算法。

如果要求两数最小公倍数？自己想想吧，小心溢出。

## 扩展欧几里得算法（Extended Eculidean Algorithm）

给定 $a,b$ 用来求解 $ax+by=(a,b)$ 的整数解。

为什么看上去这么奇怪？灵感来源于裴蜀定理（此定理参见我写的其它文章）。我们有 $ax+by=c$ 有整数解等价于 $(a,b) \mid c$，那么我们只需要解决 $ax+by=(a,b)$ 然后将 $x,y$ 均乘上 $\dfrac{c}{(a,b)}$ 即可。

那么 $ax+by=(a,b)$ 怎么解呢？

联想到辗转相除法，我们设 $ax_0+by_0=(a,b),bx_1+(a\bmod b)y_1=(b,a\bmod b)$，那么有 $bx_1+\left(a-\left\lfloor\dfrac{a}{b}\right\rfloor\times b\right)y_1=ax_0+by_0$，也就是 $bx_1+ay_1-\left\lfloor\dfrac{a}{b}\right\rfloor\times by_1=ax_0+by_0$，也就是 $ay_1+b\left(x_1-\left\lfloor\dfrac{a}{b}\right\rfloor\times y_1\right)=ax_0+by_0$，就能够得到 $x_0=y_1,y_0=x_1-\left\lfloor\dfrac{a}{b}\right\rfloor\times y_1$。然后辗转相除即可。终止条件是 $b=0$，也就是 $ax=a$，那么 $x=1$，$y$ 任意（通常设为 $0$）即可。

因为是 gcd 算法的扩展，所以习惯性地称作 exgcd 算法，实际上应当正式地叫做扩展欧几里得算法（简称扩欧）。

本来扩欧不需要计算 $(a,b)$（只是用到了辗转相除的性质），但是因为通常用来解方程 $ax+by=c$ 所以经常需要计算 $(a,b)$，也很简单在递归终止的时候 $b=0$ 就有 $(a,b)=a$。

参考代码（使用了 C++ 的引用语法）：

```cpp
// 小心溢出。
int exgcd(int a, int b, int &x, int &y)
{
    if(b == 0)
    {
        x = 1;
        y = 0;
        return a;
    }
    int res = exgcd(b, a%b, y, x);
    y -= a/b * x;
    return res;
}
```

## 通解

我们算出一组解 $ax_0+by_0=c$，假设 $(a,b)=1$（如果不是则可以让 $a,b,c$ 都除以 $(a,b)$），可以得到任意 $\langle x_0-kb,y_0+ka\rangle$（有 $k \in \Z$）都是 $ax+by=c$ 的整数解。下证任意不满足此形式的都不是整数解。

首先证明若不存在整数 $k$ 使得 $x_0-kb=x$，或不存在 $k$ 使得 $y_0-ka=y$，则 $\langle x,y\rangle $ 一定不行。

证：显然两个条件对称，只需要证明前者。考虑反证法。我们有 $k=\dfrac{x_0-x}{b}$，所以 $b\not\mid (x_0-x)$，而因为 $ax_0+by_0=ax+by$，所以 $ax_0+by_0-ax-by=0$，也就是 $a(x_0-x)+b(y_0-y)=0$，也就是 $a(x_0-x)=b(y-y_0)$，左侧不是 $b$ 的倍数（因为 $a \perp b$），而右侧必然是 $b$ 的倍数，不可能满足条件。证毕。

然后证明若 $x=x_0-k_1b,y=y_0+k_2a$，但是 $k_1\neq k_2$，则 $x,y$ 不行。

证：我们有 $ax+b(y_0+k_1a)=c$ 成立，而 $ax+by-ax-b(y_0+k_1a)=b(k_2-k_1)\neq 0$，故不满足条件。

综上，我们就证明了原命题。

### exgcd 返回的解

因为解有无穷多个（或没有，假设有解），我们有必要分析 exgcd 返回的解满足什么性质。

我们可以证明，exgcd 算法返回的解必然是绝对值之和最小的，并且必然满足 $\lvert x\rvert \le b,\lvert y \rvert \le a$。网上已经有关于此的证明，这里不多讲。

## 例题

### UVA10104 Euclid Problem

[UVA 题目链接](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=13&page=show_problem&problem=1045)、[洛谷题目链接](https://www.luogu.com.cn/problem/UVA10104)。

题目大意：给定 $A,B$，求 $Ax+By=(A,B)$ 的所有整数解中 $\lvert x\rvert+\lvert y\rvert$ 最小的整数解。

根据 exgcd 的性质我们直接使用 exgcd 返回解即可。

### 洛谷 P5656【模版】二元一次不定方程 (exgcd)

[洛谷 P5656【模版】二元一次不定方程 (exgcd)](https://www.luogu.com.cn/problem/P5656)。

首先根据裴蜀定理判断是否无解。

然后约掉 $(a,b)$，根据其通解形式求出 $x>0$ 时的 $k$ 的最大值和让 $y>0$ 时 $k$ 的最小值。根据这个范围判断有没有正整数解，如果有，则首先个数很好求，然后 $x,y$ 最小值就分别是当 $k$ 取最大和最小时的情况，然后也分别是 $y,x$ 取最大值的情况。如果没有正整数解，那么还是可以求 $x,y$ 最小值。

恶心的是要自己写向下/上取整的除法（C++ `/` 运算符的是向零取整，负数是向上取整，正数是向下）。

注意解可能爆 `int`，因为虽然 exgcd 求出的解不会，但是最终要乘上 $c$。

```cpp
#include <cstdio>

using namespace std;

long long exgcd(long long a, long long b, long long& x, long long& y)
{
    if (b == 0)
    {
        x = 1;
        y = 0;
        return a;
    }
    long long res = exgcd(b, a % b, y, x);
    y -= a / b * x;
    return res;
}

long long lowdiv(long long a, long long b)
{
    if (a == 0) return 0;
    if ((a > 0) ^ (b > 0)) return a > 0 ? -((a - b - 1) / (-b)) : -((-a+b-1)/b);
    else return a / b;
}

long long uppdiv(long long a, long long b)
{
    if (a == 0) return 0;
    if ((a > 0) ^ (b > 0)) return a / b;
    else return a > 0 ? (a + b - 1) / b : (-a - b - 1) / (-b);
}

void solve(long long a, long long b, long long c)
{
    long long x, y;
    long long gcd = exgcd(a, b, x, y);
    if (c % gcd) // 裴蜀定理判断无解
    {
        printf("-1\n");
        return;
    }
    b /= gcd; a /= gcd; c /= gcd;
    exgcd(a, b, x, y);
    x *= c; y *= c;
    // 求 x 为正整数时 k 的最大值，(x-kb, y+ka)
    // x-kb >= 1: k <= (x-1)/b
    long long kmax = lowdiv(x - 1, b);
    // y+ka >= 1: k >= (1-y)/a
    long long kmin = uppdiv(1 - y, a);
    if (kmin > kmax) printf("%lld %lld\n", x - kmax * b, y + kmin * a);
    else printf("%lld %lld %lld %lld %lld\n", kmax - kmin + 1, x - kmax * b, y + kmin * a, x - kmin * b, y + kmax * a);
}

int main()
{
    int t;
    scanf("%d", &t);
    while (t--)
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        solve(a, b, c);
    }
    return 0;
}
```

## Fun

![来源：https://www.luogu.com.cn/article/261brvvn。侵删](../../../images/handsome-while-studing-nt.png)
