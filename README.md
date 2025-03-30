# Supplementary Materials for Response to Reviewer mxJQ
## Proof for Q6

_Proof_. According to the definition of $J(\bar{\mathcal{Y}})$ and ($\tau$, $f$, $\epsilon$)-disturbing incorrect labels in Definition 1, for $\boldsymbol{x}\in J(\bar{\mathcal{Y}})$, if $j\in \bar{\mathcal{Y}}$, $\eta^{\eta^{\star}(\boldsymbol{x})}(\boldsymbol{x})- \eta^j(\boldsymbol{x}) \leq \tau$. if $j\notin \bar{\mathcal{Y}}$ and $j\neq \eta^{\star}(\boldsymbol{x})$, $\eta^{\eta^{\star}(\boldsymbol{x})}(\boldsymbol{x})- \eta^j(\boldsymbol{x}) > \tau$. Note that $b = \arg\max_{j\notin \{y\}\cup\bar{\mathcal{Y}}}\eta^j({\boldsymbol{x}})$ and for any $\boldsymbol{x}$, the correct label $y=\eta^{\star}(\boldsymbol{x})$, which means $b\notin \bar{\mathcal{Y}}$ and $b\neq \eta^{\star}(\boldsymbol{x})$. Hence, $\eta^{\eta^{\star}(\boldsymbol{x})}(\boldsymbol{x})- \eta^b(\boldsymbol{x}) > \tau$.

## Table 1 for Q7 and Q8

**Table 1:** Classification accuracy of RPLG and its variants on benchmark datasets.
||FMNIST|KMNIST|CIFAR10|CIFAR100|TinyImageNet|
|-|-|-|-|-|-|
|RPLG|**91.41±0.13**|**96.85±0.11**|**87.53±0.21**|**65.03±0.21**|**40.74±0.64**|
|RPLG-updating $\mathbf{U}_i$|90.21±0.18|95.02±0.46|86.21±0.34|62.87±0.45|35.12±1.82|
|RPLG-without rollbacking to $\mathbf{\Theta}_j^k$|90.83±0.26|94.91±0.52|85.42±0.29|62.16±0.53|33.27±1.35|

## A More Readable Rebuttal Version

Thank you so much for taking the time to review our work. We respond to the concerns about Methods And Evaluation Criteria (Q1-Q3), Theoretical Claims (Q4- Q6), and Experimental Designs Or Analyses (Q7-Q8) as follows.

For Methods And Evaluation Criteria:

1. **Q1**: "The algorithms needs to be claried more clearly and some wrong notations bring misleading to readers, especially the algorithm consists of multiple models and parameters. For example: (a) $\mathbf{U}_i$ in Line 234 is composed of $\boldsymbol{u}_i^{j}$, which are basic pseudo-lables given in Equ.(8). However, it should be reduction-based pseudo-label $u_i^{j,r}$. (b) Equ.(11) presents the loss function for the auxiliary model $\phi$ with $U_i$ as input, while Equ.(10) updates $U_i$ based on $\phi$. Do you alternatively update $\phi$ and then $U_i$ alternatively? If so, please clarify it clearly and use $t$ to denote update rule. Otherwise, it is confusing and feels like defining with each other. (c) Does $u_i^j$ in Equ.(11) refer to $\mu_i^j$ in Equ.(9) or $u_i^{j,r}$ in Equ.(10)?"

    **A1**: There seems to be a slight misalignment. Please allow us to clarify.

    (a)：In Line 234 we define $\mathbf{U}_i$ as a reduction-based matrix for the instance $\boldsymbol{x}_i$, while we let $\boldsymbol{\mu}_i$ denote the basic pseudo-label of the instance $\boldsymbol{x}_i$ in Line 212 above Eq. (7). $\mathbf{U}_i$ is explained as an intermediate variable combined with the vector $\boldsymbol{\omega}_i$ to generate the reduction-based pseudo-label $\boldsymbol{v}_i$ in Line 229 and Eq. (9).

    (b): The loss function of the auxiliary model $\mathcal{L}^{\text{aux}}$ does use $\mathbf{U}_i=[\boldsymbol{u}_i^{1},..., \boldsymbol{u}_i^{c}]$ as its input. Each branch's current prediction, $\phi(\boldsymbol{z}_i;\mathbf{\Omega}_j)$, is first used to compute $\boldsymbol{u}_i^{j}$, and then $u_i^{j}$ supervises the branch $\phi(\boldsymbol{z}_i;\mathbf{\Omega}_j)$ itself. For the sake of simplicity, we do not introduce time steps to denote the update rule. 

    (c): $u_i^{j,r}$ calculated by Eq. (10) is the element of the vector $\boldsymbol{u}_i^j$ in Eq. (11), which is not a mistake referring to $\mu_i^j$ in Eq. (9).

    We believe this clarification will help in a more accurate assessment of our work.

2. **Q2**: "In algorithm 1, you calculate $\boldsymbol{v}_i$ in Line 7 before parameter update in zero-shot problem. Why do you update $\boldsymbol{v}_i$ again in Line (10)? Moreover, why don't you similarly update $\mathbf{U}_i$ in Line (10) for calculating $\boldsymbol{q}_i$?" 

    **A2**: The difference between $\boldsymbol{v}_i$ and $\mathbf{U}_i$ in the variables used in their calculations determines their different updates. 
    
    $\boldsymbol{v}_i = \boldsymbol{\omega}_i\mathbf{U}_i$ contains the meta-learning variable $\boldsymbol{\omega}_i$ output by the meta-learner $g$, i.e., $\boldsymbol{\omega}_i = g(\boldsymbol{x}_i;\mathbf{\Gamma})$, while $\mathbf{U}_i$ does not contain such a meta-learning variable. The goal of the calculation on $\boldsymbol{v}_i$ in Algorithm 1, Line (7) is to build the dependency between $v_i$ and the meta-learner parameters $\mathbf{\Gamma}$. After the meta-learning optimization on $\mathbf{\Gamma}$, we could obtain a more optimal $\boldsymbol{\omega}_i$, which could lead to a more optimal $\boldsymbol{v}_i$ if we update $\boldsymbol{v}_i$ with $\boldsymbol{\omega}_i$ again. If we don't re-calculate $\boldsymbol{v}_i$, the meta-learning process will be unnecessary and unhelpful.

    $\mathbf{U}_i$ is calculated from the output of the auxiliary model and naturally updates as the parameters of the auxiliary model update. Note that the more optimal $\boldsymbol{\omega}_i$ obtained from the meta-learning optimization also has dependency on $\mathbf{U}_i$ in Algorithm 1, Line (7). If we change $\mathbf{U}_i$ by updating it in Algorithm 1, Line (10) again, $\boldsymbol{\omega}_i$ will not be optimal for calculating $\boldsymbol{v}_i = \boldsymbol{\omega}_i\mathbf{U}_i$.

3. **Q3**: "Why do you rollback to $\mathbf{\Theta}_j^k$ in Line (11) instead of starting from $\mathbf{\Theta}_j^{k+1}$?"

    **A3**: This is because we need to make $\mathbf{\Theta}$ return to the appropriate optimization path. 

    The goal of the update in Algorithm 1, Line (8) is to optimize $\mathbf{\Gamma}$ by further building the dependency between $\mathbf{\Theta}_j^{k+1}$ and the meta-learner parameters $\mathbf{\Gamma}$, while that in Algorithm 1, Line (11) is to pursue $\mathbf{\Theta}^{\star}$ for better prediction on unobserved instances, which is also the ultimate goal of the whole algorithm. Hence, we need to rollback to $\mathbf{\Theta}_j^k$, making it return to the appropriate optimization path. It is a common practice in the bi-level optimization of some classic meta-learning works such as MAML[1]. [1] Finn et al. Model-Agnostic Meta-Learning for Fast Adaptation of Deep Networks. ICML'17.


For Theoretical Claims:

1. **Q4**: "Some mathematical notations are wrong or misleading: (a) The definition of $\bar{\mathcal{Y}}$ is not clear. The authors denote $\bar{\mathcal{Y}}$ be the labels excluded from the label space $\mathcal{Y}$. It seems that $\bar{\mathcal{Y}}$ and $\mathcal{Y}$ is complementary but in fact $\bar{\mathcal{Y}}$ should be a subset of $\mathcal{Y}$. If I understand correctly, $\bar{\mathcal{Y}}$ should be a label space ruling out one or all non-partial labels of a given point. Therefore, it should be a point-wise concept."

    **A4**: On Page 3 Lines 154-155, we define $\bar{\mathcal{Y}}$ as the labels excluded from the label space $\mathcal{Y}$, thus $\bar{\mathcal{Y}}$ is a subset of $\mathcal{Y}$. Besides, $\bar{\mathcal{Y}}$ is not a point-wise concept. In fact, given $\bar{\mathcal{Y}}$, we could obtain a set consisting of instances whose ($\tau$, $f$, $\epsilon$)-disturbing incorrect labels constitute the label set $\bar{\mathcal{Y}}$. If there is still a misunderstanding or confusion, we plan to change $\bar{\mathcal{Y}}$ to $\mathcal{Y}^{-}$. 

2. **Q5**: "The definition of $J(x, \bar{\mathcal{Y}})$ is confusing because it is a collection of samples satisfying some conditions instead of a sample set related to a given point $x$."

    **A5**: Thank you for helping us improve the readability of the paper. $J(x, \bar{\mathcal{Y}})$ is a collection of instances satisfying some conditions related to $\bar{\mathcal{Y}}$. Hence, we plan to change $J(x, \bar{\mathcal{Y}})$ to $J(\bar{\mathcal{Y}})$ in the revised version. Meanwhile, we have already adopted the changed symbol in the proof of Q6. 

3. **Q6**: "In Theorem 1, is it possible that the range of $\epsilon'$ is empty? Imagine that there exist an $x$ such that $\eta^b(\boldsymbol{x})$ is close to $\eta^{\eta^{\star}(\boldsymbol{x})}(\boldsymbol{x})$?"

    **A6**: It can be proved through the definitions that there is always a gap with a value of $\tau$ between $\eta^{\eta^{\star}(\boldsymbol{x})}(\boldsymbol{x})$ and $\eta^b(\boldsymbol{x})$, i.e., $\eta^{\eta^{\star}(\boldsymbol{x})}(\boldsymbol{x})- \eta^b(\boldsymbol{x}) > \tau$. Therefore, the range of $\eta^b(\boldsymbol{x})$ will not cause the range of $\epsilon'$ to be empty. The proof is as follows:
    
    _Proof_. According to the definition of $J(\bar{\mathcal{Y}})$ and ($\tau$, $f$, $\epsilon$)-disturbing incorrect labels in Definition 1, for $\boldsymbol{x}\in J(\bar{\mathcal{Y}})$, if $j\in \bar{\mathcal{Y}}$, $\eta^{\eta^{\star}(\boldsymbol{x})}(\boldsymbol{x})- \eta^j(\boldsymbol{x}) \leq \tau$. if $j\notin \bar{\mathcal{Y}}$ and $j\neq \eta^{\star}(\boldsymbol{x})$, $\eta^{\eta^{\star}(\boldsymbol{x})}(\boldsymbol{x})- \eta^j(\boldsymbol{x}) > \tau$. Note that $b = \arg\max_{j\notin \{y\}\cup\bar{\mathcal{Y}}}\eta^j({\boldsymbol{x}})$ and for any $\boldsymbol{x}$, the correct label $y=\eta^{\star}(\boldsymbol{x})$, which means $b\notin \bar{\mathcal{Y}}$ and $b\neq \eta^{\star}(\boldsymbol{x})$. Hence, $\eta^{\eta^{\star}(\boldsymbol{x})}(\boldsymbol{x})- \eta^b(\boldsymbol{x}) > \tau$.
    

For Experimental Designs Or Analyses:

1. **Q7**: "Does update $\mathbf{U}_i$ in Equ.(9) in Line (10) of Algorithm 1 lead to any empirical improvement? Or is this already how it is implemented in the algorithm?"

    **A7**: We do not update $\mathbf{U}_i$ in Line 10 of Algorithm 1, the reason for which is explained at the above **A2**. Here, we provide the results of the experiment investigating it in Table 1 for reference. From Table 1, we could observe that updating $\mathbf{U}_i$ in Line 10 of Algorithm 1 does not lead to empirical improvement.

2. **Q8**: "How does the performance become if we didn't rollback $\mathbf{\Theta}_j^k$ in Line 11 of Algorithm 1?"

    **A8**: We explain the reason why we rollback to $\mathbf{\Theta}_j^k$ in Line 11 of Algorithm 1 at the above **A3**. Here, we also provide the results of the experiment investigating it in Table 1 for reference. From Table 1, we could find that RPLG without rollbacking to $\mathbf{\Theta}_j^k$ in Line 11 of Algorithm 1 performs worse than the original RPLG.

**Table 1:** Classification accuracy of RPLG and its variants on benchmark datasets.
||FMNIST|KMNIST|CIFAR10|CIFAR100|TinyImageNet|
|-|-|-|-|-|-|
|RPLG|**91.41±0.13**|**96.85±0.11**|**87.53±0.21**|**65.03±0.21**|**40.74±0.64**|
|RPLG-updating $\mathbf{U}_i$|90.21±0.18|95.02±0.46|86.21±0.34|62.87±0.45|35.12±1.82|
|RPLG-without rollbacking to $\mathbf{\Theta}_j^k$|90.83±0.26|94.91±0.52|85.42±0.29|62.16±0.53|33.27±1.35|
