# Introduction to KMP Algorithm

> 额……好多字符串模版题都能交题解啊。可惜我字符串纯自学，烂的一批。

我们先不考虑抽象的第二问（border），先来考虑第一问。

为了方便，我们令 $n=\lvert s_1 \rvert,m=\lvert s_2 \rvert$。

一个朴素的想法就是，枚举每个 $l$。

这个想法非常好，可惜时间复杂度为 $\Omicron(nm)$（且并非 $\omicron(nm)$，可以全都是同一个字符来卡掉）。显然会 TLE。

