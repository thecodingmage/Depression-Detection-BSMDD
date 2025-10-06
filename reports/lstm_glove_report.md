# Report: LSTM with Pre-trained GloVe Embeddings

## Summary

This report analyzes a Long Short-Term Memory (LSTM) model for depression detection, using the same pre-trained, frozen GloVe embeddings (`glove.6B.100d`) as a previous experiment. The primary goal was to determine if changing the recurrent neural network architecture from a GRU to the more complex LSTM could overcome the performance limitations imposed by the poorly-matched word embeddings. The embedding layer was frozen (`trainable=False`).

## Model Architecture

The model's structure uses an LSTM layer in place of the previous GRU layer. The rest of the architecture remains consistent to isolate the impact of the recurrent layer.

| Layer (Type) | Output Shape | Param # | Trainable |
| :--- | :--- | :--- | :--- |
| `embedding` (Embedding) | (None, 192, 100) | 5,682,700 | No |
| `lstm` (LSTM) | (None, 64) | 42,240 | Yes |
| `dropout` (Dropout) | (None, 64) | 0 | - |
| `dense` (Dense) | (None, 32) | 2,080 | Yes |
| `dense_1` (Dense) | (None, 1) | 33 | Yes |
| **Total** | | **5,727,053** | |
| **Trainable** | | **44,353** | |
| **Non-trainable**| | **5,682,700** | |

## Performance Metrics

The model performed very poorly, with metrics indicating a failure to learn effectively. The results are nearly identical to the GRU with GloVe model, showing no benefit from the change in architecture.

| Metric | Score |
| :--- | :--- |
| **Accuracy** | 0.5135 |
| **F1-Score** | 0.1291 |
| **AUC-ROC** | 0.5170 |

### Classification Report

The report shows an extremely low recall of **0.07** for the "Depressed (1)" class, confirming that the model failed to identify positive instances.

```
                   precision    recall  f1-score   support

Not Depressed (0)       0.51      0.96      0.66      2190
    Depressed (1)       0.62      0.07      0.13      2192

         accuracy                           0.51      4382
        macro avg       0.56      0.51      0.40      4382
     weighted avg       0.56      0.51      0.40      4382
```

### Confusion Matrix

The confusion matrix shows that the model misclassified 2034 positive samples as negative, defaulting to a naive prediction strategy.

```
[[2092   98]
 [2034  158]]
```

## Complexity Analysis

| Type | Metric | Value |
| :--- | :--- | :--- |
| **Time** | Total Training Time | 388.84 seconds (~6.5 minutes) |
| | Average Inference Time | 0.8502 ms per sample |
| **Space**| Total Parameters | 5,727,053 |
| | Model Size on Disk | 22.22 MB |

## Key Findings and Analysis

This experiment provides a critical insight: **the choice of recurrent architecture (LSTM vs. GRU) is irrelevant when the input embeddings are not meaningful.**

1.  **"Garbage In, Garbage Out":** The model's failure is a direct result of the "Domain Mismatch" of the GloVe embeddings. The LSTM layer, despite being more complex than a GRU, cannot create useful patterns from input features that lack a relevant signal. The word vectors did not carry enough information about the "Banglish" vocabulary and context, leading to a classic "Garbage In, Garbage Out" scenario.

2.  **Confirming the Bottleneck:** By swapping the GRU for an LSTM and achieving the same poor result, we can conclusively state that the **embedding strategy is the primary bottleneck**, not the recurrent layer. This is a very strong point for your paper's discussion.

3.  **Ineffective Learning Strategy:** Just like the GRU model with these embeddings, the LSTM model adopted a naive strategy of predicting the majority class (or simply failing to find a signal for the positive class), resulting in the extremely low recall score.

This result strengthens the overall narrative of your research: for this specialized task, the quality and relevance of the word embeddings are far more critical than the specific choice of recurrent architecture.