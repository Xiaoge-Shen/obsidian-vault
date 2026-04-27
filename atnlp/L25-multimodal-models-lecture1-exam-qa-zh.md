# L25 Multimodal Models Lecture 1 考试复习 QA

这份是一套面向考试的总复习问答，不按页码整理，而是按整份讲义的知识结构整理。重点覆盖 grounding、symbol grounding problem、world scopes、Vision+Language Models、任务与数据集、Transformer-based VLM 设计、训练目标、VLM 评测和 representational quality。

## 高频核心题

### Q1. Multimodal AI 是什么？

A：Multimodal AI 通过机器学习整合多种数据类型，例如文本、图像、音频或视频，以提升理解、预测和决策。

### Q2. 本讲主要关注哪两种模态？

A：从 NLP 角度关注 language 和 vision 的结合，即 Vision+Language Models。

### Q3. Vision+Language Model 是什么？

A：VLM 整合视觉和语言数据，使模型能基于图像和文本完成理解、生成和决策任务。

### Q4. 为什么需要 multimodal models？

A：语言意义不只来自文本，现实理解依赖视觉、动作、社会互动和环境经验。

### Q5. Text-only representation 的局限是什么？

A：它只能从文本共现中学习意义，无法直接接触感知世界、物体属性和行动后果。

### Q6. “Meaning is multimodal” 是什么意思？

A：词义与视觉、动作、感知、社会互动等多种经验相关，而不只是符号之间的关系。

### Q7. 为什么简单扩大 LLM 数据和规模不完全解决 grounding 问题？

A：规模能提升文本能力，但学习仍主要来自符号文本，效率低且缺少直接感知信号。

### Q8. VLM 的核心承诺是什么？

A：把语言和视觉对齐，让模型获得更接近感知世界的表示与推理能力。

## Grounding 与 Symbol Grounding

### Q9. Grounding 在 communication 中是什么意思？

A：指对话双方共享的 mutual knowledge、beliefs 和 assumptions，使交流得以成功。

### Q10. Symbol grounding problem 是什么？

A：如果符号只通过其他符号定义，符号意义如何最终连接到现实世界或感知经验。

### Q11. Chinese Room argument 想说明什么？

A：仅按规则操作符号不一定等于真正理解符号意义。

### Q12. Chinese/Chinese dictionary-go-round 说明什么？

A：如果只能用同一语言字典解释词，学习者会在无意义符号之间循环，无法落到真实经验上。

### Q13. 为什么古语言破译不只是符号内循环？

A：破译者依赖已有语言、现实知识和外部证据来把符号接到世界上。

### Q14. Perceptually grounded NLP 的目标是什么？

A：让语言系统能通过感知和与世界互动来获得符号 referents，而不是只依赖文本。

### Q15. Amodal symbols 与 grounded symbols 的区别是什么？

A：amodal symbols 只在符号系统内操作；grounded symbols 与感知、分类和现实 referents 相连。

### Q16. 为什么视觉 grounding 对 NLP 有意义？

A：视觉提供关于物体、属性、空间关系和动作的信号，可补充纯文本学习。

## Experience Grounds Language 与 World Scopes

### Q17. Experience Grounds Language 的核心观点是什么？

A：语言学习依赖学习者可访问的世界经验，不同 world scope 提供不同数据和能力。

### Q18. World scope 是什么？

A：描述学习者能访问的数据和环境范围，例如只读文本、看图像、能行动、能与人合作等。

### Q19. “不能从 radio/Internet 学语言”在讲义中是什么意思？

A：如果任务可在无感知条件下完成，就不能说明学习者已进入需要视觉或听觉的更高 world scope。

### Q20. “不能从 television 学语言”是什么意思？

A：如果行动空间和后果可以完全枚举，仍未达到需要真实行动和环境反馈的 world scope。

### Q21. “不能 by yourself 学语言”是什么意思？

A：更高 world scope 需要与人合作来实现目标，语言学习包含社会互动维度。

### Q22. World scope 层级有什么启发？

A：增加模态、行动和交互会改变模型可学习的信息类型和归纳偏置。

### Q23. Reporting bias 是什么？

A：文本倾向记录值得说的、不寻常的或社会上可表达的信息，而省略显而易见的感知事实。

### Q24. Multimodal data 如何缓解 reporting bias？

A：图像和视频直接包含视觉事实，即使文本没有明确描述，模型也能从感知信号中学习。

## VLM Tasks 与数据集

### Q25. VLM 常见任务有哪些？

A：image captioning、visual question answering、visual grounding、referring expression、visual storytelling 和 visual dialogue。

### Q26. Image captioning 是什么？

A：给定图像，模型生成描述图像内容的自然语言句子。

### Q27. 早期 image captioning 模型如何工作？

A：用 CNN 提取图像表示，并用该表示初始化 RNN language model 的 hidden state。

### Q28. 早期 captioning 中“只在开头 conditioning on image”有什么问题？

A：生成后续 tokens 时视觉信息可能逐渐丢失，难以持续对齐图像细节。

### Q29. MSCOCO 在本讲中是什么？

A：常用图像-caption 数据集，每张图像有多个人类描述，常用于 image captioning。

### Q30. Visual Question Answering 是什么？

A：输入图像和问题，模型输出答案。

### Q31. 早期 VQA 通常被建模为什么任务？

A：分类任务，把常见答案视为类别。

### Q32. VQA dataset 的特点是什么？

A：包含真实图片和问题-答案对，讲义中提到约 600k image-question pairs。

### Q33. Referential Expression Understanding 是什么？

A：给定语言描述，模型选择或预测图像中对应的 bounding box。

### Q34. Referential Expression Generation 是什么？

A：给定目标物体或 bounding box，模型生成能唯一指称它的描述。

### Q35. Referring expression 为什么重要？

A：它直接测试 visual grounding，并适用于人机协作和机器人任务。

### Q36. Visual Storytelling 是什么？

A：给定一组相关图像，模型生成连贯故事，而不只是独立描述每张图。

### Q37. VIST 数据集有什么特点？

A：通常包含同一 Flickr album 中的 5 张图像及 crowdsourced stories。

### Q38. GuessWhat?! 是什么？

A：视觉对话游戏，Questioner 通过 yes/no 问题帮助 Guesser 猜中图像中的目标物体。

### Q39. GuessWhat?! 测试什么能力？

A：视觉 grounding、问题生成、对话策略和目标定位能力。

## 视觉表示与模型设计

### Q40. 早期视觉信息如何表示？

A：早期使用 symbolic features；后来转向通过 computer vision 自动学习视觉表示。

### Q41. CNN 在视觉表示中做什么？

A：从像素中学习局部和层级特征，生成 feature maps 或 pooled features。

### Q42. Vision Transformer 的基本思想是什么？

A：把图像切成 patches，把每个 patch 当作 token，用 Transformer 处理视觉序列。

### Q43. Transformer-based VLM 为什么需要 fusion？

A：语言和视觉表示来自不同模态，需要机制让模型对齐、融合和共同推理。

### Q44. Cross-encoding 是什么？

A：把视觉和语言 tokens 一起输入同一个 Transformer，让它们在同一编码过程中交互。

### Q45. Cross-attention 是什么？

A：语言模型通过 attention 访问视觉表示，视觉信息作为可被查询的外部上下文。

### Q46. Visual prefix 是什么？

A：把视觉表示投影成 LLM 可处理的 prefix tokens，让文本 LLM 在这些视觉 tokens 条件下生成。

### Q47. 这三种 VLM 架构的共同目标是什么？

A：让模型在生成或理解语言时能使用视觉信息。

### Q48. VLM training pipeline 的主要组件是什么？

A：语言专家、视觉专家、image-text dataset、VLM 融合结构和训练 loss。

### Q49. 为什么会用 modality experts 初始化 VLM？

A：预训练语言模型和视觉模型已学习大量单模态知识，初始化可减少训练成本并提升能力。

### Q50. V+L pretraining data 通常来自哪里？

A：网页 alt-caption、interleaved text-image web pages、社交媒体图文等 internet-scale 数据。

### Q51. 为什么 noisy web image-text data 仍有用？

A：真实世界数据本来嘈杂，但大规模配对仍能提供视觉与语言 grounding 信号。

## Training Objectives

### Q52. Masked Image Modelling 是什么？

A：随机遮蔽图像 patches，让模型重构视觉表示或像素/feature。

### Q53. Language Modelling loss 是什么？

A：经典 next-token prediction 或 masked language modelling，用于学习语言生成或理解。

### Q54. Image-Text Matching 是什么？

A：给定图像和 caption，判断二者是否匹配的二分类任务。

### Q55. Masked Multimodal Modelling 是什么？

A：同时遮蔽视觉和语言部分，让模型预测两个模态中的缺失内容。

### Q56. 为什么 VLM 需要多种 training losses？

A：不同 loss 强化不同能力，例如视觉重构、语言建模、跨模态对齐和多模态融合。

### Q57. Image-text matching 与 contrastive learning 的区别是什么？

A：ITM 通常是匹配/不匹配二分类；contrastive learning 在批量中拉近匹配对、推远不匹配对。

## Evaluation

### Q58. Extrinsic evaluation 是什么？

A：用具体下游任务或应用数据集评价 VLM 表现。

### Q59. Task-specific evaluation 的优点是什么？

A：指标明确、能力聚焦、与具体应用直接相关。

### Q60. Task-specific evaluation 的局限是什么？

A：结果不一定能迁移到其他任务或表示质量。

### Q61. Multi-task evaluation 的优点是什么？

A：覆盖更广语言和视觉能力，更适合评估 general-purpose VLM。

### Q62. Multi-task evaluation 的局限是什么？

A：计算成本高，不同任务结果难以聚合，且规模和质量存在 tradeoff。

### Q63. FOIL captions 测试什么？

A：测试 VLM 是否能发现 caption 中与图像不匹配但语境 plausible 的错误词。

### Q64. SVO-Probes 关注什么？

A：关注 subject-verb-object 句子，尤其测试模型是否理解细粒度动作/动词差异。

### Q65. 为什么 verb understanding 比 object understanding 更难？

A：动词涉及动作、关系和动态事件，视觉差异可能细微且上下文依赖强。

### Q66. Winoground 是什么？

A：评估 visio-linguistic compositionality 的数据集，要求模型匹配同词不同顺序的 captions 和图像。

### Q67. 为什么 Winoground 很难？

A：模型必须理解词序、关系和组合意义，而不能只依赖词袋或对象检测。

### Q68. VLMEvalKit 是什么？

A：用于评估大量 VLM 的工具包，覆盖文档理解、图表理解、captioning 等多种 benchmark。

### Q69. MMMU 这类 benchmark 想测什么？

A：测试多学科、多模态理解和推理能力，接近 expert-level multimodal reasoning。

### Q70. Intrinsic evaluation 是什么？

A：直接评估表示质量，例如是否与人类语义判断、注意模式或脑反应对齐。

### Q71. Attention pattern comparison 在 VQA 中测什么？

A：比较模型和人类在回答问题时关注图像区域是否相似。

### Q72. Brain alignment evaluation 想回答什么？

A：多模态预训练表示是否更接近人脑对概念知识的表示。

## 考前作答模板

### Q73. 如果考“为什么需要 multimodal models”，应该怎么答？

A：说明文本-only 学习受 symbol grounding 和 reporting bias 限制，语言意义与感知、行动和社会互动相关，VLM 用视觉信号补充语言表示。

### Q74. 如果考“VLM 主要任务”，应该怎么答？

A：列 image captioning、VQA、visual grounding/referring expression、visual storytelling、visual dialogue，并各给一句定义。

### Q75. 如果考“VLM 架构设计”，应该怎么答？

A：比较 cross-encoding、cross-attention、visual prefix，说明它们都是为了融合视觉和语言信息。

### Q76. 如果考“VLM evaluation”，应该怎么答？

A：区分 extrinsic task/multi-task evaluation 和 intrinsic representation evaluation，并举 FOIL、SVO-Probes、Winoground、VLMEvalKit 例子。

