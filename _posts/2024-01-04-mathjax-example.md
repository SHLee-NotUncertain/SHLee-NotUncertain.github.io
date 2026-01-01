---
title: "MathJax 수식 예제"
date: 2024-01-04
categories: [Study]
tags: [math, tutorial]
---

이 글에서는 MathJax를 사용한 수식 표현 예제를 보여줍니다.

## 인라인 수식

문장 안에 수식을 넣으려면 `$...$`를 사용합니다. 예를 들어, 피타고라스 정리는 $a^2 + b^2 = c^2$입니다.

또한 $E = mc^2$는 유명한 아인슈타인의 공식입니다.

## 블록 수식

독립적인 수식 블록을 만들려면 `$$...$$`를 사용합니다:

$$
\frac{d}{dx}\left( \int_{0}^{x} f(u)\,du\right)=f(x)
$$

### 행렬

행렬도 표현할 수 있습니다:

$$
\begin{bmatrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{bmatrix}
$$

### 그리스 문자

다양한 그리스 문자를 사용할 수 있습니다:

$$
\alpha, \beta, \gamma, \delta, \epsilon, \theta, \lambda, \mu, \sigma, \omega
$$

### 합과 적분

$$
\sum_{i=1}^{n} i = \frac{n(n+1)}{2}
$$

$$
\int_{-\infty}^{\infty} e^{-x^2} dx = \sqrt{\pi}
$$

### 복잡한 수식

Transformer의 Attention 메커니즘:

$$
\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V
$$

정규분포:

$$
f(x) = \frac{1}{\sigma\sqrt{2\pi}} \exp\left(-\frac{(x-\mu)^2}{2\sigma^2}\right)
$$

## LaTeX 환경

align 환경도 사용 가능합니다:

$$
\begin{align}
\nabla \times \vec{\mathbf{B}} -\, \frac1c\, \frac{\partial\vec{\mathbf{E}}}{\partial t} &= \frac{4\pi}{c}\vec{\mathbf{j}} \\
\nabla \cdot \vec{\mathbf{E}} &= 4 \pi \rho \\
\nabla \times \vec{\mathbf{E}}\, +\, \frac1c\, \frac{\partial\vec{\mathbf{B}}}{\partial t} &= \vec{\mathbf{0}} \\
\nabla \cdot \vec{\mathbf{B}} &= 0
\end{align}
$$

이것은 맥스웰 방정식입니다!
