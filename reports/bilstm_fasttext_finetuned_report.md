# Report: BiLSTM with Fine-Tuned FastText Embeddings

## Summary

This report analyzes a top-tier contender model: a Bidirectional LSTM (BiLSTM) combined with **fine-tuned** pre-trained FastText embeddings. This experiment represents a maximal effort to extract performance from a pre-trained model by combining a powerful architecture (BiLSTM) with an adaptive embedding strategy (`trainable=True`). The goal is to determine if this combination can match or exceed the performance of the custom-trained Word2Vec models.

## Model Architecture

The model uses a BiLSTM architecture where the entire network, including the 17 million parameters of the FastText embedding layer, is trainable.

| Layer (Type) | Output Shape | Param # | Trainable |
| :--- | :--- | :--- | :--- |
| `embedding` (Embedding) | (None, 192, 300) | 17,048,100 | **Yes** |
| `bidirectional` (Bidirectional) | (None, 128) | 186,880 | Yes |
| `dropout` (Dropout) | (None, 128) | 0 | - |
| `dense` (Dense) | (None, 32) | 4,128 | Yes |
| `dense_1` (Dense) | (None, 1) | 33 | Yes |
| **Total** | | **17,239,141** | |
| **Trainable** | | **17,239,141** | |
| **Non-trainable**| | **0** | |

## Performance Metrics

This model achieved excellent performance, with an F1-score of **0.89**, placing it among the top-performing models in the study.

| Metric | Score |
| :--- | :--- |
| **Accuracy** | 0.8877 |
| **F1-Score** | 0.8936 |
| **AUC-ROC** | 0.9599 |

### Classification Report

The report shows strong and balanced metrics. A standout result is the very high recall of **0.94** for the "Depressed (1)" class, indicating the model is exceptionally good at identifying positive cases.

```
                   precision    recall  f1-score   support

Not Depressed (0)       0.94      0.83      0.88      2190
    Depressed (1)       0.85      0.94      0.89      2192

         accuracy                           0.89      4382
        macro avg       0.89      0.89      0.89      4382
     weighted avg       0.89      0.89      0.89      4382
```

### Confusion Matrix

The confusion matrix confirms the model's high recall for the positive class, with only 126 false negatives, the lowest of any model tested so far.

```
[[1824  366]
 [ 126 2066]]
```

## Complexity Analysis

This combination of a complex architecture and a large, trainable embedding layer resulted in a long training time and the largest model size on disk.

| Type | Metric | Value |
| :--- | :--- | :--- |
| **Time** | Total Training Time | 1701.17 seconds (~28.4 minutes) |
| | Average Inference Time | 4.6957 ms per sample |
| **Space**| Total Parameters | 17,239,141 |
| | Model Size on Disk | 197.33 MB |

## Key Findings and Analysis

1.  **Synergy of Complexity and Adaptation:** This experiment demonstrates the powerful synergy between a complex architecture (BiLSTM) and an adaptive embedding strategy (fine-tuning). The BiLSTM's ability to capture deep context and the fine-tuning process's ability to specialize the embeddings worked together to produce a high-performing model, even from a general-purpose starting point.

2.  **Peak Pre-trained Performance:** This model likely represents the performance ceiling for pre-trained embeddings on this dataset. It shows that with significant computational effort, it is possible to make a general-purpose model highly effective.

3.  **The Final Comparison (The Ultimate Trade-off):** This is the most important finding for the paper.
    * This **BiLSTM + Fine-Tuned FastText** model achieved an F1-score of **~0.89** in **~28.4 minutes**, creating a **~197 MB** model.
    * The **GRU + Word2Vec** model achieved a higher F1-score of **~0.91** in only **~13.0 minutes**, creating a **~22 MB** model.

This provides a clear, data-driven conclusion: the simpler, more efficient approach of training custom embeddings with a standard GRU not only consumes significantly fewer resources but also yields superior performance.

4.  **High Recall Specialization:** The model's exceptionally high recall (0.94) for the "Depressed" class is a noteworthy characteristic. This suggests that the combination of BiLSTM and fine-tuned FastText is particularly adept at minimizing false negatives, which could be a critical feature in a clinical application.