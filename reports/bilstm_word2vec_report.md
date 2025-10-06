# Report: BiLSTM with Custom Word2Vec Embeddings

## Summary

This report analyzes what is hypothesized to be one of the most powerful models in this study: a Bidirectional LSTM (BiLSTM) combined with custom-trained Word2Vec embeddings. This experiment integrates the most effective embedding strategy (custom Word2Vec, trained on the "Banglish" data) with a highly effective architecture for context capture (BiLSTM). The expectation is that this model will achieve state-of-the-art performance for this specific task. The embedding layer was frozen (`trainable=False`).

## Model Architecture

The model uses a BiLSTM layer to process the 100-dimensional custom Word2Vec embeddings. The bidirectional wrapper doubles the number of parameters in the recurrent layer compared to a unidirectional LSTM.

| Layer (Type) | Output Shape | Param # | Trainable |
| :--- | :--- | :--- | :--- |
| `embedding` (Embedding) | (None, 192, 100) | 5,682,700 | No |
| `bidirectional` (Bidirectional) | (None, 128) | 84,480 | Yes |
| `dropout` (Dropout) | (None, 128) | 0 | - |
| `dense` (Dense) | (None, 32) | 4,128 | Yes |
| `dense_1` (Dense) | (None, 1) | 33 | Yes |
| **Total** | | **5,771,341** | |
| **Trainable** | | **88,641** | |
| **Non-trainable**| | **5,682,700** | |

## Performance Metrics

The model achieved excellent performance, with a balanced F1-score of **0.91**, placing it at the top tier of all models tested.

| Metric | Score |
| :--- | :--- |
| **Accuracy** | 0.9108 |
| **F1-Score** | 0.9105 |
| **AUC-ROC** | 0.9700 |

### Classification Report

The report shows perfectly balanced and high precision, recall, and F1-scores across both classes, indicating a robust and unbiased model.

```
                   precision    recall  f1-score   support

Not Depressed (0)       0.91      0.91      0.91      2190
    Depressed (1)       0.91      0.91      0.91      2192

         accuracy                           0.91      4382
        macro avg       0.91      0.91      0.91      4382
     weighted avg       0.91      0.91      0.91      4382
```

### Confusion Matrix

The confusion matrix confirms the model's high accuracy and balanced handling of both classes, with a low and nearly equal number of false positives and false negatives.

```
[[2001  189]
 [ 202 1990]]
```

## Complexity Analysis

This model has a significantly longer training time compared to the unidirectional GRU, reflecting the increased computational cost of the BiLSTM architecture.

| Type | Metric | Value |
| :--- | :--- | :--- |
| **Time** | Total Training Time | 1570.11 seconds (~26.2 minutes) |
| | Average Inference Time | 1.7398 ms per sample |
| **Space**| Total Parameters | 5,771,341 |
| | Model Size on Disk | 22.74 MB |

## Key Findings and Analysis

This model's excellent performance provides several key insights for the research paper.

1.  **Synergy of Components:** The high score is a result of the powerful synergy between the custom embeddings and the bidirectional architecture. The Word2Vec embeddings provided a highly relevant feature set, and the BiLSTM layer was able to extract deep contextual meaning by processing this information both forwards and backward.

2.  **Comparison with GRU + Word2Vec:** This is the most critical finding. While the BiLSTM is a more complex and computationally expensive architecture, its F1-score (~0.91) is nearly identical to that of the much simpler **GRU + Word2Vec** model (~0.91). This suggests that for this dataset, once the word embeddings are sufficiently high-quality and domain-specific, the additional architectural complexity of a BiLSTM provides diminishing returns.

3.  **Performance vs. Efficiency Trade-off:** The BiLSTM model took approximately **26.2 minutes** to train, while the GRU model with the same embeddings took only **13.0 minutes**. Given that they achieved the same level of performance, the simpler GRU model is arguably the more efficient and practical choice for this specific problem.

In conclusion, while this model confirms that the combination of custom embeddings and a powerful architecture is a path to state-of-the-art results, it also highlights an important trade-off. A simpler GRU can be just as effective as a more complex BiLSTM if the input features (the word embeddings) are of high quality.