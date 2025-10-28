# Contents
Datasets for the "Generate but Verify: Answering with Faithfulness in RAG-based Question Answering" paper by _Simone Filice, Elad Haramaty, Guy Horowitz, Zohar Karnin, Liane Lewin-Eytan, and Alex Shtoff_, to appear in AACL-2025. The paper is about evaluating methods for generating answers that are faithful to context retrieved from a knowledge base.

The datasets in this repository were derived from the following:
- Natural Questions [1]
- BioASQ-QA [2]
- NoMiracl [3]


# Dataset adaptations
All adaptations were done to serve the purpose of the paper, which is quantifying the ability to generate answers that are faithful to the retrieved context from a knowledge base, and to adhere to a computational budget while remaining with datasets of considerable size to achieve significance.

-  **Natural questions**: This dataset comes with questions and answer without passages. We sampled 5,000 QA pairs at random. For each question us augmented with top 5 passages from an index built off Wikipedia (Dec 2018) with E5-base-v2 embeddings, and marked as either containing and not containing an answer to the question using a judge model.
-  **BioASQ-QA**: We used the BioASQ12 subset, out of which we selected the questions marked as factoid.  The data-set comes with relevant passages, and to obtain irrelevant passages, we retrieved the top-10 related passages from an index built from PubMed, and discarded the passages containing the ground-truth answer.
-  **NoMIRACL**: The original data-set comes with questions and passages, each marked as containing and not containing the answer, but does _not_ contain an answer to each question.
   *  We prompted Claude 3.5 Sonnet to generate an answer from _only_ the passages marked as those containing an answer.
   *  We shuffled the passage order in order to eliminate model bias during generation.
   *  We consider only the English part of the dataset.


# Dataset format
Each parquet file contains the following columns:
- _question (str)_: the question
- _answer (str)_: the ground-truth answer that is faithful to the relevant passages. NA if no relevant passages exist
- _passages (list[str])_: the list of passages used for answer generation
- _passages\_relevance_ (list[bool]): a 0/1 indicator for each passage containing the information required for answering the question.
- _relevance (bool)_: a 0/1 indicator for the existance of at least one relevant passage.
- _approx_token_n (int)_: approximate number of tokens in the passages

---
# References
[1]: Kwiatkowski, Tom, et al. "Natural questions: a benchmark for question answering research." Transactions of the Association for Computational Linguistics 7 (2019): 453-466. \
[2]: Krithara, Anastasia, et al. "BioASQ-QA: A manually curated corpus for Biomedical Question Answering." Scientific Data 10.1 (2023): 170. \
[3]: Thakur, Nandan, et al. "“Knowing When You Don’t Know”: A Multilingual Relevance Assessment Dataset for Robust Retrieval-Augmented Generation." Findings of the Association for Computational Linguistics: EMNLP 2024. 2024.
