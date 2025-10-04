# Report: BiGRU with Pre-trained GloVe Embeddings

This report details the performance, complexity, and architecture of the Bidirectional GRU model when using pre-trained GloVe word embeddings. The primary goal of this experiment was to compare the effectiveness of a general-purpose embedding model against a custom-trained one for this specific task.

---

### Key Finding

The model trained successfully but achieved a lower overall performance compared to the model using custom-trained Word2Vec embeddings (F1 Score: 0.8168 vs. 0.9145). This suggests that for highly specialized, non-standard text (like transliterated Bengali), the vocabulary and context learned from a specific domain are more valuable than the broad, general knowledge from a pre-trained model.

---

### Performance Metrics

The model achieved the following performance on the test set:

| Metric          | Score  |
|-----------------|--------|
| **F1 Score** | **0.8168** |
| Accuracy        | 0.8069 |
| Precision       | 0.7774 |
| Recall          | 0.8604 |
| ROC-AUC Score   | 0.8879 |

### Confusion Matrix

* **True Negatives (TN):** 1650
* **False Positives (FP):** 540
* **False Negatives (FN):** 306
* **True Positives (TP):** 1886

[[1650, 540],
[306, 1886]]


---

### Vocabulary Coverage

A key factor in this model's performance is the overlap between the dataset's vocabulary and GloVe's vocabulary.

* **Coverage:** **[FILL IN COVERAGE % FROM THE DATA PREP CELL OUTPUT]** of the dataset's unique words were found in the pre-trained GloVe model. The remaining words (misses) were treated as unknown.

---

### Complexity Analysis

| Type                  | Value                             |
|-----------------------|-----------------------------------|
| Training Time         | 1740.48 seconds (~29.0 minutes)   |
| Average Inference Time| 1.5027 ms / sample                |
| Model Parameters      | 5,886,417                         |
| Model Size on Disk    | 22.50 MB                          |

---

### Model Architecture

This is the full summary of the model's architecture from TensorFlow/Keras.

```text
Model: "sequential"
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━┓
┃ Layer (type)                    ┃ Output Shape           ┃       Param # ┃
┡━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━┩
│ embedding (Embedding)           │ (None, 192, 100)       │     5,682,700 │
├─────────────────────────────────┼────────────────────────┼───────────────┤
│ bidirectional (Bidirectional)   │ (None, 128)            │        63,744 │
├─────────────────────────────────┼────────────────────────┼───────────────┤
│ dropout (Dropout)               │ (None, 128)            │             0 │
├─────────────────────────────────┼────────────────────────┼───────────────┤
│ dense (Dense)                   │ (None, 32)             │         4,128 │
├─────────────────────────────────┼────────────────────────┼───────────────┤
│ dense_1 (Dense)                 │ (None, 1)              │            33 │
└─────────────────────────────────┴────────────────────────┴───────────────┘
 Total params: 5,886,417 (22.45 MB)
 Trainable params: 67,905 (265.25 KB)
 Non-trainable params: 5,682,700 (21.68 MB)
 Optimizer params: 135,812 (530.52 KB)