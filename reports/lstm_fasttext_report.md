# Report: LSTM with Pre-trained FastText Embeddings

## Summary

This report analyzes the performance of a Long Short-Term Memory (LSTM) model combined with pre-trained FastText embeddings (`wiki-news-300d-1M-subword.vec`). This experiment aimed to determine if the combination of a more complex recurrent architecture (LSTM) and a more advanced embedding technique (FastText with subword information) could bridge the "semantic gap" and effectively classify the specialized "Banglish" dataset. The embedding layer was kept frozen (`trainable=False`).

## Model Architecture

The model uses an LSTM layer and an embedding layer configured for the 300-dimensional FastText vectors.

| Layer (Type) | Output Shape | Param # | Trainable |
| :--- | :--- | :--- | :--- |
| `embedding` (Embedding) | (None, 192, 300) | 17,048,100 | No |
| `lstm` (LSTM) | (None, 64) | 93,440 | Yes |
| `dropout` (Dropout) | (None, 64) | 0 | - |
| `dense` (Dense) | (None, 32) | 2,080 | Yes |
| `dense_1` (Dense) | (None, 1) | 33 | Yes |
| **Total** | | **17,143,653** | |
| **Trainable** | | **95,553** | |
| **Non-trainable**| | **17,048,100** | |

## Performance Metrics

The model completely failed to learn, with performance metrics indicating a result no better than random guessing. The F1-score is extremely low at 0.0874.

| Metric | Score |
| :--- | :--- |
| **Accuracy** | 0.5094 |
| **F1-Score** | 0.0874 |
| **AUC-ROC** | 0.5026 |

### Classification Report

The report shows a critical failure to identify the positive class, with a recall score of only **0.05** for "Depressed (1)".

```
                   precision    recall  f1-score   support

Not Depressed (0)       0.50      0.97      0.66      2190
    Depressed (1)       0.63      0.05      0.09      2192

         accuracy                           0.51      4382
        macro avg       0.57      0.51      0.38      4382
     weighted avg       0.57      0.51      0.38      4382
```

### Confusion Matrix

The confusion matrix confirms that the model misclassified 2089 positive samples as negative, indicating it could not learn any distinguishing features.

```
[[2129   61]
 [2089  103]]
```

## Complexity Analysis

| Type | Metric | Value |
| :--- | :--- | :--- |
| **Time** | Total Training Time | 657.61 seconds (~11.0 minutes) |
| | Average Inference Time | 2.7294 ms per sample |
| **Space**| Total Parameters | 17,143,653 |
| | Model Size on Disk | 66.16 MB |

## Key Findings and Analysis

This experiment provides one of the most conclusive results of the entire study.

1.  **Reinforcing the "Semantic Gap":** Even when combining our most complex recurrent cell (LSTM) with our most advanced pre-trained embedding model (FastText), the model failed to learn. This provides definitive proof that the **"Semantic Gap"** between the English-language embeddings and the transliterated Bengali data is the single most critical barrier to performance.

2.  **The Embedding is the Bottleneck:** This result, when compared with previous experiments, demonstrates that the choice of RNN architecture (GRU, LSTM) is a secondary concern. The primary factor determining success or failure is the relevance of the word embeddings. The principle of **"Garbage In, Garbage Out"** applies: even a sophisticated model cannot produce meaningful results from uninformative input features.

3.  **Final Verdict on Frozen Embeddings:** The consistent failure of all models using frozen GloVe and FastText embeddings strongly concludes that this approach is not viable for this specific problem. The results serve as a powerful justification for the necessity and superiority of the custom-trained Word2Vec approach.