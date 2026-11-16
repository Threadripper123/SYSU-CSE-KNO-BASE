# 论文·Scaling Laws for Neural Language Models（OpenAI 缩放律）

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　45,087 字符　｜　283 个片段　｜　更新于 2026-11-13
来源: https://arxiv.org/abs/2001.08361
抓取: https://ar5iv.labs.arxiv.org/html/2001.08361

Scaling Laws for Neural Language Models

Jared Kaplan Johns Hopkins University, OpenAI jaredk@jhu.edu

Thanks: Equal contribution.

Contributions: Jared Kaplan and Sam McCandlish led the research. Tom Henighan contributed the LSTM experiments. Tom Brown, Rewon Child, and Scott Gray, and Alec Radford developed the optimized Transformer implementation. Jeff Wu, Benjamin Chess, and Alec Radford developed the text datasets. Dario Amodei provided guidance throughout the project.

  
Sam McCandlishOpenAI sam@openai.com Tom HenighanOpenAI henighan@openai.com Tom B. BrownOpenAI tom@openai.com Benjamin ChessOpenAI bchess@openai.com Rewon ChildOpenAI rewon@openai.com Scott GrayOpenAI scott@openai.com Alec RadfordOpenAI alec@openai.com Jeffrey WuOpenAI jeffwu@openai.com Dario AmodeiOpenAI damodei@openai.com

Abstract

We study empirical scaling laws for language model performance on the cross-entropy loss.
The loss scales as a power-law with model size, dataset size, and the amount of compute used for training, with some trends spanning more than seven orders of magnitude.
Other architectural details such as network width or depth have minimal effects within a wide range.
Simple equations govern the dependence of overfitting on model/dataset size and the dependence of training speed on model size.
These relationships allow us to determine the optimal allocation of a fixed compute budget.
Larger models are significantly more sample-efficient, such that optimally compute-efficient training involves training very large models on a relatively modest amount of data and stopping significantly before convergence.

Contents

1 Introduction

2 Background and Methods

3 Empirical Results and Basic Power Laws

4 Charting the Infinite Data Limit and Overfitting

5 Scaling Laws with Model Size and Training Time

6 Optimal Allocation of the Compute Budget

7 Related Work

8 Discussion

A Summary of Power Laws

B Empirical Model of Compute-Efficient Frontier

C Caveats

D Supplemental Figures

References

1 Introduction

Language provides a natural domain for the study of artificial intelligence, as the vast majority of reasoning tasks can be efficiently expressed and evaluated in language, and the world’s text provides a wealth of data for unsupervised learning via generative modeling. Deep learning has recently seen rapid progress in language modeling, with state of the art models [ RNSS18, DCLT18, YDY+19, LOG+19, RSR+19 ] approaching human-level performance on many specific tasks [ WPN+19 ], including the composition of coherent multi-paragraph prompted text samples [ RWC+19 ].

One might expect language modeling performance to depend on model architecture, the size of neural models, the computing power used to train them, and the data available for this training process. In this work we will empirically investigate the dependence of language modeling loss on all of these factors, focusing on the Transformer architecture [ VSP+17, LSP+18 ]. The high ceiling and low floor for performance on language tasks allows us to study trends over more than seven orders of magnitude in scale.

Throughout we will observe precise power-law scalings for performance as a function of training time, context length, dataset size, model size, and compute budget.

Figure 1: Language modeling performance improves smoothly as we increase the model size, datasetset size, and amount of compute 1 1
1

Here we display predicted compute when using a sufficiently small batch size. See Figure 13 for comparison to the purely empirical data. used for training. For optimal performance all three factors must be scaled up in tandem. Empirical performance has a power-law relationship with each individual factor when not bottlenecked by the other two.

1.1 Summary

Our key findings for Transformer language models are are as follows:

Performance depends strongly on scale, weakly on model shape:

Model performance depends most strongly on scale, which consists of three factors: the number of model parameters N N (excluding embeddings), the size of the dataset D D, and the amount of compute C C used for training. Within reasonable limits, performance depends very weakly on other architectural hyperparameters such as depth vs. width. (Section 3 )

Smooth power laws:

Performance has a power-law relationship with each of the three scale factors N, D, C N,D,C when not bottlenecked by the other two, with trends spanning more than six orders of magnitude (see Figure 1 ). We observe no signs of deviation from these trends on the upper end, though performance must flatten out eventually before reaching zero loss. (Section 3 )

Universality of overfitting:

Performance improves predictably as long as we scale up N N and D D in tandem, but enters a regime of diminishing returns if either N N or D D is held fixed while the other increases. The performance penalty depends predictably on the ratio N 0.74 / D N^{0.74}/D, meaning that every time we increase the model size 8x, we only need to increase the data by roughly 5x to avoid a penalty. (Section 4 )

Universality of training:

Training curves follow predictable power-laws whose parameters are roughly independent of the model size. By extrapolating the early part of a training curve, we can roughly predict the loss that would be achieved if we trained for much longer. (Section 5 )

Transfer improves with test performance:

When we evaluate models on text with a different distribution than they were trained on, the results are strongly correlated to those on the training validation set with a roughly constant offset in the loss – in other words, transfer to a different distribution incurs a constant penalty but otherwise improves roughly in line with performance on the training set. (Section 3.2.2 )

Sample efficiency:

Large models are more sample-efficient than small models, reaching the same level of performance with fewer optimization steps (Figure 2 ) and using fewer data points (Figure 4 ).

Figure 2: We show a series of language model training runs, with models ranging in size from 10 3 10^{3} to 10 9 10^{9} parameters (excluding embeddings).

Figure 3: As more compute becomes available, we can choose how much to allocate towards training larger models, using larger batches, and training for more steps. We illustrate this for a billion-fold increase in compute. For optimally compute-efficient training, most of the increase should go towards increased model size. A relatively small increase in data is needed to avoid reuse. Of the increase in data, most can be used to increase parallelism through larger batch sizes, with only a very small increase in serial training time required.

Convergence is inefficient:

When working within a fixed compute budget C C but without any other restrictions on the model size N N or available data D D, we attain optimal performance by training very large models and stopping significantly short of convergence (see Figure 3 ). Maximally compute-efficient training would therefore be far more sample efficient than one might expect based on training small models to convergence, with data requirements growing very slowly as D ∼ C 0.27 D\sim C^{0.27} with training compute. (Section 6 )

Optimal batch size:

The ideal batch size for training these models is roughly a power of the loss only, and continues to be determinable by measuring the gradient noise scale [ MKAT18 ]; it is roughly 1-2 million tokens at convergence for the largest models we can train. (Section 5.1 )

Taken together, these results show that language modeling performance improves smoothly and predictably as we appropriately scale up model size, data, and compute. We expect that larger language models will perform better and be more sample efficient than current models.

1.2 Summary of Scaling Laws

The test loss of a Transformer trained to autoregressively model language can be predicted using a power-law when performance is limited by only either the number of non-embedding parameters N N, the dataset size D D, or the optimally allocated compute budget C min C_{\rm min} (see Figure 1 ):

1.

For models with a limited number of parameters, trained to convergence on sufficiently large datasets:

L ⁡ ( N ) = ( N c / N ) α N; α N ∼ 0.076, N c ∼ 8.8 × 10 13 ​ (non-embedding parameters) L(N)=\left(N_{\mathrm{c}}/N\right)^{\alpha_{N}};\penalty\ \penalty\ \alpha_{N}\sim 0.076,\quad N_{\mathrm{c}}\sim 8.8\times 10^{13}\penalty\ \text{(non-embedding parameters)}

(1.1)

2.

For large models trained with a limited dataset with early stopping:

L ⁡ ( D ) = ( D c / D ) α D; α D ∼ 0.095, D c ∼ 5.4 × 10 13 ​ (tokens) L(D)=\left(D_{\mathrm{c}}/D\right)^{\alpha_{D}};\penalty\ \penalty\ \alpha_{D}\sim 0.095,\quad D_{\mathrm{c}}\sim 5.4\times 10^{13}\penalty\ \text{(tokens)}

(1.2)

3.

When training with a limited amount of compute, a sufficiently large dataset, an optimally-sized model, and a sufficiently small batch size (making optimal 2 2
2

We also observe an empirical power-law trend with the training compute C C (Figure 1 ) while training at fixed batch size, but it is the trend with C min C_{\rm min} that should be used to make predictions. They are related by equation ( 5.5 ). use of compute):

L ⁡ ( C min ) = ( C c min / C min ) α C min; α C min ∼ 0.050, C c min ∼ 3.1 × 10 8 ​ (PF-days) L(C_{\rm min})=\left(C_{\mathrm{c}}^{\rm min}/C_{\rm min}\right)^{\alpha_{C}^{\rm min}};\penalty\ \penalty\ \alpha_{C}^{\rm min}\sim 0.050,\quad C_{\mathrm{c}}^{\rm min}\sim 3.1\times 10^{8}\penalty\ \text{(PF-days)}

(1.3)

These relations hold across eight orders of magnitude in C min C_{\rm min}, six orders of magnitude in N N, and over two orders of magnitude in D D. They depend very weakly on model shape and other Transformer hyperparameters (depth, width, number of self-attention heads), with specific numerical values associated with the Webtext2 training set [ RWC+19 ]. The power laws α N, α D, α C min \alpha_{\rm N},\alpha_{\rm D},\alpha_{C}^{\rm min} specify the degree of performance improvement expected as we scale up N N, D D, or C min C_{\rm min}; for example, doubling the number of parameters yields a loss that is smaller by a factor 2 − α N = 0.95 2^{-\alpha_{N}}=0.95. The precise numerical values of N c, C c min, N_{\mathrm{c}},C_{\rm c}^{\rm min}, and D c D_{\mathrm{c}} depend on the vocabulary size and tokenization and hence do not have a fundamental meaning.

The critical batch size, which determines the speed/efficiency tradeoff for data parallelism ( [ MKAT18 ] ), also roughly obeys a power law in L L :

B crit ​ ( L ) = B ∗ L 1 / α B, B ∗ ∼ 2 ⋅ 10 8 ​ tokens, α B ∼ 0.21 B_{\rm crit}\left(L\right)=\frac{B_{\ast}}{L^{1/\alpha_{B}}},\qquad B_{\ast}\sim 2\cdot 10^{8}\text{ tokens},\ \ \alpha_{B}\sim 0.21

(1.4)

Figure 4:
Left : The early-stopped test loss L ⁡ ( N, D ) L(N,D) varies predictably with the dataset size D D and model size N N according to Equation ( 1.5 ).
Right : After an initial transient period, learning curves for all model sizes N N can be fit with Equation ( 1.6 ), which is parameterized in terms of S min S_{\rm min}, the number of steps when training at large batch size (details in Section 5.1 ).

Equation ( 1.1 ) and ( 1.2 ) together suggest that as we increase the model size, we should increase the dataset size sublinearly according to D ∝ N α N α D ∼ N 0.74 D\propto N^{\frac{\alpha_{N}}{\alpha_{D}}}\sim N^{0.74}. In fact, we find that there is a single equation combining ( 1.1 ) and ( 1.2 ) that governs the simultaneous dependence on N N and D D and governs the degree of overfitting:

L ⁡ ( N, D ) = [ ( N c N ) α N α D + D c D ] α D L(N,D)=\left[\left(\frac{N_{c}}{N}\right)^{\frac{\alpha_{N}}{\alpha_{D}}}+\frac{D_{c}}{D}\right]^{\alpha_{D}}

(1.5)

with fits pictured on the left in figure 4. We conjecture that this functional form may also parameterize the trained log-likelihood for other generative modeling tasks.

When training a given model for a finite number of parameter update steps S S in the infinite data limit, after an initial transient period, the learning curves can be accurately fit by (see the right of figure 4 )

L ⁡ ( N, S ) = ( N c N ) α N + ( S c S min ​ ( S ) ) α S L(N,S)=\left(\frac{N_{c}}{N}\right)^{\alpha_{N}}+\left(\frac{S_{c}}{S_{\rm min}(S)}\right)^{\alpha_{S}}

(1.6)

where S c ≈ 2.1 × 10 3 S_{c}\approx 2.1\times 10^{3} and α S ≈ 0.76 \alpha_{S}\approx 0.76, and S min ​ ( S ) S_{\rm min}(S) is the minimum possible number of optimization steps (parameter updates)
estimated using Equation ( 5.4 ).

When training within a fixed compute budget C C, but with no other constraints, Equation ( 1.6 ) leads to the prediction that the optimal model size N N, optimal batch size B B, optimal number of steps S S, and dataset size D D should grow as

N ∝ C α C min / α N, B ∝ C α C min / α B, S ∝ C α C min / α S, D = B ⋅ S N\propto C^{\alpha_{C}^{\rm min}/\alpha_{N}},\quad B\propto C^{\alpha_{C}^{\rm min}/\alpha_{B}},\quad S\propto C^{\alpha_{C}^{\rm min}/\alpha_{S}},\quad D=B\cdot S\quad

(1.7)

with

α C min = 1 / ( 1 / α S + 1 / α B + 1 / α N ) \alpha_{C}^{\rm min}=1/\left(1/\alpha_{S}+1/\alpha_{B}+1/\alpha_{N}\right)

(1.8)

which closely matches the empirically optimal results N ∝ C min 0.73 N\propto C_{\rm min}^{0.73}, B ∝ C min 0.24 B\propto C_{\rm min}^{0.24}, and S ∝ C min 0.03 S\propto C_{\rm min}^{0.03}. As the computational budget C C increases, it should be spent primarily on larger models, without dramatic increases in training time or dataset size (see Figure 3 ). This also implies that as models grow larger, they become increasingly sample efficient. In practice, researchers typically train smaller models for longer than would be maximally compute-efficient because of hardware constraints.
Optimal performance depends on total compute as a power law (see Equation ( 1.3 )).

We provide some basic theoretical motivation for Equation ( 1.5 ), an analysis of learning curve fits and their implications for training time, and a breakdown of our results per token. We also make some brief comparisons to LSTMs and recurrent Transformers [ DGV+18 ].

1.3 Notation

We use the following notation:

•

L L – the cross entropy loss in nats. Typically it will be averaged over the tokens in a context, but in some cases we report the loss for specific tokens within the context.

•

N N – the number of model parameters, excluding all vocabulary and positional embeddings

•

C ≈ 6 ​ N ​ B ​ S C\approx 6NBS – an estimate of the total non-embedding training compute, where B B is the batch size, and S S is the number of training steps (ie parameter updates). We quote numerical values in PF-days, where one PF-day = 10 15 × 24 × 3600 = 8.64 × 10 19 =10^{15}\times 24\times 3600=8.64\times 10^{19} floating point operations.

•

D D – the dataset size in tokens

•

B crit B_{\rm crit} – the critical batch size [ MKAT18 ], defined and discussed in Section 5.1. Training at the critical batch size provides a roughly optimal compromise between time and compute efficiency.

•

C min C_{\rm min} – an estimate of the minimum amount of non-embedding compute to reach a given value of the loss. This is the training compute that would be used if the model were trained at a batch size much less than the critical batch size.

•

S min S_{\rm min} – an estimate of the minimal number of training steps needed to reach a given value of the loss. This is also the number of training steps that would be used if the model were trained at a batch size much greater than the critical batch size.

•

α X \alpha_{X} – power-law exponents for the scaling of the loss as L ⁡ ( X ) ∝ 1 / X α X L(X)\propto 1/X^{\alpha_{X}} where X X can be any of N, D, C, S, B, C min N,D,C,S,B,C^{\rm min}.

2 Background and Methods

We train language models on WebText2, an extended version of the WebText [ RWC+19 ] dataset, tokenized using byte-pair encoding [ SHB15 ] with a vocabulary size n vocab = 50257 n_{\rm vocab}=50257. We optimize the autoregressive log-likelihood (i.e. cross-entropy loss) averaged over a 1024-token context, which is also our principal performance metric. We record the loss on the WebText2 test distribution and on a selection of other text distributions. We primarily train decoder-only [ LSP+18, RNSS18 ] Transformer [ VSP+17 ] models, though we also train LSTM models and Universal Transformers [ DGV+18 ] for comparison.

2.1 Parameter and Compute Scaling of Transformers

Operation

Parameters

FLOPs per Token

Embed

( n vocab + n ctx ) ​ d model \left(n_{{\rm vocab}}+n_{{\rm ctx}}\right)d_{{\rm model}}

4 ​ d model 4d_{{\rm model}}

Attention: QKV

n layer ​ d model ​ 3 ​ d attn n_{{\rm layer}}d_{{\rm model}}3d_{{\rm attn}}

2 ​ n layer ​ d model ​ 3 ​ d attn 2n_{{\rm layer}}d_{{\rm model}}3d_{{\rm attn}}

Attention: Mask

—

2 ​ n layer ​ n ctx ​ d attn 2n_{{\rm layer}}n_{{\rm ctx}}d_{{\rm attn}}

Attention: Project

n layer ​ d attn ​ d model n_{{\rm layer}}d_{{\rm attn}}d_{{\rm model}}

2 ​ n layer ​ d attn ​ d embd 2n_{{\rm layer}}d_{{\rm attn}}d_{{\rm embd}}

Feedforward

n layer ​ 2 ​ d model ​ d ff n_{{\rm layer}}2d_{{\rm model}}d_{{\rm ff}}

2 ​ n layer ​ 2 ​ d model ​ d ff 2n_{{\rm layer}}2d_{{\rm model}}d_{{\rm ff}}

De-embed

—

2 ​ d model ​ n vocab 2d_{{\rm model}}n_{{\rm vocab}}

Total (Non-Embedding)

N = 2 ​ d model ​ n layer ​ ( 2 ​ d attn + d ff ) N=2d_{{\rm model}}n_{{\rm layer}}\left(2d_{{\rm attn}}+d_{{\rm ff}}\right)

C forward = 2 ​ N + 2 ​ n layer ​ n ctx ​ d attn C_{\mathrm{forward}}=2N+2n_{{\rm layer}}n_{{\rm ctx}}d_{{\rm attn}}

Table 1: Parameter counts and compute (forward pass) estimates for a Transformer model. Sub-leading terms such as nonlinearities, biases, and layer normalization are omitted.

We parameterize the Transformer architecture using hyperparameters n layer n_{\rm layer} (number of layers), d model d_{{\rm model}} (dimension of the residual stream), d ff d_{\rm ff} (dimension of the intermediate feed-forward layer), d attn d_{\rm attn} (dimension of the attention output), and n heads n_{\rm heads} (number of attention heads per layer). We include n ctx n_{\rm ctx} tokens in the input context, with n ctx = 1024 n_{\rm ctx}=1024 except where otherwise noted.

We use N N to denote the model size, which we define as the number of non-embedding parameters

N \displaystyle N

≈ 2 ​ d model ​ n layer ​ ( 2 ​ d attn + d ff ) \displaystyle\approx 2d_{{\rm model}}n_{{\rm layer}}\left(2d_{{\rm attn}}+d_{{\rm ff}}\right)

= 12 n layer d model 2 with the standard d attn = d ff / 4 = d model \displaystyle=12n_{\rm layer}d_{{\rm model}}^{2}\quad\text{ with the standard }\quad d_{\rm attn}=d_{\rm ff}/4=d_{{\rm model}}

(2.1)

where we have excluded biases and other sub-leading terms.
Our models also have n vocab ​ d model n_{\rm vocab}d_{{\rm model}} parameters in an embedding matrix, and use n ctx ​ d model n_{\rm ctx}d_{{\rm model}} parameters for positional embeddings, but we do not include these when discussing the ‘model size’ N N; we will see that this produces significantly cleaner scaling laws.

Evaluating a forward pass of the Transformer involves roughly

C forward ≈ 2 ​ N + 2 ​ n layer ​ n ctx ​ d model C_{\rm forward}\approx 2N+2n_{{\rm layer}}n_{{\rm ctx}}d_{{\rm model}}

(2.2)

add-multiply operations, where the factor of two comes from the multiply-accumulate operation used in matrix multiplication. A more detailed per-operation parameter and compute count is included in Table 1.

For contexts and models with d model > n ctx / 12 d_{{\rm model}}>n_{\rm ctx}/12, the context-dependent computational cost per token is a relatively small fraction of the total compute.
Since we primarily study models where d model ≫ n ctx / 12 d_{\rm model}\gg n_{\rm ctx}/12, we do not include context-dependent terms in our training compute estimate. Accounting for the backwards pass (approximately twice the compute as the forwards pass), we then define the estimated non-embedding compute as C ≈ 6 ​ N C\approx 6N floating point operators per training token.

2.2 Training Procedures

Unless otherwise noted, we train models with the Adam optimizer [ KB14 ] for a fixed 2.5 × 10 5 2.5\times 10^{5} steps with a batch size of 512 512 sequences of 1024 1024 tokens.
Due to memory constraints, our largest models (more than 1B parameters) were trained with Adafactor [ SS18 ].
We experimented with a variety of learning rates and schedules, as discussed in Appendix D.6.
We found that results at convergence were largely independent of learning rate schedule. Unless otherwise noted, all training runs included in our data used a learning rate schedule with a 3000 step linear warmup followed by a cosine decay to zero.

2.3 Datasets

We train our models on an extended version of the WebText dataset described in [ RWC+19 ]. The original WebText dataset was a web scrape of outbound links from Reddit through December 2017 which received at least 3 karma. In the second version, WebText2, we added outbound Reddit links from the period of January to October 2018, also with a minimum of 3 karma. The karma threshold served as a heuristic for whether people found the link interesting or useful. The text of the new links was extracted with the Newspaper3k python library. In total, the dataset consists of 20.3M documents containing 96 GB of text and 1.62 × 10 10 1.62\times 10^{10} words (as defined by wc ). We then apply the reversible tokenizer described in [ RWC+19 ], which yields 2.29 × 10 10 2.29\times 10^{10} tokens. We reserve 6.6 × 10 8 6.6\times 10^{8} of these tokens for use as a test set, and we also test on similarly-prepared samples of Books Corpus [ ZKZ+15 ], Common Crawl [ Fou ], English Wikipedia, and a collection of publicly-available Internet Books.

3 Empirical Results and Basic Power Laws

To characterize language model scaling we train a wide variety of models, varying a number of factors including:

•

Model size (ranging in size from 768 to 1.5 billion non-embedding parameters)

•

Dataset size (ranging from 22 million to 23 billion tokens)

•

Shape (including depth, width, attention heads, and feed-forward dimension)

•

Context length (1024 for most runs, though we also experiment with shorter contexts)

•

Batch size ( 2 19 2^{19} for most runs, but we also vary it to measure the critical batch size)

In this section we will display data along with empirically-motivated fits, deferring theoretical analysis to later sections.

3.1 Approximate Transformer Shape and Hyperparameter Independence

Figure 5: Performance depends very mildly on model shape when the total number of non-embedding parameters N N is held fixed. The loss varies only a few percent over a wide range of shapes. Small differences in parameter counts are compensated for by using the fit to L ⁡ ( N ) L(N) as a baseline. Aspect ratio in particular can vary by a factor of 40 while only slightly impacting performance; an ( n layer, d model ) = ( 6, 4288 ) (n_{\mathrm{layer}},d_{\mathrm{model}})=(6,4288) reaches a loss within 3% of the ( 48, 1600 ) (48,1600) model used in [ RWC+19 ].

Transformer performance depends very weakly on the shape parameters n layer, n heads n_{\rm layer},n_{\rm heads}, and d ff d_{\rm ff} when we hold the total non-embedding parameter count N N fixed.
To establish these results we trained models with fixed size while varying a single hyperparameter. This was simplest for the case of n heads n_{\rm heads}. When varying n layer n_{\rm layer}, we simultaneously varied d model d_{{\rm model}} while keeping N ≈ 12 ​ n layer ​ d model 2 N\approx 12n_{\rm layer}d_{{\rm model}}^{2} fixed. Similarly, to vary d ff d_{\rm ff} at fixed model size we also simultaneously varied the d model d_{{\rm model}} parameter, as required by the parameter counts in Table 1. Independence of n layers n_{\rm layers} would follow if deeper Transformers effectively behave as ensembles of shallower models, as has been suggested for ResNets [ VWB16 ]. The results are shown in Figure 5.

3.2 Performance with Non-Embedding Parameter Count N N

Figure 6:
Left: When we include embedding parameters, performance appears to depend strongly on the number of layers in addition to the number of parameters.
Right: When we exclude embedding parameters, the performance of models with different depths converge to a single trend. Only models with fewer than 2 layers or with extreme depth-to-width ratios deviate significantly from the trend.

In Figure 6 we display the performance of a wide variety of models, ranging from small models with shape ( n layer, d model ) = ( 2,128 ) (n_{\rm layer},d_{{\rm model}})=(2,128) through billion-parameter models, ranging in shape from ( 6, 4288 ) (6,4288) through ( 207,768 ) (207,768). Here we have trained to near convergence on the full WebText2 dataset and observe no overfitting (except possibly for the very largest models).

As shown in Figure 1, we find a steady trend with non-embedding parameter count N N, which can be fit to the first term of Equation ( 1.5 ), so that

L ⁡ ( N ) ≈ ( N c N ) α N L(N)\approx\left(\frac{N_{c}}{N}\right)^{\alpha_{N}}

(3.1)

To observe these trends it is crucial to study performance as a function of N N; if we instead use the total parameter count (including the embedding parameters) the trend is somewhat obscured (see Figure 6 ). This suggests that the embedding matrix can be made smaller without impacting performance, as has been seen in recent work [ LCG+19 ].

Although these models have been trained on the WebText2 dataset, their test loss on a variety of other datasets is also a power-law in N N with nearly identical power, as shown in Figure 8.

3.2.1 Comparing to LSTMs and Universal Transformers

Figure 7:

In Figure 7 we compare LSTM and Transformer performance as a function of non-embedding parameter count N N. The LSTMs were trained with the same dataset and context length. We see from these figures that the LSTMs perform as well as Transformers for tokens appearing early in the context, but cannot match the Transformer performance for later tokens. We present power-law relationships between performance and context position Appendix D.5, where increasingly large powers for larger models suggest improved ability to quickly recognize patterns.

We also compare the performance of standard Transformers to recurrent Transformers [ DGV+18 ] in Figure 17 in the appendix. These models re-use parameters, and so perform slightly better as a function of N N, at the cost of additional compute per-parameter.

3.2.2 Generalization Among Data Distributions

We have also tested our models on a set of additional text data distributions. The test loss on these datasets as a function of model size is shown in Figure 8; in all cases the models were trained only on the WebText2 dataset. We see that the loss on these other data distributions improves smoothly with model size, in direct parallel with the improvement on WebText2. We find that generalization depends almost exclusively on the in-distribution validation loss, and does not depend on the duration of training or proximity to convergence. We also observe no dependence on model depth (see Appendix D.8 ).

Figure 8:
Left: Generalization performance to other data distributions improves smoothly with model size, with only a small and very slowly growing offset from the WebText2 training distribution.
Right: Generalization performance depends only on training distribution performance, and not on the phase of training. We compare generalization of converged models (points) to that of a single large model (dashed curves) as it trains.

3.3 Performance with Dataset Size and Compute

We display empirical trends for the test loss as a function of dataset size D D (in tokens) and training compute C C in Figure 1.

For the trend with D D we trained a model with ( n layer, n embd ) = ( 36, 1280 ) (n_{\rm layer},n_{\rm embd})=(36,1280) on fixed subsets of the WebText2 dataset. We stopped training once the test loss ceased to decrease. We see that the resulting test losses can be fit with simple power-law

L ⁡ ( D ) ≈ ( D c D ) α D L(D)\approx\left(\frac{D_{c}}{D}\right)^{\alpha_{D}}

(3.2)

in the dataset size. The data and fit appear in Figure 1.

The total amount of non-embedding compute used during training can be estimated as C = 6 ​ N ​ B ​ S C=6NBS, where B B is the batch size, S S is the number of parameter updates, and the factor of 6 6 accounts for the forward and backward passes. Thus for a given value of C C we can scan over all models with various N N to find the model with the best performance on step S = C 6 ​ B ​ S S=\frac{C}{6BS}. Note that in these results the batch size B B remains fixed for all models, which means that these empirical results are not truly optimal. We will account for this in later sections using an adjusted C min C_{\rm min} to produce cleaner trends.

The result appears as the heavy black line on the left-hand plot in Figure 1. It can be fit with

L ⁡ ( C ) ≈ ( C c C ) α C L(C)\approx\left(\frac{C_{c}}{C}\right)^{\alpha_{C}}

(3.3)

The figure also includes images of individual learning curves to clarify when individual models are optimal. We will study the optimal allocation of compute more closely later on.
The data strongly suggests that sample efficiency improves with model size, and we also illustrate this directly in Figure 19 in the appendix.

4 Charting the Infinite Data Limit and Overfitting

In Section 3 we found a number of basic scaling laws for language modeling performance. Here we will study the performance of a model of size N N trained on a dataset with D D tokens while varying N N and D D simultaneously. We will empirically demonstrate that the optimally trained test loss accords with the scaling law of Equation ( 1.5 ). This provides guidance on how much data we would need to train models of increasing size while keeping overfitting under control.

4.1 Proposed L ⁡ ( N, D ) L(N,D) Equation

Figure 9:
The early-stopped test loss L ⁡ ( N, D ) L(N,D) depends predictably on the dataset size D D and model size N N according to Equation ( 1.5 ).
Left :
For large D D, performance is a straight power law in N N. For a smaller fixed D D, performance stops improving as N N increases and the model begins to overfit. (The reverse is also true, see Figure 4.)
Right : The extent of overfitting depends predominantly on the ratio N α N α D / D N^{\frac{\alpha_{N}}{\alpha_{D}}}/D, as predicted in equation ( 4.3 ). The line is our fit to that equation.

We have chosen the parameterization ( 1.5 ) (repeated here for convenience):

L ⁡ ( N, D ) = [ ( N c N ) α N α D + D c D ] α D L(N,D)=\left[\left(\frac{N_{c}}{N}\right)^{\frac{\alpha_{N}}{\alpha_{D}}}+\frac{D_{c}}{D}\right]^{\alpha_{D}}

(4.1)

using three principles:

1.

Changes in vocabulary size or tokenization are expected to rescale the loss by an overall factor. The parameterization of L ⁡ ( N, D ) L(N,D) (and all models of the loss) must naturally allow for such a rescaling.

2.

Fixing D D and sending N → ∞ N\to\infty, the overall loss should approach L ⁡ ( D ) L(D). Conversely, fixing N N and sending D → ∞ D\to\infty the loss must approach L ⁡ ( N ) L(N).

3.

L ⁡ ( N, D ) L(N,D) should be analytic at D = ∞ D=\infty, so that it has a series expansion in 1 / D 1/D with integer powers. Theoretical support for this principle is significantly weaker than for the first two.

Our choice of L ⁡ ( N, D ) L(N,D) satisfies the first requirement because we can rescale N c, D c N_{c},D_{c} with changes in the vocabulary. This also implies that the values of N c, D c N_{c},D_{c} have no fundamental meaning.

Since we stop training early when the test loss ceases to improve and optimize all models in the same way, we expect that larger models should always perform better than smaller models. But with fixed finite D D, we also do not expect any model to be capable of approaching the best possible loss (ie the entropy of text). Similarly, a model with fixed size will be capacity-limited. These considerations motivate our second principle.
Note that knowledge of L ⁡ ( N ) L(N) at infinite D D and L ⁡ ( D ) L(D) at infinite N N fully determines all the parameters in L ⁡ ( N, D ) L(N,D).

The third principle is more speculative. There is a simple and general reason one might expect overfitting to scale ∝ 1 / D \propto 1/D at very large D D. Overfitting should be related to the variance or the signal-to-noise ratio of the dataset [ AS17 ], and this scales as 1 / D 1/D. This expectation should hold for any smooth loss function, since we expect to be able to expand the loss about the D → ∞ D\to\infty limit. However, this argument assumes that 1 / D 1/D corrections dominate over other sources of variance, such as the finite batch size and other limits on the efficacy of optimization. Without empirical confirmation, we would not be very confident of its applicability.

Our third principle explains the asymmetry between the roles of N N and D D in Equation ( 1.5 ). Very similar symmetric expressions 3 3
3

For example, one might have used L ⁡ ( N, D ) = [ ( N c N ) α N + ( D c D ) α D ] β L(N,D)=\left[\left(\frac{N_{c}}{N}\right)^{\alpha_{N}}+\left(\frac{D_{c}}{D}\right)^{\alpha_{D}}\right]^{\beta}, but this does not have a 1 / D 1/D expansion. are possible, but they would not have a 1 / D 1/D expansion with integer powers, and would require the introduction of an additional parameter.

In any case, we will see that our equation for L ⁡ ( N, D ) L(N,D) fits the data well, which is the most important justification for our L ⁡ ( N, D ) L(N,D) ansatz.

4.2 Results

We regularize all our models with 10% dropout, and by tracking test loss and stopping once it is no longer decreasing. The results are displayed in Figure 9, including a fit to the four parameters α N, α D, N c, D c \alpha_{N},\alpha_{D},N_{c},D_{c} in Equation ( 1.5 ):

Parameter

α N \alpha_{N}

α D \alpha_{D}

N c N_{c}

D c D_{c}

Value

0.076 0.076

0.103 0.103

6.4 × 10 13 6.4\times 10^{13}

1.8 × 10 13 1.8\times 10^{13}

Table 2: Fits to L ⁡ ( N, D ) L(N,D)

We obtain an excellent fit, with the exception of the runs where the dataset has been reduced by a factor of 1024 1024, to about 2 × 10 7 2\times 10^{7} tokens. With such a small dataset, an epoch consists of only 40 parameter updates. Perhaps such a tiny dataset represents a different regime for language modeling, as overfitting happens very early in training (see Figure 16 ). Also note that the parameters differ very slightly from those obtained in Section 3, as here we are fitting the full L ⁡ ( N, D ) L(N,D) rather than just L ⁡ ( N, ∞ ) L(N,\infty) or L ⁡ ( ∞, D ) L(\infty,D).

To chart the borderlands of the infinite data limit, we can directly study the extent of overfitting. For all but the largest models, we see no sign of overfitting when training with the full 22B token WebText2 dataset, so we can take it as representative of D = ∞ D=\infty. Thus we can compare finite D D to the infinite data limit by defining

δ ​ L ​ ( N, D ) ≡ L ⁡ ( N, D ) L ⁡ ( N, ∞ ) − 1 \delta L(N,D)\equiv\frac{L(N,D)}{L(N,\infty)}-1

(4.2)

and studying it as a function of N, D N,D. In fact, we see empirically that δ ​ L \delta L depends only a specific combination of N N and D D, as shown in Figure 16. This follows from the scaling law of Equation ( 1.5 ), which implies

δ ​ L ≈ ( 1 + ( N N c ) α N α D ​ D c D ) α D − 1 \delta L\approx\left(1+\left(\frac{N}{N_{c}}\right)^{\frac{\alpha_{N}}{\alpha_{D}}}\frac{D_{c}}{D}\right)^{\alpha_{D}}-1

(4.3)

Note that at large D D this formula also has a series expansion in powers of 1 / D 1/D.

We estimate that the variation in the loss with different random seeds is roughly 0.02 0.02, which means that to avoid overfitting when training to within that threshold of convergence we require

D ≳ ( 5 × 10 3 ) ​ N 0.74 D\gtrsim(5\times 10^{3})\,N^{0.74}

(4.4)

With this relation, models smaller than 10 9 10^{9} parameters can be trained with minimal overfitting on the 22B token WebText2 dataset, but our largest models will encounter some mild overfitting. More generally, this relation shows that dataset size may grow sub-linearly in model size while avoiding overfitting. Note however that this does not typically represent maximally compute-efficient training. We should also emphasize that we have not optimized regularization (eg the dropout probability) while varying dataset and model size.

5 Scaling Laws with Model Size and Training Time

In this section we will demonstrate that a simple scaling law provides a good description for the loss as a function of model size N N and training time. First we will explain how to use the results of [ MKAT18 ] to define a universal training step S min S_{\rm min}, which accounts for the fact that most of our models have not been trained at an optimal batch size. Then we will demonstrate that we can fit the model size and training time dependence of the loss using Equation ( 1.6 ). Later we will use these results to predict the optimal allocation of training compute between model size and training time, and then confirm that prediction.

5.1 Adjustment for Training at B crit ​ ( L ) B_{\rm crit}(L)

Figure 10: The critical batch size B crit B_{\rm crit} follows a power law in the loss as performance increase, and does not depend directly on the model size. We find that the critical batch size approximately doubles for every 13 % 13\% decrease in loss. B crit B_{\rm crit} is measured empirically from the data shown in Figure 18, but it is also roughly predicted by the gradient noise scale, as in [ MKAT18 ].

A simple empirical theory for the batch size dependence of training was developed in [ MKAT18 ] (see also [ SLA+18, ZLN+19 ] ). It was argued that there is a critical batch size B crit B_{\rm crit} for training; for B B up to B crit B_{\rm crit} the batch size can be increased with very minimal degradation in compute-efficiency, whereas for B > B crit B>B_{\rm crit} increases in B B result in diminishing returns. It was also argued that the gradient noise scale provides a simple prediction for B crit B_{\rm crit}, and that neither depends directly on model size except through the value of the loss that has been attained. These results can be used to predict how training time and compute will vary with the batch size. To utilize both training time and compute as effectively as possible, it is best to train with a batch size B ≈ B crit B\approx B_{\rm crit}. Training at B ≫ B crit B\gg B_{\rm crit} minimizes the number of training steps, while B ≪ B crit B\ll B_{\rm crit} minimizes the use of compute.

More specifically, it was demonstrated that for a wide variety of neural network tasks, the number of training steps S S and the number of data examples processed E = B ​ S E=BS satisfy the simple relation

( S S min − 1 ) ​ ( E E min − 1 ) = 1 \left(\frac{S}{S_{\rm min}}-1\right)\left(\frac{E}{E_{\rm min}}-1\right)=1

(5.1)

when training to any fixed value of the loss L L. Here S min S_{\rm min} is the minimum number of steps necessary to reach L L, while E min E_{\rm min} is the minimum number of data examples that must be processed.

We demonstrate the relation ( 5.1 ) for Transformers in Figure 18 in the appendix. This relation defines the critical batch size

B crit ​ ( L ) ≡ E min S min B_{\rm crit}(L)\equiv\frac{E_{\rm min}}{S_{\rm min}}

(5.2)

which is a function of the target value of the loss. Training at the critical batch size makes a roughly optimal time/compute tradeoff, requiring 2 ​ S min 2S_{\rm min} training steps and processing E = 2 ​ E min E=2E_{\rm min} data examples.

In Figure 10 we have plotted the critical batch size and gradient noise scale 4 4
4

Although the critical batch size roughly matches the gradient noise scale, we are using a direct measurements of B crit B_{\rm crit} from Figures 18 and 10 for all our later analyses. as a function of training loss for two different models. We see that B crit ​ ( L ) B_{\rm crit}(L) is independent of model size, and only depends on the loss L L. So the predictions of [ MKAT18 ] continue to hold for Transformer language models. The critical batch size can be fit with a power-law in the loss

B crit ​ ( L ) ≈ B ∗ L 1 / α B B_{\rm crit}(L)\approx\frac{B_{*}}{L^{1/\alpha_{B}}}

(5.3)

where B ∗ ≈ 2 × 10 8 B_{*}\approx 2\times 10^{8} and α B ≈ 0.21 \alpha_{B}\approx 0.21.

We have chosen this parameterization for B crit ​ ( L ) B_{\rm crit}(L) because as the loss approaches its minimum value L min L_{\rm min}, the gradient noise scale is expected to diverge, and we expect B crit B_{\rm crit} to track this noise scale. We do not know L min L_{\rm min}, as we see no sign that our models are approaching it, but L min > 0 L_{\rm min}>0 since the entropy of natural language is non-zero. Since apparently L min L_{\rm min} is much smaller than the values of L L we have achieved, we used a parameterization where B crit B_{\rm crit} diverges as L → 0 L\to 0.

We will use B crit ​ ( L ) B_{\rm crit}(L) to estimate the relation between the number of training steps S S while training at batch size B = 2 19 B=2^{19} tokens and the number of training steps while training at B ≫ B crit B\gg B_{\rm crit}. This is simply

S min ​ ( S ) ≡ S 1 + B crit ​ ( L ) / B ( minimum steps, at ​ B ≫ B crit ) S_{\rm min}(S)\equiv\frac{S}{1+B_{\rm crit}(L)/B}\qquad(\text{minimum steps, at }B\gg B_{\rm crit})

(5.4)

for any given target value L L for the loss. This also defines a critical value of the compute needed to train to L L with a model of size N N if we were to train at B ≪ B crit ​ ( L ) B\ll B_{\rm crit}(L). This is

C min ​ ( C ) ≡ C 1 + B / B crit ​ ( L ) ( minimum compute, at ​ B ≪ B crit ) C_{\rm min}(C)\equiv\frac{C}{1+B/B_{\rm crit}(L)}\qquad(\text{minimum compute, at }B\ll B_{\rm crit})

(5.5)

where C = 6 ​ N ​ B ​ S C=6NBS estimates the (non-embedding) compute used at batch size B B.

5.2 Results for L ⁡ ( N, S min ) L(N,S_{\rm min}) and Performance with Model Size and Compute

Figure 11:
When we hold either total compute or number of training steps fixed, performance follows L ⁡ ( N, S ) L(N,S) from Equation ( 5.6 ). Each value of compute budget has an associated optimal model size that maximizes performance. Mediocre fits at small S S are unsurprising, as the power-law equation for the learning curves breaks down very early in training.

Now we will use S min S_{\rm min} defined in Equation ( 5.4 ) to obtain a simple and universal fit for the dependence of the loss on model size and training time in the infinite data limit. We will fit the stable, Adam-optimized training runs using Equation ( 1.6 ), repeated here for convenience:

L ⁡ ( N, S min ) = ( N c N ) α N + ( S c S min ) α S L(N,S_{\rm min})=\left(\frac{N_{c}}{N}\right)^{\alpha_{N}}+\left(\frac{S_{c}}{S_{\rm min}}\right)^{\alpha_{S}}

(5.6)

for the loss. We include all training steps after the warmup period of the learning rate schedule, and find a fit to the data with the parameters:

Parameter

α N \alpha_{N}

α S \alpha_{S}

N c N_{c}

S c S_{c}

Value

0.077 0.077

0.76 0.76

6.5 × 10 13 6.5\times 10^{13}

2.1 × 10 3 2.1\times 10^{3}

Table 3: Fits to L ⁡ ( N, S ) L(N,S)

With these parameters, we obtain the learning curve fits in Figure 4. Though the fits are imperfect, we believe they are quite compelling given the simplicity of Equation ( 5.6 ).

The data and fits can be visualized in a different and more interesting way, as shown in Figure 11. There we study the test loss as a function of model size while fixing either the total non-embedding compute C C used in training, or the number of steps S S. For the fits we use Equation ( 5.5 ) and ( 5.4 ) along with the parameters above and Equation ( 5.6 ).

The power-law dependence of the loss on S min S_{\rm min} reflects the interplay of optimizer dynamics and the loss landscape. Since the fits are best late in training, when the loss may be approximately quadratic, the power-law should provide information about the spectrum of the Hessian of the loss. Its universality suggests that the Hessian eigenvalue density is roughly independent of model size.

5.3 Lower Bound on Early Stopping Step

The results for L ⁡ ( N, S min ) L(N,S_{\rm min}) can be used to derive a lower-bound (and rough estimate) of the step at which early stopping should occur when training is data limited.
It is motivated by the idea that finite and infinite D D learning curves for a given model will be very similar until we reach S min ≈ S stop S_{\rm min}\approx S_{\rm stop}. Thus overfitting should be proportional to the correction from simply ending training at S stop S_{\rm stop}. This will underestimate S stop S_{\rm stop}, because in reality the test loss will decrease more slowly when we have a finite D D, and therefore we will require more training steps to reach the optimal test loss at finite D D.
This line of reasoning leads to the inequality

S stop ​ ( N, D ) ≳ S c [ L ⁡ ( N, D ) − L ⁡ ( N, ∞
