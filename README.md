# Depression Detection in Transliterated Bengali Text

![Python 3.9+](https://img.shields.io/badge/Python-3.9+-blue.svg)
![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-orange.svg)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

## 📖 Overview

This project focuses on detecting depression from transliterated Bengali text, commonly found on social media platforms. It utilizes a Bidirectional GRU (BiGRU) neural network combined with custom-trained Word2Vec embeddings to perform binary classification on text data. The goal is to create an effective and practical model for mental health monitoring in a regional language context.

This model was trained on the **Bangla Social Media Depression Dataset (BSRDD)**.

---

## ✨ Features

* **Custom Word Embeddings:** Word2Vec model trained from scratch on the project's specific corpus.
* **Deep Learning Model:** A Bidirectional GRU (BiGRU) network for capturing contextual information in text sequences.
* **High Performance:** Achieves over 91% F1-Score on the test set.
* **Reproducibility:** The repository includes the trained models, tokenizer, and all necessary code to reproduce the results.
* **Scalable Structure:** Organized to easily accommodate future experiments with different models and techniques.

---

## 📂 Repository Structure

Depression-Detection-BSMDD/
├── data/
│   ├── .gitkeep
│   └── README.md
├── models/
│   └── bigru_word2vec/
│       ├── depression_detection_bigru.keras
│       ├── tokenizer.pickle
│       └── word2vec.model
├── notebooks/
│   └── 01_BiGRU_with_Word2Vec.ipynb
├── .gitignore
├── README.md
└── requirements.txt


---

## 🚀 Getting Started

Follow these instructions to set up the project locally and run the experiment.

### Prerequisites

* Python 3.9 or higher
* Pip package manager

### ⚙️ Installation

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/YOUR_USERNAME/Depression-Detection-BSMDD.git](https://github.com/YOUR_USERNAME/Depression-Detection-BSMDD.git)
    cd Depression-Detection-BSMDD
    ```

2.  **Create and activate a virtual environment:**
    * On Windows:
        ```bash
        python -m venv venv
        .\venv\Scripts\activate
        ```
    * On macOS/Linux:
        ```bash
        python3 -m venv venv
        source venv/bin/activate
        ```

3.  **Install the required dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

4.  **Download the Dataset:**
    * Download the `BSRDD_main.xlsx` file from **[Link to Dataset Here]**.
    * Place the downloaded file inside the `data/` directory.

---

## 📈 Usage

The primary code for this project is in the Jupyter Notebook inside the `notebooks/` directory.

1.  Launch Jupyter Notebook or Jupyter Lab.
2.  Open `notebooks/01_BiGRU_with_Word2Vec.ipynb`.
3.  Follow the steps in the notebook to preprocess the data, train the model, and evaluate its performance.

---

## 📊 Results Summary

This table provides a high-level comparison of all models experimented with in this project. For detailed results of each model, please see the corresponding report in the `/reports` directory.

| Model Experiment      | Key Metric (F1-Score) | Detailed Report                                      |
|-----------------------|-----------------------|------------------------------------------------------|
| **BiGRU + Word2Vec** | **0.9145** | [View Report](./reports/bigru_word2vec_report.md)    |
| *Your Next Model Name*| *Coming Soon* |                                                      |




---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome. Feel free to check the issues page if you want to contribute.

## 📄 License

This project is licensed under the MIT License. See the `LICENSE` file for details.
