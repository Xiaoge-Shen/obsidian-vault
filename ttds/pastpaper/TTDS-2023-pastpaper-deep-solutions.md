---
title: "TTDS 2023 Past Paper Deep Solutions"
subtitle: "Original-question guide + worked solutions"
author: "Generated for TTDS revision"
date: "2026-05-20"
geometry:
  - a4paper
  - margin=0.65in
fontsize: 10.5pt
CJKmainfont: "Heiti SC"
mainfont: "Helvetica Neue"
monofont: "Menlo"
colorlinks: true
header-includes:
  - \usepackage{graphicx}
---

# 使用方式

这份解析对应 `2023638_INFR11145_INFR11229.pdf`。每题都用“题目要点 + 解题思路 + 答案/计算 + 常见坑 + 考试模板”组织。中文解释为主，关键 TTDS terms 保留英文，方便考试直接写。

# Question 1: Short Answers

## Original question screenshot

\includegraphics[height=0.78\textheight,keepaspectratio]{original-question-screenshots/ttds-2023-page-2.png}

## 原题要点

10 个 short-answer concepts：`Cranfield test collection`、`smoothing`、`PageRank`、`explicit relevance feedback`、`shingle`、`polite crawler`、`eye tracking as implicit feedback`、`web search user needs`、`similar queries without shared words`、`crawler back queues`。

## 逐题解析

**(a) Cranfield paradigm 的 test collection components**  
标准答案是三件套：`document collection`、`information needs / queries / topics`、`relevance judgements / qrels`。它们让不同 IR systems 可以在同一数据、同一 queries、同一 relevance labels 下比较。

**(b) Why smoothing matters in language modelling**  
在 `query likelihood language model` 中，如果 query term 没在 document 出现，MLE 会给 $P(t|d)=0$，导致整个 query probability 变成 0。`Smoothing` 把 document model 和 collection model 混合，既避免 zero probability，也让短文档的估计更稳。

**(c) PageRank value represents what**  
`PageRank` 是 random surfer 在长期随机浏览中位于某 page 的 stationary probability。直觉上，它是 link-based importance，不直接看 query content。

**(d) Why commercial web search rarely has explicit relevance feedback**  
用户不愿额外标注 relevant/non-relevant，交互成本高，而且 web search 常是快速任务。商业搜索更常用 `implicit feedback`，如 clicks、dwell time、query reformulation。

**(e) Shingle**  
`Shingle` 是 document 中连续 token/character 的 $k$-gram，用于 near-duplicate detection。比如 word 3-shingles 可表示局部文本片段，再用 Jaccard/MinHash 比较相似度。

**(f) Polite crawler**  
`Polite crawler` 不会过度请求同一 host，会遵守 `robots.txt`、crawl-delay、per-host waiting time，避免给网站服务器造成压力。

**(g) Eye tracking as implicit feedback**  
可以。若用户长时间注视某 result/snippet/document，可能表示 attention 或 interest。但它 noisy：看了不等于 relevant，也可能是困惑。

**(h) Three major web search user needs**  
经典分类：`informational`、`navigational`、`transactional`。Informational 是找信息，navigational 是去特定网站，transactional 是完成购买、下载、订票等动作。

**(i) Detect similar queries without shared words**  
可用 `query logs`、`click-through data`、`embeddings/dense representations`、`thesaurus/query expansion`。如果两个 queries 点击同一组 documents，或 dense vectors 接近，即使没有 shared terms 也可判为 similar。

**(j) Role of back queues in URL frontier**  
`Back queues` 通常按 host/site 管理 URLs，保证同一 host 的 fetch 间隔满足 politeness。Front queues 偏 prioritisation，back queues 偏 scheduling/politeness。

## 考试模板

Short-answer 不要写散文。格式可用：`Term = definition + why it matters + one caveat/example`。

\newpage

# Question 2: PageRank Computation

## Original question screenshot

\includegraphics[height=0.78\textheight,keepaspectratio]{original-question-screenshots/ttds-2023-page-2.png}

## 原题要点

Graph:

- A -> B, C
- B -> C, D
- C -> D
- D -> A

Teleportation factor / damping factor 按讲义公式理解为 $\lambda=0.5$，即：

$$
PR_{i+1}(x)=\frac{1-\lambda}{N}+\lambda\sum_{y\to x}\frac{PR_i(y)}{L_{out}(y)}
$$

这里 $N=4$，初始 $PR_0(A)=PR_0(B)=PR_0(C)=PR_0(D)=0.25$，teleportation base 是 $(1-0.5)/4=0.125$。

## 计算步骤

Iteration 1:

- $PR_1(A)=0.125+0.5PR_0(D)=0.125+0.125=0.250$
- $PR_1(B)=0.125+0.5PR_0(A)/2=0.125+0.0625=0.188$
- $PR_1(C)=0.125+0.5(PR_0(A)/2+PR_0(B)/2)=0.250$
- $PR_1(D)=0.125+0.5(PR_0(B)/2+PR_0(C))=0.313$

Iteration 2:

- $PR_2(A)=0.125+0.5PR_1(D)=0.281$
- $PR_2(B)=0.125+0.5PR_1(A)/2=0.188$
- $PR_2(C)=0.125+0.5(PR_1(A)/2+PR_1(B)/2)=0.234$
- $PR_2(D)=0.125+0.5(PR_1(B)/2+PR_1(C))=0.297$

## 答案

| Page | Iteration 1 | Iteration 2 |
| --- | ---: | ---: |
| A | 0.250 | 0.281 |
| B | 0.188 | 0.188 |
| C | 0.250 | 0.234 |
| D | 0.313 | 0.297 |

## 常见坑

不要把 page 的 PageRank 全部给每条 outlink；要除以 out-degree。也不要忘记 teleportation base。

\newpage

# Question 3: Will ChatGPT Replace Web Search

## Original question screenshot

\includegraphics[height=0.78\textheight,keepaspectratio]{original-question-screenshots/ttds-2023-page-2.png}

## 原题要点

要求讨论 LLM/chatbot 是否会取代 web search engines。不是纯观点题，应该用 TTDS concepts 支撑。

## 深入解析

一个平衡答案通常比绝对同意/反对更稳：LLMs 会改变 search interface，但不会完全替代 search engines。

**为什么不会完全替代：**

- `Freshness`: web search 可以抓取最新网页，LLM parametric knowledge 可能过时。
- `Attribution`: search results 可给 URLs/snippets，用户能追溯 source；LLM 可能 hallucinate。
- `Navigational/transactional needs`: 用户常想去官网、买票、下载文件，search engine 更直接。
- `Corpus coverage`: search engine 的 index 可覆盖 massive web collection，LLM 不能把所有实时网页都放进 parameters。
- `Evaluation and trust`: 对医疗、法律、新闻等 high-stakes queries，需要 sources 和 ranking diversity。

**LLMs 会带来的改变：**

- 对 informational queries，LLM 可 summarise、compare、explain。
- `RAG` 可以结合 retrieval 和 generation：先 retrieve evidence，再 generate grounded answer。
- Chat interface 更适合 exploratory search 和 multi-turn clarification。

## 考试模板

我会写：LLMs complement rather than fully replace web search。Search engines provide freshness、coverage、source attribution and navigational access；LLMs provide synthesis and conversational interaction。Best system is often RAG-like: retrieval for evidence, generation for explanation.

\newpage

# Question 4: IR Evaluation Metrics

## Original question screenshot

\includegraphics[height=0.78\textheight,keepaspectratio]{original-question-screenshots/ttds-2023-page-3.png}

## 原题要点

8 个 queries A-H，每个 ranked list 用 `+`/`-` 表示 relevant/non-relevant。要算：

- Q4(a) query D 的 `F1@8`
- Q4(b) 每个 query 的 `R-precision`
- Q4(c) 自定义线性折扣的 `nDCG'@10`

## (a) F1@8 for Query D

Query D relevant ranks: 1, 3, 6, 10，所以总 relevant 数 $R=4$。

Top 8 中 relevant 有 3 个：

$$
P@8=\frac{3}{8}=0.375,\qquad R@8=\frac{3}{4}=0.750
$$

$$
F1@8=\frac{2PR}{P+R}=\frac{2(0.375)(0.750)}{0.375+0.750}=0.500
$$

## (b) R-precision

`R-precision = P@R`，其中 $R$ 是该 query 的 relevant documents 总数。

| Query | Relevant ranks | R | R-precision |
| --- | --- | ---: | ---: |
| A | 1 | 1 | 1.000 |
| B | 1-18, 20 | 19 | 0.947 |
| C | 1, 3 | 2 | 0.500 |
| D | 1, 3, 6, 10 | 4 | 0.500 |
| E | 7, 8, 9, 10, 13, 14, 15, 16 | 8 | 0.250 |
| F | 11-20 | 10 | 0.000 |
| G | 2, 3 | 2 | 0.500 |
| H | 3, 4 | 2 | 0.000 |

## (c) Modified nDCG'

题目定义 linear discount：rank 1 weight = 1，rank 10 weight = 0.1，rank > 10 weight = 0。可定义：

$$
DCG'@10=\sum_{i=1}^{10} rel_i\left(1-0.1(i-1)\right)
$$

Query D relevant ranks: 1, 3, 6, 10。

$$
DCG'@10=1+0.8+0.5+0.1=2.4
$$

Ideal ranking 把 4 个 relevant 放在 ranks 1-4：

$$
IDCG'@10=1+0.9+0.8+0.7=3.4
$$

$$
nDCG'@10=\frac{2.4}{3.4}=0.706
$$

## 什么时候 modified formula 更有用？

适合用户几乎只看 top 10，且 rank 10 之后价值近似为 0 的场景，如 web search first page、mobile search、answer recommendation。它比 log discount 更 harshly penalise late relevant documents。

\newpage

# Question 5: Expected Mutual Information

## Original question screenshot

\includegraphics[height=0.78\textheight,keepaspectratio]{original-question-screenshots/ttds-2023-page-4.png}

## 原题要点

计算 word `broccoli` 与 class `food` 的 expected mutual information。

从表中：

- Total $N=1024+1216+564+702+1020+605=5131$
- $N_{11}=122$：broccoli 且 food
- $N_{1.}=43+45+122+90+5+3=308$：broccoli 总出现 doc 数
- $N_{.1}=564$：food doc 数
- $N_{10}=308-122=186$
- $N_{01}=564-122=442$
- $N_{00}=5131-122-186-442=4381$

## 计算

公式：

$$
I(U;C)=
\frac{N_{11}}{N}\log_2\frac{NN_{11}}{N_{1.}N_{.1}}+
\frac{N_{01}}{N}\log_2\frac{NN_{01}}{N_{0.}N_{.1}}+
\frac{N_{10}}{N}\log_2\frac{NN_{10}}{N_{1.}N_{.0}}+
\frac{N_{00}}{N}\log_2\frac{NN_{00}}{N_{0.}N_{.0}}
$$

代入：

| Cell | Count | Contribution |
| --- | ---: | ---: |
| $N_{11}$ | 122 | 0.043974 |
| $N_{01}$ | 442 | -0.022599 |
| $N_{10}$ | 186 | -0.020287 |
| $N_{00}$ | 4381 | 0.025037 |

$$
I(U;C)=0.026125\approx 0.026
$$

## 常见坑

MI 不是只看 $N_{11}$。一个 word 即使在某 class 里出现不少，如果它在其他 classes 也常出现，MI 不一定高。

\newpage

# Question 6: More Short Answers

## Original question screenshot

\includegraphics[height=0.78\textheight,keepaspectratio]{original-question-screenshots/ttds-2023-page-4.png}

## (a) MI under independence

如果 term 和 class 完全 independent，则 $P(U,C)=P(U)P(C)$，log ratio 为 0，所以：

$$
I(U;C)=0
$$

## (b) pLSI limitation vs LDA

`pLSI` 没有完整 generative model for new documents，参数数量随 documents 增长，容易 overfit。`LDA` 加入 Dirichlet priors，把 document-topic distribution 作为 random variable，因此更适合 generalise to unseen documents。

## (c) Gibbs sampling in LDA

对某个 word token，先 remove 当前 topic assignment，再根据 conditional posterior 重新采样 topic：

$$
P(z_i=k|\cdots)\propto
(n_{d,k}^{-i}+\alpha)\frac{n_{k,w}^{-i}+\beta}{n_k^{-i}+V\beta}
$$

直觉：topic 要同时适合该 document，也要适合该 word。

## (d) Feature scaling

`Feature scaling` 把 features 放到相近 range，如 standardisation 或 min-max scaling。它避免大尺度 features 在 SVM/logistic regression/kNN 等模型里支配学习。

## (e) Why train/dev/test

- `training set`: fit model parameters
- `development/validation set`: tune hyperparameters、select features、choose model
- `test set`: final unbiased evaluation

如果用 test set 调参，test performance 会被污染，失去真实泛化估计。

\newpage

# Question 7: Bias in Sentiment Classifier

## Original question screenshot

\includegraphics[height=0.78\textheight,keepaspectratio]{original-question-screenshots/ttds-2023-page-4.png}

\includegraphics[height=0.78\textheight,keepaspectratio]{original-question-screenshots/ttds-2023-page-5.png}

## 原题要点

Google sentiment system 对 identity terms 给出不同 sentiment，即使 human labels 应该 neutral。问原因和 remedy。

## 为什么会发生？

可能原因：

- `Training data bias`: identity terms 在训练语料中常与 negative/positive contexts 共现，模型学到了 spurious correlation。
- `Representation bias`: word embeddings 或 features 把 demographic terms 与 sentiment-bearing words 联系起来。
- `Label bias`: annotators 或 historical data 本身带有社会偏见。
- `Domain mismatch`: model 在普通 sentiment data 上训练，但被用于 identity statements，语义不同。
- `Bag-of-words limitation`: 如果模型主要看 words 而不理解 sentence pragmatics，`I am X` 可能被某个 identity token 主导。

## Remedy 1: Counterfactual data augmentation

构造 pairs，如 `I am Christian` / `I am Sikh` / `I am gay`，保持 label 不变，只替换 identity term。

优点：直接针对 fairness bug，能减少 spurious identity-sentiment association。  
缺点：覆盖不完所有 identities 和 contexts，也可能过度平滑真实语义差异。

## Remedy 2: Bias-aware evaluation and regularisation

建立 fairness benchmark，测 `counterfactual invariance`；训练时加入 penalty，要求 identity swap 后 sentiment score 不应大幅变化。

优点：不仅修一个词，而是形成 evaluation + training discipline。  
缺点：需要额外 labels/benchmarks，且 fairness metrics 可能和 raw accuracy trade off。

## 可补充方案

人工审核高风险 outputs、domain-specific retraining、remove/protect sensitive attributes。但简单删除 identity terms 会损失语义，也不能解决 proxy bias。

\newpage

# Question 8: Misinformation Classifier Evaluation

## Original question screenshot

\includegraphics[height=0.78\textheight,keepaspectratio]{original-question-screenshots/ttds-2023-page-5.png}

## 原题要点

目标是 flag 可能包含 misinformation 的 posts，后续给 human moderators。可以接受 false positives，但非常不希望漏掉 misinformation。

## (a) 推荐 evaluation metric

应优先用 `Recall` for misinformation class：

$$
Recall=\frac{TP}{TP+FN}
$$

因为任务的核心是找出所有可能 misinformation，false negative 代价高。也可以报告 `F_\beta` with $\beta>1$，如 $F_2$，让 recall 权重大于 precision。实际系统还应看 precision，避免 moderators 被过多 false positives 淹没。

## (b) Likely works / fails examples

Likely works:

- “Paris is the capital of Japan.”

理由：结构接近训练中的 false factual claim，实体关系简单，capital-country mismatch 明显。

Likely fails:

- “They are doing a really excellent job and everyone knows the numbers are fake.”

理由：需要 context、speaker intent、world knowledge 或 discourse understanding。Bag-of-words classifier 可能被 positive words 或 vague wording 干扰。

## 考试模板

For high-recall moderation systems, optimise recall or $F_\beta$ with $\beta>1$ because missing harmful/misinformation content is worse than sending extra items to human review. Still monitor precision for moderator workload.

# Coverage Checklist

| Question | Covered TTDS topics |
| --- | --- |
| Q1 | Cranfield paradigm, smoothing, PageRank, relevance feedback, shingles, crawler politeness, implicit feedback, web user needs, query similarity, URL frontier |
| Q2 | PageRank, random surfer, teleportation/damping, iterative graph computation |
| Q3 | Web search, LLMs, RAG-style retrieval plus generation, freshness, attribution |
| Q4 | Precision, recall, F1, R-precision, DCG/nDCG, rank discounting |
| Q5 | Expected mutual information, contingency table, feature selection |
| Q6 | MI independence, pLSI vs LDA, Gibbs sampling, feature scaling, train/dev/test |
| Q7 | Text classification bias, sentiment analysis, counterfactual evaluation, mitigation |
| Q8 | Classification evaluation, recall, precision, $F_\beta$, moderation use case |
