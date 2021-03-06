##

+++++++++++++++++++++++++++++++  
<ins>Authors</ins>: I. Chalkidis et al.  
<ins>Date</ins>: 2020-10  
<ins>Tags</ins>: `bert`, `text classification`, `sequence tagging`    
+++++++++++++++++++++++++++++++  


### Intro

- Being pre-trained on generic corpora, BERT has been reported to under-perform in specialised domains, such as biomedical or scientific text. Consequently, recent work studied the effect of further pre-training BERT on domain-specific texts (BioBERT, SciBERT, ClinicalBERT, NetBERT).
- One shortcoming is that all previous work does not investigate the effect of varying the number of pre-training steps.
- More importantly, when fine-tuning for the downstream task, all previous work blindly adopts the hyper-parameter selection guidelines of Devlin et al. (2019) without further investigation.
- Finally, no previous work considers the effectiveness and efficiency of smaller models (e.g., fewer layers) in specialised domains. The full capacity of larger and computationally more expensive models may be unnecessary in specialised domains, where syntax may be more standardized, the range of topics discussed may be narrower, terms may have fewer senses etc.


### Contributions

- In this paper, they explore several approaches for applying BERT models to downstream legal tasks, evaluating on multiple datasets.
- They propose a systematic investigation of the available strategies when applying BERT in specialised domains, which are:
  1. Use BERT out of the box;
  2. Further pre-train (FP) BERT on domain-specific corpora;
  3. Pre-train BERT from scratch (SC) on domain specific corpora with a new vocabulary of sub-word units.


### Key findings

1. Further pre-training (FP) or pre-training BERT from scratch (SC) on domain specific corpora, performs better than using BERT out of the box for domain-specific tasks (both strategies are mostly comparable in three legal datasets).
2. Exploring a broader hyper-parameter range, compared to the guidelines of Devlin et al. (2019), can lead to substantially better performance.
3. Smaller BERT-based models can be competitive to larger, computationally heavier ones in specialised domains.


***

### 1. Pre-training

- Training corpora: **12 GB** of diverse English legal text from several fields (e.g., legislation, court cases, contracts) scraped from publicly available resources.
- Legal-BERT-FP (further pre-training): While Devlin et al. (2019) suggested additional steps up to 100k, they also pre-train models up to **500k** to examine the effect of prolonged in-domain pre-training when fine-tuning on downstream tasks.
- Legal-BERT-SC (scratch): they use a newly created **vocabulary** of equal size to BERT’s vocabulary. They also experiment with **LEGAL-BERT-SMALL**, a substantially smaller model, with 6 layers, 512 hidden units, and 8 attention heads (35M parameters, 32% the size of BERT-BASE).

### 2. Experiments

- <ins>Legal NLP Tasks</ins>
  - They evaluate their models on **text classification** and **sequence tagging** using 3 datasets:
    - EURLEX57K: a large-scale multi-label text classification dataset of EU laws.
    - ECHR-CASES: contains cases from the European Court of Human Rights and can be used for binary and multi-label text classification.
    - CONTRACTS-NER: dataset for named entity recognition on US contracts consisting of three subsets (namely, *contract header, dispute resolution*, and *lease details*).
- <ins>Hyperparameter tuning strategy</ins>
  - As a rule of thumb to fine-tune BERT for downstream tasks, Devlin et al. (2019) suggested a minimal hyperparameter tuning strategy relying on a gridsearch on the following ranges: 
    - learning rate {2e-5; 3e-5; 4e-5; 5e-5};
    - number of training epochs {3,4};
    - batch size {16, 32};
    - fixed dropout rate of 0.1. 
  - These not well justified suggestions are blindly followed in the literature. 
  - They found that some models still underfit after 4 epochs, the maximum suggested, thus they use early stopping based on validation loss, without a fixed maximum number of training epochs. 
  - They also consider an additional lower learning rate (1e-5) to avoid overshooting local minima, and an additional higher drop-out rate (0.2) to improve regularization. 
  - They noticed that their enriched grid-search (tuned) has a substantial impact in most of the end-tasks compared to the default hyper-parameter strategy.
- <ins>Results</ins>
  - Concerning the optimal number of (further) pre-training steps of Legal-BERT-FP (100k vs 500k):
    - The results are very similarr for EURLEX57K, while it seems that fewer pre-training steps (100k) gives better results for ECHR-CASES and CONTRACTS-NER (except for the *contract header* subset).
  - Concerning the most performant model on legal tasks:
    - For language modeling (LM):
      - A LEGAL-BERT variant always leads to lower perplexities (ppl) than both BERT-BASE and BERT-BASE tuned (for all three datasets).
      - LEGAL-BERT-FP and LEGAL-BERT-SC have similar perplexities than all other models (except for CONTRACTS-NER where SC is better).
    - For text classification on EURLEX57K:
      - BERT-BASE is far less performant (by ~0.8-7.9% F1).
      - BERT-BASE fine-tuned performs as good as LEGAL-BERT-FP and LEGAL-BERT-SC (0.1-0.2% F1 difference).
      - LEGAL-BERT-SMALL is slightly less performant than LEGAL-BERT-FP and LEGAL-BERT-SC (by 0.2-0.9% F1).
    - For text classification on ECHR-CASES:
      - For multi-label classification, LEGAL-BERT-SC performs best among all other models (then comes LEGAL-BERT-FP (-0.6% F1), LEGAL-BERT-SMALL (-1.0% F1), BERT-BASE finetuned (-2.5% F1) and BERT-BASE (-3.4% F1).
      - For binary classification, LEGAL-BERT-SC performs best while all other models perform equally.
    - For NER on CONTRACTS-NER:
      - The resuls are highly dependent on the subset concerned, there is no clear winner.
      
  
