# Report: GRU with Custom Word2Vec Embeddings

## Summary

This report details the performance and complexity of a Gated Recurrent Unit (GRU) model designed for binary depression detection. The model leverages custom-trained Word2Vec embeddings. These embeddings were generated from scratch using the project's specific "Banglish" (transliterated Bengali) dataset, creating a highly domain-specific vocabulary. The GRU architecture processes sequences of these embeddings to classify the text.

## Model Architecture

The model is a sequential neural network composed of an embedding layer, a GRU layer, and two dense layers for classification. The embedding layer was initialized with the custom Word2Vec weights and was frozen (non-trainable) during training.

| Layer (Type) | Output Shape | Param # | Trainable |
| :--- | :--- | :--- | :--- |
| `embedding` (Embedding) | (None, 192, 100) | 5,682,700 | No |
| `gru` (GRU) | (None, 64) | 31,872 | Yes |
| `dropout` (Dropout) | (None, 64) | 0 | - |
| `dense` (Dense) | (None, 32) | 2,080 | Yes |
| `dense_1` (Dense) | (None, 1) | 33 | Yes |
| **Total** | | **5,716,685** | |
| **Trainable** | | **33,985** | |
| **Non-trainable**| | **5,682,700** | |

## Performance Metrics

The model achieved high performance on the test set, demonstrating a strong ability to generalize and correctly classify unseen data.

| Metric | Score |
| :--- | :--- |
| **Accuracy** | 0.9096 |
| **F1-Score** | 0.9100 |
| **AUC-ROC** | 0.9670 |

### Classification Report

```
                   precision    recall  f1-score   support

Not Depressed (0)       0.91      0.91      0.91      2190
    Depressed (1)       0.91      0.91      0.91      2192

         accuracy                           0.91      4382
        macro avg       0.91      0.91      0.91      4382
     weighted avg       0.91      0.91      0.91      4382
```

### Confusion Matrix

The confusion matrix shows a balanced performance between the two classes, with a similar number of false positives and false negatives.

```
[[1983  207]
 [ 189 2003]]
```

## Complexity Analysis

| Type | Metric | Value |
| :--- | :--- | :--- |
| **Time** | Total Training Time | 778.66 seconds (~13.0 minutes) |
| | Average Inference Time | 0.8309 ms per sample |
| **Space**| Total Parameters | 5,716,685 |
| | Model Size on Disk | 22.10 MB |

## Key Findings and Analysis

This model stands out as the **high-performance benchmark** for this project. The excellent F1-score of **0.91** indicates a robust and reliable model.

The primary reason for this success is the **custom-trained Word2Vec embeddings**. Unlike pre-trained embeddings (like GloVe or FastText) which are trained on general English, these embeddings were built exclusively from the project's "Banglish" dataset. This allowed the model to:

1.  **Learn the Domain-Specific Vocabulary:** It created meaningful vector representations for unique, transliterated words that would not exist in standard English vocabularies.
2.  **Capture Relevant Context:** The vector relationships learned by Word2Vec (e.g., which words appear near each other) are specific to the context of mental health discussions in this language. This provides the GRU layer with highly relevant features.

The GRU architecture effectively captured the sequential dependencies within the text, and the model generalized well without significant overfitting. The results from this model strongly validate the hypothesis that for niche language domains, a custom-trained embedding strategy is superior to using general-purpose, pre-trained alternatives.