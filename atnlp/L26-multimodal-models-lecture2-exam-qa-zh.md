# L26 Multimodal Models Lecture 2 考试复习 QA

这份是一套面向考试的总复习问答，不按页码整理，而是按整份讲义的知识结构整理。重点覆盖 visual prefix VLM、visual encoder/projector/LLM 组成、patch/token 数量、contrastive encoders、cross-entropy training、SmolVLM 架构、pixel shuffle、三阶段训练、评测与 VLM 研究方向。

## 高频核心题

### Q1. 本讲的核心目标是什么？

A：理解 frontier VLM 如何设计、实现和训练，尤其是 visual prefix-based VLM。

### Q2. Visual prefix-based VLM 是什么？

A：把视觉编码器输出投影成 LLM 可处理的视觉 prefix tokens，让文本 LLM 在视觉上下文条件下生成语言。

### Q3. Visual prefix 架构包含哪些主要组件？

A：pretrained vision encoder、projector、text-only LLM backbone，以及 image-text training data。

### Q4. 为什么 visual prefix 架构流行？

A：它复用强大的 pretrained LLM 和 vision encoder，只需学习模态对齐和任务适配，工程上简单高效。

### Q5. Visual prefix 中 language expert 是什么？

A：预训练语言模型，用作生成和推理 backbone。

### Q6. Visual prefix 中 vision expert 是什么？

A：预训练视觉模型，通常是 Vision Transformer 或视觉-语言 contrastive encoder。

### Q7. Projector 的作用是什么？

A：把 vision encoder 的视觉 tokens 映射到 LLM hidden size 和表示空间，使 LLM 能处理它们。

### Q8. Visual prefix VLM 通常用什么 loss 训练？

A：next-token prediction 的 cross-entropy loss，与文本 LLM 类似。

## Visual Encoder

### Q9. Visual encoder 通常是什么？

A：预训练 Vision Transformer 或视觉-语言 contrastive encoder。

### Q10. Vision-only pretraining 可以用什么目标？

A：例如 reconstruction loss，让模型重构被遮蔽或压缩的视觉信息。

### Q11. Vision+Language pretraining 可以用什么目标？

A：contrastive loss，用 image-caption pairs 拉近匹配图文表示、推远不匹配表示。

### Q12. 为什么 V+L 预训练视觉编码器通常更适合 VLM？

A：它已学习视觉和语言之间的对齐，比纯视觉表示更容易接入语言模型。

### Q13. CLIP 是什么类型的 encoder？

A：视觉-语言 contrastive encoder，用图文对学习可迁移视觉表示。

### Q14. SigLIP 是什么？

A：一种使用 sigmoid loss 的 image-language pretraining 模型，常作为 VLM 的视觉编码器。

### Q15. SmolVLM 使用什么视觉编码器？

A：使用 SigLIP 系列视觉编码器。

### Q16. Vision Transformer 如何把图像转为 tokens？

A：把图像切成非重叠 square patches，每个 patch 作为一个视觉 token。

### Q17. 图像 patch 数量公式是什么？

A：对于方形图像，`NP = (H / P)^2`，其中 `H` 是图像边长，`P` 是 patch 边长。

### Q18. 224x224 图像、16x16 patch 有多少 patches？

A：`(224/16)^2 = 14^2 = 196`。

### Q19. 224x224 图像、32x32 patch 有多少 patches？

A：`(224/32)^2 = 7^2 = 49`。

### Q20. 224x224 图像、8x8 patch 有多少 patches？

A：`(224/8)^2 = 28^2 = 784`。

### Q21. 384x384 图像、16x16 patch 有多少 patches？

A：`(384/16)^2 = 24^2 = 576`。

### Q22. 为什么视觉 token 数量重要？

A：视觉 tokens 越多，占用上下文和显存越大，与 LLM 融合成本越高。

### Q23. 图像分辨率和 patch size 如何影响成本？

A：分辨率越高或 patch 越小，patch 数量越多，计算和 memory cost 越高。

## Projector 与训练目标

### Q24. Projector 为什么必要？

A：视觉 encoder 输出维度通常不同于 LLM hidden size，需要映射到 LLM 可接收的空间。

### Q25. Projector 可以如何实现？

A：可用 feedforward neural network、cross-attention with learnable queries 等方法。

### Q26. Projector 输出的 tokens 在 LLM 中扮演什么角色？

A：它们作为视觉 prefix tokens，提供图像上下文。

### Q27. VLM cross-entropy loss 计算在哪些 tokens 上？

A：通常只在 language tokens 上计算 loss。

### Q28. 为什么不对 visual tokens 计算 next-token loss？

A：训练监督通常是文本 tokens，没有视觉 token 的 ground-truth next-token 标签。

### Q29. Visual prefix VLM 中 ground truth 通常是什么？

A：与视觉输入相关的文本，例如 caption、答案、指令响应等。

### Q30. 训练 VLM 时可以冻结哪些组件？

A：可以冻结 LLM、vision encoder，只训练 projector；也可以后续 finetune 部分或全部组件。

### Q31. 冻结 LLM 的好处是什么？

A：保留语言模型原有能力，降低训练成本，避免 catastrophic forgetting。

### Q32. 全模型 finetuning 的好处是什么？

A：能更深度适配多模态任务，提高图像理解和指令跟随，但成本和遗忘风险更高。

## SmolVLM 架构

### Q33. 为什么讲义关注 SmolVLM？

A：它是 open source/open weight VLM，便于复现、研究和面向资源受限场景部署。

### Q34. “Closed AI Models Make Bad Baselines” 的含义是什么？

A：闭源模型难以复现和分析，不利于科研比较、进步和下游适配。

### Q35. SmolVLM 的 backbone 思路是什么？

A：用 SigLIP 作为视觉 encoder，用 SmolLM2 作为语言 backbone，并通过 projector 接入视觉 tokens。

### Q36. SmolVLM 如何处理不同分辨率图片？

A：image processor 可把高分辨率图像切成多个 sub-regions，分别编码，再把全图 downsized 编码。

### Q37. SmolVLM 如何支持视频？

A：从视频中采样 frames，把每帧作为图像编码，再作为多图输入处理。

### Q38. 为什么多图/视频会加重 token 压力？

A：每张图或每帧都会产生视觉 tokens，序列长度和显存消耗迅速增加。

### Q39. Pixel shuffle 在 SmolVLM 中解决什么问题？

A：压缩视觉 token 数量，降低 memory consumption，同时尽量保留细粒度视觉理解。

### Q40. Pixel shuffle 的核心思想是什么？

A：降低空间分辨率，同时增加深度方向的表示密度，把多个视觉特征合并为更少 tokens。

### Q41. Pixel shuffle ratio `r` 表示什么？

A：控制压缩比例和空间保真度之间的 tradeoff。

### Q42. SmolVLM 使用的 pixel shuffle ratio 是多少？

A：讲义中提到使用 `r = 4` 来减少视觉 token count。

### Q43. Pixel shuffle 的主要 tradeoff 是什么？

A：token 压缩越强，计算越省，但可能损失空间细节；压缩太弱则成本高。

### Q44. 为什么 OCR 能力对 pixel shuffle 是挑战？

A：OCR 需要保留小字和局部空间细节，过度压缩会损害文字识别。

## SmolVLM 训练阶段

### Q45. SmolVLM 训练分为哪三个阶段？

A：adaptation stage、image stage、video stage。

### Q46. Adaptation stage 的目标是什么？

A：训练 projector 使视觉表示能适配语言模型 backbone。

### Q47. Adaptation stage 中通常如何处理 LLM？

A：保持 language model frozen，重点学习 projector，并可能对 vision encoder 使用 LoRA。

### Q48. Adaptation stage 使用什么数据？

A：主要使用 noisy web data 和 captioning 数据。

### Q49. Image stage 的目标是什么？

A：提升模型对不同图像格式的理解，尤其是 OCR、PDF、charts、tables 等。

### Q50. Image stage 通常训练哪些组件？

A：讲义中 SmolVLM 会 finetune 整个模型，包括视觉和 LLM backbone。

### Q51. 为什么 image stage 仍加入少量文本数据？

A：为了保留原有 instruction-following 和语言能力，减少 catastrophic forgetting。

### Q52. Video stage 的目标是什么？

A：让模型理解 multi-image 和 video content。

### Q53. Video stage 为什么可以看作 curriculum learning？

A：训练从单图到多图再到视频，逐渐增加输入复杂度。

### Q54. 为什么数据混合是 VLM 训练关键？

A：不同数据决定模型学习 captioning、OCR、多图、视频、指令跟随等能力的平衡。

### Q55. SmolVLM 有哪些模型规模？

A：讲义提到 256M、500M 和 2.2B 等变体。

### Q56. SmolVLM-256M 适合什么场景？

A：显存占用低，适合资源受限和 edge applications。

### Q57. SmolVLM-2.2B 的特点是什么？

A：更大视觉编码器和 LLM backbone，性能更强但资源需求更高。

## Evaluation 与结果

### Q58. SmolVLM 使用什么工具做大规模评测？

A：VLMEvalKit。

### Q59. SmolVLM 评测强调什么优势？

A：在较小显存占用下保持较强性能，体现 computational efficiency。

### Q60. 为什么 language model capacity 会影响视觉任务？

A：许多视觉任务需要语言推理、OCR 后理解、指令跟随和答案生成，LLM capacity 仍很关键。

### Q61. Context length 对 VLM evaluation 有何影响？

A：更长上下文可容纳更大图像、更多视觉 tokens 或长文档信息，影响 OCR 和 document tasks。

### Q62. Larger vision encoder 的作用何时更明显？

A：当 LLM backbone 也更大时，模型更能利用更强视觉表示。

### Q63. 为什么 VLM 适合 edge devices 是重要方向？

A：低延迟、隐私、本地部署和成本控制都要求小型高效 VLM。

### Q64. SmolVLM 下游变体说明什么？

A：紧凑 VLM 可适配 document conversion、biomedical VQA、on-device retrieval 等专门场景。

## Future Research

### Q65. VLM 未来研究方向有哪些？

A：细粒度视觉理解、object-centric representations、多粒度视觉表示、VLM 内部机制、多模态真实世界系统。

### Q66. “Is it really a VLM or language with some vision?” 想问什么？

A：模型是否真正融合视觉和语言，还是主要依赖语言模型，只把视觉当弱提示。

### Q67. 为什么 mechanistic interpretability 对 VLM 重要？

A：能帮助理解两个模态如何表示、融合，以及训练后内部 representations 如何变化。

### Q68. 为什么 VLM 还不是完整 multimodal real-world model？

A：多数 VLM 主要处理图像/文本，缺少行动、环境反馈、声音、触觉和长期交互。

## 考前作答模板

### Q69. 如果考“visual prefix VLM”，应该怎么答？

A：说明用视觉 encoder 产生 visual tokens，经 projector 映射到 LLM 空间，作为 prefix 条件化文本 LLM，并用 language-token cross-entropy 训练。

### Q70. 如果考“patch 数量公式”，应该怎么答？

A：写 `NP = (H/P)^2`，并说明高分辨率或小 patch 会增加视觉 tokens、显存和上下文成本。

### Q71. 如果考“SmolVLM 三阶段训练”，应该怎么答？

A：adaptation 学 projector；image stage 强化图像/OCR/文档理解；video stage 通过 curriculum 扩展到多图和视频。

### Q72. 如果考“pixel shuffle”，应该怎么答？

A：说明它压缩视觉 token，降低空间分辨率、提高深度表示密度，在效率和空间细节之间折中。

### Q73. 如果考“VLM 训练 tradeoff”，应该怎么答？

A：答视觉 token 数量、分辨率、context length、冻结/finetune 组件、数据混合和 catastrophic forgetting 之间的权衡。

