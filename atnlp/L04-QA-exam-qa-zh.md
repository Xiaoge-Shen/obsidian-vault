# L04 Question Answering 考试复习 QA

这份是一套面向考试的总复习问答，不按页码整理，而是按整份讲义的知识结构整理。课程行政信息已省略，重点保留 QA、Reading Comprehension、SQuAD、BERT、Open-Domain QA、Natural Questions 和 Fusion-in-Decoder。

## 高频核心题

### Q1. 什么是 Question Answering？

A：Question Answering 是构建能自动回答人类自然语言问题的系统。

### Q2. QA 为什么是 NLP 中重要的 test bed？

A：因为 QA 要求系统理解问题、理解文本，并把二者对齐来产生答案，因此能测试模型对自然语言的理解能力。

### Q3. QA 中常见的问题类型有哪些？

A：常见类型包括 factoid vs. non-factoid、open-domain vs. closed-domain、simple vs. compositional、natural vs. cloze-style。

### Q4. QA 中常见的答案形式有哪些？

A：答案可以是短文本、段落、列表、yes/no、multiple choice 等。

### Q5. 本讲主要关注哪类 QA？

A：主要关注基于知识检索的问题回答，包括 reading comprehension 和 open-domain QA。

### Q6. 为什么很多 NLP 任务可以被转化成 QA？

A：因为许多任务都可以被表述为“给定输入文本和一个问题，输出答案”，例如翻译、信息抽取、词性标注、数学文字题和语言建模。

### Q7. QA 在现实中有哪些应用？

A：搜索引擎问答、智能音箱、文档问答、客服聊天机器人、政策或产品助手、法律/医学/科研文档分析等。

### Q8. 为什么说很多 LLM 当前用法本质上是 QA 的变体？

A：因为用户通常给模型一个问题或任务描述，再要求模型基于内部知识或外部上下文生成答案。

## QA 任务类型

### Q9. factoid question 是什么？

A：Factoid question 通常询问一个明确事实，答案往往是短实体、日期、地点、数字或名称。

### Q10. non-factoid question 是什么？

A：Non-factoid question 需要解释、推理、建议或长答案，不一定能用一个短实体回答。

### Q11. closed-domain QA 是什么？

A：Closed-domain QA 限定在某个特定领域或知识范围内回答问题，例如医学 QA 或公司政策 QA。

### Q12. open-domain QA 是什么？

A：Open-domain QA 面向广泛主题的问题，系统需要在大型文档集合中找到相关证据并回答。

### Q13. simple question 和 compositional question 的区别是什么？

A：Simple question 通常只需要一个事实匹配；compositional question 需要组合多个事实或多步推理。

### Q14. natural question 和 cloze-style question 的区别是什么？

A：Natural question 是人类自然提出的问题；cloze-style question 通常是填空式问题，例如预测句子中缺失的词。

### Q15. 为什么智能音箱是 QA 的典型应用？

A：因为用户经常通过自然语言直接提问，例如查天气、设闹钟、问事实或搜索产品信息。

## Reading Comprehension

### Q16. 什么是 Reading Comprehension？

A：Reading Comprehension 是给定一个文本段落和一个问题，要求系统理解段落内容并回答问题。

### Q17. Reading Comprehension 的形式化输入输出是什么？

A：输入是 paragraph `P` 和 question `Q`，输出是 answer `A`，可写作 `P, Q -> A`。

### Q18. Machine Reading Comprehension 和普通 QA 的关系是什么？

A：Machine Reading Comprehension 是 QA 的一种设置，答案通常需要从给定文本中找到。

### Q19. 什么是 extractive QA？

A：Extractive QA 要求答案逐字来自给定 context，通常是原文中的一个连续文本 span。

### Q20. extractive QA 的优点是什么？

A：答案来自原文，事实可追溯，评价相对容易，幻觉风险较低。

### Q21. extractive QA 的局限是什么？

A：如果答案没有以原文 span 的形式出现，模型就很难回答；它也不适合需要生成解释或综合多个证据的问题。

### Q22. 在 Beyoncé 示例中，模型要做的核心操作是什么？

A：模型要根据问题定位段落中对应的信息 span，例如时间、地点或活动类型。

### Q23. Reading Comprehension 为什么是现代 QA 的基础？

A：它把“理解文本并回答问题”变成可监督学习的数据集任务，为后来的预训练、检索增强和开放域 QA 奠定基础。

## SQuAD 数据集

### Q24. SQuAD 是什么？

A：SQuAD 是 Stanford Question Answering Dataset，一个大规模阅读理解数据集。

### Q25. SQuAD 的数据形式是什么？

A：它包含 passage、question、answer 三元组，答案是 passage 中的短文本 span。

### Q26. SQuAD 的 passages 来自哪里？

A：来自 English Wikipedia。

### Q27. SQuAD 的 questions 是如何得到的？

A：通过众包方式收集。

### Q28. SQuAD 为什么对神经 RC 模型很重要？

A：它提供了大规模监督数据，推动了神经阅读理解模型的发展。

### Q29. SQuAD 原始版本有什么限制？

A：它不包含答案不在 span 中的问题，也不包含不可回答问题。

### Q30. 为什么 SQuAD 曾经很流行但后来被认为 almost solved？

A：因为 2016-2018 年间它推动了大量模型进展，但后来神经模型在该数据集上超过人类表现，数据集区分度下降。

### Q31. SQuAD 和真实搜索 QA 的主要差距是什么？

A：SQuAD 给定短 passage 且答案通常是 span；真实搜索 QA 往往要从大规模文档中检索证据，还可能有歧义或无答案。

## QA 评价指标

### Q32. SQuAD 常用哪两个评价指标？

A：Exact Match 和 F1。

### Q33. Exact Match 是什么？

A：Exact Match 判断预测答案是否与参考答案完全一致，通常得分是 0 或 1。

### Q34. F1 在 QA 评价中表示什么？

A：F1 基于预测答案和参考答案的 token overlap，给部分匹配以 partial credit。

### Q35. QA 评价中为什么允许多个 valid answers？

A：同一个问题可能有多个合理表达方式，因此评价时通常取预测与所有参考答案中的最高分。

### Q36. SQuAD 评价中常见的 normalization 包括什么？

A：通常会去掉冠词、标点，并做大小写归一化。

### Q37. 为什么 F1 比 Exact Match 更宽松？

A：因为 F1 允许预测答案只匹配参考答案的一部分，从而给出部分分数。

### Q38. Exact Match 的主要问题是什么？

A：它过于严格，可能把语义正确但表述略有不同的答案判错。

### Q39. F1 的主要问题是什么？

A：它依赖词面重合，不能充分判断语义等价，也可能奖励包含多余词的答案。

### Q40. 如果一个答案被截断但包含核心词，EM 和 F1 可能如何表现？

A：EM 可能为 0，但 F1 可能因为 token overlap 给出部分分数。

## Neural Reading Comprehension

### Q41. 神经阅读理解中的输入通常如何表示？

A：输入包括 context 或 paragraph `C=<c1,...,cn>` 和 question `Q=<q1,...,qm>`。

### Q42. extractive neural RC 的输出是什么？

A：输出是答案在 context 中的 start index 和 end index，且满足 `1 <= start <= end <= n`。

### Q43. 为什么 start/end span prediction 适合 SQuAD？

A：因为 SQuAD 的答案是 passage 中的连续文本 span，因此预测起止位置即可抽取答案。

### Q44. span prediction 的局限是什么？

A：它只能抽取连续 span，难以处理需要生成、汇总、多跳推理或答案不在原文中的问题。

### Q45. BiDAF 是什么？

A：BiDAF 是 Bi-Directional Attention Flow，一种早期神经阅读理解模型。

### Q46. BiDAF 的主要结构包括什么？

A：包括 word/character embedding、contextual embedding、attention flow、modeling layer 和 output layer。

### Q47. BiDAF 中 attention flow 的核心思想是什么？

A：它同时建模 context-to-query 和 query-to-context attention，让问题和上下文互相关注。

### Q48. context-to-query attention 是什么？

A：它让每个 context token 找到 question 中最相关的信息。

### Q49. query-to-context attention 是什么？

A：它帮助模型识别 context 中哪些位置对整个 question 最重要。

### Q50. BiDAF 的输出是什么？

A：输出答案 span 的 start 和 end 位置。

### Q51. BiDAF 和现代 BERT 模型在训练方式上有什么差别？

A：BiDAF 主要从任务数据训练，除 GloVe 等词向量外没有大规模预训练；BERT 先大规模预训练，再针对 QA fine-tune。

### Q52. BiDAF 的参数规模大约是多少？

A：约 2.5M 参数。

### Q53. 为什么 BiDAF 代表了 pre-BERT 时代的模型工程？

A：它依赖专门为阅读理解设计的模块，例如 BiLSTM、多层 attention 和 span 输出结构。

## BERT-based QA

### Q54. BERT-based QA 的核心做法是什么？

A：把 question 和 paragraph 输入预训练 BERT，再 fine-tune 一个输出层预测答案 start/end span。

### Q55. BERT 预训练主要包括哪些目标？

A：讲义中展示了 Masked LM 和 NSP。

### Q56. BERT fine-tuning 在 QA 中如何组织输入？

A：通常把 question 和 paragraph 拼接输入，用 `[SEP]` 分隔，并在输出层预测答案起止位置。

### Q57. BERT 为什么比 BiDAF 更强？

A：BERT 通过大规模预训练学到通用语言表示，fine-tuning 时只需较少任务工程即可取得高性能。

### Q58. BERT-large 大约有多少参数？

A：约 340M 参数。

### Q59. BERT 的架构特点是什么？

A：它是 Transformer encoder，没有 recurrence。

### Q60. BERT 在 SQuAD 上的表现说明了什么？

A：预训练加 fine-tuning 能在阅读理解任务上接近或超过人类表现，且不需要复杂的任务专用架构工程。

### Q61. BiDAF 和 BERT-large 在 SQuAD 表现上的差异是什么？

A：BiDAF 的 F1 约为 85.8；BERT-large 达到约 85.1 EM 和 91.8 F1，接近或超过人类水平。

### Q62. 为什么 SQuAD 上超过人类表现不等于真正“解决 QA”？

A：因为 SQuAD 的设置较受限，真实 QA 还涉及检索、歧义、无答案、长文档和多证据融合。

## 为什么继续研究 Reading Comprehension

### Q63. Reading Comprehension 如何连接 old-school NLP 和现代 LLM？

A：它从特征工程和任务专用架构，过渡到 Transformer、预训练和 fine-tuning。

### Q64. Reading Comprehension 和 multi-hop QA 有什么关系？

A：Multi-hop QA 可以看作更复杂的阅读理解，需要跨多个证据片段推理。

### Q65. Reading Comprehension 和 retrieval-augmented QA 有什么关系？

A：Retrieval-augmented QA 先检索相关文本，再对检索到的文本做类似阅读理解或生成回答。

### Q66. 为什么文档 QA 可以看作高级阅读理解？

A：它要求系统理解长文档、定位相关证据，并针对用户问题产生答案。

## Open-Domain QA

### Q67. Open-Domain QA 和 Reading Comprehension 的核心区别是什么？

A：Reading Comprehension 通常给定一个 passage；Open-Domain QA 不给定小段落，而是让系统从大型文档集合中寻找证据。

### Q68. Open-Domain QA 为什么更实用？

A：现实中用户通常不会先提供正确 passage，系统必须自己从大量知识源中找到相关信息。

### Q69. Open-Domain QA 为什么更难？

A：它同时包含检索问题和答案生成或抽取问题，错误可能来自检索失败或阅读失败。

### Q70. Open-Domain QA 中的 knowledge source 通常是什么？

A：可以是 Wikipedia、新闻库、企业文档、网页或其他可信知识库。

### Q71. 为什么只依赖 LLM 参数中的知识不够？

A：参数知识可能过时、不可验证或产生 hallucination；检索可信来源能提高可追溯性和更新性。

### Q72. Open-Domain QA 的典型 pipeline 是什么？

A：先用 retriever 找相关文档或 passage，再用 reader 或 generator 生成答案。

### Q73. Retriever-Reader architecture 是什么？

A：Retriever 从非结构化文档集合中找相关文档，Reader 基于这些文档抽取或生成答案。

### Q74. Retriever 失败会导致什么？

A：如果没有检索到包含答案的证据，后面的 reader 通常无法正确回答。

### Q75. Reader 失败会导致什么？

A：即使检索到了相关证据，reader 也可能抽错 span、遗漏证据或生成错误答案。

## Dense Passage Retrieval

### Q76. Dense Passage Retrieval 的核心思想是什么？

A：把 question 和 passage/document 编码成 dense vectors，再用向量相似度检索相关 passage。

### Q77. Dense retrieval 通常如何计算相似度？

A：通常使用 question vector 和 passage vector 的 dot product。

### Q78. 为什么 dense retrieval 需要 approximate nearest neighbours？

A：因为文档集合很大，逐一精确计算相似度成本太高，ANN 可以快速找 top-k 近邻。

### Q79. Dense retrieval 和 sparse retrieval 的区别是什么？

A：Sparse retrieval 依赖词项匹配，如 TF-IDF 或 BM25；dense retrieval 用神经向量表示语义相似性。

### Q80. Dense retrieval 的优点是什么？

A：它能找到词面不完全匹配但语义相关的 passage。

### Q81. Dense retrieval 的风险是什么？

A：它可能检索到语义上看似相关但缺乏答案证据的段落，也需要训练高质量 retriever。

## Natural Questions

### Q82. Natural Questions 是什么？

A：Natural Questions 是一个大规模 QA 数据集，包含真实 Google Search queries，并配有 Wikipedia 证据。

### Q83. Natural Questions 相比 SQuAD 更真实在哪里？

A：问题来自真实搜索查询，文档是完整 Wikipedia 页面，而不是预先截好的短 passage。

### Q84. Natural Questions 中输入文档有多长？

A：讲义指出需要处理整页 Wikipedia，最长可到约 5k tokens。

### Q85. Natural Questions 为什么推动了 ODQA、RAG 和长上下文模型？

A：因为它要求模型处理真实、嘈杂、歧义、多词查询，并在完整文档中定位答案。

### Q86. Natural Questions 的标注流程是什么？

A：标注者先选择包含证据的 context 或 long answer，再在适用时选择 short answer。

### Q87. Natural Questions 中 short answer 和 long answer 的区别是什么？

A：Long answer 是较大的证据区域或上下文；short answer 是更短的直接答案 span。

### Q88. Natural Questions 有哪些挑战？

A：包括不可回答问题、没有 short answer 的问题、歧义问题和需要长答案的问题。

### Q89. 为什么真实用户问题会给 QA 带来歧义？

A：用户查询可能缺少上下文、指代不清、意图多样，系统需要判断用户真正想问什么。

## Fusion-in-Decoder

### Q90. Fusion-in-Decoder 是什么？

A：Fusion-in-Decoder 是一种 open-domain generative QA 方法，把多个检索 passage 分别编码，再在 decoder 中融合证据生成答案。

### Q91. FiD 的输入如何组织？

A：把 question 分别和每个 retrieved passage 拼接，形成多个 question-passage 输入。

### Q92. FiD 的 encoder 如何处理 passage？

A：每个 question-passage pair 在 encoder 中独立处理。

### Q93. FiD 的 decoder 如何融合证据？

A：decoder attends over 所有 retrieved passages 的 concatenated representations，在解码阶段完成 evidence fusion。

### Q94. 为什么叫 Fusion-in-Decoder？

A：因为多个 passage 的信息不是在 encoder 中提前合并，而是在 decoder 生成答案时融合。

### Q95. FiD 相比单 passage reader 的优势是什么？

A：它能从多个 passage 聚合证据，更适合 open-domain QA。

### Q96. FiD 和纯参数化生成模型相比有什么优势？

A：FiD 利用外部检索证据，不完全依赖模型参数记忆，因此更适合知识密集型问答。

### Q97. FiD 为什么被认为是现代 RAG 的基础之一？

A：它把 retrieval 和 generative model 结合起来，展示了多 passage evidence fusion 对 ODQA 的价值。

### Q98. FiD 中增加 retrieved passages 数量可能带来什么影响？

A：可能提高答案准确率，因为模型有更多证据可用；但也会增加计算成本和噪声风险。

### Q99. FiD 评价中丢弃超过 5 个词的答案可能有什么问题？

A：会偏向短 factoid 答案，惩罚合理的长答案，也无法全面评价解释型或开放式回答。

### Q100. 只用 Exact Match 评价 FiD 可能有什么问题？

A：它忽略语义等价、同义改写和部分正确答案，也可能低估生成式答案的质量。

## 综合比较题

### Q101. Reading Comprehension 和 Open-Domain QA 的区别是什么？

A：Reading Comprehension 给定 passage，重点是理解和定位答案；Open-Domain QA 不给定 passage，需要先从大规模知识库检索证据。

### Q102. SQuAD 和 Natural Questions 的区别是什么？

A：SQuAD 使用 Wikipedia passage 和 span answer，设置较干净；Natural Questions 使用真实 Google 查询和完整 Wikipedia 页面，更嘈杂、更真实，也包含无答案和长答案情况。

### Q103. BiDAF 和 BERT-based QA 的区别是什么？

A：BiDAF 是任务专用架构，依赖 BiLSTM 和 attention flow；BERT-based QA 使用大规模预训练 Transformer，再 fine-tune 预测 start/end span。

### Q104. Sparse retrieval 和 dense retrieval 的区别是什么？

A：Sparse retrieval 基于词面匹配；dense retrieval 基于神经向量语义相似度。

### Q105. Extractive QA 和 generative QA 的区别是什么？

A：Extractive QA 从上下文中抽取答案 span；generative QA 生成答案文本，可以综合多个证据但更可能产生幻觉。

### Q106. Retriever-reader 和 FiD 的关系是什么？

A：FiD 可以看作一种结合 retriever 和 generative reader 的架构，retriever 提供 passages，decoder 融合这些 passages 生成答案。

### Q107. 为什么 Open-Domain QA 是 RAG 的前身或基础？

A：ODQA 已经包含“先检索外部知识，再基于证据回答”的思想，这正是 RAG 的核心。

### Q108. 为什么 QA evaluation 比普通分类任务更复杂？

A：答案可能有多种表述、多个合理范围、部分正确情况和不可回答情况，因此很难用单一 exact label 完整评价。

## 公式与术语速记

### Q109. `P, Q -> A` 表示什么？

A：表示给定 paragraph `P` 和 question `Q`，输出 answer `A`。

### Q110. `C=<c1,...,cn>` 表示什么？

A：表示 context 是由 `n` 个 token 组成的序列。

### Q111. `Q=<q1,...,qm>` 表示什么？

A：表示 question 是由 `m` 个 token 组成的序列。

### Q112. extractive neural RC 中的输出约束是什么？

A：输出 start index 和 end index，满足 `1 <= start <= end <= n`。

### Q113. EM 的取值通常是什么？

A：通常是 0 或 1，表示预测答案是否与参考答案完全匹配。

### Q114. QA F1 如何直观理解？

A：它衡量预测答案和参考答案之间的 token overlap，并综合 precision 和 recall。

### Q115. BM25 属于什么类型的 retrieval？

A：BM25 属于 sparse retrieval 或 classical information retrieval。

### Q116. DPR 属于什么类型的 retrieval？

A：DPR 属于 dense passage retrieval。

### Q117. `top-k passages` 是什么？

A：Retriever 根据相似度返回的前 `k` 个最相关 passage。

### Q118. unanswerable question 是什么？

A：给定文档或知识源中没有足够证据回答的问题。

## 考前作答模板

### Q119. 如果考“为什么 QA 是语言理解 test bed”，应该怎么答？

A：说明 QA 需要理解问题、理解文本、定位或生成答案；很多 NLP 任务可转化成 QA，因此能综合测试语言理解能力。

### Q120. 如果考“解释 Reading Comprehension”，应该怎么答？

A：给出 `P,Q->A`，说明给定 passage 和 question，系统从 passage 内容中回答；若是 extractive QA，则答案是 passage 中的 span。

### Q121. 如果考“SQuAD 的贡献和局限”，应该怎么答？

A：贡献是提供大规模监督 RC 数据并推动神经模型发展；局限是答案通常在短 passage 中，缺少真实检索、歧义和无答案情况。

### Q122. 如果考“EM 和 F1 的区别”，应该怎么答？

A：EM 要完全匹配，严格但简单；F1 看 token overlap，能给部分正确答案分数，但仍依赖词面匹配。

### Q123. 如果考“BiDAF 和 BERT 的区别”，应该怎么答？

A：BiDAF 是专门设计的 RC 架构，使用 BiLSTM 和双向 attention；BERT 是预训练 Transformer，通过 fine-tuning 预测答案 span，性能更强且工程更少。

### Q124. 如果考“为什么 Open-Domain QA 更难”，应该怎么答？

A：答检索和阅读两个难点：系统必须先从大规模知识库找到相关 evidence，再基于 evidence 抽取或生成答案。

### Q125. 如果考“Natural Questions 为什么重要”，应该怎么答？

A：它来自真实 Google queries，使用完整 Wikipedia 页面，包含歧义、长答案和不可回答问题，推动了 ODQA、RAG 和长上下文建模。

### Q126. 如果考“解释 FiD”，应该怎么答？

A：FiD 先检索多个 passages，将每个 question-passage pair 独立编码，再让 decoder 对所有 passage 表示做 attention 并生成答案。

### Q127. 如果考“FiD 为什么是现代 RAG 的基础”，应该怎么答？

A：因为它明确结合检索和生成，让模型基于外部 passages 生成答案，并能在 decoder 中融合多个证据。

### Q128. 如果考“retrieval-based QA 的错误来源”，应该怎么答？

A：错误可能来自 retriever 没找到证据、reader/generator 理解错误、多个证据冲突，或评价指标无法认可合理答案。

