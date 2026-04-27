# L14 Retrieval-Augmented Generation 考试复习 QA

这份是一套面向考试的总复习问答，不按页码整理，而是按整份讲义的知识结构整理。重点覆盖 open-domain QA、retriever-reader pipeline、sparse/dense retrieval、efficient search、RAG 的基本思想、hallucination/attribution，以及 GraphRAG 和 Agentic RAG。

## 高频核心题

### Q1. RAG 是什么？

A：Retrieval-Augmented Generation 是让生成模型在回答前或回答中检索外部知识，并把检索到的证据作为条件生成答案。

### Q2. RAG 为什么重要？

A：它把 LLM 的 parametric memory 和外部 non-parametric memory 结合起来，可提升事实性、更新性、可追溯性和长尾知识覆盖。

### Q3. RAG 与普通 closed-book LLM 的区别是什么？

A：closed-book LLM 只依赖参数中的知识；RAG 会显式访问文档库、网页、数据库或图结构等外部来源。

### Q4. RAG 与只把所有文档塞进 long context 的区别是什么？

A：RAG 先选择相关证据再生成，成本更低、更聚焦；long context 依赖模型在大量输入中自行定位信息。

### Q5. RAG 的基本模块有哪些？

A：通常包括 corpus/index、retriever、可选 reranker、generator/reader，以及 attribution 或 verification 模块。

### Q6. RAG 最适合解决哪类问题？

A：需要外部事实、领域知识、最新信息、长尾实体或可引用证据的问题。

### Q7. RAG 的基本风险是什么？

A：检索可能漏掉关键证据、取回错误证据，生成器也可能忽略证据或编造答案。

### Q8. RAG 的核心评价不只看什么？

A：不只看最终答案是否正确，还要看检索证据是否相关、是否支持答案、引用是否可靠。

## Open-Domain QA 与 Retriever-Reader

### Q9. Open-domain QA 是什么？

A：给定自然语言问题，系统需要从大规模开放文档集合中找到答案，而不是只在给定 passage 内回答。

### Q10. Open-domain QA 与 reading comprehension 的区别是什么？

A：reading comprehension 通常给定 passage；open-domain QA 需要先检索相关 passage，再阅读并回答。

### Q11. Retriever-reader pipeline 是什么？

A：retriever 从大语料库中选出 top-k 相关文档，reader/generator 基于这些文档生成或抽取答案。

### Q12. Retriever 的输入和输出是什么？

A：输入是问题和文档集合，输出是若干候选 passages 或 documents。

### Q13. Reader 的输入和输出是什么？

A：输入是问题和检索文档，输出是答案、答案 span 或生成式回答。

### Q14. DrQA 的基本思想是什么？

A：用 TF-IDF 检索 Wikipedia 段落，再用在阅读理解数据上训练的神经 reader 抽取答案。

### Q15. DrQA 为什么是 RAG 的重要前身？

A：它明确展示了“先检索、再阅读”的 open-domain QA 范式。

### Q16. Distant supervision 在 open-domain QA 中有什么用？

A：当没有人工标注的证据段落时，可用包含答案字符串的段落作为弱监督训练 reader。

### Q17. FiD 是什么？

A：Fusion-in-Decoder 是一种把多个检索文档分别编码，再在 decoder 中融合信息生成答案的方法。

### Q18. FiD 的关键优势是什么？

A：它允许模型在生成阶段整合多个 passages，而不是过早把证据压成单一表示。

## Sparse Retrieval

### Q19. Sparse retrieval 是什么？

A：用稀疏词项向量表示 query 和 document，并根据词项重叠计算相关性。

### Q20. TF-IDF 的核心直觉是什么？

A：一个词在 query/document 中越频繁越重要，但如果它在所有文档中都常见，则区分度较低。

### Q21. TF 表示什么？

A：Term frequency，表示某个词在文本中出现的频率或次数。

### Q22. IDF 表示什么？

A：Inverse document frequency，表示词在整个文档集合中的稀有程度。

### Q23. 为什么 stop words 的 IDF 通常较低？

A：因为它们出现在大量文档中，不能有效区分文档主题。

### Q24. Sparse retrieval 通常如何比较 query 和 document？

A：把它们表示为 TF-IDF 向量，再用 cosine similarity 或类似打分函数比较。

### Q25. Sparse retrieval 的优点是什么？

A：简单、可解释、索引高效，尤其擅长精确词匹配和专有名词匹配。

### Q26. Sparse retrieval 的局限是什么？

A：它依赖词面重叠，难以处理同义改写、语义匹配和跨语言表达。

### Q27. BM25 与 TF-IDF 的关系是什么？

A：BM25 是经典 sparse retrieval 打分方法，可看作对 TF-IDF 思想的改进，加入长度归一化和饱和效应。

## Dense Retrieval

### Q28. Dense retrieval 是什么？

A：用神经编码器把 query 和 passage 映射到 dense vectors，再用向量相似度检索。

### Q29. Dense retrieval 为什么能处理语义匹配？

A：因为相似语义可被映射到邻近向量，即使词面不完全重合也能检索到相关文档。

### Q30. Bi-encoder 是什么？

A：query 和 passage 分别独立编码成向量，然后用 dot product 或 cosine similarity 打分。

### Q31. Bi-encoder 的优点是什么？

A：passage 向量可以预先计算和索引，因此适合大规模检索。

### Q32. Cross-encoder 与 bi-encoder 的区别是什么？

A：cross-encoder 联合编码 query 和 passage，精度通常更高但不能高效预索引；bi-encoder 更适合初筛检索。

### Q33. Dense Passage Retrieval 的核心贡献是什么？

A：用双编码器学习 query/passage 表示，使 open-domain QA 不再完全依赖词面匹配。

### Q34. Dense retrieval 的局限是什么？

A：可能漏掉精确实体匹配，受训练数据分布影响，并需要近似最近邻索引支持大规模搜索。

### Q35. MIPS 是什么？

A：Maximum Inner Product Search，即在大量向量中找与 query 向量内积最大的候选。

### Q36. FAISS 的作用是什么？

A：FAISS 提供高效向量索引和近似最近邻搜索，用于大规模 dense retrieval。

### Q37. IVF/k-means indexing 的直觉是什么？

A：先把向量空间聚类，查询时只搜索相关簇，从而减少需要比较的向量数量。

### Q38. 为什么 dense retrieval 需要 efficient search？

A：文档库可能包含百万到十亿级 passages，逐一计算相似度成本太高。

## RAG 生成机制

### Q39. RAG 中 non-parametric memory 指什么？

A：外部文档库、数据库或知识源，不存储在模型参数中，可更新、可检索。

### Q40. RAG 中 parametric memory 指什么？

A：模型参数中学到的语言和世界知识。

### Q41. RAG 的经典概率思想是什么？

A：对多个检索文档作为 latent evidence 进行边缘化，让生成答案依赖可能相关的文档集合。

### Q42. RAG-Sequence 和 RAG-Token 的区别是什么？

A：RAG-Sequence 对整段输出使用同一组文档；RAG-Token 可在不同 token 生成时依赖不同文档。

### Q43. 为什么 top-k retrieval 对 RAG 很关键？

A：generator 通常只能看到有限证据，top-k 决定了可用信息的上限。

### Q44. RAG 中 retriever 和 generator 可以 end-to-end 训练吗？

A：可以，经典 RAG 工作强调 retrieval 和 generation 可联合优化，但实际系统也常采用模块化训练。

### Q45. 为什么 RAG 可以提升 long-tail QA？

A：长尾知识不一定稳定存于参数中，但可以通过外部文档显式提供。

### Q46. POPQA 这类 benchmark 想测试什么？

A：测试模型对长尾实体和事实知识的回答能力，以及 RAG 对 parametric knowledge 缺口的补救。

### Q47. RAG 为什么有助于知识更新？

A：更新外部索引比重新训练大模型更便宜、更快。

### Q48. RAG 为什么不能完全消除 hallucination？

A：模型可能检索错、读错、忽略证据，或在证据不足时继续生成看似合理的答案。

## Hallucination 与 Attribution

### Q49. Hallucination 在 RAG 里指什么？

A：生成内容与事实或检索证据不一致，或者凭空编造未被证据支持的信息。

### Q50. RAG 如何降低 hallucination？

A：它提供可检查的外部证据，让生成更受检索文档约束。

### Q51. Attribution 是什么？

A：为生成答案提供来源、引用或证据连接，说明答案中哪些内容由哪些文档支持。

### Q52. 为什么 attribution 对 RAG 很重要？

A：它让用户能检查答案依据，也能帮助系统评估答案是否 faithful to evidence。

### Q53. Direct generated attribution 是什么？

A：模型在生成答案时同时生成引用或来源标记。

### Q54. Post-retrieval answering 是什么？

A：先检索并固定证据，再让模型基于这些证据回答，引用通常来自已检索材料。

### Q55. Post-generation attribution 是什么？

A：先生成答案，再尝试为答案中的声明寻找支持证据。

### Q56. Attribution 评价中的 supported 是什么？

A：答案声明能被引用证据直接支持。

### Q57. Attribution 评价中的 contradictory 是什么？

A：引用证据与答案声明相矛盾。

### Q58. Attribution 评价中的 extrapolatory 是什么？

A：答案超出了证据直接支持范围，可能是合理推断，也可能是不可靠扩展。

### Q59. 为什么“有引用”不等于“答案正确”？

A：引用可能不相关、只支持部分内容，或被模型错误解读。

### Q60. RAG 系统中常见的 failure mode 有哪些？

A：检索失败、证据排序错误、context overload、生成器忽略证据、引用不 faithful。

## RAG Taxonomy 与新方向

### Q61. RAG taxonomy 可以从哪些维度看？

A：检索粒度、证据如何融入模型、检索频率，以及是否使用多步/agentic 流程。

### Q62. Retrieval granularity 是什么？

A：检索单位的大小，例如 token、句子、段落、文档、表格行或图节点。

### Q63. Evidence incorporation 是什么？

A：检索结果如何进入模型，例如拼接进 prompt、作为 encoder 输入、或通过 memory/cross-attention 使用。

### Q64. Retrieval frequency 是什么？

A：系统检索一次、多次，还是在生成过程中动态检索。

### Q65. GraphRAG 是什么？

A：把文档组织成实体、关系、社区或图结构，并在图上检索和聚合信息来辅助生成。

### Q66. GraphRAG 适合什么场景？

A：适合多跳问题、关系密集知识、企业文档、全局摘要和需要跨文档聚合的任务。

### Q67. GraphRAG 相比普通 vector RAG 的优势是什么？

A：它显式建模实体关系和全局结构，更容易处理多跳与社区级信息。

### Q68. GraphRAG 的潜在代价是什么？

A：需要构建和维护图，抽取实体关系可能出错，系统复杂度更高。

### Q69. Agentic RAG 是什么？

A：让 agent 动态决定何时检索、检索什么、是否改写 query、是否调用工具或继续验证。

### Q70. Agentic RAG 与一次性 RAG 的区别是什么？

A：一次性 RAG 固定检索后回答；Agentic RAG 可迭代规划、检索、阅读、反思和修正。

### Q71. Agentic RAG 的优势是什么？

A：更适合复杂任务和多步问题，能根据中间结果调整检索策略。

### Q72. Agentic RAG 的风险是什么？

A：流程更慢、更贵，也可能因为错误规划或错误工具调用累积失误。

## 考前作答模板

### Q73. 如果考“RAG 的核心思想”，应该怎么答？

A：说明它把外部检索和生成结合，用 non-parametric memory 补充 LLM 参数知识，提高事实性、更新性和可追溯性。

### Q74. 如果考“sparse vs dense retrieval”，应该怎么答？

A：sparse 依赖词项重叠、可解释且高效；dense 用神经向量做语义匹配，更能处理改写但依赖训练和向量索引。

### Q75. 如果考“RAG 如何减少 hallucination”，应该怎么答？

A：答外部证据约束生成、支持 attribution；同时指出检索错或引用不 faithful 时仍会 hallucinate。

### Q76. 如果考“retriever-reader pipeline”，应该怎么答？

A：先从大语料中检索 top-k passages，再由 reader/generator 基于证据抽取或生成答案。

### Q77. 如果考“GraphRAG/Agentic RAG”，应该怎么答？

A：GraphRAG 用图结构建模关系和全局信息；Agentic RAG 用多步 agent 动态规划检索、验证和回答。
