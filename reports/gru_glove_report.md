# Report: GRU with Pre-trained GloVe Embeddings

## Summary

This report analyzes a Gated Recurrent Unit (GRU) model using pre-trained GloVe embeddings for depression detection. The architecture is identical to the high-performing Word2Vec model, but the custom embeddings have been replaced with the publicly available `glove.6B.100d` vectors. The goal of this experiment was to determine if a powerful, general-purpose embedding model could effectively classify text in a specialized language domain ("Banglish") without domain-specific training. The embedding layer was frozen (`trainable=False`) during training.

## Model Architecture

The model's structure is a sequential neural network. It is identical to the Word2Vec benchmark model to ensure a fair comparison of the embedding strategies.

| Layer (Type) | Output Shape | Param # | Trainable |
| :--- | :--- | :--- | :--- |
| `embedding` (Embedding) | (None, 192, 100) | 5,682,700 | No |
| `gru` (GRU) | (None, 64) | 31,872 | Yes |
| `dropout` (Dropout) | (None, 64) | 0 | - |
| `dense` (Dense) | (None, 32) | 2,080 | Yes |
| `dense_1` (Dense) | (None, 1) | 33 | Yes |
| **Total** | | **5,716,685** | |
| **Trainable** | | **33,985** | |
| **Non-trainable**| | **5,682,700** | |

## Performance Metrics

The model demonstrated very poor performance on the test set, with an F1-score of only 0.1115, indicating a failure to learn meaningful patterns for the classification task.

| Metric | Score |
| :--- | :--- |
| **Accuracy** | 0.5126 |
| **F1-Score** | 0.1115 |
| **AUC-ROC** | 0.5139 |

### Classification Report

The report clearly shows the model's primary failure: an extremely low recall of **0.06** for the "Depressed (1)" class. This means it correctly identified only 6% of the actual positive cases.

```
                   precision    recall  f1-score   support

Not Depressed (0)       0.51      0.96      0.66      2190
    Depressed (1)       0.63      0.06      0.11      2192

         accuracy                           0.51      4382
        macro avg       0.57      0.51      0.39      4382
     weighted avg       0.57      0.51      0.39      4382
```

### Confusion Matrix

The confusion matrix confirms the recall issue, showing that the model incorrectly classified 2058 positive samples as negative. It adopted a naive strategy of predominantly predicting the "Not Depressed" class.

```
[[2112   78]
 [2058  134]]
```

## Complexity Analysis

The training time was significantly shorter than the Word2Vec model due to `EarlyStopping` halting the process as the model failed to improve.

| Type | Metric | Value |
| :--- | :--- | :--- |
| **Time** | Total Training Time | 321.40 seconds (~5.4 minutes) |
| | Average Inference Time | 0.7952 ms per sample |
| **Space**| Total Parameters | 5,716,685 |
| | Model Size on Disk | 22.10 MB |

## Key Findings and Analysis

This model's performance was near random chance, representing a failure to learn. This is a crucial **negative result** that provides valuable insight for the research paper.

The primary reason for this failure is the **"Domain Mismatch"** between the embeddings and the dataset:

1.  **Vocabulary Gap:** The GloVe embeddings were trained on a massive English corpus (Wikipedia, etc.). The project's "Banglish" dataset contains a unique, transliterated vocabulary. A significant number of important words in the dataset do not exist in the GloVe vocabulary. For these "missed" words, the model uses a zero-vector, providing no useful information to the GRU layer.

2.  **Contextual Difference:** Even for words that exist in both vocabularies (e.g., "sad," "help"), their contextual meaning and relationships within general English text are different from their usage in the specific context of mental health discussions in transliterated Bengali. The pre-trained vectors fail to capture this necessary nuance.

The model's inability to find a signal in the data caused it to collapse into a simplistic strategy of predicting one class most of the time, leading to the extremely low recall for positive cases. This experiment strongly validates the conclusion from the first model: for this specialized language task, general-purpose pre-trained embeddings are insufficient, and a custom-trained approach is far superior.