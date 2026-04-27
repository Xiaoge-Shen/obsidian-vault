# L03 Summarisation 考试复习 QA

这份是一套面向考试的总复习问答，不按页码整理，而是按整份讲义的知识结构整理。建议先背“高频核心题”，再用后面的专题题检查自己是否真的理解。

## 高频核心题

### Q1. 什么是 summarisation？

A：Summarisation 是把较长的一个或多个文档压缩成更短、更连贯的文本，同时保留原文的关键信息、主题和主要观点。

### Q2. 一个好的摘要需要满足哪些基本要求？

A：要简洁、连贯、覆盖核心信息，并尽量避免遗漏重要内容或引入原文没有的事实。

### Q3. 设计一个 summarisation system 时要考虑哪四件事？

A：要考虑摘要类型、训练数据、模型与学习方法、评价方式。

### Q4. summarisation 的主要应用有哪些？

A：文档摘要、新闻摘要、论文摘要、邮件或 Slack 对话摘要、会议行动项提取、文本简化与压缩。

### Q5. 摘要任务最重要的难点是什么？

A：难点在于同时压缩长度、保留重要信息、保持语言流畅，并保证摘要忠实于原文。

## 任务类型

### Q6. single-document summarisation 是什么？

A：它只基于一个源文档生成摘要，例如对一篇新闻文章生成摘要。

### Q7. multi-document summarisation 是什么？

A：它基于多个相关文档生成一个综合摘要，例如把多个关于同一事件的新闻报道整合成一段摘要。

### Q8. 为什么 multi-document summarisation 通常比 single-document summarisation 更难？

A：因为多文档中可能有重复信息、冲突信息、不同观点和不同时间点的信息，而且输入上下文更长。

### Q9. multi-document summarisation 中的 redundancy problem 是什么？

A：多个文档可能反复提到同一事实，系统需要避免在摘要中重复表达相同信息。

### Q10. multi-document summarisation 中的 consistency problem 是什么？

A：不同文档可能包含互相矛盾或时间上不一致的信息，系统需要判断如何保持摘要内部一致。

### Q11. generic summarisation 是什么？

A：Generic summarisation 是不带额外查询或控制条件的普通摘要，目标是概括全文最重要的信息。

### Q12. query-focused summarisation 是什么？

A：它根据一个具体 query 来生成摘要，只总结和该 query 相关的信息。

### Q13. controllable summarisation 是什么？

A：它允许用户控制摘要属性，例如长度、主题角度、情感倾向或关注的 aspect。

### Q14. query-focused summarisation 和 controllable summarisation 的共同点是什么？

A：二者都使用额外输入或参数来影响摘要生成过程。

### Q15. query-focused summarisation 和 controllable summarisation 的区别是什么？

A：Query-focused 主要围绕用户问题或查询筛选信息；controllable 更强调控制输出属性，例如长度、风格或情感。

### Q16. extractive summarisation 是什么？

A：它从原文中选出若干原句作为摘要，不生成全新的句子。

### Q17. abstractive summarisation 是什么？

A：它生成新的语言表达来概括原文，不要求直接使用原文中的句子。

### Q18. extractive 和 abstractive 的核心区别是什么？

A：Extractive 的输出来自原文句子；abstractive 的输出可以重写、合并和概括原文内容。

### Q19. 为什么 extractive summarisation 在 LLM 时代仍然有价值？

A：因为它更保守，通常事实精度更高，幻觉风险更低，适合新闻、法律、医疗等高准确性场景。

### Q20. 为什么 abstractive summarisation 通常更难？

A：因为模型既要理解原文，又要生成新的表达，还要避免重复、遗漏和 hallucination。

## 抽取式摘要建模

### Q21. 如何把 extractive summarisation 建模成二分类问题？

A：把文档表示为句子序列 `d=(s1,...,sn)`，对每个句子预测标签 `yi∈{0,1}`，其中 `1` 表示该句应被选入摘要。

### Q22. 抽取式摘要二分类建模的两个关键问题是什么？

A：第一，训练标签从哪里来；第二，用什么目标函数训练模型。

### Q23. 没有人工句子级标签时，如何构造 extractive summarisation 的训练标签？

A：可以计算原文每个句子与人工参考摘要之间的 ROUGE 分数，把分数较高的句子标为正例。

### Q24. naive extractive summarisation 的训练流程是什么？

A：先用 ROUGE 生成伪标签，再用二分类 log-likelihood 训练模型判断每个句子是否进入摘要。

### Q25. 用 ROUGE 构造句子标签有什么潜在问题？

A：它偏向词面重合，可能把和参考摘要表面相似但不一定最重要的句子标为正例。

### Q26. 为什么 extractive summarisation 的二分类目标和最终摘要质量不完全一致？

A：因为逐句分类只判断单句是否重要，却不直接优化摘要整体的冗余度、连贯性、长度和覆盖面。

### Q27. extractive summarisation 为什么容易有高 precision？

A：因为它直接引用原文句子，不太会创造原文没有的信息。

## 生成式摘要与 Pointer-Generator

### Q28. abstractive summarisation 为什么可以看作 text generation？

A：因为它的目标是生成一段新的自然语言摘要，而不是从原文中挑选已有句子。

### Q29. pointer-generator network 要解决什么问题？

A：它要同时支持从词表生成新词和从源文复制词，从而更好处理专有名词、罕见词和原文关键短语。

### Q30. pointer-generator 的核心公式是什么？

A：`P(w)=p_gen P_vocab(w)+(1-p_gen) Σ_{i:w_i=w} a_i^t`。

### Q31. pointer-generator 公式中 `p_gen` 表示什么？

A：`p_gen` 是一个开关或混合权重，表示当前更倾向于从词表生成词，还是从源文复制词。

### Q32. pointer-generator 公式中 `P_vocab(w)` 表示什么？

A：它表示普通 decoder 从词表中生成词 `w` 的概率。

### Q33. pointer-generator 公式中 `Σ_{i:w_i=w} a_i^t` 表示什么？

A：它表示源文中所有等于 `w` 的位置在当前时间步获得的 attention 权重之和，也就是复制该词的概率。

### Q34. pointer-generator 为什么适合摘要任务？

A：摘要既需要生成自然概括，也需要准确保留原文中的人名、地名、数字和关键术语。

### Q35. coverage mechanism 是什么？

A：Coverage mechanism 会记录过去时间步已经关注过哪些源文位置，用来提醒模型避免反复关注同一内容。

### Q36. coverage mechanism 主要解决什么问题？

A：主要解决早期 seq-to-seq 摘要中常见的重复生成问题。

### Q37. coverage vector 的直观含义是什么？

A：它是过去 attention 的累计，表示源文各位置已经被模型关注了多少。

### Q38. pointer-generator 和 coverage 的关系是什么？

A：Pointer-generator 解决生成与复制的平衡；coverage 解决生成过程中的重复问题，二者可以结合使用。

## 数据集与 Baseline

### Q39. CNN/DailyMail 数据集由什么组成？

A：它由新闻文章和对应的 story highlights 组成，CNN 约 100k 对，Daily Mail 约 200k 对。

### Q40. CNN/DailyMail 的文章和摘要通常有多长？

A：文章平均约 800 词，摘要通常是 3 到 4 句，平均约 56 词。

### Q41. CNN/DailyMail 的 highlights 有什么特点？

A：它们由记者写成，风格较压缩、较 telegraphic，每条 highlight 往往相对独立，不一定形成连贯段落。

### Q42. CNN/DailyMail 为什么适合训练摘要模型？

A：因为它提供了大量新闻正文和人工摘要配对，规模较大，格式清晰。

### Q43. CNN/DailyMail 有什么局限？

A：它的摘要常受新闻结构影响，可能让简单的开头句 baseline 表现很强，也未必代表所有领域的摘要能力。

### Q44. 什么是 LEAD-3？

A：LEAD-3 是直接取文档前 3 句作为候选摘要的 baseline。

### Q45. 为什么 LEAD-3 在新闻摘要中很强？

A：因为新闻常采用倒金字塔结构，最重要的信息往往集中在文章开头。

### Q46. 如果一个复杂模型只略微超过 LEAD-3，这说明什么？

A：说明该数据集可能有明显的位置偏置，也说明模型是否真正理解全文需要进一步检查。

### Q47. XSum 数据集有什么特点？

A：XSum 是极限摘要数据集，通常要求用单句高度概括整篇文章。

### Q48. XSum 和 CNN/DailyMail 的主要区别是什么？

A：XSum 更短、更抽象、更需要概括；CNN/DailyMail 通常更长，且更容易受开头句影响。

### Q49. 为什么 XSum 通常比 CNN/DailyMail 更难？

A：因为 XSum 的摘要更像高度抽象的一句话标题或核心概括，不能简单依赖抽取原文句子。

### Q50. 讲义中还提到了哪些其他 summarisation datasets？

A：包括 XWikis、MLSUM、NewsRoom、arXiv、PubMed、BigPatent、WikiHow、Reddit TIFU 等。

### Q51. 为什么需要不同领域的数据集？

A：因为新闻、论文、专利、说明文和社交媒体的文本结构与摘要目标不同，模型需要在多种场景下被训练和评估。

## 自动评价指标

### Q52. 摘要评价可以从哪三条维度划分？

A：Human vs. automatic、word overlap vs. soft overlap、reference-free vs. reference-based。

### Q53. ROUGE 是什么？

A：ROUGE 是一种基于参考摘要和候选摘要之间 n-gram 重合的自动评价指标。

### Q54. ROUGE 的全称是什么？

A：Recall-Oriented Understudy for Gisting Evaluation。

### Q55. ROUGE-N 的基本思想是什么？

A：计算候选摘要覆盖了多少参考摘要中的 n-gram。

### Q56. ROUGE-N 的公式如何理解？

A：分子是参考摘要中被候选摘要匹配到的 n-gram 数量，分母是参考摘要中的 n-gram 总数。

### Q57. 为什么 ROUGE 更强调 recall？

A：因为摘要评价更关心候选摘要是否覆盖了参考摘要中的重要信息。

### Q58. 为什么 ROUGE 不只看 precision？

A：只看 precision 可能奖励非常短但信息不全的摘要，而摘要评价需要检查重要信息是否被覆盖。

### Q59. ROUGE-L 是什么？

A：ROUGE-L 基于参考摘要和候选摘要之间的最长公共子序列，用来衡量更长片段层面的相似性。

### Q60. ROUGE 的主要局限是什么？

A：它依赖词面重合，难以处理高质量改写，也可能忽略事实错误。

### Q61. BERTScore 是什么？

A：BERTScore 用 BERT 等模型产生的上下文词向量来计算参考摘要和候选摘要之间的语义相似度。

### Q62. BERTScore 为什么叫 soft overlap？

A：因为它不要求词完全相同，而是根据 embedding 相似度进行软匹配。

### Q63. BERTScore 中 precision 的直观含义是什么？

A：候选摘要中的每个 token 是否能在参考摘要中找到语义相近的 token。

### Q64. BERTScore 中 recall 的直观含义是什么？

A：参考摘要中的每个 token 是否能在候选摘要中找到语义相近的 token。

### Q65. BERTScore 相比 ROUGE 的优势是什么？

A：它能更好处理同义改写和语义相近但词面不同的表达。

### Q66. BERTScore 的局限是什么？

A：它仍然偏 token-level similarity，不能完全评价信息组织、事实忠实性和摘要是否真正有用。

### Q67. 为什么 lecture 说自动指标可能在优化错误目标？

A：因为高词面相似度不一定意味着信息正确；比如把 `interesting` 变成 `boring`，词面仍相似但语义完全反了。

### Q68. 为什么 human evaluation 仍然重要？

A：因为人类能更好判断摘要的相关性、流畅度、连贯性、事实忠实性和简洁性。

## 人类评价与 LLM-as-a-Judge

### Q69. 人类评价摘要时常看哪些维度？

A：常看 relevance、fluency、coherence、faithfulness、conciseness。

### Q70. faithfulness 在摘要评价中是什么意思？

A：Faithfulness 指摘要是否忠实于原文，是否避免引入原文没有或与原文矛盾的信息。

### Q71. 为什么 faithfulness 对 abstractive summarisation 特别重要？

A：因为 abstractive 系统会生成新表达，更容易产生 hallucination 或事实错误。

### Q72. pairwise ranking evaluation 是什么？

A：让评价者比较两个摘要，判断哪一个更好。

### Q73. question-based evaluation 是什么？

A：根据参考摘要生成问题，再检查候选摘要是否包含回答这些问题所需的信息。

### Q74. 为什么 question-based evaluation 可以检查 informativeness？

A：因为如果候选摘要能回答由参考摘要生成的问题，说明它保留了关键内容。

### Q75. 什么是 LLMs as a Judge？

A：它是让 LLM 像人类评价者一样对摘要进行排序、打分或判断质量。

### Q76. 为什么可以考虑用 LLM 做 judge？

A：因为评价通常被认为比生成摘要更容易，强 LLM 可能较好地识别摘要质量差异。

### Q77. LLM-as-a-Judge 有哪些风险？

A：它可能对 prompt 敏感，过度依赖表面特征，评价不稳定，也可能有偏见。

### Q78. automatic metrics 和 human/LLM evaluation 的核心区别是什么？

A：自动指标更便宜、可重复，但可能不符合人类偏好；人类或 LLM 评价更接近真实质量判断，但成本更高或稳定性更差。

## 长文本摘要与当代 LLM

### Q79. 为什么 long text summarisation 难？

A：因为输入太长，模型容易遗漏关键信息，且计算成本和上下文限制都会增加。

### Q80. hierarchical-style summarisation 是什么？

A：先把长文切成 chunks，分别摘要，再把局部摘要拼接成新文档继续摘要。

### Q81. hierarchical-style summarisation 的优点是什么？

A：它能绕开上下文长度限制，使模型可以处理更长的文档。

### Q82. hierarchical-style summarisation 的潜在问题是什么？

A：早期 chunk 摘要一旦遗漏信息，后续阶段通常无法恢复；局部摘要之间也可能缺乏全局一致性。

### Q83. 什么是 lost-in-the-middle problem？

A：LLM 处理长文本时，容易更关注开头和结尾，忽略中间部分的重要信息。

### Q84. lost-in-the-middle 对摘要有什么影响？

A：摘要可能低估或遗漏文本中部的重要信息，导致覆盖不均衡。

### Q85. GPT-3 新闻摘要实验说明了什么？

A：人类可能更喜欢 GPT-3 的摘要，但自动指标不一定给它更高分，说明自动指标和人类偏好之间存在错位。

### Q86. 为什么商业 LLM 现在能很好地做中等长度摘要？

A：因为预训练和指令调优让它们具备较强的语言理解、压缩和生成能力，很多场景下直接 prompting 就有效。

### Q87. 当代 LLM 摘要仍然有什么核心问题？

A：仍然可能 hallucinate，也可能在长文本中遗漏重要信息。

### Q88. 当前 summarisation community 更关注哪些方向？

A：更关注多语言数据和更细粒度、更可靠的评价方法。

## 综合比较题

### Q89. Extractive summarisation 和 abstractive summarisation 各有什么优缺点？

A：Extractive 更准确、幻觉少，但不够灵活；abstractive 更自然、更能概括，但更容易遗漏或产生事实错误。

### Q90. ROUGE 和 BERTScore 的区别是什么？

A：ROUGE 主要看 n-gram 词面重合；BERTScore 用上下文 embedding 做语义软匹配。

### Q91. CNN/DailyMail 和 XSum 哪个更适合测试抽象概括能力？

A：XSum 更适合，因为它要求用非常短的一句话高度概括全文，不能简单复制开头句。

### Q92. 为什么 LEAD-3 是评价新闻摘要模型时必须考虑的 baseline？

A：因为它简单但强，如果模型不能明显超过它，就很难证明模型学到了超越新闻位置偏置的能力。

### Q93. 为什么自动评价指标可能和人类评价不一致？

A：自动指标通常依赖和参考摘要的相似度，而人类更关注可读性、信息组织、事实正确性和整体有用性。

### Q94. 为什么 abstractive summarisation 比机器翻译更容易出现评价困难？

A：因为同一篇文章可以有很多合理摘要，信息选择本身就有主观性，而不是只有一种明确对应输出。

### Q95. 如果一个摘要 ROUGE 高但事实错误，应该如何评价？

A：它不能算高质量摘要，因为事实忠实性是摘要质量的核心维度，不能被词面重合弥补。

### Q96. 为什么 summarisation evaluation 本身是一个复杂研究问题？

A：因为摘要质量涉及信息覆盖、语言质量、事实忠实性、简洁性和用户需求，单一指标很难完整衡量。

## 公式与术语速记

### Q97. `d=(s1,...,sn)` 在抽取式摘要中表示什么？

A：它表示一个文档由 `n` 个句子组成。

### Q98. `yi=1` 在抽取式摘要中表示什么？

A：表示句子 `si` 应被选入摘要。

### Q99. `p_gen` 在 pointer-generator 中表示什么？

A：表示生成模式和复制模式之间的混合权重。

### Q100. `a_i^t` 在 pointer-generator 中表示什么？

A：表示时间步 `t` 时模型对源文第 `i` 个位置的 attention 权重。

### Q101. ROUGE-N 中的 `N` 表示什么？

A：表示使用 n-gram 的长度，例如 ROUGE-1 使用 unigram，ROUGE-2 使用 bigram。

### Q102. reference-based evaluation 是什么？

A：用人工参考摘要作为标准，比较候选摘要与参考摘要之间的相似性或信息覆盖。

### Q103. reference-free evaluation 是什么？

A：不依赖参考摘要，直接根据原文和候选摘要评估质量，例如检查忠实性或相关性。

### Q104. hallucination 在摘要中是什么意思？

A：摘要生成了原文没有支持的信息，或生成了与原文矛盾的信息。

### Q105. informativeness 在摘要评价中是什么意思？

A：摘要是否包含足够多的关键信息，能否帮助读者理解原文的主要内容。

## 考前作答模板

### Q106. 如果考“比较 extractive 和 abstractive”，应该怎么答？

A：先定义二者，再比较输出来源、优缺点和风险。Extractive 选原文句子，精度高但不灵活；abstractive 生成新表达，更自然但更容易 hallucinate。

### Q107. 如果考“为什么 multi-document summarisation 更难”，应该怎么答？

A：从三个点答：多文档有重复信息，需要去冗余；多文档可能有冲突，需要保持一致；输入更长，会带来上下文和计算困难。

### Q108. 如果考“解释 pointer-generator”，应该怎么答？

A：说清它是生成和复制的混合模型，用 `p_gen` 控制从词表生成还是从源文复制，因此适合保留专有名词、数字和关键词。

### Q109. 如果考“ROUGE 的局限”，应该怎么答？

A：ROUGE 依赖词面重合，容易低估合理改写，也可能高估词面相似但事实错误的摘要。

### Q110. 如果考“为什么需要 human evaluation”，应该怎么答？

A：因为自动指标不能完整评价 relevance、fluency、coherence、faithfulness、conciseness 等质量维度，尤其难以发现事实错误和信息选择问题。

### Q111. 如果考“LLM-as-a-Judge 的利弊”，应该怎么答？

A：优点是成本低、可扩展，且评价任务相对生成任务更容易；缺点是受 prompt 影响、稳定性不足，可能过度看重表面质量。

### Q112. 如果考“长文本摘要的挑战”，应该怎么答？

A：长文本会带来上下文限制、信息遗漏和 lost-in-the-middle；分层摘要能缓解长度问题，但可能在早期阶段丢失信息。

