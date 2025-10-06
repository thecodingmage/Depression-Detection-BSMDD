# Report: BiLSTM with Pre-trained GloVe Embeddings

## Summary

This report details the final experiment in our series: a Bidirectional LSTM (BiLSTM) model combined with pre-trained, frozen GloVe embeddings (`glove.6B.100d`). Following the failure of unidirectional models with these embeddings, this experiment aims to see if the powerful bidirectional architecture can compensate for the "domain mismatch" of the general-purpose GloVe vectors, providing a direct comparison to the BiLSTM with FastText experiment.

## Model Architecture

The model uses a BiLSTM layer to process the 100-dimensional GloVe embeddings. The architecture is consistent with the other BiLSTM models to ensure a fair comparison.

| Layer (Type) | Output Shape | Param # | Trainable |
| :--- | :--- | :--- | :--- |
| `embedding` (Embedding) | (None, 192, 100) | 5,682,700 | No |
| `bidirectional` (Bidirectional)| (None, 128) | 84,480 | Yes |
| `dropout` (Dropout) | (None, 128) | 0 | - |
| `dense` (Dense) | (None, 32) | 4,128 | Yes |
| `dense_1` (Dense) | (None, 1) | 33 | Yes |
| **Total** | | **5,771,341** | |
| **Trainable** | | **88,641** | |
| **Non-trainable**| | **5,682,700** | |

## Performance Metrics

The model achieved a respectable F1-score of **0.80**. This is a dramatic improvement over the unidirectional GloVe models but falls short of the top-performing models in this study.

| Metric | Score |
| :--- | :--- |
| **Accuracy** | 0.8010 |
| **F1-Score** | 0.8008 |
| **AUC-ROC** | 0.8795 |

### Classification Report

The report shows a balanced model with consistent precision and recall across both classes.

```
                   precision    recall  f1-score   support

Not Depressed (0)       0.80      0.80      0.80      2190
    Depressed (1)       0.80      0.80      0.80      2192

         accuracy                           0.80      4382
        macro avg       0.80      0.80      0.80      4382
     weighted avg       0.80      0.80      0.80      4382
```

### Confusion Matrix

The confusion matrix confirms the model's balanced ability to classify both positive and negative instances, a significant improvement over its unidirectional counterparts.

```
[[1757  433]
 [ 439 1753]]
```

## Complexity Analysis

The training time is substantial, which is expected for the BiLSTM architecture.

| Type | Metric | Value |
| :--- | :--- | :--- |
| **Time** | Total Training Time | 1377.92 seconds (~23.0 minutes) |
| | Average Inference Time | 2.3641 ms per sample |
| **Space**| Total Parameters | 5,771,341 |
| | Model Size on Disk | 22.74 MB |

## Key Findings and Analysis

This final experiment provides a clear and comprehensive conclusion to the study.

1.  **Architecture's Role:** The BiLSTM architecture was again able to extract a meaningful signal from the general-purpose GloVe embeddings where unidirectional models failed. This confirms that bidirectional processing is a powerful tool for compensating for weaker input features by creating a richer understanding of context.

2.  **Comparative Hierarchy:** This model's performance fits perfectly into the hierarchy we have uncovered.
    * It dramatically outperformed the **frozen unidirectional GloVe models** (~0.80 vs. ~0.13 F1-score).
    * It performed worse than the **fine-tuned GRU with GloVe** model (~0.80 vs. 0.87 F1-score), suggesting that adapting the embeddings with a simpler architecture was more effective than using a more complex architecture on frozen embeddings.
    * It performed significantly worse than the **custom Word2Vec models** (~0.80 vs. ~0.91 F1-score), cementing the status of the custom embeddings as the superior strategy.

3.  **Final Verdict:** This experiment concludes the series by definitively showing that while architectural choices (like using a BiLSTM) can mitigate the problem of poor embeddings, they cannot fully solve it. The most effective path to high performance remains the use of high-quality, domain-specific embeddings from the start.