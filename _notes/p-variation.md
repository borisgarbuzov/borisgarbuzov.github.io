---
layout: page
title: Notes on \(p\)-variation
---

<script async src="https://cdn.jsdelivr.net/npm/mathjax@4/tex-mml-chtml.js"></script>

$$
\newcommand{\LV}{\text{LV}}
\newcommand{\SV}{\text{SV}}
\newcommand{\mbR}{\mathbb{R}}
\newcommand{\mcP}{\mathcal{P}}
\newcommand{\les}{\leqslant}
\newcommand{\ges}{\geqslant}
\newcommand{\abs}[1]{\left\lvert #1 \right\rvert}
$$

For an arbitrary function $$f\colon [0,T]\to\mbR$$ and $$p>0$$ define

$$
\LV_p(f) = \text{LV}_p(f,[0,T]) = \lim_{\Delta(\mcP)\to 0+} S_p(f;\mcP),\\
\SV_p(f) = \text{SV}_p(f,[0,T]) = \sup_{\mcP} S_p(f;\mcP),
$$

where

$$
S_p(f;\mcP) = \sum_{i=1}^n \bigl(f(t_i) - f(t_{i-1})\bigr)^p.
$$

Note that the limit may not exist, whereas the supremum always exists, even though it can be infinite.

**Proposition 1.** _It holds_

$$
\LV_p(f) \les \SV_p(f).
$$

_Proof._ This follows immediately from the fact that for any partition $$\mcP$$ it holds

$$
S_p(f;\mcP) \les \sup_{\mcP'} S_p(f;\mcP') = \SV_p(f). \qquad \square
$$

**Example 2.** Consider the function

$$
f(t) =
\begin{cases}
0, & \text{if } 0 \les t < 1/2, \\
h, & \text{if } 1/2 \les t \les 1,
\end{cases}
$$

where $$h$$ is an arbitrary real number. It is clear that

$$
\LV_p(f) = \SV_p(f) = \abs{h}^p.
$$

Depending on the value of $$\abs{h}$$, we have the following situations:

- if $$\abs{h} < 1$$, then both $$\LV_p(f)$$ and $$\SV_p(f)$$ are decreasing in $$p$$,
- if $$\abs{h} = 1$$, then both $$\LV_p(f)$$ and $$\SV_p(f)$$ are constant in $$p$$,
- if $$\abs{h} > 1$$, then both $$\LV_p(f)$$ and $$\SV_p(f)$$ are increasing in $$p$$.

**Example 3.** Consider the function

$$
f(t) =
\begin{cases}
0,       & \text{if } 0 \les t < 1/3, \\
h_1,     & \text{if } 1/3 \les t < 2/3, \\
h_1+h_2, & \text{if } 2/3 \les t \les 1,
\end{cases}
$$

where $$h_1, h_2 \ges 0$$ are arbitrary real numbers. It is clear that

$$
\LV_p(f) = h_1^p + h_2^p.
$$

Now take an arbitrary partition $$\mcP$$ of the interval $$[0,T]$$ and consider two cases:

- If the interval $$[1/3, 2/3)$$ does not contain any points of the partition, then

$$
S_p(f;\mcP) = (h_1+h_2)^p.
$$

- If the interval $$[1/3, 2/3)$$ contains at least one point of the partition, then

$$
S_p(f;\mcP) = h_1^p + h_2^p.
$$

Therefore,

$$
\SV_p(f) = \max\!\bigl\{(h_1+h_2)^p,\, h_1^p+h_2^p\bigr\}.
$$

Note that, generally speaking, we do not know which of the two values is larger.
For instance, if $$h_1=h_2=1/2$$ and $$p=2$$, we have

Which of the two values is larger depends on $$p$$. For instance, if
$$h_1 = h_2 = 1/2$$ and $$p = 2$$, we have

$$
(h_1+h_2)^p = 1 > 1/2 = h_1^p + h_2^p
$$

However, if $$h_1 = h_2 = 1/2$$ and $$p = 1/2$$, we have

$$
(h_1+h_2)^p = 1 < \sqrt{2} = h_1^p + h_2^p
$$

**Lemma 4.** _For any $$x \ges 0$$ it holds_

$$
\begin{aligned}
1+x^p&\ges (1+x)^p,\qquad\text{if $0<p<1$,}\\
1+x^p&\les (1+x)^p,\qquad\text{if $p\ges 1$.}
\end{aligned}
$$

_Proof._ Consider the function

$$
g(x) = (1+x)^p - 1 - x^p.
$$

For $$0<p<1$$ we have

$$
g'(x)=p(1+x)^{p-1}-px^{p-1}<0,
$$

and for $$p\ges 1$$ we have

$$
g'(x)=p(1+x)^{p-1}-px^{p-1}\ges 0.
$$

Therefore, for any $$x\ges 0$$, we get

$$
\begin{aligned}
g(x)&\les g(0)=0,\qquad\text{if $0<p<1$,}\\
g(x)&\ges g(0)=0,\qquad\text{if $p\ges 1$,}
\end{aligned}
$$

which implies the required inequalities.

Using this lemma, we get that if $$p\ges 1$$, then

$$
(h_1+h_2)^p=h_1^p\left(1+\frac{h_2}{h_1}\right)^p\ges h_1^p\left(1+\frac{h_2^p}{h_1^p}\right)=h_1^p+h_2^p,
$$

and if $$0<p<1$$, then

$$
(h_1+h_2)^p=h_1^p\left(1+\frac{h_2}{h_1}\right)^p\les h_1^p\left(1+\frac{h_2^p}{h_1^p}\right)=h_1^p+h_2^p.
$$

Therefore,

$$
\SV_p(f)=
\begin{cases}
h_1^p+h_2^p,&\text{if $0<p<1$,}\\
(h_1+h_2)^p,&\text{if $p\ges 1$.}
\end{cases}
$$
