# 如何编写数据生成代码

## 先举例

这是洛谷 P1001 的数据生成代码：

```plaintext
a = random(int, -1000000000, 1000000000)
b = random(-1000000000, 1000000000)
$data += a + ' ' + b
```

另一种版本：

```plaintext
t = makefunction{randint(-1e9, 1e9)} // 这里使用 random(-1e9, 1e9) 是错误的，类型会被推断为 long double
$data += t() + ' ' + t()
```

这是洛谷 P3371 的数据生成代码：

```plaintext
value_maker = makefunction{ randsum(int, 0, (1<<31)-1, random(0, (1<<31)-1)) }
makestringgraph = makefunction{ nmax : int, mmax : int, graph2string(randomgraph(random(1, nmax), random(1, mmax), value_maker), "{u} {v} {w}\n") }

if($id_ratio <= 0.2) { $data += makestringgraph(5, 15) }
elif($id_ratio <= 40) { $data += makestringgraph(100, 10000) }
else if($id_ratio <= 70) { $data += makestringgraph(1000, 100000) }
else $data += makestringgraph(10000, 500000)
```

这是洛谷 P4779 的数据生成代码（简化版本，只卡了 SLF 优化）：

```plaintext
value_maker = makefunction{ randsum(int, 0, 1e9, random(0, makeint(1e9))) }
$data += SPFAhacker(random(1, 100000), random(1, 200000), ["SLF"])
```

## 完整语法

### 变量

普通的类型：基本类型（包括 `int`、`long double` 和 `rational`）、`function` 和 `type`。

解释：

- `int`：不限大小的整数。实际上使用压 $64$ 个二进制位的压位高精度实现，但是当数字使用 `long long` 或 `unsigned long long` 能够存下的时候使用这两者。
- `long double`：C++ `long double` 类型。
- `rational`：有理数类型。分母使用特化的无符号 `int` 实现，分子使用 `int`。
- `function`：函数。
- `type`：类型。

一些更强的类型：`graph`、`vector`。

如同 Python，一个变量的类型可以自由切换。
