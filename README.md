# 第一章 基础数学和统计工具

> - 本章介绍算法分析中常用的渐进性符号和一些概率不等式.

## 1 渐进性分析与复杂度

> - 本节其详情可见算法笔记中[渐进记号部分](https://forget-eve.github.io/Algorithm-design-analysis/#/?id=_121-%e6%b8%90%e8%bf%9b%e8%ae%b0%e5%8f%b7)

- [x] 假设 $f(n)$ 和 $g(n)$ 是两个关于问题规模 $n$ 的函数，它们之间的渐进性关系定义如下：

- $f(n) = Θ(g(n)): ∃n_0, c_1, c_2 > 0, s.t., 当 n > n_0 时,c_1g(n) ≤ f(n) ≤ c_2g(n)$

- $f(n) = O(g(n)): ∃n_0, c_2 > 0, s.t., 当 n > _n0 时,f(n) ≤ c_2g(n)$

- $f(n) = o(g(n)): ∀c > 0, ∃n_0, s.t., 当 n > n_0 时,f(n) < c_g(n)$

- $f(n) = Ω(g(n)): ∃n_0, c_1 > 0, s.t., 当 n > n_0 时,f(n) ≥ c_1g(n)$

- $f(n) = ω(g(n)): ∀c > 0, ∃n_0, s.t., 当 n > n_0 时,f(n) > c_g(n)$

- [x] 渐进关系反应的是当 $n$ 非常大时函数的量级关系，也可以理解为函数增长速率的大小关系。 $Θ$ 表示当 $n$ 非常大时，两个函数的以相同的速度增长，属于同一量级，类似于 $=$ 关系。 $O$ 和 $Ω$ 则类似于 $≤$ 和 $≥$ ， $o$ 和 $ω$ 则类似于 $<$ 和 $>$

- [x] 例如 $f(n) = 2n^2 + n + 4$ ，可以说 $f(n) = Θ(n^2)$ ；也可以说 $f(n) = O(n^2)$ 或 $f(n) = O(n^3)$ ，可以是 $f(n) = o(n^2 \log n)$ ；还可以说 $f(n) = Ω(n^2)$ 或 $f(n) = Ω(n \log n)$ 或 $f(n) = ω(n)$

- Example 1.1( `主定理的证明` ). $T(n) = aT(\frac{n}{b}) + f(n)$ ，求 $T(n)$
> - Solution.

$$T(n)=a(aT(\frac{n}{b^2})+f(\frac{n}{b}))+f(n)=a^2T(\frac{n}{b^2})+af(\frac{n}{b})+f(n)=\dots(逐项展开)$$

> - 由于最终要将 $n$ 分为 $1$ ，即看 $k$ 取何值时，有 $\frac{n}{b^k}=1$ ，可以求得 $k= \log _b n$ ，所以有

$$T(n)= a^{\log _b n}·T(1) +\sum\limits^{\log _b n-1} _{i=0} a^i·f(\frac{n}{b^i})=Θ(n^{\log _b a}) + \sum\limits^{\log _b n-1} _{i=0} a^i·f(\frac{n}{b^i})$$

> > 注：最后一步中由 $a^{\log _b n}·T(1)$ 变为 $Θ(n^{\log _b a})$ 是由于 $T(1)$ 为常量，假设为 $C$ ，则前式中可以变为 $C a^{\log _b n}$ ，现在来比较 $a^{\log _b n}$ 和 $n^{\log _b a}$ 的大小，由于这样比较不太直观那么同时取自然对数，则相当于比较 $\log _b n\ln a$ 和 $\log _b a\ln n$ 的大小关系，那么取函数 $f(n)=\log _b n\ln a -\log _b a\ln n$ ，那么此时对 $f(n)$ 求导数有 $f(n)'= \ln a \frac{1}{\ln b n} - \frac{\log _b a}{n}= \frac{\log _b a}{n}-\frac{\log _b a}{n}(换底公式)=0$ ，那么不难可知两个函数的增率是一致的，那么根据渐进性关系的 `几何意义` (此时就不用严格去推理了)就可以知道 **$a^{\log _b n}·T(1)=\theta (n^{\log _b a})$** ，作为结论可以记下。

> - (1) 当 $f(n) = \theta (n^{\log _b a})$ 时，

$$\sum\limits^{\log _b n-1} _{i=0} a^i·f(\frac{n}{b^i})=
\sum\limits^{\log _b n-1} _{i=0} a^i· \theta ((\frac{n}{b^i})^{\log _ba}) =\sum\limits^{\log _b n-1} _{i=0} \theta(n^{\log _ba}) = Θ(n^{\log _ba} \log_b n)$$

> > 注：第一步是由于直接带入，第二步由于 $a^i$ 就是常数，则 $a^i· \theta ((\frac{n}{b^i})^{\log _ba})=\theta ((\frac{n}{b^i})^{\log _ba})$ ，而由于 $\frac{1}{b^{i \log _ba}}$ 也是常数，所以有 $\theta ((\frac{n}{b^i})^{\log _ba})=\theta(n^{\log _ba})$ ，最后一步对 $\theta(n^{\log _ba})$ 进行 $\log _b n$ 求和可以得到。

> - 因此， $T(n) = Θ(n^{\log _b a}\log n)$

> - (2) 当 $f(n) = O(n^{\log _b a−ϵ})$ 时( $\epsilon > 0$ ，下同)，

$$\sum\limits^{\log _b n-1} _{i=0} a^i·f(\frac{n}{b^i})=\sum\limits^{\log _b n-1} _{i=0} a^i·O((\frac{n}{b^i})^{\log _b a−ϵ})=\sum\limits^{\log _b n-1} _{i=0} O(n^{\log _b a−ϵ}·(b^ϵ)^i)= O(n^{\log _b a−ϵ})·O(\frac{n^ϵ − 1}{b^ϵ − 1})= O(n^{\log _b a−ϵ}) · O(n^ϵ)= O(n^{\log _ba})$$

> > 注：第一步还是直接带入，第二步则是因为 $(\frac{1}{b^i})^{\log _b a−ϵ}=\frac{1}{b^{i (\log _b a−ϵ)}}=\frac{1}{b^{\log _b a^i−iϵ}}=\frac{b^iϵ}{b^{\log _b a^i}}=\frac{b^iϵ}{a^i}$ ，将外面的 $a^i$ 乘入则为 $b^{iϵ}=(b^ϵ)^i$ ，第三步则是利用渐进性关系乘法法则，将其分为两个项，便于后续将第四步中的 $1$ 去掉
> >
> > 则有 $O(n^{\log _b a−ϵ}·(b^ϵ)^i)=O(n^{\log _b a−ϵ})·O((b^ϵ)^i))$ ，前者不含 $i$ 可在求和时提出，此时对后一项求和，也就是等比数列求和即可，而由于 $\frac{1}{b^ϵ-1}$ 为常数，所以第四步可以把其和 $1$ 忽略，第五步是利用$O$ 的性质， $O(f(x))·O(h(x))=O(f(x)·h(x))$ ，证明见下
> > > 证明:假设 $k(x)=F(x)·H(x)$ ，其中 $F(x)=O(f(x)), H(x)=O(h(x))$ 由定义有 $∃n_0, c_1,c_2 > 0 ,当 n > _n0 时, F(x) ≤ c_1f(n),H(x) ≤ c_2h(x)$ ，那么 $k(n)=F(x)·H(x)≤ c_1c_2 f(x)·h(x)$ , 则 $k(x)=O(f(x)·h(x))$ ，则可证。

> - 因此， $T(n) = O(n^{\log _b a})$

(3) 当 $f(n) = Ω(n^{\log _b a+ϵ})$ , 且存在常数 $c < 1$ 使得当 $n$ 足够大时有 $af(\frac{n}{b}) ≤ cf(n)$ 时，

$$\sum\limits^{\log _b n-1} _{i=0} a^i·f(\frac{n}{b^i}) ≤ \sum\limits^{\log _b n-1} _{i=0} c^i f(n) + O(1) \ \ \ \  \ \  \\  \ \ \ \  \ \ \ \ \(1)$$

$$≤ f(n)\frac{1}{1 − c}+ O(1)= O(f(n))$$

> - 式 $(1)$ 中的 $O(1)$ 用于覆盖那些 $n$ 不够大使得条件中不等式不成立的项。因此， $T(n) =O(f(n))$ 。结合 $T(n)$ 的定义可知 $T(n) = Ω(f(n))$ ，综合来看， $T(n) = Θ(f(n))$ 。

## 2 概率不等式

### 2.1 Markov’s Inequality

### 2.2 Chebyshev’s Inequality

### 2.3 Chernoff Bound
