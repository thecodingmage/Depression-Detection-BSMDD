# Report: BiGRU with Word2Vec

This report details the performance, complexity, and architecture of the Bidirectional GRU model trained with custom Word2Vec embeddings.

---

### Performance Metrics

The model achieved the following performance on the test set:

| Metric    | Score  |
|-----------|--------|
| **F1-Score** | **0.9145** |
| Accuracy  | 0.9133 |
| Precision | 0.9023 |
| Recall    | 0.9270 |

---

### Complexity Analysis

| Type                  | Value                       |
|-----------------------|-----------------------------|
| Training Time         | ~28.1 minutes               |
| Average Inference Time| ~1.58 ms / sample           |
| Model Parameters      | 5,750,605                   |
| Model Size on Disk    | 22.50 MB                    |

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