## Deep learning in law: early adaptation and legal word embeddings trained on large corpora

+++++++++++++++++++++++++++++++  
<ins>Authors</ins>: I. Chalkidis et al.  
<ins>Date</ins>: 2018-12  
<ins>Tags</ins>: `survey`, `legal word vectors`, `information retrieval`, `text classification`, `information extraction`  
+++++++++++++++++++++++++++++++


### Intro

- The application of Deep Neural Networks in legal analytics has increased significantly. Deep Neural Networks have been rapidly replacing rule-based approaches, dictionary-based models and traditional machine learning techniques, which in theirmajority require intensive manual feature engineering. The reason lies in the fact of their poor performance when dealing with the words polysemy, synonyms and the multiplicity of the way humans imprint and structure text. The above-said techniques fall short in capturing language semantics and complicated linguistic structures along with their long-distance relationships, as humans do.

- Traditionally, researchers incorporated manually crafted knowledge bases and patterns to capture legal concepts, terms of interest and synonyms that were defined beforehand. Many works relied on the structure of the legal documents to be able to segment and process text. Nevertheless, the presumed structure was not consistent across different laws and legislations. The language is heterogeneous, the legal concepts evolve and the maintenance of legal knowledge bases is tedious and expensive.

- Their primary focus is to explore the word feature representations that are commonly used to feed Neural Networks or capture semantic similarities among text snippets.


### Contributions

- In this survey, they discuss a representative selection of the most notable recent articles that incorporate Deep Learning methods, oriented to address different tasks on legal corpora. They particularly focus on three main fields: 
  - **text classification**: they present some representative works relevant with to the categorization of textual units, such as sentences, paragraphs, sections or even long documents;
  - **information extraction**: they review characteristic articles related to sequence labeling (tagging) tasks, such as chunking and named-entity recognition;
  - **information retrieval**: they sort works that tackle the problem of retrieving articles of interest out of a collection of legal documents or articles that entail a query.
- They also share pre-trained legal word embeddings model (called **Law2Vec**) based on the word2vec skip-gram model over large corpora of English legislations (123,000 documents).

***

### A. Text classification

#### Classifying sentential modality in legal language: a use case in financial regulations, acts and directives (O’Neill et al., 2017)

- Classical logic may be ineffective in classifying sentential modality as modality in legal language strongly relies on the use of modal verbs which have multiple functions and may also be misused in several occasions. These are strong indications that the context in each case is very important for deciphering the actual role of these modal verbs, hence a machine-learning approach seems more promising.
- The authors experimented with a training set consisting of ~1,900 annotated (by domain-experts) sentences including obligations, prohibitions, and permissions.
- The authors experimented with various methods, including **Logistic Regression, SVMs, AdaBoost, Gradient Boosting and Random Forests**. Those methods were examined using two alternative feature representations:
  - Baselines: n-grams, POS tags and normalized tf-idf scores;
  - Sentence concatenations of word embeddings: Google embeddings (pre-trained from Google news articles) and legal embeddings (pre-trained over a corpus of 7,500 EU legislation documents).
- The authors also experimented with neural network methods including a **Multi-Layer Perceptron (MLP)** with 2 hidden layers, unidirectional and bidirectional **LSTMs**, followed by a fully-connected layer, multi-filter **CNNs**, and CNNs followed by LSTMs and a fully-connected hidden layer. All neural methods were examined using two alternative feature representations. First, only the Google vectors were considered. Second, the authors used their domain-specific pre-trained word and phrase embeddings, to feed LSTMs or CNNs.
- The conclusions were the following:
  - Classic ML algorithms were vastly overfitting the training set.
  - All Neural Networks (except the naive MLP) outperformed the classic ML algorithms by over 10% on average. This indicates the superiority of RNNs and more specifically LSTMs when dealing with complex language semantics.
  - BiLSTMs was the best-of method, outperforming the unidirectional LSTMs and naive CNNs approaches.
  - Providing domain-specific (legal) embeddings further improved the performance of the neural methods.
  

#### Obligation and prohibition extraction using hierarchical RNNs (Chalkidis et al., 2018)

- They improved the state-of-art on sentence modality classification by proposing a more refined scheme of annotations that incorporate sub-sentence classification.
- The authors experimented with an in-house dataset of ~9,400 sections from the main bodies of a hundred randomly selected English service agreements. The sections were then split into ~45,100 sub-sentences annotated by law students.
- They used domain-specific pre-trained word embeddings and POS embeddings obtained by applying word2vec to ~750K and ~50K English contracts respectively, as well as token shape embeddings (e.g., all capital, all digit). Each token was represented by the concatenation of its word, POS and shape embeddings.
- They experimented with several **LSTM-based methods**.
- The conclusions were the following:
  - The self-attention mechanism led to an overall improvement compared to the plain BiLSTM.
  - The hierarchical model significantly outperformed the other methods.
  - The hierarchical model is faster to train than the previous ones, even though it had more parameters.
  
#### Inducing predictive models for decision support in administrative adjudication ([Branting et al., 2017](branting2017inducing.md))

- They experimented with predictive models for assisting Administrative Adjudication.
- The authors two datasets for training:
  - Board of Veterans Appeals (BVA) cases: ~3,800 4-class samples and ~1,600 2-class (*met* or *unmet*) samples.
  - World Intellectual Property Organization (WIPO) related to complaints about domain names that possibly violated trademarks: ~5,600 2-class (*transferred* or *not transferred*) samples (but quite imbalance with a 10-to-1 ratio).
- The authors experimented with several methods:
  - Baseline: **SVMs** that operate on n-gram frequency vectors.
  - Neural network: **Hierarchical Attention Network (HAN)** that has a 2-level encoding mechanism for text classification tasks. In the first level, a BiGRU encoder to produce context-aware word embeddings, which are subsequently summed up based on an attention mechanism that indicates the weighting scheme to form sentence representations. In the second level, the BiGRU produce context-aware sentence representations that are also followed by a similar attention mechanism to create a final document (paragraph) representation. The latter finally passes through a fully-connected layer and softmax function in order to be classified. Here, the authors processed documents that comprised multiple sections (paragraphs), so they extended the former model with an intermediate fully-connected layer, which combined the underlying paragraph representations in order to form a deeper document representation (NB: the authors stated that a 3rd GRU encoder on top of the paragraph encoders was also examined, but the fully-connected layer performed better).
- The neural model achieved a mean F1 of 73.8% in BVA cases while the SVM achieved a mean F1 of 73.1%. For the WIPO dataset, the neural model achieved a mean F1 of 94.4% while the SVM achieved 95%. Hence, it is not clear wheter one approach performed better.


#### Discussion

- In the presented articles, gated RNNs (i.e., LSTMs, GRUs) were utilized as the main component of the systems (neural networks) in order to provide context-aware/ task-specific word representations.
- The two latter articles exploited hierarchical networks to better encode information with respect to the text structure and the cross-segment relationships.
- However, there is no extended research with respect to CNN-based architectures that are rapidly introduced in text classification tasks, offering competitive results while being trained much faster than the gated RNNs.

***

### B. Information extraction

#### Recurrent neural network‑based models for recognizing requisite and effectuation parts in legal texts (Nguyen et al., 2018)

- They proposed several approaches that utilize Deep Learning models to recognize requisite and effectuation parts in legal texts (i.e., to solve the RRE task, which can be modeled as a sequence labeling problem).
- The relationship between requesite and effectuation parts is the following: requisite part ⇒ effectuation part. Requisite and effectuation parts are constructed from one or more logical parts such as antecedence parts, consequence parts, and topic parts. Each logical part carries a specific meaning of legal texts according to its type. A consequent part describes a law provision, an antecedent part describes cases or the context in which the law provision can be applied, and a topic part describes subjects related to the law provision.
- They used two related datasets for the given task:
  - Japanese National Pension Law RRE dataset (JPL-RRE): contains sentences that were segmented into chunks. Each chunk was labelled using lower level categories (topic parts, antecedent, and consequent parts).
  - Japanese Civil Code RRE dataset (JCC-RRE): includes the English-translated version of the Japanese Civil Code and was manually annotated with three type of logical parts: requisite, effectuation and exception parts.
- The authors represented each token using the following features: word embeddings (self-trained by the networks or pre-trained using word2vec over a
collection of Japanese legal documents), self-trained POS tag embeddings, and self-trained chunk embeddings (verb and noun phrases).
- The authors experimented with four derivatives of **BiLSTM-CRF**:
  - A single BiLSTM-CRF model: This model was initially fed with word, POS tag, and chunk for each token, which were embedded in three individual vectors representations. These three embeddings were then concatenated and passed into a bidirectional lstm chain, which produced context-aware token embeddings. Forward and backward context-aware token embeddings were then concatenated and passed into a chain of linear CRFs, which classified each token.
  - Three consecutive BiLSTM-CRF models: each model predicted another group of tags (requisite, effectuation, exception) and was trained independently.
  - A cascaded network consisting of three BiLSTM-CRF models: trained jointly based on the losses from all CRF layers. Each BiLSTM-CRF sub-model was fed with the initial feature representations (word, pos tag, chunk), concatenated with the bilstm outputs of the previous sub-models. Each CRF layer predicted another group of tags (requisite, effectuation, exception).
  - A cascaded network comprised three BiLSTM-MLP-CRF models: similar to the previous one, while it also incorporated one or two fully-connected layers between each bidirectional LSTM chain and the corresponding CRF layer. The initial feature representations (word, pos tag, chunk) are concatenated with the MLP outputs of the previous sub-models, instead of the BiLSTM outputs.
- Results:
  - For the JPL-RRE dataset, the BiLSTM model outperformed the CRF baseline by 4.46%.
  - For the JCC-RRE dataset, the joint BiLSTM-MLP-CRF model with two fully-connected hidden layers performed better that all other models and outperformed by 4.54% the CRF baseline.
  
#### A deep learning approach to contract element extraction (Chalkidis et al., 2017)

- They focused on extracting contract elements (e.g., contractor names, legislation references, start and end dates, amounts), a task which is similar to named entity recognition.
- They used a dataset of ~3,500 English contracts, annotated with eleven types of contract element.
- The authors examined three alternative **LSTM-based methods**:
  - A bidirectional LSTM chain fed with the concatenation of word, pos tag and token-shape (custom embedding representingd six possible shapes of tokens: uppercase, lowercase, first character uppercase, digits, punctuations, other) embeddings, which produced context-aware token embeddings. The context-aware token embeddings passed through a fully-connected layer followed by a sigmoid activation function.
  - Same as first method but injected an additional unidirectional (forward) LSTM chain between the first bidirectional LSTM chain and the fully-connected layer.
  - Same as first method but instead of using a fully connected layer or additional LSTMs, it employed a linear chain of CRFs.
- The authors evaluated the new methods in two different manners. Firstly, they evaluated the performance of classification token by token. Secondly, they evaluated the performance of classification over complete contract elements, which were multi-word expressions.
- Results for the per token evaluation:
  - The three LSTM-based methods performed better than Logistic Regression and linear Support Vector Machines operating on fixed-sized sliding windows of tokens (their previous work).
  - The second method obtained top F1 scores for all but the *contract period* type.
- Results for the per element evaluation:
  - Overall, BiLSTM-CRF appeared to be better than the second method.
  
#### Discussion

- In the aforementioned articles, LSTMs were utilized in order to provide context-aware/task-specific word representations.
- Both articles reported great performance improvements utilizing BiLSTM-CRF models compared to traditional ML algorithms (i.e., SVMs, LR, CRFs), while they also reported marginal improvement compared to networks that do not employ CRFs on top of LSTMs.

***

### C. Information retrieval

This section discusses seven papers that address the retrieval of relevant excerpts of text with respect to a particular query. The majority of the papers address the **legal question answering task** of the Competition on Legal Information Extraction/ Entailment (COLIEE) from 2014 to 2017. Particularly, the competition focuses on two aspects related to a binary (yes/no) question answering as follows:
  - Phase 1: reading a question Q and extract the legal articles of the Civil Code that are relevant to the question. 
  - Phase 2: returning a yes or no answer if the retrieved articles from phase one entail or not the question Q.


#### A convolutional neural network in legal question answering (Kim et al., 2015)

- In phase 1 of COLIEE:
  - They introduced an ad-hoc information retrieval method for retrieving Japan civil law articles related to a given question by employing a **TF–IDF weighting method** to capture the correlation of a query to an article, according to the word sets overlapping. The parameters were normalized to prevent a bias towards longer documents, which is a well-known error in ranking methods.
  - The **Ranking SVM model** was alternatively applied to rank relevant documents according to users’ feedback, where the three types of features were used: binary representations of lemmas, dependency pairs in order to capture the prominent semantic content and TF–IDF scores.
- In phase 2 of COLIEE:
  - They introduced a binary classification model for *yes/no* answering to the legal queries. 
  - The authors assumed that the correct answer has a high semantic similarity to a question. 
  - The semantic representation of the questions was comprised of word embeddings and linguistic features. 
  - They trained a **CNN-based classifier** based on a triple <img src="https://render.githubusercontent.com/render/math?math=(q_i, a_{ij},y_i)">, where <img src="https://render.githubusercontent.com/render/math?math=q_i"> was the question, <img src="https://render.githubusercontent.com/render/math?math=a_{ij}"> was the *j*th sentence of the *i*th article, and <img src="https://render.githubusercontent.com/render/math?math=y_i"> was the response (i.e., yes or no). The classifier learned the probability <img src="https://render.githubusercontent.com/render/math?math=p(y=yes| q, a)"> of a sentence being relevant to a question.
  - Authors trained 50-dimensional word embedding with the word2vec model based on the provided data.
  - The accuracy of the proposed CNN model with the pre-trained word embeddings and other relevant linguistic features outperformed a linear SVM model, as also a rule-based model with K-means clustering. The highest accuracy reported was 63.87%.

  
#### Legal question answering system using neural attention (Morimoto et al., 2017)

- In phase 1 of COLIEE:
  - The methodology used to identify the similarity of a query to a civil law article comprised the extraction of the requirement (condition), the effect (conclusion) in law articles and the examined query T.
  - The authors extracted the legal requirement and effect parts from queries and articles using rule-based (i.e., pattern-matching) methods. 
  - The distance between a query Q and an article T was calculated as the sum of the **Word Movers Distances** of the required parts of Q and T and the Word Movers Distances between the effect parts of Q and T. 
  - The articles to be retrieved should exceed a pre-defined threshold, that was tuned until the optimal one was identified.
- In phase 2 of COLIEE:
  - The entailment model was based on Neural Networks with attention mechanism. The civil_vec[i] was the vector representation of the i-th article, which was a TF-IDF weighted sum of the word representations (embeddings) in the article, while t2_vec was the vector representation of the query given the same formula. The calculation of the attention was done by the inner product of the transpose of civil_vec and t2_vec, which led to a single vector whose size was the number of articles.
  - The authors pre-trained domain-specific word embeddings of 50 and 200 dimensions using word2vec on judgment documents extracted by the Japanese Supreme Judicial Court.
  - The classification task was tackled by introducing a **multi-layer perceptron (MLP)**, which received as inputs the concatenated attention_civil and t2_vec vectors.
  - The highest accuracy reported was 66.67%.
  
#### Legal information retrieval using topic clustering and neural networks (Nanda et al., 2017)

- In phase 1 of COLIEE:
  - They used a combination of a partial string matching and a topic clustering method in order to tackle the Information Retrieval task.
  - Initially, they applied a simple **pattern matching method** by capturing the similarity of a Question Q and an answer A based on the intersection of the common words. The problem with that approach is that due to the polysemy and synonymy of words, some queries may be relevant to particular articles without significant intersection of the words used.
  - Therefore, the authors introduced a method that relies upon the semantic similarity. They used the **Latent Dirichlet Allocation (LDA) topic model** to represent each article and query with a topic vector.
- In phase 2 of COLIEE:
  - They introduced a system which combines **LSTMs with CNNs**.
  - The authors used the 300-dimensional word embeddings from the Google News vectors to represent the articles and the queries.
  - The neural model is the following:
    - The sentential sequences of the article and the query concatenated to a 600-dimensional vector which was fed into the LSTM.
    - The LSTM layer projected 64-dimensional vectors that were fed sequentially into a convolutional layer of six filters. A maxpooling layer retained the most prominent features.
    - In the end, two fully connected layers were added, where the latter is followed by the softmax function to distribute values between 0 and + 1 for representing non-entailment and entailment respectively.


#### Legal question answering using ranking SVM and deep convolutional neural network (Do et al., 2017)

- In phase 1 of COLIEE:
  - A **Ranking SVM model** was used on the feature vectors of the query-article (QA) pair.
  - For feature engineering, they considered the **TF–IDF, Euclidean, Manhattan, Jaccard distances, as also the Latent Semantic Indexing (LSI), and Latent Dirichlet Allocation (LDA)** using the gensim library.
- In phase 2 of COLIEE:
  - The QA pair-fed a binary **CNN classifier** for question answering.
  
  
#### Matching law cases and reference law provision with a neural attention model (Tang et al., 2016)

- They tackled the problem of finding law provisions relevant to law cases.
- They proposed a neural attention model for automatically matching reference law provisions.
- The complexity of the problem was due to the fact that each law case is related to many law provision references, based on the case specifics. Rule-based and keyword-based methods were not effective enough to capture the semantics of legal documents.
- The model is summarized as follows:
  - The 50-dimensional word vectors used to represent the input words were trained from scratch, with an embedding layer as part of the neural network.
  - Two independent **bidirectional LSTMs** with the same output dimensionality were used to build context-aware representations for the words of the case and the law provision.
  - A word-by-word self-attention mechanism was used on top of each LSTM layer to produce the case and law provision representations (vectors).
  - The Cosine and Euclidean distance were used to identify the element-wise and absolute distance of the vectors.
  - An LSTM was fed with both vector distances and, at the last layer, a linear layer, and a log-softmax produced the output label.


#### A semi‑supervised training method for semantic search of legal facts in Canadian immigration cases (Nejadgholi et al., 2017)

- They introduced a semi-supervised approach to identify factasserting sentences from Canadian Immigration cases that are semantically close to a given query.
- A single annotated dataset was used for training 100-dimensional word embeddings and a semi-supervised classifier that consisted of 46,000 immigration and refugee cases.
- The authors selected a semi-supervised approach using an annotated dataset of 12,220 annotated sentences from 150 cases.
- A binary classifier was trained to distinguish sentences between *facts* and *non-facts*.
- The classifier was a **fully-connected shallow neural network** with one hidden layer fed with the concatenationof word embeddings of each sentence.
- The trained classifier was then deployed over an unlabelled corpus of sentences, which includes a broader vocabulary, to create additional automatically annotated data in order to re-train the classifier and improve the sentence encoder.
- The cosine similarity of a query and a fact sentence was calculated, based on the vector representation produced by the hidden layer of the trained classifier. For every single query, the top three most similar fact sentences were selected as relevant.
- The proposed method outperformed commonly used classifiers when the training set is relatively small. Particularly, the proposed model was significantly the most accurate classifier reaching an accuracy of 90%. SVMs with TF–IDF feature representation and domain-specific word embeddings achieved 81% and 83% respectively.


#### Discussion

- **CNN-based models** have been widely adopted in order to tackle information retrieval tasks.
- The research community is moving from intensive feature engineering towards more simplified networks that encode the text inputs by using standalone CNNs or combined with LSTMs.
- Researchers improve the performance by adding additional features produced by various methods (e.g., **LDA, LSI, BM25** and well-known word distances).
