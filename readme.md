# ⚡ Hybrid SoC Predictor with Error Correction (BiGRU + Transformer)

This repository implements a **two-stage deep learning pipeline** for **battery State of Charge (SoC)** prediction using **time-series modeling** and **post-prediction error correction**.  
It combines the sequential learning power of a **Bidirectional GRU (BiGRU)** with the context-awareness of a **Transformer**, resulting in a highly robust prediction architecture capable of handling **low-SoC edge cases** and improving **prediction stability**.

---

## 🚀 Key Features

- **Dual-Stage Learning Framework**
  - **Stage 1:** A **BiGRU with Multi-Head Attention** predicts the base SoC curve.
  - **Stage 2:** A **Transformer-based error correction model** learns to predict residual errors from Stage 1 and adjusts outputs for improved accuracy.

- **Error-Aware Weighted Loss Function**
  - Assigns higher penalty to **low SoC ranges** where predictive accuracy is most critical.

- **Dynamic Data Handling**
  - Automatically loads or **generates dummy datasets** if source files are missing, ensuring end-to-end reproducibility.

- **Robust Feature Engineering**
  - Incorporates **lagged**, **differential**, and **rolling statistical** features for richer time-series representation.

- **Self-Adaptive Training**
  - Includes **EarlyStopping** and **ReduceLROnPlateau** callbacks to prevent overfitting and dynamically tune learning rates.

- **Comprehensive Visualization**
  - Generates detailed **SoC vs. time**, **error correction**, and **smoothed Savitzky–Golay filtered** performance plots.

---

## 🧩 Architecture Overview

Input CSVs
│
├── Data Loading & Cleaning
│
├── Feature Engineering
│ ├─ Lag Features
│ ├─ Derivative Features
│ ├─ Rolling Mean/Std/Min/Max
│
├── Sequence Construction (Sliding Window)
│
├── Stage 1: BiGRU + Multi-Head Attention
│ └─ Weighted MSE Loss (Low-SoC Focused)
│
├── Stage 2: Transformer (Error Correction)
│ └─ Learns Residual Errors from Stage 1
│
├── Evaluation Metrics
│ ├─ MSE / MAE / R²
│ ├─ % Improvement after Correction
│
└── Visualization & Reporting


---

## 🧠 Core Concepts

### 🔹 BiGRU with Multi-Head Attention
Captures both **past and future dependencies** while refining attention on the most relevant temporal features.

### 🔹 Transformer for Error Correction
Learns **systematic residual patterns** (bias, drift, temporal error trends) from BiGRU predictions and corrects them.

### 🔹 Weighted Loss Function
Gives **extra importance to low SoC** values to reduce underestimation risk in critical discharge zones:
```python
low_soc_threshold = 25.0
high_error_weight = 15.0

📊 Performance Metrics

The script outputs a full comparison of pre- and post-correction performance:

Metric	BiGRU (Base)	BiGRU + Transformer (Corrected)	 % Improvement
MSE	Lower = Better	             ✅ Improved	            ✔ Significant
MAE	Lower = Better	             ✅ Improved	            ✔ Noticeable
R²	Higher = Better	             ✅ Improved	            ✔ Higher Fit

Additionally, the script prints standard deviation and mean of residual errors for stability analysis.

.

🧰 Requirements

Run "pip install -r requirements.txt" to install all the required dependencies.

📂 File Structure
project/
│
├── dataset/
│   ├── 2019.1.13.xlsx     # Base training dataset (normal SoC)
│   ├── 2019.1.21.xlsx     # Test dataset (contains low SoC)
│
├── hybrid_soc_predictor_with_error_correction.py
└── README.md

If datasets are missing, the script will automatically generate synthetic data to preserve functionality.

🧪 Usage
▶️ Run the training and evaluation:
python hybrid_soc_predictor_with_error_correction.py


The script will:

1. Load (or generate) training and test data.

2. Perform feature engineering.

3. Train the BiGRU model.

4. Train the Transformer on BiGRU residuals.

5. Evaluate and visualize results.


📈 Visualization Outputs

The notebook automatically produces:

1. Actual vs Predicted SoC

2. Error Prediction Over Time

3. Smoothed SoC Curve (Savitzky–Golay Filter)

4. Comparative performance logs

⚙️ Customization

You can tweak key hyperparameters:

Parameter	        Default	        Description
TIME_STEPS	          15	   Sliding window length
learning_rate	    0.0005	   Learning rate for Adam
high_error_weight     15.0	   Emphasis on low SoC penalty
num_heads	          4	       Transformer attention heads
dropout_rate	  0.4 - 0.5	   Regularization strength

🧩 Error Correction Impact

The second stage effectively reduces prediction drift and enhances robustness near critical discharge points, quantified via:

ΔMSE (Mean Squared Error reduction)

ΔMAE (Mean Absolute Error reduction)

ΔR² (Improvement in explained variance)

📜 License

This project is released under the MIT License.
Feel free to use, modify, and extend it for research or production use.