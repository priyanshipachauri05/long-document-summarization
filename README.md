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


