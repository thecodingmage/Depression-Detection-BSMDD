# Report: BiLSTM with Fine-Tuned GloVe Embeddings

## Summary

This report analyzes the final model in our comprehensive study: a Bidirectional LSTM (BiLSTM) architecture combined with **fine-tuned** pre-trained GloVe embeddings. By setting `trainable=True`, this experiment allows the powerful BiLSTM to adapt the general-purpose GloVe vectors to the specific "Banglish" dataset. The goal is to evaluate its performance and efficiency against all other models, particularly the custom Word2Vec and the fine-tuned FastText variants.

## Model Architecture

The model uses a BiLSTM architecture where all layers, including the 100-dimensional GloVe embedding layer, are trainable.

| Layer (Type) | Output Shape | Param # | Trainable |
| :--- | :--- | :--- | :--- |
| `embedding` (Embedding) | (None, 192, 100) | 5,682,700 | **Yes** |
| `bidirectional` (Bidirectional) | (None, 128) | 84,480 | Yes |
| `dropout` (Dropout) | (None, 128) | 0 | - |
| `dense` (Dense) | (None, 32) | 4,128 | Yes |
| `dense_1` (Dense) | (None, 1) | 33 | Yes |
| **Total** | | **5,771,341** | |
| **Trainable** | | **5,771,341** | |
| **Non-trainable**| | **0** | |

## Performance Metrics

This model achieved excellent, top-tier performance, with a balanced F1-score of nearly **0.90**.

| Metric | Score |
| :--- | :--- |
| **Accuracy** | 0.8957 |
| **F1-Score** | 0.8960 |
| **AUC-ROC** | 0.9582 |

### Classification Report

The report shows strong and highly balanced precision and recall across both classes, indicating a robust and reliable model.

```
                   precision    recall  f1-score   support

Not Depressed (0)       0.90      0.89      0.90      2190
    Depressed (1)       0.89      0.90      0.90      2192

         accuracy                           0.90      4382
        macro avg       0.90      0.90      0.90      4382
     weighted avg       0.90      0.90      0.90      4382
```

### Confusion Matrix

The confusion matrix confirms the model's balanced performance, with an almost equal and low number of errors for both classes.

```
[[1957  233]
 [ 224 1968]]
```

## Complexity Analysis

A key finding for this model is its remarkable efficiency. Despite being a fine-tuned BiLSTM, its training time was significantly lower than other complex fine-tuned models.

| Type | Metric | Value |
| :--- | :--- | :--- |
| **Time** | Total Training Time | 601.86 seconds (~10.0 minutes) |
| | Average Inference Time | 1.3626 ms per sample |
| **Space**| Total Parameters | 5,771,341 |
| | Model Size on Disk | 66.09 MB |

## Key Findings and Analysis

This final experiment yields a powerful and nuanced conclusion for your research.

1.  **Top-Tier Performance:** With an F1-score of ~0.90, this model firmly establishes itself in the top tier of performers, nearly matching the custom Word2Vec models. This confirms that combining a strong architecture (BiLSTM) with an adaptive training strategy (fine-tuning) is a highly effective approach.

2.  **The Most Efficient Fine-Tuned Model:** This is a critical finding. The **BiLSTM + Fine-Tuned GloVe** model (~10 minutes training time) was almost **3 times faster** to train than the **BiLSTM + Fine-Tuned FastText** model (~28 minutes) while achieving a slightly better F1-score. The primary reason is the smaller embedding dimension (100d for GloVe vs. 300d for FastText), which dramatically reduces the number of parameters to update.

3.  **Final Hierarchy:** This result solidifies the final performance hierarchy:
    * **Tier 1 (Best):** Custom Word2Vec models (~0.91 F1-score, moderate training time).
    * **Tier 2 (Excellent):** Fine-tuned BiLSTM models (~0.90 F1-score, high training time).
    * **Tier 3 (Good):** Fine-tuned unidirectional models (~0.87 F1-score, high training time).
    * **Tier 4 (Failed):** All models with frozen pre-trained embeddings.

**Overall Conclusion:** While the custom-trained Word2Vec models remain the overall winners in terms of peak performance and efficiency, this **BiLSTM with Fine-Tuned GloVe** model stands out as the best and most efficient of all the pre-trained embedding strategies. It provides a compelling balance of high accuracy and manageable computational cost, making it a strong runner-up and an excellent point of comparison.