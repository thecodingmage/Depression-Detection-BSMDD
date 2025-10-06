# Report: LSTM with Fine-Tuned FastText Embeddings

## Summary

This report analyzes the performance of a Long Short-Term Memory (LSTM) model where the pre-trained FastText embeddings were **fine-tuned** (`trainable=True`). After the complete failure of the model with frozen FastText embeddings, this experiment investigates if allowing the model to adapt these subword-aware vectors can bridge the "semantic gap" and enable effective learning on the "Banglish" dataset.

## Model Architecture

The model architecture is an LSTM network. By setting `trainable=True`, all 17.1 million parameters in the model, including the entire FastText embedding matrix, are updated during training.

| Layer (Type) | Output Shape | Param # | Trainable |
| :--- | :--- | :--- | :--- |
| `embedding` (Embedding) | (None, 192, 300) | 17,048,100 | **Yes** |
| `lstm` (LSTM) | (None, 64) | 93,440 | Yes |
| `dropout` (Dropout) | (None, 64) | 0 | - |
| `dense` (Dense) | (None, 32) | 2,080 | Yes |
| `dense_1` (Dense) | (None, 1) | 33 | Yes |
| **Total** | | **17,143,653** | |
| **Trainable** | | **17,143,653** | |
| **Non-trainable**| | **0** | |

## Performance Metrics

Fine-tuning transformed the model from a complete failure into a strong performer. The F1-score jumped from a near-random 0.09 to a very respectable **0.87**.

| Metric | Score |
| :--- | :--- |
| **Accuracy** | 0.8688 |
| **F1-Score** | 0.8739 |
| **AUC-ROC** | 0.9337 |

### Classification Report

The report shows a well-balanced model, with a particularly strong recall for the "Depressed (1)" class, indicating it is very effective at identifying positive cases.

```
                   precision    recall  f1-score   support

Not Depressed (0)       0.90      0.83      0.86      2190
    Depressed (1)       0.84      0.91      0.87      2192

         accuracy                           0.87      4382
        macro avg       0.87      0.87      0.87      4382
     weighted avg       0.87      0.87      0.87      4382
```

### Confusion Matrix

The confusion matrix confirms the strong recall for the positive class, with only 199 false negatives compared to 1993 true positives.

```
[[1814  376]
 [ 199 1993]]
```

## Complexity Analysis

This was the most computationally expensive model to train, with the longest training time and the largest final model size on disk.

| Type | Metric | Value |
| :--- | :--- | :--- |
| **Time** | Total Training Time | 2249.40 seconds (~37.5 minutes) |
| | Average Inference Time | 3.0451 ms per sample |
| **Space**| Total Parameters | 17,143,653 |
| | Model Size on Disk | 196.23 MB |

## Key Findings and Analysis

1.  **Fine-Tuning Success:** This experiment successfully demonstrates that fine-tuning can make even a poorly matched pre-trained embedding model effective. By allowing the 17 million embedding parameters to be updated, the model was able to "drag" the initial, unhelpful FastText vectors into positions that were meaningful for classifying the "Banglish" text.

2.  **FastText vs. GloVe (Fine-Tuned):** A key finding is that the fine-tuned FastText model (F1 ~0.87) and the fine-tuned GloVe model (F1 ~0.88) achieved nearly identical performance. This suggests that once the computationally expensive process of fine-tuning is undertaken, the initial theoretical advantages of one pre-trained model over another (e.g., FastText's subword information) may become negligible. Both models converge to a similar level of "good-but-not-best" performance.

3.  **The High Cost of Adaptation:** The success of this model came at a very high price. With a training time of nearly 40 minutes and a disk size of almost 200 MB, it is by far the most resource-intensive model.

4.  **Custom Embeddings Remain Superior:** Despite the impressive recovery via fine-tuning, this model's performance **still does not surpass the simpler, faster, and smaller custom Word2Vec models** (F1-score ~0.87 vs. ~0.91). This serves as the strongest confirmation of the project's central thesis: for this specialized domain, building custom embeddings from the ground up is the most effective and efficient path to state-of-the-art results.