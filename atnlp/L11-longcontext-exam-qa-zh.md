# L11 Long-Context 考试复习 QA

这份是一套面向考试的总复习问答，不按页码整理，而是按整份讲义的知识结构整理。重点覆盖长上下文建模动机、positional encodings、RoPE、context extension、efficient attention、long-context evaluation、Needle in the Haystack、LongBench 和 lost-in-the-middle。

## 高频核心题

### Q1. 什么是 long-context modelling？

A：Long-context modelling 是让模型处理、检索并推理非常长的输入或输出序列，长度可从数万到百万 tokens。

### Q2. 为什么需要 long-context modelling？

A：现实任务常包含长文档、代码库、法律材料、agent 历史和长推理过程；如果只能截断或摘要，可能丢失关键信息。

### Q3. 没有长上下文能力时，系统通常怎么处理长输入？

A：通常使用 retrieval、truncation 或 summarisation，但这些方法都可能遗漏关键内容。

### Q4. 长上下文建模的三个核心挑战是什么？

A：训练数据 mismatch、计算成本高、任务本身更难。

### Q5. training data mismatch 指什么？

A：大多数预训练数据是短文档，模型在训练中很少见到真实长文档，因此长上下文推理会遇到分布偏移。

### Q6. positional generalization failure 是什么？

A：模型在训练中只见过较短位置，推理时遇到更远位置可能无法正确泛化位置关系。

### Q7. 为什么 self-attention 是长上下文的计算瓶颈？

A：因为 full attention 需要计算所有 token pair 的注意力，复杂度随序列长度平方增长。

### Q8. “long context ≠ effective long-range reasoning” 是什么意思？

A：即使模型支持很长输入，也不代表它能有效找到、使用和推理中间的关键信息。

## Positional Encodings

### Q9. Transformer 为什么需要 positional encodings？

A：Transformer 并行处理 tokens，token embeddings 本身不包含顺序信息，所以需要位置编码表示 token 顺序。

### Q10. token embedding 和 positional encoding 如何结合？

A：通常相加：`z_pos = x_pos + PE_pos`。

### Q11. 为什么不能直接用整数位置编码？

A：整数位置值无界、数值大小会支配 embedding、训练外位置无法泛化，且相邻位置差异不具备良好缩放性质。

### Q12. sinusoidal positional encoding 的核心思想是什么？

A：使用不同频率的 sine 和 cosine 函数为每个位置生成固定向量。

### Q13. sinusoidal PE 中低频维度表示什么？

A：低频维度变化慢，更适合表示全局位置。

### Q14. sinusoidal PE 中高频维度表示什么？

A：高频维度变化快，更适合表示局部位置。

### Q15. sinusoidal PE 的优点有哪些？

A：确定性、无需学习、每个位置有独特 fingerprint，并且点积中包含相对距离信息。

### Q16. sinusoidal PE 是否总能很好外推到更长序列？

A：不一定。讲义指出它在超过训练长度时外推效果并不好。

### Q17. positional encoding design 的核心问题是什么？

A：如何让模型既能表示位置，又能在更长序列上稳定泛化。

## RoPE 与 Context Extension

### Q18. RoPE 是什么？

A：RoPE 是 Rotary Position Embedding，通过旋转 query/key 向量来编码位置。

### Q19. RoPE 和 sinusoidal addition 的区别是什么？

A：sinusoidal PE 把位置加到向量上；RoPE 用位置旋转向量，使相对位置以角度差形式进入 attention。

### Q20. RoPE 为什么适合表示相对距离？

A：两个位置的旋转角度差会自然反映相对距离，attention 能直接感知相对位置。

### Q21. RoPE 为什么只作用在 Q 和 K 上，而不作用在 V 上？

A：位置主要影响 attention score 的计算，Q/K 决定注意力关系；V 是被聚合的内容，不需要同样旋转。

### Q22. RoPE 的旋转变换有什么数学性质？

A：它是正交变换，保持向量长度，因此不会扭曲内容 magnitude。

### Q23. RoPE 的优点是什么？

A：稳定、方向敏感、保留向量长度，并内建相对位置信息。

### Q24. RoPE 为什么仍然难以直接外推到更长上下文？

A：高频旋转会在很远位置快速 wrap around，使相对距离变得噪声化。

### Q25. Position Interpolation 的核心思想是什么？

A：把更长上下文的位置索引缩放回预训练长度范围内，让模型在熟悉的位置区间内运行。

### Q26. Position Interpolation 的例子是什么？

A：如果原范围到 2048，要扩展到 4096，可以把位置 `m` 映射成 `m/2`。

### Q27. Position Interpolation 的问题是什么？

A：线性压缩位置会让相邻 token 更难区分。

### Q28. NTK Scaling 的核心思想是什么？

A：保留位置索引，但调整 RoPE 旋转频率，使长上下文中 attention 行为更稳定。

### Q29. Position Interpolation 和 NTK Scaling 的区别是什么？

A：Position Interpolation 压缩位置索引；NTK Scaling 调整旋转频率。

### Q30. YaRN 是什么？

A：YaRN 是一种 RoPE context extension 方法，对不同波长维度采用分段位置插值。

### Q31. YaRN 为什么是“分段”的？

A：短波长维度不插值，中间波长部分插值，长波长维度做位置插值，从而兼顾局部和全局位置。

### Q32. 为什么长上下文训练昂贵？

A：长序列导致 attention、activation 存储和训练数据成本大幅增加。

## Efficient Attention

### Q33. Transformer attention 的基本公式是什么？

A：`Attention(Q,K,V)=softmax(QK^T/sqrt(d))V`。

### Q34. 为什么 full attention 是 `O(n^2)`？

A：因为 `QK^T` 产生 `n x n` attention matrix，需要计算每个 token 对每个 token 的关系。

### Q35. sparse attention 是什么？

A：Sparse attention 只计算部分 token pair 的注意力，而不是所有 pair。

### Q36. sparse attention 如何降低复杂度？

A：通过限制每个 token 只关注局部窗口、间隔窗口或少数全局 token，可把复杂度接近降到 `O(n)`。

### Q37. sliding window attention 是什么？

A：每个 token 只关注附近固定窗口内的 tokens。

### Q38. dilated sliding window attention 是什么？

A：在滑动窗口基础上间隔采样更远位置，使模型能覆盖更大范围。

### Q39. global + sliding window attention 是什么？

A：大多数 token 用局部窗口，少数全局 token 可以关注整段序列，用于全局理解。

### Q40. Longformer 的启发是什么？

A：借鉴 CNN 的 locality，只计算局部和少数全局 attention，使模型能处理更长文档。

### Q41. sparse attention 的代价是什么？

A：效率提升通常伴随 attention 质量和全局信息整合能力的折中。

### Q42. KV caching 如何帮助长上下文推理？

A：KV caching 避免重复计算已经处理过的 key/value，降低自回归生成时的计算成本。

### Q43. KV compression 解决什么问题？

A：压缩长期历史中的 key/value 表示，减少长上下文推理的显存和计算压力。

### Q44. 为什么 100k+ attention 仍然难用？

A：即使上下文窗口足够长，attention 在远距离上会退化，模型未必能有效使用远处信息。

## Long-Context Evaluation

### Q45. 为什么长上下文需要专门评价？

A：因为支持长输入和真正使用长输入是两回事，需要测试模型是否能在长文档中定位和推理关键信息。

### Q46. Needle in the Haystack 是什么？

A：把一个关键事实放进很长的无关文本中，要求模型找出该事实，用来测试长上下文检索能力。

### Q47. Needle in the Haystack 中 haystack 和 needle 分别是什么？

A：Haystack 是大量无关填充文本；needle 是隐藏在其中的关键事实。

### Q48. Needle test 通常调节哪些变量？

A：调节 context length 和 needle depth，即文本长度和 needle 所在位置。

### Q49. Needle test 的优点是什么？

A：简单、可控、能快速 sanity check 模型是否能访问长上下文中的信息。

### Q50. Needle test 的局限是什么？

A：它偏简单检索，不等于真实长文档理解或复杂推理。

### Q51. LongBench v2 是什么？

A：LongBench v2 是长上下文多任务 benchmark，包含从 8k 到 2M words 的上下文和多类任务。

### Q52. LongBench v2 包含哪些任务类型？

A：包括单文档 QA、多文档 QA、长 in-context learning、长对话历史理解、代码仓库理解和长结构化数据理解。

### Q53. LongBench 相比 Needle test 更真实在哪里？

A：它覆盖真实文档、多任务、多领域和专家标注，比单一事实检索更接近真实长上下文使用。

### Q54. 为什么长上下文 benchmark 仍然困难？

A：标注成本高、专家耗时长、任务复杂，而且自动指标不一定能完全评价推理质量。

## Lost in the Middle

### Q55. lost-in-the-middle 是什么？

A：模型在长输入中更容易利用开头和结尾信息，而对中间信息利用较差。

### Q56. lost-in-the-middle 的表现是什么？

A：当相关信息位于上下文中间时，模型表现显著下降，形成 U-shaped performance curve。

### Q57. 为什么 lost-in-the-middle 重要？

A：它说明长上下文窗口不等于均匀有效的信息访问。

### Q58. “attention ≠ reasoning” 在这里是什么意思？

A：模型即使能 attend 到中间 token，也不一定能正确使用这些信息完成任务。

### Q59. lost-in-the-middle 对 RAG 有什么启发？

A：检索到的关键证据应合理排序和突出，否则即使放入上下文也可能被模型忽略。

### Q60. 长上下文和 RAG 的关系是什么？

A：长上下文能直接容纳更多信息；RAG 通过检索减少输入负担。二者各有成本和可靠性问题。

## 综合比较题

### Q61. Long context 和 RAG 的区别是什么？

A：Long context 试图把更多信息直接放进模型窗口；RAG 先检索相关信息再生成，减少无关内容但依赖检索质量。

### Q62. Position Interpolation、NTK Scaling 和 YaRN 的共同目标是什么？

A：都试图让基于 RoPE 的模型在超过预训练长度的上下文中更稳定工作。

### Q63. Efficient attention 和 RoPE scaling 解决的问题有何不同？

A：RoPE scaling 解决位置泛化；efficient attention 解决长序列计算成本。

### Q64. 为什么只增大 context window 不够？

A：模型可能仍有检索失败、注意力退化、lost-in-the-middle 和长程推理不足。

### Q65. 长上下文任务中的质量和效率 tradeoff 是什么？

A：更完整 attention 质量高但贵；稀疏或压缩 attention 便宜但可能损失信息整合能力。

### Q66. 为什么长上下文模型可能仍需 retrieval？

A：百万级上下文昂贵且不一定有效，retrieval 可以把输入聚焦到更相关的信息。

### Q67. 为什么真实长文档比 synthetic needle 更难？

A：真实文档需要多证据整合、排序、领域理解、歧义处理和跨段推理。

## 公式与术语速记

### Q68. `z_pos = x_pos + PE_pos` 表示什么？

A：表示 token embedding 与位置编码相加形成输入表示。

### Q69. `QK^T` 在 attention 中表示什么？

A：表示所有 query 和 key 的两两相似度矩阵。

### Q70. `O(n^2)` 中 `n` 是什么？

A：序列长度；full attention 计算量随 token 数平方增长。

### Q71. RoPE 中 rotation angle 与什么有关？

A：与 token 位置和频率有关，通常可写作 `theta = omega_i * pos`。

### Q72. Needle depth 是什么？

A：needle 在长上下文中的相对位置，通常从开头到结尾变化。

## 考前作答模板

### Q73. 如果考“长上下文三大挑战”，应该怎么答？

A：答 training data mismatch、quadratic attention cost、task difficulty，并说明长窗口不等于长程推理。

### Q74. 如果考“为什么需要 positional encoding”，应该怎么答？

A：Transformer 并行处理 token，没有天然顺序信息，所以必须注入位置；整数位置不稳定，sinusoidal/RoPE 更合适。

### Q75. 如果考“解释 RoPE”，应该怎么答？

A：RoPE 用位置角度旋转 Q/K，使相对位置成为角度差，保留向量长度并让 attention 感知距离。

### Q76. 如果考“如何扩展 RoPE context”，应该怎么答？

A：提 Position Interpolation、NTK Scaling 和 YaRN：分别压缩位置、调整频率、分段插值。

### Q77. 如果考“Needle in the Haystack 的意义和局限”，应该怎么答？

A：它是可控的长上下文检索 sanity check，但只测找事实，不等于真实长文档推理。

### Q78. 如果考“lost-in-the-middle”，应该怎么答？

A：定义为模型更擅长利用开头/结尾而忽视中间信息，说明长上下文访问不均匀，attention 不等于有效推理。

