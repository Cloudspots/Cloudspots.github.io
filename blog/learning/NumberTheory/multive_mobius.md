# 积性函数、莫比乌斯函数与莫比乌斯反演

## 数论函数

定义域为正整数、值域为复数（的子集）的函数称为积性函数。

有很多常见的数论函数。不想列举了反正后面会有。

## 积性函数

满足对于任意 $x \perp y$（数论中代表 $x$ 与 $y$ 互质，即 $(x,y)=1$，下同）都有 $f(xy)=f(x)f(y)$ 的数论函数称之为**积性函数**。

满足对于任意正整数 $x,y$（不要求互质）都有 $f(xy)=f(x)f(y)$ 的数论函数称之为**完全积性函数**。

比如 $f(x)=x,f(x)=x^2,f(x)=x^{114514},f(x)=\sqrt{x}$ 都是完全积性函数，而因数个数、因数和是积性函数（不是完全积性的），莫比乌斯函数（后面讲）也是积性的。

另一个好玩的积性函数是 $\varepsilon(x)=[x=1]$，其中方括号是艾弗森括号（说真的，方括号在数学中表示的东西真的太多了。。），也就是 $\varepsilon(x)=\begin{cases}1&x=1\\0&\text{otherwise}\end{cases}$。

### 性质

性质 $1$：积性函数 $f(x)$ 必须满足 $f(1)=1$ 或 $f(x)=0$。因为 $1\perp 1$，并且 $f(1\times 1)=f^2(1)$，所以 $f(1)=f^2(1)$，所以 $f(1)$ 为 $0$ 或 $1$。如果 $f(1)=0$，则因为任意 $x$ 都满足 $x \perp 1$，所以 $f(x)=f(x \times 1)=f(x)f(1)=0\square$。显然，零函数并不好玩。以下讨论的积性函数中没有零函数。

性质 $2$：对于积性函数 $f(x)$，知道所有形如 $f(p^a)$ 其中 $p$ 是质数 $a$ 是正整数，就可以确定 $f$ 的所有值了。证明：考虑质因数分解即可。$\square$

性质 $3$：对于完全积性函数 $f(x)$，知道所有质数的值就可以确定 $f$ 的所有值了。因为对 $f(p)$ 反复乘法可以得到所有 $f(p^a)$，然后利用性质 $1$ 可以得到结果。$\square$

性质 $4$：$f(x)$ 积性等价于 $g(x)=\displaystyle\sum_{d \mid x} f(d)$ 积性。

证明：

先证充分性。首先 $a \mid (xy)$ 等价于 $\exist b\mid x,c\mid y\text{ s.t }bc=a$。证明显然，略去。所以 $g(xy)$（$x \perp y$）为 $\displaystyle\sum_{d \mid (xy)}f(d)=\sum_{d_1\mid x}\sum_{d_2\mid y}f(d_1d_2)=\sum_{d_1\mid x}\sum_{d_2\mid y}f(d_1)f(d_2)=\left(\sum_{d_1\mid x}f(d_1)\right)\left(\sum_{d_2\mid y}f(d_2)\right)=g(x)g(y)$，证毕！

后证必要性。采用反证法（实际上差不多是数学归纳的另一种形式），如果存在一个 $a,b$ 满足 $a\perp b$ 且 $f(a)f(b)\neq f(ab)$，那么找到 $ab$ 最小的（有多个随便选一个）。那么 $g(ab)=\displaystyle\sum_{d \mid ab}f(d)=\displaystyle\sum_{x\mid a}\sum_{y\mid b}f(xy)\color{red}\neq\color{normal}\displaystyle\sum_{x\mid a}\sum_{y\mid b}f(x)f(y)=g(a)g(b)$。我们能够断言不等是因为只有 $x=a,y=b$ 时不等。这违反了 $g$ 是积性函数的性质，矛盾，证毕！

还有很多有趣的性质，但是这里先不多说了。

但是，性质 $4$ 比较有意思……我们知道了 $f$ 自然能够知道 $g$，但是知道了 $g$ 又怎么知道 $f$ 呢？我们先研究一个简单情况。

> 我们可以用性质 $4$ 证明 $\varphi$ 函数（因数个数）积性。

## 反用性质 $4$

### 简单情况 - 莫比乌斯函数

假设 $g=\varepsilon$。求 $f(x)$。

根据性质 $2$，我们只需要求 $f(p^n)$。我们知道当 $n\ge 2$ 时 $g(p^n)=g(p^{n-1})=0$，但是两者只差一个 $f(p^n)$，所以 $n \ge 2$ 时 $f(p^n)=0$。$n=1$ 时 $g(p)=0,g(p^0)=1$，所以 $f(p)=-1$。这样我们就能够知道 $f$ 的定义。将其命名为莫比乌斯函数 $\mu$（很多情况下被误写作 miu，但是实际上是 mu，但是输入法需要打 miu 才能打出来）：

$$ \mu(n)=\begin{cases}(-1)^k&n=p_1p_2p_3\dots p_k,p_i\text{ 两两不同}\\0&\text{otherwise}\end{cases} $$

### 莫比乌斯反演

研究一般情况下怎么做。同样研究 $f(p^n)$。我们同样使用 $g(p^n)$ 和 $g(p^{n-1})$，得到 $f(p^n)=g(p^n)-g(p^{n-1})$，乘起来得到 $f(x)=\displaystyle\prod_{i=1}^k g({p_i}^{\alpha_i})-g({p_i}^{\alpha_i-1})$。这并不好看啊？考虑将其展开以得到一个美观的形式。注意到展开后只会出现 $g(d)$ 满足 $d$ 的质因子分解的每个次数和 $x$ 的都最多差 $1$，也就是 $\mu\left(\dfrac{x}{d}\right)\neq 0$，并且其符号就是 $\mu\left(\dfrac{x}{d}\right)$！（感叹号不是阶乘），所以得到 $f(x)=\displaystyle\sum_{d\mid x} \mu\left(\dfrac{x}{d}\right)g(d)$，好看吧！

这就是听起来非常困难的莫比乌斯反演。它有什么用？推式子……

## 例题

### 洛谷 P1390 公约数的和

$2\le n\le 2\times 10^6$，求 $\displaystyle\sum_{i=1}^n \sum_{j=i+1}^n \gcd(i,j)$。

这题有不用莫反的做法，但是我们拿它练习莫反，所以要用莫反。

解法：首先显然原式 $=\displaystyle\sum_{i=1}^n\sum_{j=1}^{i-1}\gcd(i,j)=\dfrac{\displaystyle\left(\sum_{1\le i,j\le n}\gcd(i,j)\right)-\left(\sum_{i=1}^n \gcd(i,i)\right)}{2}$。我们只需要求 $\sum_{1\le i,j\le n}\gcd(i,j)$。

考虑分类。当 $\gcd=d$ 时有多少种情况？即求 $\displaystyle\sum_{i=1}^n\sum_{j=1}^n[\gcd(i,j)=d]$，也就是 $\displaystyle\sum_{i=1}^{\frac{n}{d}}\sum_{j=1}^{\frac{n}{d}} [\gcd(i,j)=1]$。

因为 $[x=1]=\varepsilon(x)$，所以 $[\gcd(i,j)=1]=\varepsilon(\gcd(i,j))$。

这个时候乱用一次莫比乌斯反演。$\displaystyle\sum_{i=1}^{\frac{n}{d}}\sum_{j=1}^{\frac{n}{d}}\sum_{k\mid i,k\mid j}\mu(k)$。

考虑交换求和顺序。$\displaystyle\sum_{k=1}^n\mu(k)\sum_{1\le i,j\le \frac{n}{d}}[k\mid i,k\mid j]$，把后面那坨展开就是 $\displaystyle \left(\sum_{i=1}^{\frac{n}{d}} [k\mid i]\right)\left(\sum_{j=1}^{\frac{n}{d}} [k\mid j]\right)$，注意到 $i,j$ 相同，所以是 $\displaystyle\left(\sum_{i=1}^{\frac{n}{d}}[k\mid i]\right)^2$。

里面那一坨显然就是 $\left\lfloor\dfrac{n}{kd}\right\rfloor$，所以整个是 $\displaystyle\sum_{k=1}^n \mu(k)\left\lfloor\dfrac{n}{kd}\right\rfloor^2$。然后在最外面关于 $d$ 求和，得到：

$$\sum_{d=1}^n d\sum_{k=1}^n \mu(k)\left\lfloor\dfrac{n}{kd}\right\rfloor^2$$

注意到 $kd\le n$ 才不为 $0$，引入换元，$p=kd$。然后可以把 $d$ 去掉。同时我们发现分数的向下取整的平方不是很好办，考虑移到最旁边暴力处理。

$$\displaystyle
\begin{aligned}&\sum_{d=1}^nd\sum_{k=1}^n\mu(k)\left\lfloor\dfrac{n}{p}\right\rfloor^2\\
=&\sum_{1\le d,k\le n} \dfrac{p}{k}\mu(k)\left\lfloor\dfrac{n}{p}\right\rfloor^2\\
=&\sum_{p=1}^n\left\lfloor\dfrac{n}{p}\right\rfloor^2\sum_{k\mid p} k\mu\left(\dfrac{p}{k}\right)\end{aligned}$$

右边这个东西是一个莫比乌斯反演的标准形式，也就是我们要寻求一个函数 $f(p)$ 满足 $\displaystyle\sum_{p\mid n} f(p)=n$，有点经验的人应该能够一眼秒出这个函数就是 $\varphi(n)$。不过不知道这个没关系，我们只需要知道因为 $g(x)=k$ 是积性的，所以 $\displaystyle\sum_{k\mid p}k\mu\left(\dfrac{p}{k}\right)$ 也一定是积性的，可以线性筛求值即可。我们先预处理出 $\varphi$ 的值，然后暴力算即可。时间复杂度线性。

还记得开头吗？注意最后我们要减去 $\dfrac{n(n+1)}{2}$ 并除以 $2$。

时间复杂度：线性。另外这题还可以多测，此时最后一步需要数论分块解决。

```cpp
#include <cstdio>

using namespace std;

int phi[2000005];
bool isp[2000005];
int p[2000005];

int main()
{
    int n;
    scanf("%d", &n);
    // 首先线性筛求 phi
    phi[1] = 1;
    int cur = 0;
    for (int i = 2; i <= n; i++)
    {
        if (!isp[i])
        {
            p[++cur] = i;
            phi[i] = i - 1;
        }
        for (int j = 1; j <= cur && i * p[j] <= n; j++)
        {
            isp[i * p[j]] = true;
            if (i % p[j] == 0)
            {
                phi[i * p[j]] = phi[i] * p[j];
                break;
            }
            else phi[i * p[j]] = phi[i] * (p[j] - 1);
        }
    }
    // 然后求值
    long long sum = 0;
    for (int i = 1; i <= n; i++)
    {
        sum += 1ll * (n / i) * (n / i) * phi[i];
    }
    printf("%lld\n", (sum - 1ll * n * (n + 1) / 2) / 2);
    return 0;
}
```

### 洛谷 P2522 \[HAOI2011\] Problem B

多测，求 $\displaystyle\sum_{i=a}^b \sum_{j=c}^d [\gcd(i,j)=k]$。

书接上回。

我们知道当 $a=b=1,b=d=n$ 时答案是 $\displaystyle\sum_{k=1}^n \mu(k)\left\lfloor\dfrac{n}{kd}\right\rfloor^2$，而当 $b\neq d$ 时只需要把 $\left\lfloor\dfrac{n}{kd}\right\rfloor^2$ 更改为 $\left\lfloor\dfrac{b}{kd}\right\rfloor\left\lfloor\dfrac{d}{kd}\right\rfloor$，求和上界改为 $\min(b,d)$ 即可。

运用二维前缀和思想可线性过单测，多测？

注意到这个式子能数论分块（$\mu$ 前缀和线性筛即可）（二维数论分块和一维几乎一样，不过是从一个值变动改为两个值的任何一个变动了即可），然后就这样了。时间复杂度是 $\mathcal O\left(V+\sum\left(\sqrt{a}+\sqrt{b}\right)\right)$，其中 $V$ 是预处理复杂度。代码懒得写了。

## 参考文献

说了我数论几乎全是自学的嘛（截至目前），有问题欢迎提出。

参考文献：

- [OI-wiki](https://oi-wiki.org/math/number-theory/mobius/) 是神！
- [洛谷 Kublic 的文章](https://www.luogu.com.cn/article/0n8ulnfi) %%% Orz。
