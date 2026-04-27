# L08 Parameter-efficient Finetuning 考试复习 QA

这份是一套面向考试的总复习问答，不按页码整理，而是按整份讲义的知识结构整理。重点覆盖 full fine-tuning 的成本、ICL 的缺点、PEFT 的动机、三类 PEFT、parameter composition、LoRA、input composition、prompt tuning、function composition 和 adapters。

## 高频核心题

### Q1. 什么是 fine-tuning？

A：Fine-tuning 是在预训练模型基础上继续训练，使模型适应某个任务、领域或行为目标。

### Q2. 为什么模型越大，full fine-tuning 越困难？

A：因为需要更新和存储所有参数、梯度和优化器状态，显存、计算和存储成本都很高。

### Q3. 为什么讲义说 standard transfer learning formula breaks down？

A：在超大模型时代，像过去那样为每个任务完整 fine-tune 一个模型变得成本过高，不再实用。

### Q4. 什么是 Parameter-Efficient Fine-Tuning？

A：PEFT 是只训练少量新增或选定参数，同时冻结大部分预训练模型参数的 fine-tuning 方法。

### Q5. PEFT 的核心目标是什么？

A：用更少可训练参数和更低训练成本，尽量接近 full fine-tuning 的下游性能。

### Q6. 为什么 training 的 memory constraint 比 inference 更严重？

A：训练不仅要存模型参数，还要存 gradients、activations 和 optimizer states；推理通常不需要这些。

### Q7. PEFT 为什么在 LLM 时代重要？

A：LLM 参数量巨大，full fine-tuning 昂贵且难部署；PEFT 让大模型适配多个任务更现实。

### Q8. PEFT 是不是全新思想？

A：不是。早期计算机视觉中更新最后一层、NLP 中冻结 embeddings 或部分参数都体现了类似思想。

### Q9. 为什么过去 full fine-tuning 常常效果更好，现在又回到 PEFT？

A：因为大模型过度参数化，少量参数调整也能达到强效果，而 full fine-tuning 的成本在 LLM 上太高。

### Q10. PEFT 常见的三大类是什么？

A：Parameter composition、input composition 和 function composition。

## In-context Learning 与 PEFT 动机

### Q11. In-context learning 是什么？

A：ICL 是在 prompt 中提供任务说明或示例，让模型在不更新参数的情况下完成任务。

### Q12. ICL 的主要优点是什么？

A：不需要训练或更新模型参数，使用方便，适合快速测试和少样本任务。

### Q13. ICL 的第一个缺点是什么？

A：效率低，因为每次预测都要重新处理完整 prompt 和 examples。

### Q14. ICL 的性能问题是什么？

A：Prompting 通常不如 fine-tuning 稳定或高效，尤其在复杂任务上可能性能较差。

### Q15. ICL 为什么对 prompt wording 敏感？

A：模型的输出会受提示词措辞、格式和示例排列影响，导致结果不稳定。

### Q16. ICL 为什么对 examples order 敏感？

A：少样本示例顺序可能改变模型注意到的模式，从而改变预测。

### Q17. random labels 也能给 ICL 带来非平凡效果说明什么？

A：说明模型从 prompt 中学到的内容并不总是简单的标签映射，ICL 机制仍不完全清楚。

### Q18. PEFT 和 ICL 的关系是什么？

A：二者都是避免 full fine-tuning 的适配方式；ICL 不训练参数，PEFT 训练少量参数。

### Q19. PEFT 相比 ICL 的一个优势是什么？

A：PEFT 把适配信息存入少量参数，不需要每次都把长示例放进 prompt。

### Q20. PEFT 相比 ICL 的一个代价是什么？

A：PEFT 仍然需要训练过程和任务数据，而 ICL 可以直接通过 prompt 使用。

## PEFT 形式化框架

### Q21. 讲义中如何把神经网络分解？

A：把网络 `f:X->Y` 分解成多个函数组合 `f_theta1 ⊙ ... ⊙ f_thetan`。

### Q22. PEFT 中 `theta` 和 `phi` 分别表示什么？

A：`theta` 是原预训练模型参数，通常冻结；`phi` 是新增或可训练的模块参数。

### Q23. PEFT 的通常训练方式是什么？

A：固定原模型参数 `theta`，只更新少量模块参数 `phi`。

### Q24. parameter composition 的形式是什么？

A：`g_i(x)=f_{theta_i ⊕ phi}(x)`，即通过某种方式修改或组合原模型参数。

### Q25. input composition 的形式是什么？

A：`g_i(x)=f_{theta_i}([x, phi])`，即把可学习参数作为输入的一部分拼接进去。

### Q26. function composition 的形式是什么？

A：`g_i(x)=f_{theta_i} ⊙ f_phi(x)`，即给原模型函数组合一个新的可训练函数。

### Q27. 这三类 PEFT 的核心区别是什么？

A：Parameter composition 改权重，input composition 改输入，function composition 加新函数模块。

### Q28. 为什么这三类方法通常冻结原模型？

A：冻结原模型能大幅降低训练成本，并让多个任务共享同一个 base model。

## Parameter Composition

### Q29. parameter composition 是什么？

A：它通过修改、选择或低秩更新原模型参数来适配新任务。

### Q30. parameter composition 包括哪些子类？

A：包括 sparse subnetworks、structured composition 和 low-rank composition。

### Q31. sparse subnetworks 的核心思想是什么？

A：只激活或训练模型中的一部分参数，用稀疏性降低可训练参数量。

### Q32. pruning 和 sparse subnetworks 有什么关系？

A：Pruning 是常见稀疏方法，通过移除或屏蔽部分网络连接形成更小子网络。

### Q33. structured composition 是什么？

A：预先定义一组参数，例如某层、某些层或某类组件，只更新这些参数。

### Q34. BitFit 是什么？

A：BitFit 是一种只更新 bias vectors 的 PEFT 方法。

### Q35. structured composition 的优点是什么？

A：简单、参数量少、容易实现，能利用已有模型结构。

### Q36. structured composition 的局限是什么？

A：预定义哪些参数可训练可能不够灵活，性能取决于选择的结构是否合适。

### Q37. low-rank composition 的核心思想是什么？

A：假设有效参数更新位于低维空间，用低秩结构表示更新，从而减少训练参数。

### Q38. 为什么 low-dimensional subspace 可能有效？

A：大模型过度参数化，许多任务适配可能不需要完整参数空间，只需低维方向。

### Q39. 讲义中的低秩 fine-tuning 形式是什么？

A：`g = f_{theta + P phi}`，其中 `P` 把低维参数映射回完整参数空间。

### Q40. `P in R^{Dxd}` 中 `D` 和 `d` 表示什么？

A：`D` 是完整模型参数维度，`d` 是降低后的低维空间大小。

## LoRA

### Q41. LoRA 是什么？

A：LoRA 是 Low-Rank Adaptation，通过学习低秩矩阵来表示权重更新。

### Q42. LoRA 的核心公式是什么？

A：`h=[W0 + ΔW]x = [W0 + BA]x`。

### Q43. LoRA 中 `W0` 表示什么？

A：`W0` 是冻结的预训练权重。

### Q44. LoRA 中 `ΔW` 表示什么？

A：`ΔW` 是任务适配时学习到的权重增量。

### Q45. LoRA 中 `B` 和 `A` 的作用是什么？

A：`B` 和 `A` 是两个低秩矩阵，它们的乘积 `BA` 近似完整权重更新 `ΔW`。

### Q46. LoRA 为什么 parameter-efficient？

A：因为只训练低秩矩阵 `A` 和 `B`，参数量远小于完整权重矩阵。

### Q47. LoRA 在讲义 notation 中如何表示？

A：`g_i=f_{theta_i + B_i A_i}`，对选定函数集合 `G` 中的层应用低秩更新。

### Q48. LoRA 通常应用在 Transformer 的哪些权重上？

A：常应用在 self-attention 的 `W_Q`、`W_K`、`W_V`，有时也应用在其他投影矩阵上。

### Q49. 在 attention 中 LoRA 如何更新 `W_Q`？

A：`W_Q` 被替换为 `W_Q + ΔW_Q = W_Q + B_Q A_Q`。

### Q50. LoRA 的 rank `r` 控制什么？

A：控制低秩更新的容量和参数量；`r` 越大，表达能力和成本越高。

### Q51. 为什么 LoRA 初始化时常让初始模型输出不变？

A：为了从原预训练模型的行为开始训练，避免一开始破坏模型，同时让梯度能更新低秩参数。

### Q52. 讲义中为什么 `B=0`、`A` 随机初始化？

A：这样初始 `BA=0`，模型初始等于原模型；但训练时梯度仍能推动参数变化。

### Q53. LoRA 的推理效率为什么好？

A：低秩更新可以与原权重合并，推理时不必显著增加模型大小或计算路径。

### Q54. LoRA 的一个主要优势是什么？

A：参数效率高、性能强，常能接近 full fine-tuning。

### Q55. LoRA 的一个潜在限制是什么？

A：如果 rank 太低或应用层选择不当，表达能力可能不足。

## Input Composition 与 Prompt Tuning

### Q56. input composition 是什么？

A：它通过给模型输入拼接可学习向量或 prompt 来适配任务。

### Q57. input composition 的公式是什么？

A：`g_i(x)=f_{theta_i}([phi_i, x])`。

### Q58. standard prompting 如何看作 input composition？

A：离散文本 prompt 被 embedding layer 映射后，相当于在输入中加入提示向量。

### Q59. standard prompting 的主要问题是什么？

A：模型对 prompt wording 和 examples order 很敏感。

### Q60. prompt tuning 是什么？

A：Prompt tuning 是直接学习一段连续 prompt embeddings，并把它们 prepend 到输入前。

### Q61. prompt tuning 中可训练参数通常是什么？

A：通常是一组连续 prompt embeddings 组成的矩阵。

### Q62. prompt tuning 和 manual prompting 的区别是什么？

A：Manual prompting 使用人写的离散文本；prompt tuning 学习连续向量，不一定对应可读文字。

### Q63. prompt tuning 为什么参数效率很高？

A：因为只训练输入层的一小组 prompt embeddings。

### Q64. prompt tuning 的主要局限是什么？

A：适配容量有限，尤其在小模型或困难任务上表现较差。

### Q65. 为什么 prompt tuning 在大模型上更有效？

A：大模型本身能力强，少量输入向量更容易引导已有能力；小模型容量不足时 prompt 很难弥补。

### Q66. prompt tuning 对 inference efficiency 有什么影响？

A：它会扩展输入长度，增加上下文窗口占用和推理成本。

### Q67. multi-layer prompt tuning 是什么？

A：不是只在输入层学习 prompt，而是在模型每一层学习可训练 prompt 参数。

### Q68. multi-layer prompt tuning 如何用于 Transformer？

A：通常把连续 prompt 拼接到 self-attention 的 keys 和 values 中。

### Q69. multi-layer prompt tuning 相比普通 prompt tuning 的优势是什么？

A：它增加适配容量，让任务信息能影响模型多个层级。

### Q70. multi-layer prompt tuning 的代价是什么？

A：会增加更多参数和计算，同时仍可能影响上下文或 attention 计算效率。

## Function Composition 与 Adapters

### Q71. function composition 是什么？

A：它在预训练模型函数中加入新的任务特定函数模块。

### Q72. function composition 的公式是什么？

A：`g_i(x)=f_{theta_i} ⊙ f_phi(x)`。

### Q73. adapters 是什么？

A：Adapters 是插入预训练模型中的小型可训练模块，用来适配新任务。

### Q74. Transformer 中典型 adapter 由什么组成？

A：通常包括 down-projection `W_D`、activation `sigma` 和 up-projection `W_U`。

### Q75. adapter 的公式是什么？

A：`f_phi_i(x)=W_U[σ(W_D x)]`。

### Q76. adapter 中 down-projection 的作用是什么？

A：把 hidden state 投影到较低维 bottleneck，减少可训练参数。

### Q77. adapter 中 up-projection 的作用是什么？

A：把低维表示投影回原 hidden size，使其能与主模型表示结合。

### Q78. adapter 通常放在哪里？

A：通常放在 multi-head attention 后和/或 feed-forward layer 后。

### Q79. adapters 为什么属于 function composition？

A：因为它们不是直接修改原权重，而是在原模型函数旁边加入新函数模块。

### Q80. adapters 的参数量由什么决定？

A：主要由 hidden size 和 adapter bottleneck size 决定。

### Q81. adapters 的一个优势是什么？

A：训练效率较好，不需要计算冻结参数的梯度，且常有较强性能和稳定性。

### Q82. adapters 的一个代价是什么？

A：推理时增加额外函数操作，可能降低 inference efficiency。

### Q83. 讲义中 adapters 的 robustness 结果说明什么？

A：Adapters 在不同任务和学习率下可能比 full fine-tuning 更稳定。

### Q84. adapters 的 sample efficiency 指什么？

A：在较少训练样本下，adapters 仍可能取得较好表现。

### Q85. 为什么 adapters 可能 match or outperform standard fine-tuning？

A：它们限制可训练容量，可能减少过拟合，同时保留预训练模型主体知识。

## 三类 PEFT 比较

### Q86. parameter composition 的参数效率如何？

A：通常较高，例如 LoRA 可以只训练少量参数。

### Q87. parameter composition 的训练效率如何？

A：可能不如最理想情况，因为某些方法如 pruning 需要重新训练或迭代。

### Q88. parameter composition 的 inference efficiency 如何？

A：通常很好，LoRA 等方法可合并进原权重，不增加明显模型大小。

### Q89. parameter composition 的性能如何？

A：通常较强，LoRA 等方法可以达到很好的 downstream performance。

### Q90. input composition 的参数效率如何？

A：非常高，因为只添加少量 prompt 参数。

### Q91. input composition 的训练效率如何？

A：训练参数少，但会扩展输入上下文，整体效率不一定高。

### Q92. input composition 的 inference efficiency 如何？

A：较差，因为每次推理都要处理额外 prompt tokens。

### Q93. input composition 的性能如何？

A：通常依赖模型规模，大模型上更好，小模型和困难任务上较弱。

### Q94. function composition 的参数效率如何？

A：不一定最高，因为 adapters 参数量取决于 hidden size 和插入位置。

### Q95. function composition 的训练效率如何？

A：较好，因为不需要对冻结参数求梯度，只训练新增模块。

### Q96. function composition 的 inference efficiency 如何？

A：较差，因为新增函数会增加推理操作。

### Q97. function composition 的性能如何？

A：通常较强，adapters 可能匹配或超过 standard fine-tuning。

### Q98. 哪些 PEFT 方法最常用？

A：讲义总结中指出 LoRA 和 adapters 是实践中最常用的 PEFT 方法。

### Q99. 如果关注最小推理开销，优先考虑哪类方法？

A：通常优先考虑 parameter composition，尤其是可合并权重的 LoRA。

### Q100. 如果关注极少可训练参数，prompt tuning 有什么吸引力？

A：它只训练输入 prompt embeddings，参数量极少。

### Q101. 如果任务很难且模型较小，prompt tuning 可能有什么问题？

A：适配容量不足，表现可能明显差于 LoRA 或 adapters。

## 综合比较题

### Q102. PEFT 和 full fine-tuning 的区别是什么？

A：Full fine-tuning 更新全部模型参数；PEFT 冻结大部分原模型，只更新少量新增或选定参数。

### Q103. PEFT 和 in-context learning 的区别是什么？

A：ICL 不训练参数，只靠 prompt；PEFT 训练少量参数，把任务适配信息存入模型模块。

### Q104. LoRA 和 adapters 的区别是什么？

A：LoRA 是低秩权重更新，属于 parameter composition；adapters 是新增小型函数模块，属于 function composition。

### Q105. Prompt tuning 和 adapters 的区别是什么？

A：Prompt tuning 在输入侧添加可学习向量；adapters 在模型层内部添加可训练函数模块。

### Q106. LoRA 和 prompt tuning 的区别是什么？

A：LoRA 改变选定权重的低秩增量；prompt tuning 只学习输入前缀，不直接改模型权重。

### Q107. 为什么 LoRA 通常比 prompt tuning 更强？

A：LoRA 直接作用于 attention 权重，适配容量更大；prompt tuning 只通过输入引导模型，容量有限。

### Q108. 为什么 adapters 可能比 full fine-tuning 更稳定？

A：它们限制可训练参数并保留预训练模型主体，可能减少过拟合和训练不稳定。

### Q109. 为什么 PEFT 适合多任务部署？

A：多个任务可共享同一个 base model，只切换小型 LoRA/adapters/prompt 参数，存储和维护成本低。

### Q110. PEFT 是否总能替代 full fine-tuning？

A：不一定。某些任务需要更大模型改动或强领域迁移时，full fine-tuning 可能仍有优势。

## 公式与术语速记

### Q111. `f:X->Y` 表示什么？

A：表示一个从输入空间 `X` 到输出空间 `Y` 的神经网络。

### Q112. `theta_i` 表示什么？

A：表示原预训练模型中第 `i` 个函数或层的参数。

### Q113. `phi` 表示什么？

A：表示 PEFT 中新增或可训练的模块参数。

### Q114. `h=[W0+BA]x` 表示什么？

A：表示 LoRA 用低秩矩阵乘积 `BA` 作为原权重 `W0` 的增量。

### Q115. `W_D in R^{kxd}` 和 `W_U in R^{dxk}` 在 adapter 中表示什么？

A：`W_D` 是降维矩阵，`W_U` 是升维矩阵，二者形成 bottleneck adapter。

### Q116. bottleneck 在 adapter 中的作用是什么？

A：通过较小中间维度限制参数量和适配容量，提高参数效率。

### Q117. `G` 在 LoRA notation 中表示什么？

A：表示被选择应用 LoRA 更新的函数或层集合。

### Q118. `r` 在 LoRA 中表示什么？

A：表示低秩更新的 rank，控制参数量和表达能力。

## 考前作答模板

### Q119. 如果考“为什么需要 PEFT”，应该怎么答？

A：答 full fine-tuning 对 LLM 太贵，需要存梯度和优化器状态；PEFT 冻结大模型，只训练少量参数，降低成本并保持性能。

### Q120. 如果考“三类 PEFT”，应该怎么答？

A：按 parameter composition、input composition、function composition 分别定义：改权重、改输入、加新函数模块，并举 LoRA、prompt tuning、adapters。

### Q121. 如果考“解释 LoRA”，应该怎么答？

A：LoRA 冻结原权重 `W0`，学习低秩矩阵 `B` 和 `A`，用 `BA` 表示权重更新 `ΔW`，常用于 Transformer attention 权重。

### Q122. 如果考“为什么 LoRA 初始化保持原模型不变”，应该怎么答？

A：让初始 `BA=0`，模型一开始等于预训练模型，避免破坏已有能力，同时训练时梯度可以更新低秩参数。

### Q123. 如果考“prompt tuning 的优缺点”，应该怎么答？

A：优点是参数极少；缺点是增加上下文长度、适配容量有限，对小模型和难任务表现较差。

### Q124. 如果考“adapters 的结构”，应该怎么答？

A：说明 adapter 是插入 Transformer 的小模块，通常包含 down-projection、activation、up-projection，并放在 attention 或 FFN 后。

### Q125. 如果考“LoRA vs adapters”，应该怎么答？

A：LoRA 是低秩权重更新，可合并、推理高效；adapters 是新增模块，训练稳定但推理增加额外操作。

