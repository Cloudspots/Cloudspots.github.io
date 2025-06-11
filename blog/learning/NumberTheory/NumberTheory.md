# 数论

## 裴蜀定理

> 本段 copy 自洛谷 @[LionBlaze](https://www.luogu.com/user/911054) 的文章 bezout，没有经得作者同意。以下是正文。
>
> 注：下文中 $(a,b)$ 均指 $\gcd(a,b)$，即 $a$ 和 $b$ 的最大公约数。而 $a \perp b$ 代表 $a,b$ 互质（在数论中表示互质，在几何中表示两直线垂直），即 $(a,b)=1$。

裴蜀定理是一个数学定理，它指出，对于任意整数 $a,b,c$，$ax+by=c$ 有整数解当且仅当 $(a,b)\mid c$。

### 证明

首先证明 $(a,b)\mid c$ 时有解。

我们令 $u=\frac{a}{(a,b)},v=\frac{b}{(a,b)},w=\frac{c}{(a,b)}$，则原方程等价于 $ux+vy=w$。

什么，这啥都没变啊？这里 $u\perp v$。

显然这个方程有解当且仅当 $ux\equiv w\pmod v$。考虑模 $v$ 完全剩余系，因为 $u,v$ 互质，所以将完全剩余系元素都乘上 $u$ 还是模 $v$ 完全剩余系，必然有和 $w$ 同余的元素，证毕。

然后证明 $(a,b)\nmid c$ 时无解。因为 $(a,b)\mid ax+by$，而 $(a,b)\nmid c$，故 $ax+by\neq c$，证毕。

~~我对这个东西证明记得特别清楚，因为有一次数学老师要求默写证明结果没写出来。~~

### 推广

将其推广到 $n$ 元形式：$\sum_{i=1}^n a_ix_i=c$ 有整数解当且仅当 $(a_1,a_2,a_3,\dots,a_n)\mid c$。

#### 证明推广形式

反复应用二元形式即可：首先 $a_1x_1+a_2x_2$ 能够表示所有是 $(a_1,a_2)$ 的倍数的数，接下来合并 $a_3$，然后以此类推，就能够表示所有是 $(a_1,a_2,a_3,\dots,a_n)$ 的倍数的数。

### 应用

还是在数学中应用比较多，通常用于证明一些方程的解的存在性，如二元一次不定方程，同余方程。但是在 OI 中也有很少的一些应用，比如[【模版】裴蜀定理](https://www.luogu.com.cn/problem/P4549)。

显然这样可以表示所有的 $A$ 的最大公约数的倍数，而必须是正整数，所以就是最大公约数本身。我敢肯定所有正经（不是 `puts("I cannot solve this problem, but I still can say: \nNo, Commander!")` 这样的东西）的非正解做法都比正解长。

参考代码：

```cpp
#include <cstdio>

using namespace std;

int igcd(int x, int y) { return y == 0 ? x : igcd(y, x % y); }

int gcd(int x, int y) { return igcd(x > 0 ? x : -x, y > 0 ? y : -y); }

int main()
{
    int n;
    scanf("%d", &n);
    int r = 0;
    for(int i=1;i<=n;i++)
    {
        int k;
        scanf("%d", &k);
        r = gcd(r, k);
    }
    printf("%d\n", r);
    return 0;
}
```

~~代码太短，建议评红。~~

[record](https://www.luogu.com.cn/record/219553668)。

## exgcd

> 本段 copy 自洛谷 @[LionBlaze](https://www.luogu.com/user/911054) 的文章 exgcd，没有经得作者同意。以下是正文。
