## Similarity and Relevance of Court Decisions: A Computational Study on CJEU Cases

+++++++++++++++++++++++++++++++  
<ins>Authors</ins>: K. Moodley et al.  
<ins>Date</ins>: 2019-12  
<ins>Tags</ins>: `text similarity`  
+++++++++++++++++++++++++++++++  


### Intro

- Identification of relevant or similar court decisions is a core activity in legal decision making for case law researchers and practitioners. With an ever increasing body of case law, a manual analysis of court decisions can become practically impossible. As a result, some decisions are inevitably overlooked.
- The advent of text similarity measures (both syntactic and semantic) has meant that potentially relevant cases can be identified without the need to manually read them. However, how close do these measures come to approximating the notion of relevance captured in the citation network?


### Contributions

- In this paper, they use some popular text similarity algorithms to generate a *court decision similarity network (CDSN)* - an analogue of the *court decision citation network (CDCN)* - in which links between decisions imply high textual similarity. The goal of their study is to evaluate the size of overlap between
the CDSNs generated by selected text similarity algorithms and the CDCN.
- They focus on judgements by the Court of Justice of the European Union (CJEU) as published in the EUR-Lex database.
- Their results show that similarity of the full texts of CJEU court decisions does not closely mirror citation behaviour, there is a substantial overlap. In particular, they found syntactic measures surprisingly outperform semantic ones in approximating the citation network.

***

### Notion of relevance

- In law generally, the concept of relevance has been previously studied and there have been attempts to define it for Legal Information Retrieval (LIR) tasks (e.g., 6 types listed: *algorithmic, topical, bibliographic, cognitive, situational* and *domain*). However, to date, there has been no measurable specialisation of this definition for case law.
- Many such LIR systems are implemented in proprietary software such as [ROSS](https://rossintelligence.com) and [Lex Machina](https://lexmachina.com). However, one caveat of to these platforms is that many of they do not explain why they found particular cases relevant, and therefore, it is difficult to measure and benchmark their legal merit.
- In order to establish a benchmark for completeness, we need to capture an understanding of relevance in a legal context, case law in particular. 
  - One possible strategy to achieve this is to solicit legal experts to annotate court decision texts with information (e.g. legal principles, topics and arguments) that they use to evaluate case relevance. This strategy is  ideal for the long-term.
  - An alternative strategy in the short-term that demands less time and resources would be to select our base understanding for relevance to be equivalent to citation as captured in the *court decision citation network (CDCN)*. Accepting this notion of relevance, they compare it to several state-of-the-art text similarity algorithms used to create a *court decision similarity network (CDSN)*. That way, they can evaluate the size of the overlap between the two networks.


### Methodology

- **Corpus**: 13,828 decision texts by the CJEU as published in the EUR-Lex database. They also extracted the citations and subject matters (keywords denoting legal topics that a case deals with) for each case.
- **Case sampling**: analysing all 13,828 CJEU cases would require in the region of 95 million similarity checks for each algorithm. They therefore elected to focus on a sample subset of the CJEU CDCN. They selected three topics of cases for our evaluation based on their currently heightened societal relevance: *data protection* (~40 samples), *social policy* (~700 samples) and *public health* (~180 samples).
- **Selection of text similarity measures**: 
  - *Syntactic measures*: TF-IDF and n-grams (with cosine similarity for computing document similarity), and Jaccard distance.
  - *Semantic measures*: GoogleNews embbedings, Law2Vec embbedings and their own CJEU embbedings pre-trained on their corpus (with cosine similarity and word mover distance for document similarity).
- **Evaluation setup**:
  - For each of their sample case, they calculated the top 20 similar cases.
  - For each similarity link computed by the algorithms, they checked in the CDCN (for the sample cases) if there is a citation link between these same cases. If there is a citation link, we count it as an overlap. They record the overlap counts per case, per case topic and per algorithm.
  
  
### Results

- N-grams with N=5 proved to be the method with the largest overlap. 
- It is a surprise that syntactic measures far outperform semantic measures because it was hypothesised that ambiguity in meaning would be an important factor in legal text. However, although the semantic measures have far lower overlap with the CDCN, they do find overlaps which the syntactic measures miss.
- It is confirmed for all methods that text similarity does not perform well at capturing these citations (most likely because the cases would be textually dissimilar reflecting their different legal topics).
- Cosine similarity was found to be a poor measure of case similarity (in the sense that agrees with the CDCN). WMD is a substantial improvement on cosine similarity but still far behind the performance of the syntactic measures.


### Challenges and limitations

- One limitation of this study is that they only compare similarity links with direct citations from the CDCN. In general, there may be multiple indirect paths between two nodes in the CDCN, and these paths could still capture relevance between cases.
- It also remains an open question of how close they could ever get to reconstructing the citation network (purely from the content of court decisions). Some reasons include: not all court decisions are published online; not all relevant information about a case are published in the text.
- Finally, they adopted citation as the notion of relevance. However, this overlooks other notions of relevance (e.g. where cases are substantively related but the judge forgot to include a citation between them).


### Future work

- Extend the study to understand if the findings we obtained generalise to: (1) other legal topics for cases in the CJEU network, and (2) other court decision corpora (e.g. ECHR decisions).
- They also plan to evaluate additional text similarity measures (both semantic and syntactic). From the semantic perspective, the recent siamese networks appear to be promising, as well as the Latent Dirichlet Allocation (LDA) and Latent Semantic Analysis (LSA) methods for identifying abstract topics from text. Further syntactic approaches include Dice’s coefficient and Manhattan distance.
