# Report: GRU with Fine-Tuned FastText Embeddings

## Summary

This report details the performance of a Gated Recurrent Unit (GRU) model combined with **fine-tuned** pre-trained FastText embeddings. Following the complete failure of the frozen FastText model, this experiment investigates if fine-tuning (`trainable=True`) can enable the simpler GRU architecture to adapt the embeddings and learn effectively from the specialized "Banglish" dataset.

## Model Architecture

The model uses a GRU architecture where all layers, including the large 300-dimensional FastText embedding layer, are made trainable.

| Layer (Type) | Output Shape | Param # | Trainable |
| :--- | :--- | :--- | :--- |
| `embedding` (Embedding) | (None, 192, 300) | 17,048,100 | **Yes** |
| `gru` (GRU) | (None, 64) | 70,272 | Yes |
| `dropout` (Dropout) | (None, 64) | 0 | - |
| `dense` (Dense) | (None, 32) | 2,080 | Yes |
| `dense_1` (Dense) | (None, 1) | 33 | Yes |
| **Total** | | **17,120,485** | |
| **Trainable** | | **17,120,485** | |
| **Non-trainable**| | **0** | |

## Performance Metrics

Fine-tuning resulted in a dramatic performance recovery. The F1-score surged from a near-random 0.09 in the frozen model to a strong **0.86**, indicating that the model successfully learned to classify the text.

| Metric | Score |
| :--- | :--- |
| **Accuracy** | 0.8690 |
| **F1-Score** | 0.8646 |
| **AUC-ROC** | 0.9415 |

### Classification Report

The report shows a well-balanced model with solid precision and recall for both classes.

```
                   precision    recall  f1-score   support

Not Depressed (0)       0.85      0.90      0.87      2190
    Depressed (1)       0.90      0.84      0.86      2192

         accuracy                           0.87      4382
        macro avg       0.87      0.87      0.87      4382
     weighted avg       0.87      0.87      0.87      4382
```

### Confusion Matrix

The confusion matrix shows a reasonably balanced number of errors for both the positive and negative classes.

```
[[1976  214]
 [ 360 1832]]
```

## Complexity Analysis

The success of fine-tuning came at a very high computational cost, marked by a very long training time and a large model size on disk.

| Type | Metric | Value |
| :--- | :--- | :--- |
| **Time** | Total Training Time | 2052.06 seconds (~34.2 minutes) |
| | Average Inference Time | 1.5018 ms per sample |
| **Space**| Total Parameters | 17,120,485 |
| | Model Size on Disk | 195.96 MB |

## Key Findings and Analysis

1.  **Effectiveness of Fine-Tuning:** This experiment confirms that fine-tuning is a powerful technique for adapting pre-trained embeddings to a new domain. It successfully transformed a failed model into a high-performing one by allowing the network to adjust the initial FastText vectors to be more relevant to the "Banglish" context.

2.  **Architecture vs. Training Strategy:** When comparing this model (F1 ~0.86) to the **BiLSTM with frozen FastText** model (F1 ~0.79), it appears that fine-tuning a simpler architecture (GRU) was more effective than using a more complex architecture (BiLSTM) with frozen embeddings.

3.  **The Cost-Performance Conclusion:** This model provides a final, compelling piece of evidence for the overall study. While it achieved good performance (F1 ~0.86), it did so at an extremely high cost (~34 minutes training time, ~196 MB size). This performance is still notably lower than the best custom Word2Vec models (F1 ~0.91), which were also significantly faster to train (~13-26 minutes) and much smaller (~22 MB).

This reinforces the main conclusion: for this specialized dataset, the most effective and efficient approach is to build custom embeddings from the domain-specific data, rather than adapting general-purpose embeddings through computationally expensive fine-tuning.