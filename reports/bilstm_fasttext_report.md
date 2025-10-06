# Report: BiLSTM with Pre-trained FastText Embeddings

## Summary

This report analyzes a Bidirectional Long Short-Term Memory (BiLSTM) model combined with pre-trained, frozen FastText embeddings (`wiki-news-300d-1M-subword.vec`). Following the failure of unidirectional models (GRU, LSTM) with these embeddings, this experiment tested the hypothesis that a more powerful bidirectional architecture could extract a meaningful signal from the same input features. The embedding layer was kept frozen (`trainable=False`).

## Model Architecture

The model replaces the unidirectional LSTM with a Bidirectional LSTM layer. This layer processes the input sequence both forwards and backward, creating a richer contextual representation.

| Layer (Type) | Output Shape | Param # | Trainable |
| :--- | :--- | :--- | :--- |
| `embedding` (Embedding) | (None, 192, 300) | 17,048,100 | No |
| `bidirectional` (Bidirectional) | (None, 128) | 186,880 | Yes |
| `dropout` (Dropout) | (None, 128) | 0 | - |
| `dense` (Dense) | (None, 32) | 4,128 | Yes |
| `dense_1` (Dense) | (None, 1) | 33 | Yes |
| **Total** | | **17,239,141** | |
| **Trainable** | | **191,041** | |
| **Non-trainable**| | **17,048,100** | |

## Performance Metrics

Unlike the unidirectional models with FastText, the BiLSTM model achieved a respectable level of performance, with an F1-score of approximately 0.79.

| Metric | Score |
| :--- | :--- |
| **Accuracy** | 0.7814 |
| **F1-Score** | 0.7859 |
| **AUC-ROC** | 0.8588 |

### Classification Report

The report shows a reasonably balanced model, with similar precision and recall for both classes. This is a dramatic improvement over the ~50% accuracy of the unidirectional models.

```
                   precision    recall  f1-score   support

Not Depressed (0)       0.79      0.76      0.78      2190
    Depressed (1)       0.77      0.80      0.79      2192

         accuracy                           0.78      4382
        macro avg       0.78      0.78      0.78      4382
     weighted avg       0.78      0.78      0.78      4382
```

### Confusion Matrix

The confusion matrix is well-balanced, indicating that the model learned to distinguish between both classes effectively, unlike its unidirectional counterparts.

```
[[1666  524]
 [ 434 1758]]
```

## Complexity Analysis

The training time for this model was significantly longer, likely because the increased complexity allowed it to learn for more epochs before `EarlyStopping` was triggered.

| Type | Metric | Value |
| :--- | :--- | :--- |
| **Time** | Total Training Time | 2100.46 seconds (~35.0 minutes) |
| | Average Inference Time | 5.6238 ms per sample |
| **Space**| Total Parameters | 17,239,141 |
| | Model Size on Disk | 67.26 MB |

## Key Findings and Analysis

This experiment provides a nuanced and important finding for the paper.

1.  **Architecture Can Compensate for Poor Embeddings:** The most significant finding is that a more powerful architecture (BiLSTM) was able to partially compensate for the "semantic gap" of the pre-trained FastText embeddings. By processing the text in both directions, the model could create richer contextual representations and find signals that the unidirectional GRU and LSTM models missed entirely.

2.  **Performance Still Sub-Optimal:** While the F1-score of ~0.79 is respectable, it is still significantly lower than the performance of the models using custom-trained Word2Vec embeddings (~0.91). This confirms that while architecture plays a role, the **quality and relevance of the embeddings remain the most critical factor** for achieving state-of-the-art performance on this task.

3.  **Computational Cost:** The improved performance came at a high cost. The training time of 35 minutes is substantially longer than for any of the unidirectional models, highlighting the computational trade-off of using more complex architectures.

In conclusion, this model is a successful "proof of concept" that bidirectional processing is highly effective for this task. However, it also reinforces the primary thesis that even a strong architecture cannot fully overcome the limitations of a mismatched embedding strategy.