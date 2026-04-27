# L10 Knowledge Distillation 考试复习 QA

这份是一套面向考试的总复习问答，不按页码整理，而是按整份讲义的知识结构整理。根据 `examguide.pdf`，Lecture 10 从 slide 27 到结尾不考，因此本文件聚焦 slide 1-26 的核心内容：proprietary/open-source LLM 对比、knowledge distillation 的动机、KD recipe、六种获取知识的方法，以及四种将知识转移给 student LLM 的方法。

## 高频核心题

### Q1. 什么是 Knowledge Distillation？

A：Knowledge Distillation 是用强大的 teacher model 产生知识或监督信号，再训练较小或开源的 student model 学习这些能力。

### Q2. 在 LLM 语境中，teacher 和 student 分别是什么？

A：Teacher 通常是性能更强的大模型或专有模型；student 通常是较小、成本更低或开源的模型。

### Q3. LLM distillation 的主要目标是什么？

A：把强模型的能力转移到更便宜、更可控、更易部署的模型上。

### Q4. 为什么要从 proprietary LLM 蒸馏到 open-source LLM？

A：为了缩小开源模型和昂贵专有模型的性能差距，同时降低成本、提高可控性和隐私性。

### Q5. Knowledge Distillation 的核心 trick 是什么？

A：从更大的 teacher LLM 中抽取数据或反馈，再用标准训练方法进一步训练 student LLM。

### Q6. KD 在 LLM 中有哪三个角色？

A：增强模型能力、传统压缩以提升效率、通过自生成知识实现自我改进。

### Q7. KD 为什么能提升 accessibility？

A：它让较小机构或本地设备也能使用接近强模型能力的 student model，而不总是调用昂贵 API。

### Q8. KD 和普通 supervised fine-tuning 有什么关系？

A：KD 常常先由 teacher 生成数据或标签，再用这些数据对 student 做 supervised fine-tuning 或其他训练。

### Q9. KD 是否一定需要访问 teacher 内部参数？

A：不一定。很多方法只需要 black-box API 输出；但 feature-based 或 divergence-based 方法通常需要 white-box access。

### Q10. 为什么 KD 在 LLM 时代特别重要？

A：强模型昂贵且常是黑盒，开源或小模型可控但能力弱；KD 提供了连接二者的实用方法。

## Proprietary 与 Open-source LLM

### Q11. proprietary LLM 的优点是什么？

A：通常鲁棒、能回答多种 query、持续更新维护、性能较高。

### Q12. proprietary LLM 的缺点是什么？

A：常作为黑盒 API 提供，无法直接检查 activations；有隐私风险；按 token 收费成本高。

### Q13. 为什么 black-box API 对研究和调试不方便？

A：因为无法访问内部表示、梯度或中间状态，难以深入分析模型行为。

### Q14. proprietary LLM 的隐私问题是什么？

A：用户数据需要发送到外部服务，可能不适合敏感数据或受监管场景。

### Q15. open-source LLM 的优点是什么？

A：可完全控制模型和表示、成本较低、可本地运行、隐私风险较小，训练数据来源有时更透明。

### Q16. open-source LLM 的缺点是什么？

A：性能通常低于顶级专有模型，fine-tuning 和 instruction coverage 可能更少，本地服务部署也更复杂。

### Q17. 为什么 open-source LLM 更适合隐私敏感场景？

A：因为可以本地运行，不需要把数据发给外部 API。

### Q18. 为什么 open-source LLM 可能更难服务化？

A：需要自己处理推理硬件、吞吐、延迟、部署和维护，而不是直接调用 API。

### Q19. KD 如何结合 proprietary 和 open-source 的优点？

A：用 proprietary model 生成高质量知识，再训练 open-source student，使其更便宜、可控、可本地部署。

### Q20. KD 是否能完全消除 teacher 和 student 的差距？

A：不一定。Student 的容量、数据质量和训练方法都会限制最终能力。

## KD Recipe

### Q21. LLM KD 的四个基本阶段是什么？

A：选择 teacher 的目标 skill/domain 和 template，准备 seed knowledge，生成 distillation knowledge，再用生成知识训练 student。

### Q22. template `I` 在 KD 中表示什么？

A：`I` 是给 teacher LLM 的特定 instruction 或 prompt template，用来引导生成目标知识。

### Q23. seed knowledge `S` 是什么？

A：`S` 是给 teacher 的初始输入，可以是 demonstrations、已有数据或 meta-knowledge。

### Q24. distillation knowledge 是什么？

A：Teacher 根据 template 和 seed knowledge 生成的可用于训练 student 的样本、标签、解释、反馈或特征。

### Q25. KD 中的 `Parse` 函数做什么？

A：把 teacher 的原始生成输出解析成结构化知识样本，例如 `(x,y)` 或带反馈的数据。

### Q26. 获取 KD 数据的公式是什么？

A：`D_I^kd = {Parse(o,s) | o ~ p_T(o | I ⊕ s), s in S}`。

### Q27. `p_T` 表示什么？

A：`p_T` 是 teacher LLM 的生成分布。

### Q28. `I ⊕ s` 表示什么？

A：表示把 instruction/template `I` 和 seed example `s` 组合成 teacher 的输入。

### Q29. student 训练目标的总体形式是什么？

A：`L = Σ_I L_I(D_I^kd; θ_S)`，即在不同 template 产生的数据上优化 student 参数。

### Q30. `θ_S` 表示什么？

A：表示 student model 的参数。

### Q31. 为什么可能有多个 templates `I`？

A：不同 template 可以覆盖不同任务形式、技能或领域，增强数据多样性。

### Q32. KD recipe 中最容易出问题的环节是什么？

A：数据质量控制。Teacher 生成错误、重复或偏置样本会直接影响 student。

## 获取知识的方法

### Q33. 讲义把获取知识的方法分成哪六类？

A：Labelling、Expansion、Data curation、Feature、Feedback 和 Self-knowledge。

### Q34. Approach 1: Labelling 是什么？

A：给定输入 `x`，让 teacher LLM 生成对应输出 `y`，形成标注数据。

### Q35. Labelling 的形式化定义是什么？

A：`D^(lab) = {(x,y) | x ~ X, y ~ p_T(y | I ⊕ c ⊕ x)}`。

### Q36. Labelling 中的 `c` 表示什么？

A：`c` 是 demonstrations，即若干输入输出示例 `{(x_i,y_i)}`。

### Q37. Labelling 的 seed knowledge 通常来自哪里？

A：可以来自已有 datasets，也可以来自人工或半自动收集的输入集合。

### Q38. Labelling 中为什么常加入 chain-of-thought？

A：CoT 可帮助 teacher 生成更高质量或更可解释的答案，也能让 student 学习推理过程。

### Q39. Labelling 的优点是什么？

A：输入来自已有数据，任务分布更可控，适合为无标签或弱标签数据补标签。

### Q40. Labelling 的局限是什么？

A：依赖已有输入集合，teacher 生成的标签可能错误或带偏见。

### Q41. Approach 2: Expansion 是什么？

A：只给 demonstrations，让 teacher 先生成新的输入 `x`，再为这些输入生成输出 `y`。

### Q42. Expansion 的形式化定义是什么？

A：`D^(exp) = {(x,y) | x ~ p_T(x | I ⊕ c), y ~ p_T(y | I ⊕ x)}`。

### Q43. Expansion 的优点是什么？

A：不需要大量已有数据，只用少量 examples 就能扩展出更多训练样本，也较少涉及真实隐私数据。

### Q44. Expansion 的主要问题是什么？

A：数据质量高度依赖 teacher，生成样本可能彼此相似，缺乏多样性。

### Q45. Approach 3: Data curation 是什么？

A：不依赖具体 examples，而是用 meta-information `m`，例如领域说明，让 teacher 从头合成数据。

### Q46. Data curation 的形式化定义是什么？

A：`D^(cur) = {(x,y) | x ~ p_T(x | I ⊕ m), y ~ p_T(y | I ⊕ x)}`。

### Q47. Data curation 的优点是什么？

A：可以从领域描述、主题或元信息直接合成数据，适合缺少现成样本的领域。

### Q48. UltraChat 如何体现 data curation？

A：通过给 LLM 主题并提示其生成 instructions 和 conversations 来构造数据。

### Q49. coding domain 中如何使用 data curation？

A：可以提示 LLM 生成 Python exercises、solutions 和 code snippets。

### Q50. Data curation 的风险是什么？

A：如果 meta-information 不充分或 teacher 有偏差，合成数据可能片面、错误或不真实。

### Q51. Approach 4: Feature 是什么？

A：用 teacher LLM 的内部表示或输出分布作为额外知识来训练 student。

### Q52. Feature 方法的形式化定义是什么？

A：`D^(feat) = {(x,y,phi_feat(x,y;θ_T)) | x ~ X, y ~ Y}`。

### Q53. Feature 方法中 `phi_feat` 表示什么？

A：表示从 teacher 中提取 feature knowledge 的操作，例如输出分布或内部表示。

### Q54. Feature 方法和前三种方法的主要区别是什么？

A：Feature 方法需要 teacher 的内部表示或分布，通常要求 white-box access。

### Q55. Feature 方法的优点是什么？

A：能提供比硬标签更细粒度的信息，让 student 学到 teacher 的不确定性和表示结构。

### Q56. Feature 方法的缺点是什么？

A：难以用于强 proprietary LLM，因为通常无法访问其内部表示。

### Q57. Approach 5: Feedback 是什么？

A：Student 先对输入生成回答，teacher 再对 student 输出进行评价、纠正或提供偏好反馈。

### Q58. Feedback 方法的形式化定义是什么？

A：`D^(fb) = {(x,y,phi_fb(x,y;θ_T)) | x ~ X, y ~ p_S(y|x)}`。

### Q59. Feedback 方法中 `p_S` 表示什么？

A：表示 student LLM 的生成分布。

### Q60. Feedback 方法为什么适合改进 student？

A：Teacher 可以专门关注 student 出错或表现不足的样本，提供有针对性的纠正。

### Q61. Feedback 方法如何用于 preference datasets？

A：Teacher 可以比较多个 student outputs，产生偏好数据，例如 `(x, y_w, y_l)`。

### Q62. Approach 6: Self-knowledge 是什么？

A：一个模型既当 student 又当 teacher，通过生成、过滤、增强或评价自己的输出来自我改进。

### Q63. Self-knowledge 的形式化定义是什么？

A：`D^(sk) = {(x,y,phi_sk(x,y)) | x ~ S, y ~ p_S(y | I ⊕ x)}`。

### Q64. Self-knowledge 中 `phi_sk` 可以是什么？

A：可以是对输出的过滤、奖励、增强、评价或改写。

### Q65. Self-Instruct 如何体现 self-knowledge？

A：它使用 GPT-3 expansion 生成数据，并迭代扩大 seed data 来进行自我增强。

### Q66. Self-knowledge 的风险是什么？

A：模型可能放大自身错误和偏差，导致数据多样性下降或质量退化。

## 将知识转移给 Student

### Q67. 拿到 distillation knowledge 后，还需要做什么？

A：需要选择训练目标和算法，把 teacher 产生的知识转移到 student LLM。

### Q68. 讲义列出的知识转移方法有哪些？

A：Supervised finetuning、divergence/similarity methods、reinforcement learning 和 rank optimisation。

### Q69. Approach 1: Supervised finetuning 是什么？

A：用 curated/distilled dataset 继续训练 student model，使其学习 teacher 生成的输入输出映射。

### Q70. SFT 为什么听起来简单但实际有“black art”？

A：效果受数据质量、学习率、训练轮数、格式、混合比例和过拟合控制等很多细节影响。

### Q71. SFT 在 KD 中的典型用法是什么？

A：把 teacher 生成的 instruction-response pairs 当作训练数据，fine-tune student。

### Q72. SFT 是否可用于 self-distillation？

A：可以。模型生成或筛选自己的数据后，也可以用 SFT 继续训练自身。

### Q73. Approach 2: Divergence 是什么？

A：让 student 的输出分布接近 teacher 的输出分布，例如最小化 `D(p_T(y|x), p_S(y|x))`。

### Q74. Divergence 方法需要什么条件？

A：通常需要 white-box teacher 或至少能获得 teacher 的完整输出分布。

### Q75. 为什么 divergence 比 hard-label SFT 信息更丰富？

A：它传递整个概率分布，而不只是单个正确答案。

### Q76. Divergence 方法中的 `D` 是什么？

A：表示某种 divergence function，例如 KL divergence，用来衡量两个分布差异。

### Q77. Similarity 方法是什么？

A：让 student 的 feature representations 与 teacher 的 feature representations 更相似。

### Q78. Similarity 方法的目标形式是什么？

A：最大化 `E[L_F(Φ_T(f_T(x,y)), Φ_S(f_S(x,y)))]`。

### Q79. Similarity 方法中 `f_T` 和 `f_S` 表示什么？

A：表示从 teacher 和 student 数据或模型中得到的 feature maps。

### Q80. `Φ_T` 和 `Φ_S` 的作用是什么？

A：把 teacher 和 student 的 feature maps 转换到相同形状或可比较空间。

### Q81. Similarity 方法更常见于哪类模型？

A：更常见于 encoder-based LMs，而不太常用于 decoder LLMs。

### Q82. Approach 3: Reinforcement learning 在 KD 中如何使用？

A：先用 teacher feedback 训练 reward model，再训练 student 生成高 reward 的输出，同时保持接近 reference model。

### Q83. RL 方法的第一阶段是什么？

A：用 teacher 生成的 feedback data 训练 reward model `r_phi`。

### Q84. RL 中典型 feedback data 是什么形式？

A：偏好数据 `(x, y_w, y_l)`，其中 `y_w` 比 `y_l` 更受偏好。

### Q85. RL 方法的第二阶段是什么？

A：训练 student 最大化 reward model 给出的奖励，同时控制不要偏离 reference/original model 太远。

### Q86. RL 方法为什么需要 reference model 约束？

A：为了防止 student 为追求 reward 过度偏离原有语言能力或产生 reward hacking。

### Q87. Approach 4: Ranking optimisation 是什么？

A：直接用偏好数据优化 student，使 preferred response 的相对概率高于 rejected response。

### Q88. DPO 属于哪类方法？

A：DPO 属于 ranking optimisation 方法。

### Q89. DPO 目标中的 `y_w` 和 `y_l` 表示什么？

A：`y_w` 是偏好或获胜回答，`y_l` 是较差或失败回答。

### Q90. DPO 中 `π_ref` 表示什么？

A：表示 reference policy，通常是原始或冻结模型，用于约束新模型不要偏离太远。

### Q91. DPO 中 `β` 的直观作用是什么？

A：控制偏好优化强度和相对 reference model 的偏离程度。

### Q92. Ranking optimisation 和 RL 的区别是什么？

A：RL 通常先训练 reward model 再优化 policy；DPO 等 ranking optimisation 直接从偏好数据优化模型。

### Q93. 为什么 DPO 在 LLM alignment 中常被使用？

A：它避免显式训练 reward model 和复杂 RL loop，直接利用 preference data 训练。

## 综合比较题

### Q94. Labelling 和 Expansion 的区别是什么？

A：Labelling 给定输入 `x` 让 teacher 生成标签 `y`；Expansion 连输入 `x` 也由 teacher 根据 demonstrations 生成。

### Q95. Expansion 和 Data curation 的区别是什么？

A：Expansion 依赖 demonstrations；Data curation 依赖 meta-information，例如主题或领域说明。

### Q96. Feature 和 Feedback 的区别是什么？

A：Feature 使用 teacher 内部表示或分布；Feedback 使用 teacher 对 student 输出的评价或纠正。

### Q97. Feedback 和 Self-knowledge 的区别是什么？

A：Feedback 通常 teacher 和 student 是不同模型；Self-knowledge 中同一个模型生成并评价或改进自己的输出。

### Q98. SFT 和 divergence distillation 的区别是什么？

A：SFT 学习 teacher 生成的离散答案；divergence 方法让 student 拟合 teacher 的完整输出分布。

### Q99. Divergence 和 similarity distillation 的区别是什么？

A：Divergence 对齐输出分布；similarity 对齐内部特征或表示。

### Q100. RL 和 DPO 的区别是什么？

A：RL 需要 reward model 并通过奖励训练 policy；DPO 直接用偏好对优化 preferred vs rejected response 的概率差。

### Q101. Black-box KD 和 white-box KD 的区别是什么？

A：Black-box KD 只使用 teacher 输出；white-box KD 使用 teacher 内部分布、hidden states 或 features。

### Q102. 哪些 KD 方法适合 proprietary teacher？

A：Labelling、Expansion、Data curation 和 Feedback 更适合，因为只需要 API 输出或评价。

### Q103. 哪些 KD 方法通常需要 open-source 或 white-box teacher？

A：Feature、divergence 和 similarity methods 通常需要访问内部表示或输出分布。

### Q104. KD 和 instruction tuning 的关系是什么？

A：Teacher 可以生成 instruction-response 数据，student 用这些数据进行 instruction tuning。

### Q105. KD 和 synthetic data 的关系是什么？

A：KD 通常通过 teacher 生成 synthetic training data 或 synthetic feedback 来训练 student。

### Q106. KD 的主要风险是什么？

A：Student 可能继承 teacher 的错误、偏见和 hallucination；生成数据也可能缺乏多样性或质量不稳定。

### Q107. 为什么 KD 不只是 compression？

A：在 LLM 中 KD 还用于能力增强、指令跟随、领域适配和自我改进，不只是把模型变小。

## 公式与术语速记

### Q108. `D_I^kd` 表示什么？

A：表示由 template `I` 引导 teacher 生成并解析出的 distillation dataset。

### Q109. `p_T(o | I ⊕ s)` 表示什么？

A：表示 teacher 在给定 template 和 seed knowledge 后生成输出 `o` 的概率分布。

### Q110. `p_S(y | x)` 表示什么？

A：表示 student model 在输入 `x` 下生成输出 `y` 的概率。

### Q111. `D^(lab)` 表示什么？

A：表示通过 teacher labelling 生成的输入输出数据集。

### Q112. `D^(exp)` 表示什么？

A：表示通过 teacher expansion 生成新输入和对应输出的数据集。

### Q113. `D^(cur)` 表示什么？

A：表示基于 meta-information 从头合成的数据集。

### Q114. `D^(feat)` 表示什么？

A：表示带有 teacher feature knowledge 的数据集。

### Q115. `D^(fb)` 表示什么？

A：表示 teacher 对 student outputs 提供反馈的数据集。

### Q116. `D^(sk)` 表示什么？

A：表示模型通过 self-knowledge 生成和增强的数据集。

### Q117. `y_w > y_l` 表示什么？

A：表示在偏好数据中，`y_w` 比 `y_l` 更好或更受偏好。

### Q118. `π_θ` 在 DPO 中表示什么？

A：表示正在优化的 student policy 或模型分布。

## 考前作答模板

### Q119. 如果考“什么是 KD”，应该怎么答？

A：说明 teacher/student 关系：用强模型生成数据、标签、反馈或分布，再训练 student 学习 teacher 能力，以降低成本或增强开源模型。

### Q120. 如果考“为什么需要 KD”，应该怎么答？

A：从 proprietary 模型强但贵、黑盒和隐私风险；open-source 模型可控但弱；KD 试图结合两者优势来回答。

### Q121. 如果考“KD recipe”，应该怎么答？

A：按四步答：确定目标技能和 template，准备 seed knowledge，让 teacher 生成 distillation knowledge，用生成数据训练 student。

### Q122. 如果考“获取知识的六种方法”，应该怎么答？

A：列出 labelling、expansion、data curation、feature、feedback、self-knowledge，并各用一句话解释。

### Q123. 如果考“black-box vs white-box distillation”，应该怎么答？

A：Black-box 只能用 teacher 输出或评价；white-box 能用内部分布和 hidden features，信息更丰富但强专有模型通常不可用。

### Q124. 如果考“把知识转移给 student 的方法”，应该怎么答？

A：列出 SFT、divergence/similarity、RL 和 ranking optimisation；说明 SFT 用答案，divergence/similarity 对齐分布或表示，RL/DPO 用偏好反馈。

### Q125. 如果考“DPO 和 RLHF 的区别”，应该怎么答？

A：RLHF 通常训练 reward model 再用 RL 优化；DPO 直接用 preference pairs 优化模型相对 reference policy 的偏好概率。

