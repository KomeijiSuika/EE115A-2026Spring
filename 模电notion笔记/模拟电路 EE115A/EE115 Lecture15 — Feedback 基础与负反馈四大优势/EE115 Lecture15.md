# EE115 Lecture15

## EE115 Analog Circuits模拟电路

Dr．Hao Ren 任豪
Office：SIST Bldg 3－328
renhao＠shanghaitech．edu．cn

## Outline

- Feedback
- SEDTRA/SMITH book page 807-820;

## Feedback

- Feedback is a powerful technique that finds wide applications in analog circuits. Negative feedback allows high precision signal processing
- Desensitize the gain
- Reduce nonlinear distortion
- Reduce the effect of noise
- Control the input and output resistances
- Extend the bandwidth of the amplifier

## The general feedback structure

- A: open loop gain
- $\beta$ : feedback factor
    
    ![](EE115%20Lecture15/images35844b74-5010-4281-ba9a-9e43108d309c-04_546_1271_594_25.jpg)
    

$$
\begin{aligned}
& x_{o}=A x_{i} \\
& x_{f}=\beta x_{o} \\
& x_{i}=x_{s}-x_{f} \\
& A_{f} \equiv \frac{x_{o}}{x_{s}} \\
& A_{f}=\frac{A}{1+A \beta}
\end{aligned}
$$

- $\mathrm{A} \beta$ :loop gain
- $A \beta \gg 1$

$$
A_{f} \simeq \frac{1}{\beta}
$$

## The loop gain

- $\mathrm{A} \beta$ :loop gain
- $\mathrm{A} \beta \gg 1$

$$
A_{f} \simeq \frac{1}{\beta}
$$

- The sign of $\mathrm{A} \beta$ determines the polarity of the feedback
- The magnitude of A $\beta$ determines how close the closed-loop gain Af is to the ideal value of $1 / \beta$.
- The magnitude of $\mathrm{A} \beta$ determines the amount of feedback $(1+\mathrm{A} \beta)$ the magnitude of the various improvements in amplifier performance

$$
\begin{aligned}
& x_{r}=-A \beta x_{t} \\
& A \beta=-\frac{x_{r}}{x_{t}}
\end{aligned}
$$

Figure 11.2 Determining the loop gain by breaking the feedback loop at the output of the basic amplifier, applying a test signal $x_{t}$, and measuring the returned signal $x_{r}: A \beta \equiv -x_{r} / x_{t}$.

## Example

![](EE115%20Lecture15/images35844b74-5010-4281-ba9a-9e43108d309c-06_709_2313_242_186.jpg)

(a) Assume that the op amp has infinite input resistance and zero output resistance. Find an expression for the feedback factor

$\beta$

.
(b) Find the condition the open-loop gain

$A$

must satisfy so that the closed-loop gain

$A_{f}$

is almost entirely determined by the feedback network. Also, give the value of

$A_{f}$

in this case.
(c) If the open-loop gain

$A=10^{4} \mathrm{~V} / \mathrm{V}$

, find

$R_{2} / R_{1}$

to obtain a closed-loop gain

$A_{f}$

of

$10 \mathrm{~V} / \mathrm{V}$

.
(d) What is the amount of feedback in decibels?
(e) If

$V_{s}=1 \mathrm{~V}$

, find

$V_{o}, V_{f}$

, and

$V_{i}$

.
(f) If

$A$

decreases by

$20 \%$

, what is the corresponding decrease in

$A_{f}$

?

![](EE115%20Lecture15/images35844b74-5010-4281-ba9a-9e43108d309c-07_369_721_128_828.jpg)

(a)

![](EE115%20Lecture15/images35844b74-5010-4281-ba9a-9e43108d309c-07_480_929_17_1570.jpg)

(b)

1. To be able to see more clearly the direct correspondence between the circuit in Fig. 11.3(a) and the block diagram in Fig. 11.1, we replace the op amp with its equivalent-circuit model, as shown in Fig. 11.3(b). Since the op amp is assumed to have infinite input resistance and zero output resistance, its model is simply an ideal voltage-controlled voltage source of gain $A$. From Fig. 11.3(b) we observe that the feedback network, consisting of the voltage divider ( $R_{1}, R_{2}$ ), is connected directly to the output and feeds a signal $V_{f}$ to the inverting input terminal of the op amp. It is important at this point to note that the zero output resistance of the op amp causes the output voltage to be $A V_{i}$ irrespective of the values of $R_{1}$ and $R_{2}$. That is what we meant by the statement that in the block diagram of Fig. 11.1, the feedback network is assumed to not load the basic amplifier. Now we can easily determine the feedback factor $\beta$ from

$$
\beta \equiv \frac{V_{f}}{V_{o}}=\frac{R_{1}}{R_{1}+R_{2}}
$$

Let’s next examine how $V_{f}$ is subtracted from $V_{s}$ at the input side. The subtraction is effectively performed by the differential action of the op amp; by its very nature, a differential-input amplifier takes the difference between the signals at its two input terminals. Observe also that because the input resistance of the op amp is assumed to be infinite, no current flows into the negative input terminal of the op amp and that the feedback network does not load the amplifier at the input side.
(b) The closed-loop gain $A_{f}$ is given by

$$
A_{f}=\frac{A}{1+A \beta}
$$

## Example

![](EE115%20Lecture15/images35844b74-5010-4281-ba9a-9e43108d309c-08_391_509_570_222.jpg)

![](EE115%20Lecture15/images35844b74-5010-4281-ba9a-9e43108d309c-08_382_634_1068_189.jpg)

To make $A_{f}$ nearly independent of $A$, we must ensure that the loop gain $A \beta$ is much larger than unity

$$
A \beta \gg 1
$$

in which case

$$
A_{f} \simeq 1 / \beta
$$

Thus,

$$
A / A_{f} \gg 1
$$

or equivalently,

$$
A \gg A_{f}
$$

and

$$
A_{f} \simeq \frac{1}{\beta}=1+\frac{R_{2}}{R_{1}}
$$

1. For $A=10^{4} \mathrm{~V} / \mathrm{V}$ and $A_{f}=10 \mathrm{~V} / \mathrm{V}$, we see that $A \gg A_{f}$, thus we can select $R_{1}$ and $R_{2}$ to obtain

$$
\beta \simeq \frac{1}{A_{f}}=0.1
$$

Thus,

$$
\frac{1}{\beta}=1+\frac{R_{2}}{R_{1}}=A_{f}=10
$$

which yields

$$
R_{2} / R_{1}=9
$$

A more exact value for the required ratio $R_{2} / R_{1}$ can be obtained from

$$
\begin{aligned}
& A_{f}=\frac{A}{1+A \beta} \\
& 10=\frac{10^{4}}{1+10^{4} \beta}
\end{aligned}
$$

which results in

$$
\beta=0.0999
$$

and,

$$
\frac{R_{2}}{R_{1}}=9.01
$$

![](EE115%20Lecture15/images35844b74-5010-4281-ba9a-9e43108d309c-09_401_515_566_218.jpg)

![](EE115%20Lecture15/images35844b74-5010-4281-ba9a-9e43108d309c-09_382_636_1068_187.jpg)

（d）The amount of feedback is

$$
1+A \beta=\frac{A}{A_{f}}=\frac{10^{4}}{10}=1000
$$

which is 60 dB ．
（e）For $V_{s}=1 \mathrm{~V}$ ，

$$
\begin{aligned}
& V_{o}=A_{f} V_{s}=10 \times 1=10 \mathrm{~V} \\
& V_{f}=\beta V_{o}=0.0999 \times 10=0.999 \mathrm{~V} \\
& V_{i}=\frac{V_{o}}{A}=\frac{10}{10^{4}}=0.001 \mathrm{~V}
\end{aligned}
$$

Note that if we had used the approximate value of $\beta=0.1$ ，we would have obtained $V_{f}=1 \mathrm{~V}$ and $V_{i}=0 \mathrm{~V}$ ．
（f）If A decreases by $20 \%$ ，thus becoming

$$
A=0.8 \times 10^{4} \mathrm{~V} / \mathrm{V}
$$

the value of $A_{f}$ becomes

$$
A_{f}=\frac{0.8 \times 10^{4}}{1+0.8 \times 10^{4} \times 0.0999}=9.9975 \mathrm{~V} / \mathrm{V}
$$

that is，it decreases by $0.025 \%$ ，which is less than the percentage change in $A$ by approximately a factor $(1+A \beta)$ ．

## Feedback gain desensitivity

$$
\begin{aligned}
A_{f} & =\frac{A}{1+A \beta} \\
d A_{f} & =\frac{d A}{(1+A \beta)^{2}} \\
\frac{d A_{f}}{A_{f}} & =\frac{1}{(1+A \beta)} \frac{d A}{A}
\end{aligned}
$$

- the percentage change in Af (due to variations in some circuit parameter) is smaller than the percentage change in A by a factor equal to the amount of feedback. For this reason, the amount of feedback, $1+\mathrm{A} \beta$, is also known as the desensitivity factor.

## Feedback bandwidth extension

$$
A(s)=\frac{A_{M}}{1+s / \omega_{H}}
$$

$$
\begin{gathered}
A_{f}(s)=\frac{A_{M} /\left(1+A_{M} \beta\right)}{1+s / \omega_{H}\left(1+A_{M} \beta\right)} \\
\omega_{H f}=\omega_{H}\left(1+A_{M} \beta\right)
\end{gathered}
$$

![](EE115%20Lecture15/images35844b74-5010-4281-ba9a-9e43108d309c-11_890_1511_849_355.jpg)

Figure 11.4 Application of negative feedback reduces the midband gain, increases

$f_{H}$

, and reduces

$f_{L}$

, all by the same factor, (

$1+A_{M} \beta$

), which is equal to the amount of feedback.

## Feedback interference reduction

![](EE115%20Lecture15/images35844b74-5010-4281-ba9a-9e43108d309c-12_260_945_440_297.jpg)

(a)

![](EE115%20Lecture15/images35844b74-5010-4281-ba9a-9e43108d309c-12_659_1401_783_69.jpg)

(b)

$$
S / I=V_{s} / V_{n}
$$

$$
V_{o}=V_{s} \frac{A_{1} A_{2}}{1+A_{1} A_{2} \beta}+V_{n} \frac{A_{1}}{1+A_{1} A_{2} \beta}
$$

$$
\frac{S}{I}=\frac{V_{s}}{V_{n}} A_{2}
$$

## Reduction in nonlinear distortion

![](EE115%20Lecture15/images35844b74-5010-4281-ba9a-9e43108d309c-13_942_1724_398_257.jpg)

Figure 11.6 Illustrating the application of negative feedback to reduce the nonlinear distortion in amplifiers. Curve (a) shows the amplifier transfer characteristic (

$v_{O}$

versus

$v_{I}$

) without feedback. Curve (b) shows the characteristic (

$v_{O}$

versus

$v_{S}$

) with negative feedback (

$\beta=0.01$

) applied.

$$
A_{f 2}=\frac{100}{1+100 \times 0.01}=50
$$

## The Feedback Voltage Amplifier

The Series-Shunt Feedback Topology

![](EE115%20Lecture15/images35844b74-5010-4281-ba9a-9e43108d309c-14_964_1697_611_483.jpg)

## Examples of Series-Shunt Feedback Amplifiers

![](EE115%20Lecture15/images35844b74-5010-4281-ba9a-9e43108d309c-15_1468_1491_296_302.jpg)

## Homeworkxx-due May xxth 11p.m.

SEDRA/SMITH Book chapter 9 problem xx