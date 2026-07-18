# 浅谈 BSGS 算法

**BSGS**（Baby-Step Giant-Step）算法是 Shank 发明的一个用于在 $\mathcal O(\sqrt p)$ 时间复杂度内计算**离散对数**的算法，要求模数为质数。扩展 BSGS 不需要模数为质数。

何为离散对数？普通的对数 $\log_ab=c$ 相当于求一个 $c$ 满足 $a^c=b$。而离散对数就是如题目所说，给定 $p,b,n$，求 $b^l\equiv n\pmod p$。

首先显然如果 $l$ 存在则必然存在 $l<p$，因为 $b^{p-1}\equiv 1\pmod p$，如果你还不知道费马小定理请离开，你现在不应该看这篇文章。

考虑一个显然的暴力做法：依次检查。虽然是暴力，但是我们不至于傻到每求一个 $b^l$ 都跑一次快速幂 ${}^1$，显然是可以递推的。

BSGS 则利用了幂运算的性质 $a^b\times a^c=a^{b+c}$（在模意义下显然也成立），使用类似分块的做法，做到了根号级别的时间复杂度。

首先我们暴力求出当 $l\le \sqrt{p}$ 时的所有余数，如果有 $n$ 则可以直接返回答案。否则，我们可以相应地求出 $\sqrt{p}<l\le 2\sqrt{p}$ 时的答案，然后判断。以此类推。

看上去并没有什么作用，但是实际上，如果 $b^{l+\sqrt{p}}\equiv n\pmod p$，那么 $b^{l}\equiv n\times(b^{\sqrt{p}})^{-1}\pmod p$。同时 $b^{-\sqrt p}$ 也是一个常数可以预处理，我们就可以利用我们求的所有当 $l\le \sqrt p$ 时的余数快速判断了（把 $l\le \sqrt p$ 的所有余数存到一个 `set` 或者哈希表 `unordered_set` 或者其它奇奇怪怪的数据结构中）。

进一步地，如果 $b^{l+k\sqrt p}\equiv n\pmod p$，则 $b^l\equiv n\times (b^{-\sqrt p})^k\pmod p$。所以我们就得到了总时间复杂度为 $\mathcal O(\sqrt n)$ 的计算离散对数的做法。

我们上面都假设 $\sqrt p$ 是整数，实际上不可能是，但是我们取 $\sqrt p$ 在正确性上并没有什么实际意义，只是为了保证时间复杂度，所以取 $\lfloor \sqrt p \rfloor$ 和 $\lceil \sqrt p \rceil$ 也行。

注意本题不仅仅要判断存在性，还要输出解。所以我们不能只保存余数，还要保存下标。

附上丑陋的代码。

```cpp
#include <cstdio>
#include <unordered_map>

using namespace std;

unordered_map<int, int> um;

long long qpow(int x, int y, int p)
{
    if(y == 0) return 1;
    if(y == 1) return x;
    long long r = qpow(x, y >> 1, p);
    r = r * r % p;
    if(y & 1) r = r * x % p;
    return r;
}

int main()
{
    int p, b, n;
    scanf("%d%d%d", &p, &b, &n);
    int sqrtp = 0;
    long long mul = 1;
    // 0 ~ sqrtp
    for(int i=0;1ll*i*i<=p;i++)
    {
        sqrtp = i;
        if(!um.count(mul = (i == 0 ? 1 : mul * b % p))) um[mul] = i;
        if(mul == n)
        {
            printf("%d\n", i);
            return 0;
        }
    }
    sqrtp++; mul = mul * b % p;
    long long invmul = qpow(mul, p - 2, p), mulll = 1;
    // i*sqrtp ~ (i+1)*sqrtp-1
    for(int i=1;1ll*sqrtp*i<=p;i++)
    {
        if(um.count((mulll = mulll * invmul % p) * n % p))
        {
            printf("%d\n", um[mulll * n % p] + sqrtp * i);
            return 0;
        }
    }
    printf("no solution\n");
    return 0;
}
```

[record](https://www.luogu.com.cn/record/221950438)。

${}^1$：倒是有一种底数确定的 $\mathcal O(\sqrt n)$ 预处理，$\Theta(1)$ 查询的光速幂算法。核心思想和 BSGS 十分相似——预处理出 $1\le i\le \sqrt p$ 的 $a^i$ 和 $a^{i\times \lfloor \sqrt p\rfloor}$，然后用这两个凑出答案。用这个的话也不会劣化我们的暴力时间复杂度，毕竟是常数时间查询的。
