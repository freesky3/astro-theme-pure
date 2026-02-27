---
title: brain2模块笔记
publishDate: 2026-02-26 23:48:00
description: '模拟神经网络'
tags:
  - Computation Neuroscience
heroImage: { src: './Brain2.png', color: '#142959' }
language: '中文'
---

## 1. 导入

Brian2 的一个核心特性是它的物理单位检查系统（如 `mV`, `ms`, `nA`）。为了让数学函数（如 `sin`, `exp`, `log`）能够正确处理带单位的数值，Brian2 实际上重写了 NumPy 的部分同名函数。

如果你不想使用 `from brian2 import *`（因为这会污染全局命名空间），可以像下面这样写：



```python
import brian2.numpy_ as np  # 这是一个特殊包装过的 NumPy
import brian2.only as br2   # 只导入 Brian2 的核心功能
```

## 2. Pysical Units

### 1. 基础单位 (Base Units)

Brian 严格遵循国际单位制 (SI)。它定义了 7 个基本单位，你可以直接在代码中使用它们的全称：

* **电流**: `amp` / `ampere`
* **质量**: `kilogram` / `kilogramme`
* **时间**: `second`
* **长度**: `metre` / `meter`
* **物质的量**: `mole` / `mol`
* **热力学温度**: `kelvin`
* **发光强度**: `candela`

### 2. 导出单位 (Derived Units)

基于基本单位，Brian 定义了神经科学中常用的导出单位：

* **电荷/电容**: `coulomb`, `farad`
* **频率**: `hertz`
* **能量/功率**: `joule`, `watt`
* **电压/电阻/电导**: `volt`, `ohm`, `siemens`
* **体积/浓度**: `liter` / `litre`, `molar`
* **压强/质量**: `pascal`, `gram`

### 3. 单位前缀与缩写规则

为了方便表达极小或极大的数值（如神经元的毫伏级电压），Brian 支持标准前缀：

* **常用前缀**: `p` (pico), `n` (nano), `u` (micro), `m` (milli), `k` (kilo), `M` (mega), `G` (giga), `T` (tera)。例如 `msiemens` 等于 $0.001 \times siemens$。
* **两个特例**:
  1. `kilogram` 不再接受额外前缀（防止双重前缀）。
  2. `metre` / `meter` 额外支持 `centi` (厘米) 前缀，即 `cmetre`。
* **常用缩写**: 系统预设了一些极简缩写，如 `ms` (毫秒), `mV` (毫伏), `nS` (纳西门子), `Hz` (赫兹), `cm` (厘米)。

### 4. 关键安全限制：禁止单字母缩写

这是初学者最容易踩的坑：**Brian 不允许使用单字母缩写作为单位**。

* **错误用法**: `v = 10 * V` 或 `g = 5 * S`。
* **原因**: `V` 或 `S` 在 Python 编程中太容易被当作普通变量名（如 `V` 代表 Voltage 数组）。
* **正确做法**: 必须使用 `mV` 或 `volt`，以及 `nS` 或 `siemens`。

### 5. 三种方法去除单位

| **方法**              | **描述**                                                     | **示例代码**                  |
| --------------------- | ------------------------------------------------------------ | ----------------------------- |
| **除以单位 (推荐)**   | 将变量除以你想要的单位，最安全且刻度清晰。                   | `tau / ms` (结果为 20.0)      |
| **转换为 NumPy 数组** | 使用 `asarray()` (不复制) 或 `array()` (复制) 转换为基准单位数组。 | `asarray(rates)`              |
| **使用下划线变量**    | 访问状态变量时在名称后加下划线 `_`。                         | `G.v_` (直接获取电压的纯数值) |

### 6. 关于温度单位转化

```python
from brian2.units.constants import zero_celsius

celsius_temp = 27
abs_temp = celsius_temp*kelvin + zero_celsius
```

### 7. Constants

| **常数名称 (Constant)**            | **符号 (Symbol)** | **Brian 名称**        | **数值与单位 (Value)**                         |
| ---------------------------------- | ----------------- | --------------------- | ---------------------------------------------- |
| 阿伏伽德罗常数 (Avogadro constant) | $N_A, L$          | `avogadro_constant`   | $6.022140857 \times 10^{23} \text{ mol}^{-1}$  |
| 玻尔兹曼常数 (Boltzmann constant)  | $k$               | `boltzmann_constant`  | $1.38064852 \times 10^{-23} \text{ J K}^{-1}$  |
| 真空介电常数 (Electric constant)   | $\epsilon_0$      | `electric_constant`   | $8.854187817 \times 10^{-12} \text{ F m}^{-1}$ |
| 电子质量 (Electron mass)           | $m_e$             | `electron_mass`       | $9.10938356 \times 10^{-31} \text{ kg}$        |
| 元电荷 (Elementary charge)         | $e$               | `elementary_charge`   | $1.6021766208 \times 10^{-19} \text{ C}$       |
| 法拉第常数 (Faraday constant)      | $F$               | `faraday_constant`    | $96485.33289 \text{ C mol}^{-1}$               |
| 普适气体常数 (Gas constant)        | $R$               | `gas_constant`        | $8.3144598 \text{ J mol}^{-1} \text{ K}^{-1}$  |
| 真空磁导率 (Magnetic constant)     | $\mu_0$           | `magnetic_constant`   | $12.566370614 \times 10^{-7} \text{ N A}^{-2}$ |
| 摩尔质量常数 (Molar mass constant) | $M_u$             | `molar_mass_constant` | $1 \times 10^{-3} \text{ kg mol}^{-1}$         |
| 摄氏零度 ($0^\circ\text{C}$)       | -                 | `zero_celsius`        | $273.15 \text{ K}$                             |

Note that these constants are not imported by default, you will have to explicitly import them from brian2.units.constants.
```python
from brian2 import *
from brian2.units.constants import zero_celsius, gas_constant as R, faraday_constant as F

celsius_temp = 27
T = celsius_temp*kelvin + zero_celsius
factor = R*T/F
```

### 8. Import Units

Brain also generates squared and cubed versions by appending a number.

You can import these units from the package `brian2.units.allunits` – accordingly, an `from brian2.units.allunits import *` will result in everything from `Ylumen3` (cubed yotta lumen) to `ymol` (yocto mole) being imported.

### 9. [In-place operations on quantities](https://brian2.readthedocs.io/en/stable/user/units.html#id8)

数量数组：原地修改（可变性）

当你对一个包含多个数值的单位数组（Quantity array）使用 `+=` 时，它会直接修改内存中的原数组。

```python
q = [1, 2] * mV  # 创建一个数组对象
r = q            # r 和 q 指向内存中同一个地址
q += 1*mV        # 原地修改数组内容
```

当你对单个带有单位的数值（Scalar quantity）进行运算时，它不会修改原对象，而是创建一个新的结果。

```python
x = 1 * mV
y = x            # y 指向数值 1*mV
x *= 2           # 实际上是创建了新值 2*mV 并让变量名 x 指向它
```

## 2. Models and neuron groups

### 1. model quation

通过`NeuronGroup`指定生成神经元群的数量和对应的微分方程：

```python
G = NeuronGroup(10, 'dv/dt = -v/(10*ms) : volt')

tau = 10*ms
G = NeuronGroup(10, 'dv/dt = -v/tau : volt')
```

Brian needs the model to be given in the form of differential equations, but you might see the integrated form of synapses in some textbooks and papers. See [Converting from integrated form to ODEs](https://brian2.readthedocs.io/en/stable/user/converting_from_integrated_form.html) for details on how to convert between these representations.

If a variable should be taken as a *parameter* of the neurons, i.e. if it should be possible to vary its value across neurons, it has to be declared as part of the model description:

```python
G = NeuronGroup(10, '''dv/dt = -v/tau : volt
                       tau : second''')

# To make complex model descriptions more readable, named subexpressions can be used
G = NeuronGroup(10, '''dv/dt = I_leak / Cm : volt
                       I_leak = g_L*(E_L - v) : amp''')
```

### 2. Noise

In addition to ordinary differential equations, Brian allows you to introduce random noise by specifying a [stochastic differential equation](https://en.wikipedia.org/wiki/Stochastic_differential_equation). Brian uses the physicists' notation used in the [Langevin equation](https://en.wikipedia.org/wiki/Langevin_equation), representing the "noise" as a term $\xi(t)$, rather than the mathematicians' stochastic differential $dW_t$. The following is an example of the [Ornstein-Uhlenbeck process](https://en.wikipedia.org/wiki/Ornstein–Uhlenbeck_process) that is often used to model a leaky integrate-and-fire neuron with a stochastic current:

```python
G = NeuronGroup(10, 'dv/dt = -v/tau + sigma*sqrt(2/tau)*xi : volt')
```

You can start by thinking of `xi` as just a Gaussian random variable with mean 0 and standard deviation 1. However, it scales in an unusual way with time and this gives it units of `1/sqrt(second)`.

### 3. [Threshold and reset](https://brian2.readthedocs.io/en/stable/user/models.html#id9)

Whenever the threshold condition is fulfilled, the reset statements will be executed. 

```python
v_r = -70*mV  # reset potential
G = NeuronGroup(10, '''dv/dt = -v/tau : volt
                       v_th : volt  # neuron-specific threshold''',
                threshold='v > v_th', reset='v = v_r')
```

### 4. [Refractoriness](https://brian2.readthedocs.io/en/stable/user/models.html#id10)

To make a neuron non-excitable for a certain time period after a spike, the refractory keyword can be used:

```
G = NeuronGroup(10, 'dv/dt = -v/tau : volt', 
				threshold='v > -50*mV',
                reset='v = -70*mV', refractory=5*ms)
```

默认行为：锁定在 Reset 电位

除了这种简单的固定时长限制外，该关键字还支持更复杂的表达方式（例如，不仅禁止触发脉冲，还可以在不应期内让膜电位保持在某个固定值，或者使用变量来定义时长）。参考[Refractoriness](https://brian2.readthedocs.io/en/stable/user/refractoriness.html)

### 5. [State variables](https://brian2.readthedocs.io/en/stable/user/models.html#id11)

Differential equations and parameters in model descriptions are stored as *state variables* of the [`NeuronGroup`](https://brian2.readthedocs.io/en/stable/reference/brian2.groups.neurongroup.NeuronGroup.html#brian2.groups.neurongroup.NeuronGroup). In addition to these variables, Brian also defines two variables automatically: `i`: The index of a neuron. `N`: The total number of neurons.

All state variables can be accessed and set as an attribute of the group. To get the values without physical units (e.g. for analysing data with external tools), use an underscore after the name:

```python
# 初始化神经元组并赋值
>>> G = NeuronGroup(10, '''dv/dt = -v/tau : volt
...                        tau : second''', name='neurons')
>>> G.v = -70*mV
>>> G.v
<neurons.v: array([-70., -70., -70., -70., -70., -70., -70., -70., -70., -70.]) * mvolt>

# 使用下划线获取无单位的原始数值 (SI unit: Volts)
>>> G.v_  # values without units
<neurons.v_: array([-0.07, -0.07, -0.07, -0.07, -0.07, -0.07, -0.07, -0.07, -0.07, -0.07])>

# 使用字符串表达式为每个神经元分配不同的 tau 值
# 其中 i 是神经元索引，N 是总数
>>> G.tau = '5*ms + (1.0*i/N)*5*ms'
>>> G.tau
<neurons.tau: array([ 5. ,  5.5,  6. ,  6.5,  7. ,  7.5,  8. ,  8.5,  9. ,  9.5]) * msecond>

# 使用条件表达式（String Indexing）进行切片赋值
# 将所有 tau > 7.25ms 的神经元电压设置为 -60mV
>>> G.v['tau > 7.25*ms'] = -60*mV
>>> G.v
<neurons.v: array([-70., -70., -70., -70., -70., -60., -60., -60., -60., -60.]) * mvolt>
```

### 6. [Subgroups](https://brian2.readthedocs.io/en/stable/user/models.html#id12)

```python
G = NeuronGroup(10, '''dv/dt = -v/tau : volt
                       tau : second''',
                threshold='v > -50*mV',
                reset='v = -70*mV')
# Create subgroups
G1 = G[:5]
G2 = G[5:]

# This will set the values in the main group, subgroups are just "views"
G1.tau = 10*ms
G2.tau = 20*ms
```

注意我们不能直接对`NeuronGroup`进行不连续切片：`G[[3, 5, 7]]`，但是我们可以用下面的方法代替：

```python
# 在模型定义中增加一个标记位（可以是布尔值或整数）
G = NeuronGroup(10, '''...
                       is_target : boolean''')

# 手动标记你不连续的神经元
target_indices = [3, 5, 7]
G.is_target = False
G.v[target_indices] = -60*mV # 直接对主组的不连续索引操作
G.is_target[target_indices] = True

# 后续操作时使用字符串筛选
# 比如只让这些特定的神经元受到刺激
G.I['is_target == True'] = 10*nA
```

### 7. [Shared variables](https://brian2.readthedocs.io/en/stable/user/models.html#id13)

通常情况下，`NeuronGroup` 中的变量（如膜电位 `v`）是**向量化**的，即每个神经元都有自己独立的值。但有时你需要一个对组内**所有神经元都相同**的变量，这就是“共享变量”。

**限制**：

* 不能在只针对“部分”神经元的上下文中使用（例如 `reset` 语句中，因为重置通常只发生在刚放电的特定神经元上）。
* 如果代码块中同时写共享变量和普通向量变量，**共享变量的赋值必须写在前面**。

```python
G = NeuronGroup(10, '''shared_input : volt (shared)
                       dv/dt = (-v + shared_input)/tau : volt
                       tau : second''', name='neurons')
```

### 8. Subexpressions

By default, subexpressions are re-evaluated whenever they are used. Sometimes it is useful to instead only evaluate a subexpression once and then use this value for the rest of the time step.

**关键标志：`(constant over dt)`**：

* 如果你希望在**一个时间步内 ($dt$)** 只计算一次该值，并在随后的所有计算中复用该值，就需要加上这个标志。
* **重要场景**：当子表达式包含随机函数（如 `rand()`）时，**必须**使用这个标志。否则，如果你用 `StateMonitor` 记录它，记录下来的值会和神经元方程里实际使用的值不一样（因为每次调用都生成了新的随机数）。

### 8. [Storing state variables](https://brian2.readthedocs.io/en/stable/user/models.html#id14)
