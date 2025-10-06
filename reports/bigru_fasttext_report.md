# Report: BiGRU with FastText

This report details the performance, complexity, and architecture of the Bidirectional GRU model trained with pre-trained FastText embeddings.

---

### Performance Metrics

The model's final performance on the unseen test set is as follows.

| Metric        | Score  |
| :------------ | :----- |
| **F1-Score** | **0.7890** |
| Accuracy      | 0.7855 |
| Precision     | 0.7767 |
| Recall        | 0.8016 |
| ROC-AUC Score | 0.8660 |

---

### Complexity Analysis

| Type                   | Value               |
| :--------------------- | :------------------ |
| Total Training Time    | ~61.9 minutes       |
| Average Inference Time | ~4.03 ms / sample   |
| Model Parameters       | 17,192,805          |
| Model Size on Disk     | 66.73 MB            |

---

### Confusion Matrix

The confusion matrix below visualizes the model's performance on the test set. The labels are "0" for Not Depressed and "1" for Depressed.

|                          | **Predicted Not Depressed (0)** | **Predicted Depressed (1)** |
| :----------------------- | :------------------------------ | :-------------------------- |
| **Actual Not Depressed (0)** | 1685 (TN)                       | 505 (FP)                    |
| **Actual Depressed (1)** | 435 (FN)                        | 1757 (TP)                   |

---

### Model Architecture

This is the full summary of the model's architecture from TensorFlow/Keras.

```text
Model: "sequential"
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━┓
┃ Layer (type)                    ┃ Output Shape           ┃       Param # ┃
┡━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━┩
│ embedding (Embedding)           │ (None, 192, 300)       │    17,048,100 │
├─────────────────────────────────┼────────────────────────┼───────────────┤
│ bidirectional (Bidirectional)   │ (None, 128)            │       140,544 │
├─────────────────────────────────┼────────────────────────┼───────────────┤
│ dropout (Dropout)               │ (None, 128)            │             0 │
├─────────────────────────────────┼────────────────────────┼───────────────┤
│ dense (Dense)                   │ (None, 32)             │         4,128 │
├─────────────────────────────────┼────────────────────────┼───────────────┤
│ dense_1 (Dense)                 │ (None, 1)              │            33 │
└─────────────────────────────────┴────────────────────────┴───────────────┘
 Total params: 17,482,217 (66.69 MB)
 Trainable params: 144,705 (565.25 KB)
 Non-trainable params: 17,048,100 (65.03 MB)
 Optimizer params: 289,412 (1.10 MB)