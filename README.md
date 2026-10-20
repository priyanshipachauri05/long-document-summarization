### Long Document Summarization

#### A. Project Overview

This project explores hierarchical summarization od long documents through a reproduction of the research paper _Context-Aware Hierarchical Merging for Long Document Summarization_ by Litu Ou and Mirella Lapata, University of Edinburgh.

This work investigates how contextual information can be selected and incorporated during recursive summary merging, with goal of reducing information loss and error propagation by LLMs while summarising the documents that exceeds the context window of the Large Language Models.

This implementation examines different context-selection and integration strategies and evaluates their behavior using established NLP evaluation metrices.

#### B. Background and motivation

Long documents like the ones encountered while going through research papers, legal documents that takes much time to read and analyze. Nowadays, Large Language Models are often used for summarising the documents, but due to their limited context window it is unlikely that the summary generated will be reliable. 

Since the length of document offers problem hence it is convenient to divide the document into smaller chunks and summarise them individually and then merge the summaries hierarchically to get the final summary of the document. But summarising chunks independently could eventually lead to losing the relationship between the sections. Also, hierarchical merging introduces the problem of error propagation in which the error at lower level of hierarchical merging stages propagates to the higher level and affects the authenticity of the final summary.

Therefore, the research paper suggested the approach of context-aware hierarchical merging, where the context from the original chunk is added while merging the lower-level summaries so as to ensure that the error is minimized and the final summary contains all the facts of the original document.

The research paper compares three context retrieval techniques (Extract, Retrieve and Cite-Based) along with two context merging techniques (Support and Replace) resulting in 6 context-aware hierarchical merging techniques- Extract-Support, Extract-Replace, Retrieve-Support, Retrieve-Replace, Cite-Support and Cite-Replace. Along with them, there two baseline models- Zero-shot and HMerge to compare the context-aware summarization model.

#### Methodology

1. Document chunking

   The input document is divided into a sequence of smaller non-overlapping chunks based on           predefined maximum chunk size. Each chunk represents an independent unit that can be processed     by the LLM without exceeding its context window.

2. Chunk Summarization

   Each chunk is summarized by the Llama 3.1 Instant Large Language Model. Each document is given     as an input to the LLM with a summarization prompt to generate an abstractive summary. The
   abstractive summaries ensure the generated summaries contain the key information rather than
   extracting the sentences from the original text.

3. Context Selection

   Three methods of context selection were employed namely, Retrieve, Extract and Cite

   3.1 Retrieve-based Context Selection

   It is based on RAG (Retrieve Augmented Generation) method, here the summary is used as a query
   to the LLM to retrieve the relevant context from the chunk, and then use the retrieved context
   and the generated summary and merge them into the summary for higher level.

   3.2 Extract-based Context Selection

   The Extract context selection strategy identifies the most informative sentences from the
   source document using frequency-based sentence ranking approach. The document is first divided
   into individual sentences, after which a word frequency table is constructed by counting the
   occurrence of each word in the document. Every sentence is then assigned a score based on the
   cumulative frequencies of the words it contains. Sentences with higher scores are considered
   more informative and are selected as the contextual evidence for subsequent summarization.

   3.3 Citation-based Context Selection

   During lower-level summary generation, the input passage is assigned a unique citation label and
   labeled passages are provided to the Llama 3.1 model, which generate the summary containing the
   citations to the passage used during summary generation. Then while context selection, the
   source passage with highest ranked citations are selected as context for subsequent stages of
   hierarchical merging.

4. Context Merging

   Two Context merging techniques were introduced in the research paper: Support and Replace. In the Replace Integration strategy, the intermediat
   summary is replaced with the context selected from the source document. In the Support Integration strategy, the previous
   hierarchical level summary is retained and supplemented with the selected contextual evidence.

#### Experimental results

<img width="745" height="692" alt="image" src="https://github.com/user-attachments/assets/30dd7bdc-9598-494d-a6a4-1fb5706cae9a" />

#### Observations

   o ROUGE scores: Cite + Support has the highest ROUGE-1 and ROUGE-L scores, while Cite + Replace shares the highest ROUGE-2 score with H-Merge.
   
   o Semantic Similarity: The BERTScore values are relatively close across the most methods, although Cite + Support reported score is notably lower.
   
   o Context Integration: Support and Replace produce different results depending on the context selection strategy, suggesting that their effects should be examined            together rather than individually.
   
   o No single method dominates all the reported metrics, making it important to evaluate summarization quality from multiple perspective.
