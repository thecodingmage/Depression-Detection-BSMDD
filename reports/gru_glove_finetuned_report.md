# Report: GRU with Fine-Tuned GloVe Embeddings

## Summary

This report analyzes the performance of a Gated Recurrent Unit (GRU) model where the pre-trained GloVe embeddings were **fine-tuned** during training. This is a direct evolution of the previous experiment with frozen embeddings. By setting `trainable=True` on the embedding layer, the model was allowed to update the initial GloVe word vectors to better fit the nuances of the "Banglish" dataset. The goal was to see if this adaptation could overcome the "domain mismatch" observed in the previous model.

## Model Architecture

The model's structure is identical to the previous GRU models. However, the key difference is that all layers, including the massive embedding layer, are now trainable. This dramatically increases the number of trainable parameters.

| Layer (Type) | Output Shape | Param # | Trainable |
| :--- | :--- | :--- | :--- |
| `embedding` (Embedding) | (None, 192, 100) | 5,682,700 | **Yes** |
| `gru` (GRU) | (None, 64) | 31,872 | Yes |
| `dropout` (Dropout) | (None, 64) | 0 | - |
| `dense` (Dense) | (None, 32) | 2,080 | Yes |
| `dense_1` (Dense) | (None, 1) | 33 | Yes |
| **Total** | | **5,716,685** | |
| **Trainable** | | **5,716,685** | |
| **Non-trainable**| | **0** | |

## Performance Metrics

Fine-tuning resulted in a **dramatic improvement** in performance. The F1-score jumped from 0.11 in the frozen model to 0.87, indicating that the model was now able to learn effective patterns for classification.

| Metric | Score |
| :--- | :--- |
| **Accuracy** | 0.8733 |
| **F1-Score** | 0.8724 |
| **AUC-ROC** | 0.9446 |

### Classification Report

The report shows a well-balanced model, with strong precision and recall for both the positive and negative classes.

```
                   precision    recall  f1-score   support

Not Depressed (0)       0.87      0.88      0.87      2190
    Depressed (1)       0.88      0.87      0.87      2192

         accuracy                           0.87      4382
        macro avg       0.87      0.87      0.87      4382
     weighted avg       0.87      0.87      0.87      4382
```

### Confusion Matrix

The confusion matrix confirms the balanced performance, with the model correctly identifying a large number of samples from both classes.

```
[[1929  261]
 [ 294 1898]]
```

## Complexity Analysis

The performance gain from fine-tuning came at a significant computational cost, with a nearly 3x increase in training time and model size.

| Type | Metric | Value |
| :--- | :--- | :--- |
| **Time** | Total Training Time | 902.00 seconds (~15.0 minutes) |
| | Average Inference Time | 1.1802 ms per sample |
| **Space**| Total Parameters | 5,716,685 |
| | Model Size on Disk | 65.46 MB |

## Key Findings and Analysis

This experiment demonstrates the power and the risks of fine-tuning pre-trained embeddings.

1.  **Overcoming the Domain Mismatch:** By making the embedding layer trainable, the model was able to significantly overcome the initial "domain mismatch." During training, it adjusted the general-purpose GloVe vectors, moving them to positions in the vector space that were more meaningful for the specific task of "Banglish" depression detection. This resulted in the massive performance leap from an F1-score of 0.11 to 0.87.

2.  **Evidence of Overfitting:** While the final result was strong, the training logs showed classic signs of overfitting (training accuracy reaching ~99% while validation accuracy peaked much earlier). The `EarlyStopping` callback was crucial; it stopped the training and restored the model from its best-performing epoch (epoch 4), preventing the final model from being degraded by overfitting.

3.  **Comparison to Custom Word2Vec:** This is a key finding for the paper. Although fine-tuning worked well (F1-score of 0.87), it still **did not outperform the simpler, custom-trained Word2Vec model** (F1-score of 0.91). This suggests that starting with highly relevant, domain-specific embeddings is more effective and computationally cheaper than adapting general-purpose embeddings through expensive fine-tuning.

This model serves as an excellent case study on the trade-offs between performance, computational cost, and the risk of overfitting when using fine-tuning.