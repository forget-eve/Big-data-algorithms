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

- **Example 1.1( `主定理的证明` )** . $T(n) = aT(\frac{n}{b}) + f(n)$ ，求 $T(n)$
> - **Solution.**

$$T(n)=a(aT(\frac{n}{b^2})+f(\frac{n}{b}))+f(n)=a^2T(\frac{n}{b^2})+af(\frac{n}{b})+f(n)=\dots(逐项展开)$$

> - 由于最终要将 $n$ 分为 $1$ ，即看 $k$ 取何值时，有 $\frac{n}{b^k}=1$ ，可以求得 $k= \log _b n$ ，所以有

$$T(n)= a^{\log _b n}·T(1) +\sum\limits^{\log _b n-1} _{i=0} a^i·f(\frac{n}{b^i})=Θ(n^{\log _b a}) + \sum\limits^{\log _b n-1} _{i=0} a^i·f(\frac{n}{b^i})$$

> > 注：最后一步中由 $a^{\log _b n}·T(1)$ 变为 $Θ(n^{\log _b a})$ 是由于 $T(1)$ 为常量，假设为 $C$ ，则前式中可以变为 $C a^{\log _b n}$ ，现在来比较 $a^{\log _b n}$ 和 $n^{\log _b a}$ 的大小，由于这样比较不太直观那么同时取自然对数，则相当于比较 $\log _b n\ln a$ 和 $\log _b a\ln n$ 的大小关系，那么取函数 $f(n)=\log _b n\ln a -\log _b a\ln n$ ，那么此时对 $f(n)$ 求导数有 $f(n)'= \ln a \frac{1}{\ln b n} - \frac{\log _b a}{n}= \frac{\log _b a}{n}-\frac{\log _b a}{n}(换底公式)=0$ ，那么不难可知两个函数的增率是一致的，那么根据渐进性关系的 `几何意义` (此时就不用严格去推理了)就可以知道 **$a^{\log _b n}·T(1)=Θ(n^{\log _b a})$** ，作为结论可以记下。

> - (1) 当 $f(n) = \theta (n^{\log _b a})$ 时，

$$\sum\limits^{\log _b n-1} _{i=0} a^i·f(\frac{n}{b^i})=
\sum\limits^{\log _b n-1} _{i=0} a^i· \theta ((\frac{n}{b^i})^{\log _ba}) =\sum\limits^{\log _b n-1} _{i=0} \theta(n^{\log _ba}) = Θ(n^{\log _ba} \log_b n)$$

> > 注：第一步是由于直接带入，第二步由于 $a^i$ 就是常数，则 $a^i· Θ((\frac{n}{b^i})^{\log _ba})=Θ((\frac{n}{b^i})^{\log _ba})$ ，而由于 $\frac{1}{b^{i \log _ba}}$ 也是常数，所以有 $Θ((\frac{n}{b^i})^{\log _ba})=Θ(n^{\log _ba})$ ，最后一步对 $Θ(n^{\log _ba})$ 进行 $\log _b n$ 求和可以得到。

> - 因此， $T(n) = Θ(n^{\log _b a}\log n)$

> - (2) 当 $f(n) = O(n^{\log _b a−ϵ})$ 时( $\epsilon > 0$ ，下同)，

$$\sum\limits^{\log _b n-1} _{i=0} a^i·f(\frac{n}{b^i})=\sum\limits^{\log _b n-1} _{i=0} a^i·O((\frac{n}{b^i})^{\log _b a−ϵ})=\sum\limits^{\log _b n-1} _{i=0} O(n^{\log _b a−ϵ}·(b^ϵ)^i)= O(n^{\log _b a−ϵ})·O(\frac{n^ϵ − 1}{b^ϵ − 1})= O(n^{\log _b a−ϵ}) · O(n^ϵ)= O(n^{\log _ba})$$

> > 注：第一步还是直接带入，第二步则是因为 $(\frac{1}{b^i})^{\log _b a−ϵ}=\frac{1}{b^{i (\log _b a−ϵ)}}=\frac{1}{b^{\log _b a^i−iϵ}}=\frac{b^iϵ}{b^{\log _b a^i}}=\frac{b^iϵ}{a^i}$ ，将外面的 $a^i$ 乘入则为 $b^{iϵ}=(b^ϵ)^i$ ，第三步则是利用渐进性关系乘法法则，将其分为两个项，便于后续将第四步中的 $1$ 去掉
> >
> > 则有 $O(n^{\log _b a−ϵ}·(b^ϵ)^i)=O(n^{\log _b a−ϵ})·O((b^ϵ)^i))$ ，前者不含 $i$ 可在求和时提出，此时对后一项求和，也就是等比数列求和即可，而由于 $\frac{1}{b^ϵ-1}$ 为常数，所以第四步可以把其和 $1$ 忽略，第五步是利用$O$ 的性质， $O(f(x))·O(h(x))=O(f(x)·h(x))$ ，证明见下
> > > 证明:假设 $k(x)=F(x)·H(x)$ ，其中 $F(x)=O(f(x)), H(x)=O(h(x))$ 由定义有 $∃n_0, c_1,c_2 > 0 ,当 n > _n0 时, F(x) ≤ c_1f(n),H(x) ≤ c_2h(x)$ ，那么 $k(n)=F(x)·H(x)≤ c_1c_2 f(x)·h(x)$ , 则 $k(x)=O(f(x)·h(x))$ ，则可证。

> - 因此， $T(n) = O(n^{\log _b a})$

> - (3) 当 $f(n) = Ω(n^{\log _b a+ϵ})$ , 且存在常数 $c < 1$ 使得当 $n$ 足够大时有 $af(\frac{n}{b}) ≤ cf(n)$ 时，

$$\sum\limits^{\log _b n-1} _{i=0} a^i·f(\frac{n}{b^i}) ≤ \sum\limits^{\log _b n-1} _{i=0} c^i f(n) + O(1) \ \ \ \  \ \  \\  \ \ \ \  \ \ \ \ \(1)$$

$$≤ f(n)\frac{1}{1 − c}+ O(1)= O(f(n))$$

> - 式 $(1)$ 中的 $O(1)$ 用于覆盖那些 $n$ 不够大使得条件中不等式不成立的项。因此， $T(n) =O(f(n))$ 。结合 $T(n)$ 的定义可知 $T(n) = Ω(f(n))$ ，综合来看， $T(n) = Θ(f(n))$ 。

$$
T(n)=
\begin{cases}
  O(𝑛^{\log_b{a}}), & 𝑓(𝑛)=𝑂(𝑛^{(\log_b{𝑎})−\epsilon}) for \ some \ 𝜀 > 0\newline
  Θ(𝑛^{\log_b{a}}\log_b n), & 𝑓(𝑛)= Θ(𝑛\log_b{𝑎}) \newline
  Θ(𝑓(𝑛)) , & 𝑓(𝑛)=\Omega(𝑛^{(\log_b{a})+\epsilon}) for \ some \ 𝜀 > 0 \text{ and } 𝑎𝑓(\frac{𝑛}{𝑏})≤ 𝑐𝑓(𝑛) for \ large \ 𝑛 , 𝑐 < 1
\end{cases}
$$

## 2 概率不等式

### 2.1 Markov’s Inequality

- [x] **Theorem 2.1.**  给定随机变量 $X > 0，∀k > 0$ ，有

$$Pr[X ≥ k · E[X]] ≤\frac{1}{k}$$

> - **Proof.**

$$E[X]= \int _0^{\infty}xf_X(x)dx≥\int ^{\infty} _{k·E[X]}xf_X(x)dx≥ k · E[X]\int ^{\infty} _{k·E[X]}f_X(x)dx= k · E[X] · Pr[X ≥ k · E[X]]$$

> - 第一步是期望的定义，第二步是由于，条件中所给出的随机变量 $X>0$ 且由于概率密度函数 $f_X(x)>0$ ，所以 $xf_X(x)>0$ ，所以缩短积分区间，明显会导致积分值变小，第三步是由于，在区间 $[ k · E[X] ,\infty]$ 上有 $x \geq k · E[X]$ ，所以将积分中的 $x$ 变为 $k · E[X]$ 提出会变小，第四步就是类似 $Pr[X \geq k]$ 的定义。

- [x] 通过简单代换，Markov’s Inequality 也可以表示为

$$Pr[X ≥ a] ≤\frac{E[X]}{a}$$

> - 推导过程: $Pr[X \geq a]=Pr[X \geq \frac{a}{E[X]}·E[X]]$ ，将上面本源公式中的 $k$ 看作此处的 $\frac{a}{E[X]}$ 即可得 $Pr[X ≥ a] ≤\frac{E[X]}{a}$ 

### 2.2 Chebyshev’s Inequality

- [x] **Theorem 2.2.**  给定随机变量 $X$ ，有

$$Pr[|X − E[X]|^2 ≥ k · Var[X]] ≤ \frac{1}{k}$$

- [x] 或者表示为

$$Pr[|X − E[X]| ≥ k] ≤ \frac{Var[X]}{k^2}$$

> - **Proof.** 将 $|X − E[X]|^2$ 视为随机变量，直接利用 [`Markov’s Inequality`](?id=_21-markovs-inequality) 即可证明。
> - 本源公式可以由于 $E[|X − E[X]|^2]=Var[X]$ 直接证明，而下面的一种形式推导过程为 $Pr[|X − E[X]| ≥ k]=Pr[|X − E[X]|^2 ≥ k^2]$ ，这是由于 $|X − E[X]|>0,k>0$ ，所以不会改变概率分布大小，那么根据上面 `Markov’s Inequality` 的第二种形式，也将 $|X − E[X]|^2$ 视为随机变量即可证明。

### 2.3 Chernoff Bound

- [x] **Theorem 2.3.** 给定 $n$ 个独立的随机变量 $X_1,\dots , X_n$ ，满足 $0 ≤ X_i ≤ 1$ 。令 $S =\sum\limits^n_{i=1} X_i$ , $µ = E[S]$ , $δ ∈ (0, 1)$ , 则

$$Pr[|S − µ| ≤ δµ] ≥ 1 − e^{−O(\frac{δ^2µ^2}{n})} \ \ \ \ \  \  \ \ \  (2)$$

> - **Proof.** 在此处先证明 $Pr[S > (1 + δ)µ]$ 部分，并假定考虑 $X_i$ 为 `Bernoulli` 变量(非0即1)时(即 $Pr[X_i = 1] = p_i, Pr[X_i = 0] = 1 − p_i$ )

$$Pr[S > (1 + δ)µ] = Pr[e^{λS} > e^{λ(1+δ)µ}]$$

$$\ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ ≤\frac{1}{e^{λ(1+δ)µ}}E[e^{λS}] \ \ \ \ \ \ \ \ \ \ \ \ \ \ (3)$$

$$\ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \  =\frac{1}{e^{λ(1+δ)µ}}\prod\limits ^n_{i=1}E[e^{λX_i}] \ \ \ \ \ \ (4)$$

$$\ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ =\frac{1}{e^{λ(1+δ)µ}}\prod\limits ^n_{i=1}(p_ie^λ + (1 − p_i))$$

$$\ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ ≤\frac{1}{e^{λ(1+δ)µ}}\prod\limits ^n_{i=1} e^{p_i(e^λ−1)} \ \ \ \ (5)$$

$$\ \ \ \ \ \ \ \ \ \ \ \ \ \ \ =\frac{e^{µ(e^λ−1)}}{e^{λ(1+δ)µ}}$$

$$\ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ = (\frac{e^δ}{(1 + δ)^{1+δ}})^µ \ \ \ \ \ \ \ (6)$$

> - 上式中 $(3)$ 可直接由 `Markov’s Inequality` 得到(很明显，随机变量 $e^{λS}$ 和取值 $e^{λ(1+δ)µ}$ 均大于 $0$ ，有不等式的第二种形式可得到)。 $(4)$ 由 $X_i$ 独立可得 $E[e^{λS}]=E[e^{\sum\limits_{i=1}^n λX_i}]=E[\prod\limits ^n_{i=1} e^{λX_i}]=\prod\limits ^n_{i=1}E[e^{λX_i}]$。 $(5)$ 利用不等式 $1 + x ≤ e^x$ 进行放缩获得。令 $e^λ = 1 + δ$ 代入得到  $(6)$。对上式最后的结果取对数分析：

$$\ln (\frac{e^δ}{(1 + δ)^{1+δ}})^µ=µ[δ − (1 + δ) \ln(1 + δ)]$$

$$ \ \ \ \ \ \  \ \ \ \ \ \  \ \ \ \ \ \ \ \ \ \ \ \  \ \ \ \ \ \ ≤ µ[δ − (1 + δ) \frac{δ}{1 + \frac{δ}{2}}] \ \ \ \ \ \ (7)$$

$$ \ \ \ \ \ \ = − \frac{δ^2}{2 + δ}µ$$

- [x] 式 $(7)$ 利用不等式 $\ln (1 + x) ≥\frac{x}{1+\frac{x}{2}}$ 放缩可得。将上述结果结合，可得

$$Pr[S > (1 + δ)µ] ≤ e^{−\frac{δ^2}{2+δ}µ} ≤ e^{−\frac{δ^2}{3}µ}$$

> - 关于 `Chernoff Bound` 的证明还可以使用其他的放缩方式，需要使用下述引理：

- [x] **Lemma 2.4 ( `Hoeffding lemma` )**. 假设随机变量 $a ≤ X ≤ b$ 且 $E[X] = 0$ ，那么对于 $∀λ$，有

$$E[e^{λX}] ≤ e^{\frac{λ^2(b − a)^2}{8}}$$

> - 关于 `Chernoff Bound` 的另一种证明方式如下：

> - **Proof.** 同前述步骤，可以得到

$$Pr[S > (1 + δ)µ] ≤ \frac{1}{e^{λ(1+δ)µ}}E[e^{λS}]$$

> - 使用 `Hoeffding lemma` 对 $E[e^{λS}]$ 进行放缩：

$$E[e^{λS}] = \prod\limits ^n_{i=1} E[e^{λX_i}]=\prod\limits ^n_{i=1} e^{λp_i}·E[e^{λ(X_i−p_i)}]$$

$$≤\prod\limits ^n_{i=1} e^{λp_i}·e^{\frac{λ^2}{8}}\ \ \ \ \ \ \ (8)$$

$$= e^{\frac{λ^2}{8}n+λµ}$$

> - 式 $(8)$ 即为使用 `Hoeffding` 引理所得(由于变量 $X_i-p_i$ 取值范围是 $-p_i \leq X_i-p_i \leq 1-p_i$ 即 $b-a=1$ ，而 $E[X_i-p_i]=E[X_i]-E[p_i]=p_i-p_i=0$ ，前者为 $p_i$ 是因为伯努利变量的定义，而后者是因为 $p_i$ 在此处相当于一个常量，故可以用该引理，前一步的拆分也是为了使得期望为0，可以使用引理)。将上述结果与之前分析相结合：

$$Pr[S > (1 + δ)µ] ≤\frac{e^{\frac{λ^2}{8}n+λµ}}{e^{λ(1+δ)µ}}=e^{\frac{λ^2}{8}n−λδµ}=e^{−\frac{2δ^2µ^2}{n}}$$

> - 上式最后一步中，令 $λ =\frac{4δµ}{n}$  即可得到对应结果。

- [x] 可以看到，两种证明方式最终获得结果在形式上有细微差异，但都可以写为式 $(2)$ 的渐进形式。`Chernoff Bound` 表明 $S$ 落在该区间之外的概率是和区间大小呈负指数关系。如果我们考虑的是 $\lbrace X_i\rbrace$ 的均值而不是它们之和时，使用 `Chernoff Bound` 会在 $e$ 的负指数上引入随机变量个数 $n$ ，这意味着我们估计的精度是和采样个数呈负指数关系，这个结论远强于 `Markov’s Inequality` 和 `Chebyshev’s Inequality` (可以试着用这两个不等式分析 $S$ 或均值并进行比对)
