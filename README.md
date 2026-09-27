# SinBrief: A Hybrid Framework for Abstractive Text Summarisation of Sinhala Legal Documents

SinBrief is a hybrid abstractive summarisation framework for Sinhala legal documents that does not require human-annotated training data. The framework combines domain-aware word graph construction with a sentence scoring mechanism to generate abstractive summaries from Sinhala legal text.

---
## Repository Structure

```text
SinBrief/
├── Dataset/               SinhaLegal dataset (train/test split)
├── Main_Method/           Summary generation notebooks for all 5 scorer configurations
├── Models/                Sentence scorer training notebooks and CPT training
├── Evaluations/           Evaluation metric computation notebooks
├── Outputs/               Generated summaries and evaluation results
├── Prototype/             Prototype interface for the framework
└── Legal_Keywords.txt     Predefined Sinhala legal keyword list
```

##  Models

Five sentence scoring models are used within the framework:

- mBERT (`bert-base-multilingual-cased`)
- Llama 3.1 8B (`meta-llama/Meta-Llama-3.1-8B`)
- Falcon 7B (`tiiuae/falcon-7b`)
- LASER (`laser_encoders`)
- CPT Llama 3.1 ([SinBrief-Legal-Llama3.1](https://huggingface.co/Minduli-Lasandi/SinBrief-Legal-Llama3.1))

---

##  Pipeline

1. **Sentence Extraction** — Split document by punctuation, filter short sentences
2. **Sentence Clustering** — Group semantically similar sentences using agglomerative clustering
3. **Word Graph Construction** — Build domain-aware directed word graph with legal keyword weighting
4. **Candidate Generation** — Generate abstractive candidates via weighted random walks
5. **Candidate Scoring** — Score candidates using a weakly supervised neural scorer
6. **Summary Generation** — Select top 2 candidates per cluster, apply length control (300–500 characters)

---

##  Dataset

This framework uses the [SinhaLegal](https://github.com/Minduli-Lasandi/SinhaLegal) dataset comprising 1,206 Sinhala legal documents (Acts and Bills), split 80/20 into training (964) and test (242) sets.

---

##  Evaluation Metrics

The following reference-free evaluation metrics were used:

| Metric | Description |
|---|---|
| Coverage | Proportion of summary tokens from extractive fragments |
| Density | Average squared length of extractive fragments |
| Compression Ratio | Ratio of summary length to source length |
| SummaC | Factual consistency via NLI-based scoring |
| Self-BERTScore | Semantic similarity to source document |

The legal keywords indicated in the Legal_Keywords.txt file has been chosen on a word frequency analysis. Additionally this has also been validated by an Attorney-at-Law to check the legal relevance. 

---

## 📦 Requirements

```bash
pip install transformers torch sinling networkx scikit-learn
pip install sentencepiece accelerate bitsandbytes peft
pip install laser_encoders bert_score summac nltk
pip install pandas numpy matplotlib
```

---
##  Prototype 

Run the file in the prototype folder or visit [SinBrief](https://huggingface.co/spaces/Minduli-Lasandi/SinBrief) on Hugging Face to get run the demo.

---
## Acknowledgements

We thank Ms. Sadini Jaburagoda, Attorney-at-Law, for reviewing the Sinhala legal keyword set and providing expert feedback on its domain relevance.
