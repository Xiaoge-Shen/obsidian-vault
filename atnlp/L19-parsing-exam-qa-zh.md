# L19 Syntax and Parsing 考试复习 QA

这份是一套面向考试的总复习问答，不按页码整理，而是按整份讲义的知识结构整理。重点覆盖 syntax 的作用、phrase-structure 与 dependency parsing、grammar properties、PCFG、parser learning、tree linearisation、constituent parsing metrics、Transformer parsers、span scoring、CYK/dynamic programming、factored attention 和解析结果。

## 高频核心题

### Q1. Syntax 在 NLP 中为什么重要？

A：Syntax 描述词如何组合成短语和句子，帮助模型理解结构、歧义和长距离关系。

### Q2. Parsing 是什么？

A：Parsing 是为句子构建句法结构，例如 constituency tree 或 dependency tree。

### Q3. 句法解析可以帮助哪些任务？

A：机器翻译、信息抽取、语义解析、问答、关系抽取和语言学分析等。

### Q4. 为什么现代 neural models 仍然值得研究 parsing？

A：即使 LLM 隐式学习结构，显式 parsing 仍有可解释性、可控性、评测和结构泛化价值。

### Q5. 句法结构解决什么歧义？

A：例如 attachment ambiguity，即一个短语到底修饰哪个成分。

### Q6. Phrase-structure parsing 是什么？

A：把句子分析为嵌套 constituents，如 NP、VP、PP 等短语结构。

### Q7. Dependency parsing 是什么？

A：把句子表示为词与词之间的 head-dependent 关系。

### Q8. Constituency 和 dependency 的核心区别是什么？

A：constituency 关注短语边界和层级；dependency 关注词之间的依存关系。

## Syntax 与 Grammar

### Q9. Phrase-structure tree 的叶子是什么？

A：句子中的词或 tokens。

### Q10. Phrase-structure tree 的内部节点是什么？

A：短语类别或非终结符，如 NP、VP、S。

### Q11. Dependency tree 的节点是什么？

A：通常是词。

### Q12. Dependency tree 的边表示什么？

A：一个词作为 head 支配另一个 dependent，边可带依存关系标签。

### Q13. 好的 grammar 应满足哪些性质？

A：能覆盖语言现象、避免过度生成、结构可解释、可学习，并能支持高效解析。

### Q14. Overgeneration 是什么？

A：grammar 生成了不合法或不自然的句子。

### Q15. Undergeneration 是什么？

A：grammar 无法生成实际合法的句子。

### Q16. Ambiguity 在 grammar 中是什么意思？

A：同一句子可能有多个合法 parse trees。

### Q17. 为什么自然语言 grammar 很难写？

A：语言有大量例外、歧义、递归结构、领域差异和非标准表达。

### Q18. Parser 的目标不只是判断什么？

A：不只是判断句子是否合法，还要找出最可能或最有用的结构。

## PCFG

### Q19. Context-free grammar 包含什么？

A：非终结符、终结符、产生式规则和起始符号。

### Q20. CFG rule 的形式是什么？

A：例如 `A -> beta`，表示非终结符 `A` 可展开为符号串 `beta`。

### Q21. PCFG 是什么？

A：Probabilistic Context-Free Grammar，为每条 CFG rule 赋予概率。

### Q22. PCFG 中同一左侧规则概率应满足什么？

A：对同一非终结符左侧的所有展开规则，概率和为 1。

### Q23. PCFG 如何计算一棵 parse tree 的概率？

A：把树中所有使用的产生式规则概率相乘。

### Q24. PCFG parser 通常找什么？

A：给定句子，找概率最高的 parse tree。

### Q25. PCFG 的优点是什么？

A：简单、可解释，能把结构规则和概率偏好结合起来。

### Q26. PCFG 的局限是什么？

A：独立性假设强，规则概率缺少上下文，难以捕捉词汇和长距离依赖。

### Q27. Lexicalised PCFG 想解决什么？

A：把 head word 信息纳入规则概率，以更好处理词汇选择和 attachment。

## Parser Learning

### Q28. Supervised parser learning 需要什么？

A：需要 treebank，即带有人工 parse tree 标注的句子。

### Q29. Supervised parsing 的优点是什么？

A：标注结构明确，训练目标清晰，通常性能较高。

### Q30. Supervised parsing 的缺点是什么？

A：treebank 标注昂贵，语言和领域覆盖有限。

### Q31. Unsupervised grammar induction 是什么？

A：在没有 gold parse trees 的情况下，从原始句子中学习 grammar 或结构。

### Q32. Unsupervised parsing 为什么困难？

A：可行结构很多，缺少明确监督，模型可能学到与人类标注不一致的结构。

### Q33. “Grammar as a foreign language” 的核心思想是什么？

A：把 parse tree 线性化成序列，让 seq2seq 模型像翻译一样从句子生成树表示。

### Q34. Tree linearisation 为什么有用？

A：它把结构预测问题转化为序列生成问题，便于使用 neural sequence models。

### Q35. Tree linearisation 的风险是什么？

A：生成的序列可能不是合法树，且结构约束不如显式 parsing algorithm 强。

### Q36. 为什么 grammar learning 和 parsing 不完全一样？

A：grammar learning 学规则或结构分布；parsing 是给具体句子找结构。

## Evaluation

### Q37. Constituent precision 是什么？

A：预测 constituents 中有多少比例出现在 gold tree 中。

### Q38. Constituent recall 是什么？

A：gold constituents 中有多少比例被模型预测出来。

### Q39. F1 在 parsing 中是什么？

A：precision 和 recall 的调和平均，用于综合评价 constituent overlap。

### Q40. 为什么只看 sentence-level exact match 不够？

A：完全匹配太严格，无法反映模型部分结构预测正确的情况。

### Q41. Unlabelled constituent evaluation 是什么？

A：只比较短语 span 是否正确，不要求非终结符标签正确。

### Q42. Labelled constituent evaluation 是什么？

A：既要求 span 正确，也要求短语标签正确。

### Q43. 为什么 parsing evaluation 可能受标注体系影响？

A：不同 treebank 对短语类别、空节点、协调结构等有不同标注约定。

### Q44. Dependency parsing 常用哪些指标？

A：UAS 和 LAS，分别评估 head 是否正确以及 head 和 dependency label 是否都正确。

## Transformer Parsing

### Q45. Transformer-based parser 的基本思想是什么？

A：用 Transformer 编码句子上下文，再对可能 constituents 或 actions 打分。

### Q46. Kitaev & Klein parser 的核心方向是什么？

A：用 self-attention 表示句子，并通过 span scoring 和 chart parsing 预测 constituency tree。

### Q47. Span-based constituency parsing 是什么？

A：为句子中每个可能 span 预测是否构成 constituent 以及对应标签。

### Q48. Span score 表示什么？

A：某个 `[i, j]` 区间作为特定 constituent 的得分。

### Q49. 为什么 span 数量是 `O(n^2)`？

A：长度为 `n` 的句子有所有可能起止位置组合。

### Q50. 为什么 parsing 需要全局结构约束？

A：局部高分 spans 可能互相交叉或无法组成合法树，需要全局最优解码。

### Q51. Chart parsing 在这里有什么作用？

A：在所有合法树中动态规划寻找总 span score 最高的结构。

### Q52. CYK algorithm 适用于什么？

A：适用于 context-free grammar 的动态规划解析，特别是二叉化 grammar。

### Q53. CYK 的基本递推是什么？

A：对每个 span 尝试所有 split，把左右子 span 的最佳得分与当前规则或 span 得分组合。

### Q54. Backpointers 的作用是什么？

A：记录最佳 split 和子结构，便于从 chart 中恢复完整 parse tree。

### Q55. 为什么 dynamic programming 适合 parsing？

A：parse tree 有重叠子问题，较大 span 的最优结构可由较小 span 的最优结构组成。

### Q56. Parsing complexity 为什么值得关注？

A：句子越长，可能结构数量快速增长；高效解码对于实际 parser 很重要。

## Factored Attention 与 Neural Details

### Q57. Factored attention 的动机是什么？

A：让 self-attention 更好地区分内容和位置信息，帮助句法结构建模。

### Q58. Content attention 关注什么？

A：关注 token 表示之间的语义或词汇相关性。

### Q59. Position attention 关注什么？

A：关注相对或绝对位置关系，帮助模型感知 span 和层级结构。

### Q60. 为什么 position information 对 parsing 特别重要？

A：constituents 是连续 span，句法结构强依赖 token 顺序和边界。

### Q61. Neural parser 相比 PCFG 的优势是什么？

A：能利用上下文表示、词汇语义和大规模预训练，提高解析准确率。

### Q62. Neural parser 的局限是什么？

A：可解释性较弱，可能依赖数据偏差，并且结构约束需要额外解码保证。

### Q63. 为什么预训练表示能提升 parsing？

A：预训练模型已学习大量词法、语义和句法模式，parser 可在此基础上更容易预测结构。

### Q64. Transformer 是否天然输出合法 parse tree？

A：不是。通常需要 span decoding、chart parsing 或其他约束方法产生合法树。

## 概念对比题

### Q65. Constituency parsing 与 dependency parsing 如何选择？

A：如果关心短语结构和层级，用 constituency；如果关心词间 head-dependent 关系，用 dependency。

### Q66. PCFG 与 neural parser 的区别是什么？

A：PCFG 用显式概率规则；neural parser 用 learned representations 和打分函数，通常性能更强但解释更难。

### Q67. Top-down 和 bottom-up parsing 的区别是什么？

A：top-down 从根节点展开；bottom-up 从词或小 span 组合成大结构。

### Q68. Local decision 与 global decoding 的区别是什么？

A：local decision 单独判断局部结构；global decoding 找整体合法且得分最高的 tree。

### Q69. Grammar induction 与 treebank supervised parsing 的区别是什么？

A：grammar induction 无 gold trees，目标更不确定；supervised parsing 直接学习人工标注结构。

### Q70. 为什么 syntax 不是只属于传统 NLP？

A：现代神经模型仍需要处理结构泛化、层级组合和可解释性，syntax 提供重要分析工具。

## 考前作答模板

### Q71. 如果考“phrase-structure vs dependency”，应该怎么答？

A：phrase-structure 表示短语层级和 constituents；dependency 表示词与词的 head-dependent 关系。

### Q72. 如果考“PCFG 如何给树赋概率”，应该怎么答？

A：说明每条 rule 有概率，同一左侧概率和为 1，一棵树概率是所有使用规则概率的乘积。

### Q73. 如果考“parsing F1”，应该怎么答？

A：precision 看预测 constituents 中多少正确，recall 看 gold constituents 中多少被找回，F1 是二者调和平均。

### Q74. 如果考“Transformer constituency parser”，应该怎么答？

A：用 Transformer 编码句子，为所有 spans 和 labels 打分，再用 chart/CYK-style dynamic programming 找合法最高分树。

### Q75. 如果考“为什么需要 dynamic programming”，应该怎么答？

A：解析树空间巨大，但 spans 有重叠子问题；DP 可复用子 span 最优解并通过 backpointers 恢复最佳树。
