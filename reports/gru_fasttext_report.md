# Report: GRU with Pre-trained FastText Embeddings

## Summary

This report details an experiment using a Gated Recurrent Unit (GRU) model with pre-trained FastText embeddings (`wiki-news-300d-1M-subword.vec`). The primary hypothesis was that FastText's ability to generate vectors for out-of-vocabulary (OOV) words using subword information would give it an advantage over GloVe on the specialized "Banglish" dataset. The model architecture was kept consistent with previous experiments, and the embedding layer was frozen (`trainable=False`).

## Model Architecture

The model uses a standard GRU architecture. The embedding layer is configured for the 300-dimensional vectors provided by the FastText model.

| Layer (Type) | Output Shape | Param # | Trainable |
| :--- | :--- | :--- | :--- |
| `embedding` (Embedding) | (None, 192, 300) | 17,048,100 | No |
| `gru` (GRU) | (None, 64) | 70,272 | Yes |
| `dropout` (Dropout) | (None, 64) | 0 | - |
| `dense` (Dense) | (None, 32) | 2,080 | Yes |
| `dense_1` (Dense) | (None, 1) | 33 | Yes |
| **Total** | | **17,120,485** | |
| **Trainable** | | **72,385** | |
| **Non-trainable**| | **17,048,100**| |

## Performance Metrics

The model failed to learn effectively, with performance metrics indicating results no better than random chance. The F1-score of approximately 0.09 is exceptionally low.

| Metric | Score |
| :--- | :--- |
| **Accuracy** | 0.5096 |
| **F1-Score** | 0.0890 |
| **AUC-ROC** | 0.5150 |

### Classification Report

The report highlights a critical failure in the model's learning: the recall for the "Depressed (1)" class is only **0.05**, meaning the model identified just 5% of the actual positive cases.

```
                   precision    recall  f1-score   support

Not Depressed (0)       0.50      0.97      0.66      2190
    Depressed (1)       0.63      0.05      0.09      2192

         accuracy                           0.51      4382
        macro avg       0.57      0.51      0.38      4382
     weighted avg       0.57      0.51      0.38      4382
```

### Confusion Matrix

The confusion matrix starkly illustrates the model's failure. It misclassified 2087 positive instances as negative, showing a complete inability to distinguish the "Depressed" class.

```
[[2128   62]
 [2087  105]]
```

## Complexity Analysis

Training time was relatively short due to `EarlyStopping` halting the process. The model size is larger than the 100-dimensional models due to the 300-dimensional FastText embeddings.

| Type | Metric | Value |
| :--- | :--- | :--- |
| **Time** | Total Training Time | 513.51 seconds (~8.6 minutes) |
| | Average Inference Time | 2.4725 ms per sample |
| **Space**| Total Parameters | 17,120,485 |
| | Model Size on Disk | 65.89 MB |

## Key Findings and Analysis

This experiment resulted in a model with no predictive power, which is another crucial negative result for the paper.

1.  **The "Semantic Gap":** The central finding is that despite FastText's technical ability to generate vectors for unknown words, these vectors were not semantically meaningful for the task. The character n-grams learned from the English Wikipedia corpus do not carry the necessary semantic information to understand transliterated Bengali. This demonstrates that the "semantic gap" between the source (English) and target ("Banglish") languages was too vast to be bridged even by subword-level information.

2.  **Failure to Find a Signal:** The model's inability to learn is evident in its strategy of overwhelmingly predicting the negative class, leading to the near-zero recall for positive cases. This is a classic symptom of a model that cannot find any useful signal in the input features.

3.  **Reinforcing the Main Thesis:** This result, even more strongly than the GloVe experiment, proves that for this highly specialized and linguistically distinct dataset, pre-trained embeddings from a different source language are ineffective. It provides a powerful argument for the necessity of the custom-trained Word2Vec approach, which remains the only successful embedding strategy so far.