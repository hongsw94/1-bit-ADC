# Channel Estimation
## Channel estimation for uplink massive MIMO with 1-bit-ADC 

### _Abstract_
- propose an approach for channel estimation that is applicable for both flat and frequency-selective fading.
- derive closed-form expressions for the achievable rate in flat fading 
- Low SNR 

## $\text{I. Introduction}$ 
Massive MIMO - Power consumption matters 

The use of **Low Resolution(1-3bits) ADCs** is a potential solution to this problem [11]-[18]. 

[11] - Capacity Maximizing transmit signals for one-bit ADCs : Discrete unlike infinite resolution where Gaussian is optimal

Channel Estimation with 1-bit-ADC: 
[20]~[28]
- [24] - modified EM algorithm using sparsity
- [25] - near Maximum Likelihood (nML) (Channel Estimator & detector proposed)
- [28] - low complexity channel estimator 

## $\text{II. System Model}$ 

![image](../images/fig1.png)

single-cell with 
- $K$ : Number of single-antenna user 
- $M$ : Number of antenna in BS 
- $M \gg K \gg 1$
- received signal at the BS 

$$
\begin{equation}
\textbf{y} = \sqrt{\rho_{d}}\textbf{Hs} + \textbf{n}
\end{equation}
$$

- $\textbf{n} \backsim \mathcal{CN} (\mathbf{0, I}_M)$
- Channel Matrix: $\textbf{H} \in \mathbb{C}^{M \times K}$ 
- Vectorized Channel Matrix: $\underline{\textbf{h}} = \text{vec}(\mathbf{H})$
- Assume:  $E\{|s_k|^{2}\}=1$ 
    
all users have the same level of large-scale fading/$\textbf{SNR}$ $\rho_d$

The quantized signal obtained after the one-bit ADCs:

$$
\begin{equation}
\textbf{r} = \mathcal{Q}(\textbf{y}) = \mathcal{Q}(\sqrt{\rho_d}\mathbf{Hs}+\mathbf{n})
\end{equation}
$$

- $\mathcal{Q}$ represents (one-bit) Quantization function 
- $\mathcal{Q} = \frac{1}{\sqrt{2}}(\text{sign}(\mathfrak{R}(\cdot)) + j \text{sign}(\mathfrak{R}(\cdot))$ 


Coherence Interval is divided into two parts: dedicated to 

1. __training__ : all $K$ users simultaneously transmit their pilot sequences of $\tau$ symbols each to BS
2. data transmission

### 1. Training 
all $K$ users simultaneously transmit their pilot sequences of $\tau$ symbols each to Base station 

$$
\begin{equation}
\textbf{Y}_p = \sqrt{\rho_p}\mathbf{H\Phi}^T + \mathbf{N}_p
\end{equation}
$$

- $\mathbf{Y}_p \in \mathbb{C}^{M\times \tau}$ : received signal
- $\mathbf{\Phi} \in \mathbb{C}^{\tau \times K}$ : **pilot matrix** (Column wise orthogonal)
- i.e., $\mathbf{\Phi}^T\mathbf{\Phi} = \tau \mathbf{I}_K$ 

    implies $\tau \geq K$

**Change it to vector form** 

$$
\begin{equation}
\text{vec}(\mathbf{Y}_p) = \mathbf{y}_p = \bar{\mathbf{\Phi}}\underline{\mathbf{h}} + \underline{\mathbf{n}}_p
\end{equation}
$$

- $\bar{\mathbf{\Phi}} = \mathbf{\Phi} \otimes \sqrt{\rho_p} \mathbf{I}_M$ 
- $\underline{\mathbf{n}}_p = \text{vec}(\mathbf{N}_p)$

$$ 
\begin{equation}
\mathbf{r}_p = \mathcal{Q}(\mathbf{y}_p)
\end{equation}
$$


#### A. _Bussgang-Based Channel Estimator_

**channel estimation** in one-bit systems rely on 

1. ML algorithm (Maximum-Likelihood) 
2. Iterative algorithms 

both methods have high complexity

Find an operator using Bussgang decomposition using statistical equivalence 

The Bussgang decomposition is written 

$$
\begin{equation}
\mathbf{r}_p = \mathcal{Q}(\mathbf{y}_p) =\mathbf{A}_p \mathbf{y}_p + \mathbf{q}_p
\end{equation}
$$

where $\mathbf{A}_p$ is the linear operator and $\mathbf{q}_p$ is the statistically equivalent quantizer noise. 

The matrix $\mathbf{A}_p$ is chosen to make $\mathbf{q}_p$ uncorrelated with $\mathbf{y}_p$ 

- equivalent to

1. minimize the power of the equivalent quantizer noise which yields 

$$
\begin{equation}
\mathbf{A}_p = \mathbf{C}^{H}_{\mathbf{y}_p \mathbf{r}_p} \mathbf{C}_{\mathbf{y}_p}^{-1}
\end{equation}
$$

- $\mathbf{C}_{\mathbf{y}_p \mathbf{r}_p}^{H}$: [cross-correlation matrix](https://en.wikipedia.org/wiki/Cross-correlation_matrix) between $\mathbf{r}_p$ and $\mathbf{y}_p$ 
- $\mathbf{C}_{\mathbf{y}_p}$: auto-correlation matrix of $\mathbf{y}_p$

$$
\begin{equation}
    \mathbf{C}_{\mathbf{y}_p \mathbf{r}_p} = \sqrt{\frac{2}{\pi}} \mathbf{C}_{\mathbf{y}_p} \text{diag}(\mathbf{C}_{\mathbf{y}_p})^{-\frac{1}{2}} \triangleq \sqrt{\frac{2}{\pi}} \mathbf{C}_{\mathbf{y}_p} \mathbf{\Sigma}_{\mathbf{y}_p}^{-\frac{1}{2}}
\end{equation}
$$

by substituting (4) to (6), 

$$
% \begin{equation}
    \begin{align*}
        \mathbf{r}_p &= \mathcal{Q}(\mathbf{y}_p) = \mathbf{A}_p (\overbrace{\mathbf{\bar{\Phi} \underline{h}} + \mathbf{\underline{n}}_p}^{\mathbf{y}_p}) + \mathbf{q}_p \\ 
        &= \underbrace{\mathbf{A}_p \mathbf{\bar{\Phi}}}_{\mathbf{\tilde{\Phi}}} \mathbf{\underline{h}} + 
            \underbrace{\mathbf{A}_p\mathbf{\underline{n}}_p + \mathbf{q}_p}_{\mathbf{\tilde{n}}_p}\\
        &= \mathbf{\tilde{\Phi}} \mathbf{\underline{h}} + \mathbf{\tilde{n}}_p
    \end{align*}
% \end{equation}
$$

- $\mathbf{\tilde{\Phi}} \in \mathbb{C}^{M_\tau \times M_\tau}$ 
- $\mathbf{\tilde{n}}_p \in \mathbb{C}^{M_\tau \times 1}$ 


Assuming $\mathbf{C}_{\underline{\mathbf{h}}}=\mathbf{I}_{MK}$ $\Longrightarrow$ for simplicity and can be modified to include general case of the [covariance matrix](https://en.wikipedia.org/wiki/Covariance_matrix)

By substituting (8) to (7):

$$
    \begin{align*}
        \mathbf{A}_p &= \mathbf{C}^{H}_{\mathbf{y}_p \mathbf{r}_p} \mathbf{C}_{\mathbf{y}_p}^{-1}\\
                     &= \left(\sqrt{\frac{2}{\pi}} \mathbf{C}_{\mathbf{y}_p} \text{diag}(\mathbf{C}_{\mathbf{y}_p})^{-\frac{1}{2}}\right)^{H} \mathbf{C}_{\mathbf{y}_p}^{-1}\\
                     &= \sqrt{\frac{2}{\pi}} \text{diag}(\mathbf{C}_{\mathbf{y}_p})^{-\frac{1}{2}} \mathbf{C}_{\mathbf{y}_p}^{H} \mathbf{C}_{\mathbf{y}_p}^{-1}\\
                     &= \sqrt{\frac{2}{\pi}} \text{diag}(\mathbf{C}_{\mathbf{y}_p})^{-\frac{1}{2}} \mathbf{C}_{\mathbf{y}_p} \mathbf{C}_{\mathbf{y}_p}^{-1} \\
                     &= \sqrt{\frac{2}{\pi}} \text{diag}(\mathbf{C}_{\mathbf{y}_p})^{-\frac{1}{2}}\\
                     &= \sqrt{\frac{2}{\pi}} \text{diag} \left( \left( \underline{\Phi \Phi^{H}} \otimes \rho_{p} \mathbf{I}_{M} \right) + \mathbf{I}_{M\tau} \right)^{-\frac{1}{2}} \quad (\mathbf{A} \otimes \mathbf{B})(\mathbf{C} \otimes \mathbf{D}) = (\mathbf{AC}) \otimes (\mathbf{BD})
    \end{align*}
$$

- As you can see from the underlined $\Phi \Phi^{H}$
- $\mathbf{A}_p$ depends on the choice of pilot sequences 
  
In order to obtain a simple expression of $\mathbf{A}_p$, pilot sequences that consist of the submatrices of the [DFT (Discrete Fourier Transform)](https://en.wikipedia.org/wiki/Discrete_Fourier_transform) will be considered [[DFT matrix](https://en.wikipedia.org/wiki/DFT_matrix)]

By using the DFT pilot sequences: 

1. All elements of the matrix have the same magnitude. $\rightarrow$ simple peak power constraints
2. The diagonal terms of $\Phi \Phi^{H}$ are always equal to $K$ 


Thus, can be simplified as 
$$
\begin{equation}
    \mathbf{A}_p = \sqrt{\frac{2}{\pi} \frac{1}{K\rho_p + 1}} \mathbf{I}_{M\tau} \triangleq \alpha_p \mathbf{I}_{M\tau}
\end{equation}
$$

and the statistically equivalent linear model is 

$$
% \begin{equation*}
    \begin{align*}
        \mathbf{r}_p &= \mathbf{A}_p \mathbf{\bar{\Phi}} \mathbf{\underline{h}} + \mathbf{A}_p \mathbf{\underline{n}}_p + \mathbf{q}_p\\
        &= \alpha_p \mathbf{\bar{\Phi}} \mathbf{\underline{h}} + \alpha_p \mathbf{\underline{n}}_p + \mathbf{q}_p\\
        &= \mathbf{\tilde{\Phi}} \mathbf{\underline{h}} + \mathbf{\tilde{n}}_p
    \end{align*}
% \end{equation*}
$$

