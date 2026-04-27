# L16 Mechanistic Interpretability 考试复习 QA

这份是一套面向考试的总复习问答，不按页码整理，而是按整份讲义的知识结构整理。根据 `examguide.pdf`，Lecture 16 的 slide 29 到最后不考；本文件聚焦可考核心：interpretability 类型、mechanistic interpretability 的目标、linear algebra 基础、features/concepts、monosemanticity/polysemanticity、privileged basis、superposition、toy model、sparse autoencoders、circuits 和 universality。

## 高频核心题

### Q1. Interpretability 为什么重要？

A：现代神经网络是复杂黑箱，interpretability 帮助我们理解、调试、信任和约束模型行为。

### Q2. Mechanistic interpretability 是什么？

A：试图 reverse-engineer 模型内部计算机制，解释具体能力或行为由哪些 features、neurons、layers 和 circuits 实现。

### Q3. Mechanistic interpretability 与 behavioral evaluation 的区别是什么？

A：behavioral evaluation 看输入输出表现；mechanistic interpretability 看模型内部表示和计算过程。

### Q4. 为什么 mechanistic interpretability 对安全有意义？

A：如果能定位模型内部的欺骗、有害能力或事实表示，就可能更可靠地监控和干预模型。

### Q5. Interpretability 的几类方法有哪些？

A：behavioral、attributional、concept-based 和 mechanistic approaches。

### Q6. Behavioral interpretability 看什么？

A：把模型当黑箱，通过输入输出行为推断模型能力、偏差和 failure modes。

### Q7. Attributional methods 看什么？

A：估计输入特征、tokens 或 neurons 对输出的贡献，例如梯度或 saliency。

### Q8. Concept-based methods 看什么？

A：研究模型内部是否编码人类可理解概念，并用 probes 或 representation directions 检测。

### Q9. Mechanistic methods 的目标是什么？

A：找到可解释的内部组件和它们之间的因果连接，解释模型如何完成任务。

## Linear Algebra 基础

### Q10. 为什么 mechanistic interpretability 需要 linear algebra？

A：模型 activations、weights 和 features 都可看作高维向量和线性变换。

### Q11. Vector direction 在表示学习中意味着什么？

A：方向可表示某种特征或概念，向量在该方向上的投影大小表示特征强度。

### Q12. Vector length 表示什么？

A：表示向量 magnitude，可能影响 activation 强度或相似度。

### Q13. Dot product 的直觉是什么？

A：衡量两个向量方向是否对齐，也可表示一个向量在另一个方向上的投影程度。

### Q14. Dot product 大说明什么？

A：两个向量方向相似，或者一个表示在某个 feature direction 上有强投影。

### Q15. Span 是什么？

A：一组向量通过线性组合能表示的所有向量集合。

### Q16. Basis 是什么？

A：能唯一表示向量空间中任意向量的一组独立向量。

### Q17. 为什么 basis choice 很重要？

A：同一个向量空间在不同 basis 下坐标不同，有些 basis 更接近人类可解释 features。

## Features 与 Concepts

### Q18. Feature 在 mechanistic interpretability 中是什么？

A：模型内部表示中的一个方向或模式，会在特定输入、概念或行为上被激活。

### Q19. 如何用方向理解 feature activation？

A：把 activation 向量投影到 feature direction 上，投影越大表示该 feature 越强。

### Q20. Feature alignment 是什么？

A：检查某个 learned direction 是否与人类定义的概念或标签对齐。

### Q21. Linear probe 是什么？

A：在 frozen activations 上训练线性分类器，检测表示中是否包含某个可线性读出的信息。

### Q22. Probe 权重向量如何解释？

A：probe 的权重方向可看作分类边界的法向量，也可近似表示被检测概念的方向。

### Q23. Probe 能证明模型真的使用了某概念吗？

A：不能。Probe 只能说明信息可被读出，不一定说明模型在原始任务中因果使用它。

### Q24. Feature 和 concept 的区别是什么？

A：feature 更像模型内部的基本表示方向；concept 是人类理解的概念，可能由多个 features 共同实现。

### Q25. Irreducible feature 是什么？

A：不能进一步分解为更简单可解释子特征的基本表示单元。

### Q26. 为什么 features 不一定等同于 neurons？

A：features 可以分布在多个 neurons 上，一个 neuron 也可能同时参与多个 features。

## Monosemanticity 与 Polysemanticity

### Q27. Monosemantic neuron 是什么？

A：一个 neuron 主要对应一个清晰、可解释的 feature。

### Q28. Polysemantic neuron 是什么？

A：一个 neuron 对多个不相关 features 都会激活。

### Q29. 为什么 polysemanticity 是解释困难？

A：如果一个 neuron 同时编码多个含义，直接解释或干预它会影响多个行为。

### Q30. 为什么 monosemanticity 对安全有帮助？

A：如果能找到单一有害 feature，就更容易监控、抑制或编辑相关行为。

### Q31. Privileged basis 是什么？

A：模型内部存在某个自然坐标系，使坐标轴与真实 irreducible features 对齐。

### Q32. 如果存在 privileged basis，有什么好处？

A：features 可直接对应坐标轴或 neurons，解释和干预会更简单。

### Q33. 如果不存在 privileged basis，会怎样？

A：features 可能以任意方向分布在表示空间中，单个 neuron 解释会不可靠。

### Q34. Polysemanticity 可能来自哪些原因？

A：非 privileged basis、噪声冗余，以及 superposition。

### Q35. Superposition 是什么？

A：模型在低维 activation space 中用重叠方向编码多于维度数量的 features。

### Q36. Superposition 和 polysemanticity 的区别是什么？

A：superposition 是表示层面的多 feature 压缩；polysemanticity 是 neuron 层面表现为多个含义混在同一单元。

### Q37. 为什么模型会使用 superposition？

A：features 很多但激活稀疏，模型可以把不常同时出现的 features 叠放在相同维度中以节省容量。

### Q38. Sparsity 如何影响 superposition？

A：越稀疏的 features 越少同时激活，越适合共享维度；但过度稀疏也可能导致 dead features。

## Toy Model of Superposition

### Q39. Toy model 想说明什么？

A：即使在简单线性模型中，特征稀疏和容量限制也会自然导致 superposition。

### Q40. Toy model 中输入 `x` 表示什么？

A：一组 features 的激活值，通常假设其中很多 features 为 0，即 sparse。

### Q41. Toy model 中 `W` 的作用是什么？

A：把高维 feature vector 压缩到低维 hidden representation，再尝试重构输入。

### Q42. Toy model 的重构目标是什么？

A：用低维隐藏层尽可能重构原始 features，同时根据 feature importance 加权损失。

### Q43. Feature importance 在 toy model 中有什么作用？

A：更重要的 feature 错误代价更大，因此模型更倾向保留它们。

### Q44. 为什么重要 features 更可能被分配独立方向？

A：它们错误成本高，模型不愿让它们与其他 features 强烈干扰。

### Q45. 为什么低重要度或稀疏 features 更容易 superpose？

A：它们不常出现或错误代价低，共享方向带来的干扰成本较小。

### Q46. `W^T W` 的 off-diagonal 项可以表示什么？

A：不同 features 在隐藏表示中方向的相似或干扰程度。

### Q47. Column norm 可以表示什么？

A：某个 feature 在隐藏空间中被保留的强度或分布程度。

### Q48. Toy model 对真实 LLM 的启发是什么？

A：polysemantic neurons 可能不是偶然 bug，而是模型在容量限制下高效编码稀疏 features 的结果。

## Sparse Autoencoders

### Q49. Sparse autoencoder 在 interpretability 中用于做什么？

A：从模型 activations 中学习一组稀疏、可解释的 feature directions。

### Q50. SAE 的基本结构是什么？

A：encoder 把 activation 映射为稀疏 feature coefficients，decoder 用 feature dictionary 重构 activation。

### Q51. SAE 中 `c = ReLU(Mx + b)` 表示什么？

A：把原始 activation `x` 编码成非负稀疏 feature activations `c`。

### Q52. SAE 中 `xhat = M^T c` 表示什么？

A：用 learned features 的加权和重构原始 activation。

### Q53. SAE 的 reconstruction loss 作用是什么？

A：要求 learned features 能保留原 activation 中的重要信息。

### Q54. SAE 的 L1 penalty 作用是什么？

A：鼓励 `c` 稀疏，让每个输入只激活少量 features，从而更可解释。

### Q55. 为什么 SAE 可以帮助“解开” superposition？

A：它允许用比原 neurons 更多的 learned features 表示 activation，并通过 sparsity 找到更接近真实 features 的方向。

### Q56. SAE learned feature 是否一定真实可解释？

A：不一定。它需要人工或自动分析验证，且可能学到噪声、数据集 artifact 或不稳定方向。

### Q57. SAE 相比直接看 neurons 的优势是什么？

A：它不假设 neuron 就是 feature，而是在 activation space 中寻找更合适的 feature basis。

### Q58. SAE 的一个关键 tradeoff 是什么？

A：重构质量、稀疏性和可解释性之间需要平衡。

## Circuits 与 Universality

### Q59. Circuit 在 mechanistic interpretability 中是什么？

A：一组相互连接的 features、neurons、attention heads 或 layers，共同实现某个具体计算。

### Q60. 为什么只找单个 feature 不够？

A：模型行为通常由多个组件交互产生，需要理解信息如何在组件间流动。

### Q61. Motif 是什么？

A：在不同任务、位置或模型中重复出现的局部计算结构。

### Q62. 为什么 motifs 重要？

A：它们可能揭示神经网络反复使用的通用算法片段。

### Q63. Universality hypothesis 是什么？

A：不同模型可能学到相似的内部特征、circuits 或算法结构。

### Q64. Weak universality 是什么？

A：不同模型可能学到相似功能但实现细节不完全相同。

### Q65. Strong universality 是什么？

A：不同模型甚至可能学到高度相似的 features 和 circuits。

### Q66. Universality 为什么对 interpretability 有帮助？

A：如果 circuits 可复用，研究一个模型的发现可能迁移到其他模型。

### Q67. Universality 为什么不能直接假设？

A：架构、数据、规模和训练目标不同都可能导致内部机制不同。

### Q68. Mechanistic interpretability 的核心难点是什么？

A：模型维度巨大、features 可能 superposed、行为分布复杂，并且相关性不等于因果性。

### Q69. 为什么 causal evidence 很重要？

A：只有干预 feature 或 circuit 并观察行为变化，才能更有力说明它确实参与某计算。

### Q70. 讲义中可考范围到哪里为止？

A：到 circuits/universality 相关核心概念为止；slide 29 之后的具体方法细节不纳入本复习集。

## 概念对比题

### Q71. Behavioral、attributional、mechanistic 的区别是什么？

A：behavioral 看输入输出，attributional 估计贡献，mechanistic 试图解释内部组件如何因果实现行为。

### Q72. Feature、neuron、concept 的区别是什么？

A：feature 是内部表示方向，neuron 是坐标单元，concept 是人类语义概念；三者可能相关但不等同。

### Q73. Probe 和 SAE 的区别是什么？

A：probe 检测信息是否可线性读出；SAE 学习稀疏 feature dictionary 来分解 activation。

### Q74. Superposition 和 privileged basis 的关系是什么？

A：如果 features 超过维度且使用重叠方向，就难以在原 neuron basis 中得到清晰单义表示；SAE 试图寻找更好的 feature basis。

### Q75. Monosemanticity 与 interpretability 的关系是什么？

A：monosemantic units 更容易命名、监控和干预，因此被视为 interpretability 的理想目标之一。

### Q76. Polysemanticity 为什么可能是效率策略？

A：模型利用稀疏性把多个不常共现的 features 放在同一神经元或方向上，以节省容量。

### Q77. SAE 中 sparsity 为什么关键？

A：没有 sparsity，autoencoder 可学到难解释的密集重构；sparsity 迫使表示分解成少量 active features。

### Q78. Circuits 与 features 的关系是什么？

A：features 是局部表示单元，circuits 是多个 features/组件之间的连接和计算流程。

## 考前作答模板

### Q79. 如果考“mechanistic interpretability 是什么”，应该怎么答？

A：说明它 reverse-engineer 模型内部机制，寻找 features、neurons、layers 和 circuits 如何因果产生行为。

### Q80. 如果考“superposition”，应该怎么答？

A：说明模型用低维空间中重叠方向编码更多稀疏 features，导致 polysemantic neurons，但提高容量效率。

### Q81. 如果考“SAE 为什么有用”，应该怎么答？

A：说明 SAE 从 activations 中学习稀疏 feature dictionary，用 L1 penalty 鼓励少量 features 激活，帮助解开 superposition。

### Q82. 如果考“probe 的局限”，应该怎么答？

A：probe 能说明信息可被读出，但不能证明模型在任务中实际使用该信息。

### Q83. 如果考“circuits/universality”，应该怎么答？

A：circuits 是实现某计算的组件网络；universality 假设不同模型可能学到相似 features 或 circuits，但需实证验证。
