# Report: LSTM with Fine-Tuned GloVe Embeddings

## Summary

This report analyzes the performance of a Long Short-Term Memory (LSTM) model where the pre-trained GloVe embeddings were **fine-tuned** during training (`trainable=True`). This experiment is a direct follow-up to the failed LSTM model with frozen embeddings. The objective was to assess if allowing the model to adapt the general-purpose GloVe vectors could enable the LSTM architecture to learn effective patterns from the specialized "Banglish" dataset.

## Model Architecture

The model's structure is identical to the frozen LSTM model, but with the critical difference that all layers, including the embedding layer, are now trainable. This significantly increases the number of parameters the model must learn during training.

| Layer (Type) | Output Shape | Param # | Trainable |
| :--- | :--- | :--- | :--- |
| `embedding` (Embedding) | (None, 192, 100) | 5,682,700 | **Yes** |
| `lstm` (LSTM) | (None, 64) | 42,240 | Yes |
| `dropout` (Dropout) | (None, 64) | 0 | - |
| `dense` (Dense) | (None, 32) | 2,080 | Yes |
| `dense_1` (Dense) | (None, 1) | 33 | Yes |
| **Total** | | **5,727,053** | |
| **Trainable** | | **5,727,053** | |
| **Non-trainable**| | **0** | |

## Performance Metrics

Fine-tuning led to a dramatic and successful improvement in performance. The F1-score surged from 0.13 in the frozen model to a strong **0.88**, demonstrating that the model was able to learn effectively.

| Metric | Score |
| :--- | :--- |
| **Accuracy** | 0.8777 |
| **F1-Score** | 0.8792 |
| **AUC-ROC** | 0.9411 |

### Classification Report

The report shows a well-balanced model with strong and consistent precision and recall for both classes.

```
                   precision    recall  f1-score   support

Not Depressed (0)       0.89      0.87      0.88      2190
    Depressed (1)       0.87      0.89      0.88      2192

         accuracy                           0.88      4382
        macro avg       0.88      0.88      0.88      4382
     weighted avg       0.88      0.88      0.88      4382
```

### Confusion Matrix

The confusion matrix confirms the model's balanced predictive power, with a similar number of errors for both the positive and negative classes.

```
[[1896  294]
 [ 242 1950]]
```

## Complexity Analysis

The significant performance gain came at the cost of a very long training time, highlighting the computational expense of fine-tuning a large embedding layer.

| Type | Metric | Value |
| :--- | :--- | :--- |
| **Time** | Total Training Time | 1472.19 seconds (~24.5 minutes) |
| | Average Inference Time | 0.8899 ms per sample |
| **Space**| Total Parameters | 5,727,053 |
| | Model Size on Disk | 65.57 MB |

## Key Findings and Analysis

1.  **Fine-Tuning is Effective:** This experiment confirms that fine-tuning is a viable strategy to bridge the "domain mismatch" for pre-trained embeddings. By allowing the GloVe vectors to be adjusted, the model successfully learned the specific features of the "Banglish" data, turning a failed model into a high-performing one.

2.  **LSTM vs. GRU Performance:** A crucial finding is that the fine-tuned LSTM model (F1-score ~0.88) performed almost identically to the fine-tuned GRU model (F1-score ~0.87). This suggests that for this particular problem, once the embeddings are properly adapted, the choice between LSTM and GRU has a negligible impact on the final performance.

3.  **Superiority of Custom Embeddings:** Despite its success, this model's performance still falls short of the custom Word2Vec models (~0.88 vs. ~0.91). This reinforces the central conclusion of the study: starting with domain-specific embeddings (Word2Vec) is a more effective and computationally cheaper strategy than adapting general-purpose embeddings through expensive fine-tuning.