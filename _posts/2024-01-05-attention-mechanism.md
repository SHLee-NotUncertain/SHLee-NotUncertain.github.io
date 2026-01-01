---
title: "Attention Mechanism 수식 정리"
date: 2024-01-05
categories: [Computer Vision]
tags: [deep learning, attention, transformer, paper review]
---

Attention 메커니즘은 현대 딥러닝의 핵심 구성 요소입니다. 이 글에서는 Attention의 수학적 원리를 정리합니다.

## 1. Scaled Dot-Product Attention

가장 기본적인 attention 메커니즘은 다음과 같이 정의됩니다:

$$
\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V
$$

여기서:
- $Q \in \mathbb{R}^{n \times d_k}$: Query 행렬
- $K \in \mathbb{R}^{m \times d_k}$: Key 행렬
- $V \in \mathbb{R}^{m \times d_v}$: Value 행렬
- $d_k$: Key의 차원 (scaling factor)

### Softmax 함수

Attention score를 확률 분포로 변환하기 위해 softmax를 사용합니다:

$$
\text{softmax}(z_i) = \frac{e^{z_i}}{\sum_{j=1}^{n} e^{z_j}}
$$

### Scaling의 필요성

$\sqrt{d_k}$로 나누는 이유는 dot product의 크기가 커지면 softmax의 gradient가 매우 작아지기 때문입니다. $d_k$가 클 때:

$$
\text{Var}(q \cdot k) = d_k \cdot \text{Var}(q) \cdot \text{Var}(k)
$$

## 2. Multi-Head Attention

여러 개의 attention head를 병렬로 사용하여 다양한 representation을 학습합니다:

$$
\text{MultiHead}(Q, K, V) = \text{Concat}(\text{head}_1, \ldots, \text{head}_h)W^O
$$

각 head는:

$$
\text{head}_i = \text{Attention}(QW_i^Q, KW_i^K, VW_i^V)
$$

여기서 학습 가능한 파라미터는:
- $W_i^Q \in \mathbb{R}^{d_{\text{model}} \times d_k}$
- $W_i^K \in \mathbb{R}^{d_{\text{model}} \times d_k}$
- $W_i^V \in \mathbb{R}^{d_{\text{model}} \times d_v}$
- $W^O \in \mathbb{R}^{hd_v \times d_{\text{model}}}$

일반적으로 $d_k = d_v = d_{\text{model}} / h$를 사용합니다.

## 3. Self-Attention

Self-attention은 입력 시퀀스 자체에서 Q, K, V를 모두 생성합니다:

$$
\begin{align}
Q &= XW^Q \\
K &= XW^K \\
V &= XW^V
\end{align}
$$

여기서 $X \in \mathbb{R}^{n \times d}$는 입력 시퀀스입니다.

## 4. Positional Encoding

Transformer는 위치 정보를 직접 학습하지 않으므로 positional encoding을 추가합니다:

$$
\begin{align}
PE_{(pos, 2i)} &= \sin\left(\frac{pos}{10000^{2i/d_{\text{model}}}}\right) \\
PE_{(pos, 2i+1)} &= \cos\left(\frac{pos}{10000^{2i/d_{\text{model}}}}\right)
\end{align}
$$

여기서:
- $pos$: 위치 (0부터 시작)
- $i$: 차원 인덱스
- $d_{\text{model}}$: 모델의 차원

## 5. Cross-Attention

Encoder-Decoder 구조에서 decoder는 encoder의 출력에 attention을 적용합니다:

$$
\text{CrossAttention}(Q_{\text{dec}}, K_{\text{enc}}, V_{\text{enc}}) = \text{softmax}\left(\frac{Q_{\text{dec}}K_{\text{enc}}^T}{\sqrt{d_k}}\right)V_{\text{enc}}
$$

## 6. Masked Self-Attention

Auto-regressive 생성을 위해 미래 토큰을 보지 못하도록 masking을 적용합니다:

$$
\text{mask}(s_{ij}) = \begin{cases}
s_{ij} & \text{if } j \leq i \\
-\infty & \text{if } j > i
\end{cases}
$$

Masking 후 softmax를 적용하면:

$$
\text{softmax}(-\infty) = 0
$$

## 7. Computational Complexity

Self-attention의 시간 복잡도는:

$$
\mathcal{O}(n^2 \cdot d)
$$

여기서 $n$은 시퀀스 길이, $d$는 차원입니다. 이는 긴 시퀀스에서 문제가 됩니다.

### Efficient Attention 변형들

**Linear Attention**:
$$
\text{Attention}(Q, K, V) = \phi(Q)(\phi(K)^TV)
$$

시간 복잡도: $\mathcal{O}(n \cdot d^2)$

**Sparse Attention**: 모든 위치가 아닌 일부 위치만 attend
$$
\text{Attention}_{sparse}(Q, K, V) = \text{softmax}\left(\frac{QK_{\text{sparse}}^T}{\sqrt{d_k}}\right)V_{\text{sparse}}
$$

## 8. Vision Transformer (ViT)

이미지를 패치로 나누고 각 패치를 임베딩합니다:

$$
\mathbf{z}_0 = [\mathbf{x}_{\text{class}}; \mathbf{x}_p^1\mathbf{E}; \mathbf{x}_p^2\mathbf{E}; \cdots; \mathbf{x}_p^N\mathbf{E}] + \mathbf{E}_{\text{pos}}
$$

여기서:
- $\mathbf{x}_p^i \in \mathbb{R}^{P^2 \cdot C}$: $i$번째 패치 ($P \times P$ 크기)
- $\mathbf{E} \in \mathbb{R}^{(P^2 \cdot C) \times D}$: 패치 임베딩 행렬
- $\mathbf{E}_{\text{pos}} \in \mathbb{R}^{(N+1) \times D}$: 위치 임베딩

## 9. Attention Weights 시각화

특정 위치 $i$에서 다른 위치들에 대한 attention weight는:

$$
\alpha_{ij} = \frac{\exp(s_{ij})}{\sum_{k=1}^n \exp(s_{ik})}
$$

여기서 $s_{ij} = \frac{q_i \cdot k_j}{\sqrt{d_k}}$

모든 attention weight의 합은 1입니다:
$$
\sum_{j=1}^n \alpha_{ij} = 1
$$

## 결론

Attention 메커니즘은 시퀀스의 각 위치가 다른 모든 위치를 직접 참조할 수 있게 하여, RNN의 장거리 의존성 문제를 해결했습니다.

주요 장점:
1. 병렬화 가능 ($\mathcal{O}(1)$ 시퀀스 연산)
2. 장거리 의존성 학습
3. 해석 가능한 attention weights

주요 단점:
1. $\mathcal{O}(n^2)$ 메모리 및 시간 복잡도
2. Inductive bias 부족 (데이터가 많이 필요)

## References

- Vaswani et al., "Attention is All You Need", NeurIPS 2017
- Dosovitskiy et al., "An Image is Worth 16x16 Words", ICLR 2021
