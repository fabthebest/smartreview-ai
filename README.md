# SmartReview AI

**I built a sentiment analysis system from zero Python knowledge to a production-ready transformer, in one notebook.**

[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)](https://python.org)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.0+-EE4C2C?logo=pytorch&logoColor=white)](https://pytorch.org)
[![HuggingFace](https://img.shields.io/badge/HuggingFace-Transformers-FFD21E?logo=huggingface&logoColor=black)](https://huggingface.co)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/186Tu09NoetTWf6Cq97aHauxdcKe1ijoM?usp=sharing)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)

## What this project does

SmartReview AI analyzes movie reviews and predicts whether they are positive or negative. It does this four different ways, each more powerful than the last, inside a single Colab notebook that runs top to bottom.

The project exists to teach AI/ML/DL/NLP through building, not through theory. Every phase produces a working model. Every failure teaches something specific. Every result connects to the next phase.

## Results

| Phase | Method | Accuracy | Training time | What it proves |
|-------|--------|----------|--------------|----------------|
| 1 | Exploratory analysis | n/a | instant | Simple features (word count, caps ratio) have near-zero correlation with sentiment. Max correlation: 0.17. You cannot solve this problem without ML. |
| 2 | TF-IDF + Logistic Regression | **88.3%** | 0.4 seconds | Word frequency alone gets you surprisingly far. "worst" carries a weight of -8.57, the single most powerful signal in the entire vocabulary. Negative language is more distinctive than positive language. |
| 3 | LSTM neural network (PyTorch) | **81.1%** | ~3 minutes | Training a neural network from scratch on 25K samples with 5M parameters is unstable. The model learns but plateaus well below the classical baseline. This is the experiential proof that pre-training matters. |
| 4 | DistilBERT (fine-tuned) | **91.4%** | 12 minutes | A transformer pre-trained on 3.3 billion words crushes every previous approach. It has 14x more parameters than the LSTM but does not overfit because those parameters already encode the structure of English. |

## Visual results

| Phase 1: Feature correlations | Phase 2: Confusion matrix |
|:---:|:---:|
| ![Phase 1]<img width="1389" height="989" alt="phase1_exploration" src="https://github.com/user-attachments/assets/742423a7-d9d9-49ba-a81b-a665610a6292" />
 | ![Phase 2]<img width="715" height="590" alt="phase2_confusion" src="https://github.com/user-attachments/assets/e85e2889-930f-4299-8f8b-8f2ffddd47f9" />
 |

| Phase 3: LSTM overfitting | Phase 4: Attention heatmap |
|:---:|:---:|
| ![Phase 3]<img width="1189" height="490" alt="phase3_training" src="https://github.com/user-attachments/assets/121499ab-7bee-4bad-a181-e4b8488bf523" />
 | ![Phase 4]<img width="941" height="790" alt="phase4_attention" src="https://github.com/user-attachments/assets/85bc61b6-f091-4bc2-8dc1-b12f42ca50ba" />
 |

## What the attention heatmap reveals

The sentence "The movie was not good but the acting was brilliant" produces three visible patterns in the transformer's attention:

**Negation binding.** "not" attends to itself at 0.19 and to "was" at 0.11. It locks onto its local clause and modifies "good" contextually. TF-IDF treated "not" and "good" as two independent signals. The transformer understands they form one unit.

**Semantic grouping.** "brilliant" attends to itself at 0.20 and to "acting" at 0.14. The model groups "the acting was brilliant" as a coherent positive statement, separate from the negative first clause.

**The pivot word.** "but" has the strongest self-attention in the sentence at 0.18. It acts as a separator between the negative clause and the positive clause. The LSTM would have had to carry the meaning of "not good" through its hidden state across this word. The transformer reads the entire sentence in parallel.

## The most interesting errors

The model's most confident mistakes (99.8% confidence, wrong prediction) are reviews like "Stupidly beautiful. This movie epitomizes the 'so bad it's good' genre." The model predicts NEGATIVE. The IMDB label says POSITIVE. The model is arguably correct: the text IS negative about the film. The reviewer enjoys it precisely because it is bad. This is intentional sarcasm that even humans interpret differently depending on context.

These errors reveal the boundary of what current NLP can do. Sarcasm, irony, and "so bad it's good" sentiment require understanding the reviewer's intent, not just their words. This is an open research problem.

## How I learned from this

I did not study AI theory first. I ran code, observed results, changed one variable, and watched what broke.

Changing `learning_rate` from 2e-5 to 0.1 in Phase 4 destroyed the model. This taught me that pre-trained weights need gentle updates. Changing `max_features` from 10,000 to 100 in Phase 2 dropped accuracy to ~75%. This taught me that vocabulary size controls model capacity. Watching the LSTM score 81.1% while the transformer scored 91.4% with the same data taught me that architecture and pre-training matter more than training time.

The method: run it, change one thing, re-run, observe the difference. The understanding comes from the delta, not from the textbook.

## Project structure

```
SmartReview_AI.ipynb          # Complete notebook, Phases 1 to 4
README.md                     # This file
results/
  phase1_exploration.png      # Feature correlation heatmap
  phase2_confusion.png        # Confusion matrix
  phase3_training.png         # LSTM training curves
  phase4_attention.png        # Transformer attention heatmap
```

## Upcoming phases

**Phase 5** will add LLM generation and RAG with ChromaDB. The Phase 4 embeddings become a semantic search engine: "find reviews about battery life" returns the most relevant reviews even if they never use the word "battery."

**Phase 6** will introduce reinforcement learning concepts. A reward model scores generated summaries, and Direct Preference Optimization aligns the generator with human quality standards.

**Phase 7** will ship the whole system as a product. FastAPI backend, React dashboard, Docker deployment, Stripe billing. The end state: a SaaS tool that companies pay for.

## Run it yourself

1. Open the notebook in [Google Colab](https://colab.research.google.com)
2. Set runtime to T4 GPU (Runtime > Change runtime type)
3. Run every cell from top to bottom
4. Total time: ~20 minutes for all 4 phases

## Tech stack

Python, pandas, NumPy, Matplotlib, Seaborn, scikit-learn, PyTorch, HuggingFace Transformers, Google Colab

## License

Apache 2.0
