# Report: LSTM with Word2Vec

This report details the performance, complexity, and architecture of the LSTM model trained with custom Word2Vec embeddings.

---

### Performance Metrics

The model achieved the following performance on the test set:

| Metric        | Score  |
| :------------ | :----- |
| **F1-Score** | **0.8314** |
| Accuracy      | 0.8252 |
| Precision     | 0.8034 |
| Recall        | 0.8613 |
| ROC-AUC Score | 0.8768 |

---

### Complexity Analysis

| Type                   | Value               |
| :--------------------- | :------------------ |
| Training Time          | ~16.4 minutes       |
| Average Inference Time | ~1.00 ms / sample   |
| Model Parameters       | 5,727,153           |
| Model Size on Disk     | 65.58 MB            |

---

### Confusion Matrix

The confusion matrix below visualizes the model's performance on the test set. The labels are "0" for Not Depressed and "1" for Depressed.

|                    | **Predicted Not Depressed (0)** | **Predicted Depressed (1)** |
| :----------------- | :------------------------------ | :-------------------------- |
| **Actual Not Depressed (0)** | 1728 (TN)                       | 462 (FP)                    |
| **Actual Depressed (1)** | 304 (FN)                        | 1888 (TP)                   |

---

### Model Architecture

This is the full summary of the model's architecture from TensorFlow/Keras.

```text
Model: "sequential"
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━┓
┃ Layer (type)                    ┃ Output Shape           ┃       Param # ┃
┡━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━┩
│ embedding (Embedding)           │ (None, 192, 100)       │     5,682,800 │
├─────────────────────────────────┼────────────────────────┼───────────────┤
│ lstm (LSTM)                     │ (None, 64)             │        42,240 │
├─────────────────────────────────┼────────────────────────┼───────────────┤
│ dropout (Dropout)               │ (None, 64)             │             0 │
├─────────────────────────────────┼────────────────────────┼───────────────┤
│ dense (Dense)                   │ (None, 32)             │         2,080 │
├─────────────────────────────────┼────────────────────────┼───────────────┤
│ dense_1 (Dense)                 │ (None, 1)              │            33 │
└─────────────────────────────────┴────────────────────────┴───────────────┘
 Total params: 17,181,461 (65.54 MB)
 Trainable params: 5,727,153 (21.85 MB)
 Non-trainable params: 0 (0.00 B)
 Optimizer params: 11,454,308 (43.69 MB)