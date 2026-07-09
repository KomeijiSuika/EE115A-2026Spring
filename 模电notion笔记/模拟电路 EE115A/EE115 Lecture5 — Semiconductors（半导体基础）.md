# EE115 Lecture5 — Semiconductors（半导体基础）

<aside>
📘

**主题：** 半导体物理基础 —— 从硅晶体 → 载流子 → 掺杂 → 两种电流（漂移 + 扩散）→ Einstein 关系。

**教材：** Sedra/Smith — *Microelectronic Circuits*，pp.135–148（Ch.3）。

**核心脉络：** 硅的共价键晶体（本征）→ 热生成电子/空穴对（$np=n_i^2$）→ 掺杂造出 N/P 型（多子 / 少子）→ 载流子在电场下**漂移**、在浓度梯度下**扩散** → Einstein 关系把迁移率 $\mu$ 和扩散系数 $D$ 用 $V_T$ 连起来。这是后面 PN 结、二极管、MOSFET 的物理地基。

</aside>

## 1️⃣ 硅与晶体结构

- **为什么选硅做 IC？** 硅是 **IV 族元素**（4 个价电子），原料丰富、能长出稳定氧化层 $\text{SiO}_2$、工艺成熟。
- 硅原子以 **共价键（covalent bond，共价键）** 结合成 **金刚石立方（Diamond Cubic）** 晶格，每个原子和 4 个邻居共享电子。
- 在 **0 K** 时，所有价电子都被「锁」在共价键里 → 没有自由载流子 → 硅表现得像 **绝缘体（insulator）**。

[Slide 4 — 硅晶体结构（Diamond Cubic）与 2-D 共价键表示，Fig 3.1](https://app.notion.com)

Slide 4 — 硅晶体结构（Diamond Cubic）与 2-D 共价键表示，Fig 3.1

<aside>
🎯

**口诀：** 0 K 全锁死 → 绝缘；升温才有戏 —— 半导体的导电性来自「被热打断的键」。

</aside>

## 2️⃣ 电子、空穴与本征半导体

### 热生成与复合

- **热生成（Thermal Generation）：** 室温下热能打断部分共价键，产生**自由电子**和**空穴（hole，空穴）**。
- **空穴**：电子离开后留下的空位；相邻电子填进来 → 空穴「移动」→ 可当作一个**带正电的粒子**处理。
- **复合（Recombination，复合）：** 自由电子和空穴随机游走，电子填回空穴即复合。稳态下生成率 = 复合率。

[Slide 5 — 热生成产生电子-空穴对、空穴移动与复合，Fig 3.2](https://app.notion.com)

Slide 5 — 热生成产生电子-空穴对、空穴移动与复合，Fig 3.2

### 本征半导体（Intrinsic，本征）

纯硅中电子浓度 $n$ = 空穴浓度 $p$ = 本征载流子浓度 $n_i$：

$$
n = p = n_i = B\,T^{3/2}\,e^{-E_g/(2kT)}
$$

- $E_g$：带隙能量（bandgap），硅 $E_g=1.12\ \text{eV}$。
- $k$：玻尔兹曼常数 $=8.62\times10^{-5}\ \text{eV/K}$。
- 室温 $T=300\ \text{K}$：$n_i\approx1.5\times10^{10}\ \text{cm}^{-3}$。
- 硅约有 $5\times10^{22}\ \text{atoms/cm}^3$ → 自由载流子比例极小。

**质量作用定律（无论是否掺杂都成立）：**

$$
np = n_i^2
$$

<aside>
🎯

**口诀：** $np=n_i^2$ 是「守恒律」—— 掺杂把多子拉高，少子就被同比例压低，乘积永远锁定在 $n_i^2$。$n_i$ 随温度 **指数级** 上升（$T^{3/2}e^{-E_g/2kT}$ 里指数项主导）。

</aside>

## 3️⃣ 掺杂：N 型与 P 型

掺杂（doping）= 往硅里掺入杂质原子，人为地大幅提高某一种载流子浓度。

### N 型半导体（施主 / donor）

- 掺入 **V 族**元素 P（磷）/ As（砷），比硅多 1 个价电子 → 贡献 1 个自由电子，称 **施主（donor，施主）**。
- $n_n = N_D$（施主浓度），少子 $p_n = \dfrac{n_i^2}{N_D}$。
- 关系：$n_n \gg n_i \gg p_n$。**电子 = 多子（majority）**，空穴 = 少子（minority）。
- 例：$N_D=10^{17}\ \text{cm}^{-3}$ → $n_n=10^{17}$，$p_n=\dfrac{(1.5\times10^{10})^2}{10^{17}}\approx2.2\times10^{3}\ \text{cm}^{-3}$。

[Slide 7 — 五价元素掺杂形成 N 型（每个施主贡献一个自由电子），Fig 3.3](https://app.notion.com)

Slide 7 — 五价元素掺杂形成 N 型（每个施主贡献一个自由电子），Fig 3.3

### P 型半导体（受主 / acceptor）

- 掺入 **III 族**元素 B（硼），比硅少 1 个价电子 → 留下 1 个空穴，称 **受主（acceptor，受主）**。
- $p_p = N_A$（受主浓度），少子 $n_p = \dfrac{n_i^2}{N_A}$。
- 关系：$p_p \gg n_i \gg n_p$。**空穴 = 多子**，电子 = 少子。

[Slide 8 — 三价元素硼掺杂形成 P 型（每个受主产生一个空穴），Fig 3.4](https://app.notion.com)

Slide 8 — 三价元素硼掺杂形成 P 型（每个受主产生一个空穴），Fig 3.4

| 类型 | 掺杂元素 | 多子 | 少子 | 浓度关系 |
| --- | --- | --- | --- | --- |
| **N 型** | V 族 P / As（施主） | 电子 $n_n=N_D$ | 空穴 $p_n=n_i^2/N_D$ | $n_n\gg n_i\gg p_n$ |
| **P 型** | III 族 B（受主） | 空穴 $p_p=N_A$ | 电子 $n_p=n_i^2/N_A$ | $p_p\gg n_i\gg n_p$ |

<aside>
🎯

**口诀：** N 型「负」电子多（donor 多送电子），P 型「正」空穴多（acceptor 收电子留洞）。多子 ≈ 掺杂浓度，少子用 $np=n_i^2$ 反算。

</aside>

## 4️⃣ 漂移电流 Drift Current（漂移电流）

施加电场 $E$ 时，空穴顺 $E$ 漂移、电子逆 $E$ 漂移，但两者电流方向都沿 $E$：

$$
v_{p\text{-}drift} = \mu_p E,\qquad v_{n\text{-}drift} = -\mu_n E
$$

- $\mu_p$ 空穴迁移率、$\mu_n$ 电子迁移率。本征硅：$\mu_n=1350$，$\mu_p=480\ \text{cm}^2/\text{V·s}$（$\mu_n\approx2.5\,\mu_p$，电子更「灵活」）。

电流密度：

$$
J = qpv_{p\text{-}drift} + qnv_{n\text{-}drift} = q(p\mu_p + n\mu_n)E = \sigma E
$$

- 电导率 $\sigma = q(p\mu_p + n\mu_n)$ [S/cm]，电阻率 $\rho = 1/\sigma$ [Ω·cm]。

[Slide 9 — 电场 E 下空穴顺场、电子逆场漂移，两者电流均沿 E，Fig 3.5](https://app.notion.com)

Slide 9 — 电场 E 下空穴顺场、电子逆场漂移，两者电流均沿 E，Fig 3.5

<aside>
🎯

**口诀：** 漂移 = 电场「推」着跑，$J=\sigma E$ 就是半导体版欧姆定律；电子、空穴跑反方向，但带反电荷 → 电流同向、相加。

</aside>

## 5️⃣ 扩散电流 Diffusion Current（扩散电流）

**扩散：** 粒子从高浓度向低浓度移动；载流子带电，扩散即形成电流。电流正比于浓度梯度：

$$
J_{p\text{-}diff} = -qD_p\frac{dp(x)}{dx},\qquad J_{n\text{-}diff} = qD_n\frac{dn(x)}{dx}
$$

- $D_p$、$D_n$：空穴 / 电子扩散系数（diffusivity，扩散率）[cm²/s]。本征硅：$D_p=12$，$D_n=35\ \text{cm}^2/\text{s}$。
- 总扩散电流密度：

$$
J_{diff} = -qD_p\frac{dp(x)}{dx} + qD_n\frac{dn(x)}{dx}
$$

[Slide 10 — 空穴扩散与电子扩散（浓度梯度驱动），Fig 3.6 & 3.7](https://app.notion.com)

Slide 10 — 空穴扩散与电子扩散（浓度梯度驱动），Fig 3.6 & 3.7

<aside>
🎯

**口诀：** 漂移看**电场**、扩散看**浓度梯度**。注意符号：空穴顺梯度下坡（负号），电子带负电再翻一个号 → $J_{n\text{-}diff}$ 取正。

</aside>

## 6️⃣ Einstein 关系 + 例题

### Einstein 关系

扩散系数与迁移率不是独立的，二者通过热电压 $V_T$ 绑定：

$$
\frac{D_n}{\mu_n} = \frac{D_p}{\mu_p} = V_T = \frac{kT}{q}
$$

- 室温 $V_T = 26\ \text{mV}$。
- 物理意义：平衡态下，浓度梯度形成的扩散电流恰好被内建电场的漂移电流抵消 → **总电流为零**，由此推出 $D/\mu=V_T$。

### 例题：求空穴扩散电流

给定空穴浓度分布 $p(x)=p_o\,e^{-x/L_p}$，求 $x=0$ 处电流密度（$p_o=10^{16}\ \text{cm}^{-3}$，$L_p=1\ \mu\text{m}=10^{-4}\ \text{cm}$，$D_p=12\ \text{cm}^2/\text{s}$）：

$$
J_p = -qD_p\frac{dp}{dx} = -qD_p\left(-\frac{p_o}{L_p}e^{-x/L_p}\right)\Bigg|_{x=0} = \frac{qD_p p_o}{L_p}
$$

$$
J_p = \frac{(1.6\times10^{-19})(12)(10^{16})}{10^{-4}} = 192\ \text{A/cm}^2
$$

截面积 $A=100\ \mu\text{m}^2 = 10^{-6}\ \text{cm}^2$，故

$$
I_p = J_p\cdot A = 192 \times 10^{-6} = 1.92\times10^{-4}\ \text{A} = 192\ \mu\text{A}
$$

<aside>
🎯

**口诀：** 指数分布求梯度，$x=0$ 处 $|dp/dx|=p_o/L_p$；记住 $D/\mu=V_T=26\text{mV}$ 一式连接漂移与扩散两套世界。

</aside>

## 📝 作业 / Deadline

- [x]  **Homework 2** — Sedra/Smith Ch.2 题 2.95, 2.97, 2.109, 2.112；Ch.3 题 3.2, 3.3, 3.8：截止 March 31, 2026 11:00 PM。

## 🧩 本节总结

<aside>
🧠

**半导体三句话：**

1. **本征 → 掺杂：** 纯硅靠热生成出等量电子-空穴（$n=p=n_i$）；掺 V 族得 N 型（电子多子）、掺 III 族得 P 型（空穴多子），但 $np=n_i^2$ 恒成立。
2. **两种电流：** 漂移由**电场**驱动（$J=\sigma E$，$\sigma=q(p\mu_p+n\mu_n)$）；扩散由**浓度梯度**驱动（$J\propto dp/dx$）。真实器件里两者并存。
3. **Einstein 关系：** $D/\mu=V_T=kT/q=26\text{mV}$ 把扩散与漂移统一，是后面 PN 结建立内建电势 $V_o$、推导二极管方程的钥匙。
</aside>

## 📎 原始 Slides

[EE115 Lecture5.pdf](EE115%20Lecture5%20%E2%80%94%20Semiconductors%EF%BC%88%E5%8D%8A%E5%AF%BC%E4%BD%93%E5%9F%BA%E7%A1%80%EF%BC%89/EE115_Lecture5.pdf)