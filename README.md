# Summary: Quantization and Fine-Tuning in LLMs

---

## 📖 Overview
This document provides a conceptual framework for understanding how **Quantization** enables the fine-tuning and deployment of Large Language Models (LLMs) on resource-constrained hardware by reducing memory precision.

---

## 🛠 Key Definitions

| Term | Definition |
| :--- | :--- |
| **Quantization** | Converting data from high-bit formats (e.g., FP32) to lower-bit formats (e.g., INT8). |
| **Full Precision (FP32)** | 32-bit floating-point numbers; the standard for high-accuracy training. |
| **Half Precision (FP16)** | 16-bit floating-point numbers; reduces memory by 50% vs FP32. |
| **UINT8** | 8-bit unsigned integers (range: 0–255). |
| **Calibration** | The process/formula used to map high-precision weights to lower-bit representations. |

---

## 🏗 Quantization Techniques

### 1. Symmetric Quantization
* **Assumption:** Weights are distributed symmetrically around zero.
* **Mechanism:** Uses a single **Scale Factor** for linear mapping.
* [0......1000] -> [0-255]
  
min_max_scaler = (xmax - xmin)/(qmax-qmin) = 1000-0/255-0 = 3.92 (This is Scale)

* for number 250, round(250/3.92) = 64,(quantized value)


### 2. Asymmetric Quantization
* **Assumption:** Weights are skewed or non-symmetric.
* **Mechanism:** Introduces a **Zero-Point** (offset) to ensure the range is correctly mapped to the target bit-width.
* [-20...1000] -> [0-255]

min_max_scaler = (xmax - xmin)/(qmax-qmin) = 1000+20/255-0 = 4 = 4 (This is Scale
* Round(-20/4) = -5, here -5+5 = 0, so 5 is zero point here.

---

## 🔄 Modes of Execution

1.  **Post-Training Quantization (PTQ):**
    * Quantizing a model after it has been fully trained.
    * *Trade-off:* Fast and easy, but may result in a slight drop in accuracy.
    * Pretrained model -> Calibration -> Quantized Model (but data loss occurs and accuracy descreases)
2.  **Quantization-Aware Training (QAT):**
    * Simulates quantization error during the training/fine-tuning process.
    * *Trade-off:* More computationally intensive, but retains higher accuracy.
    * Quantization Aware Training: Pretrained model -> Quantization-> Fine-Tuning (take new training data) -> Quantized Model

---

## 📊 Technical Parameters & Structures

### Mathematical Components
* **Scale Factor:** The ratio used to map the original float range to the new integer range.
* **Zero Point:** The integer value in the quantized range that corresponds to "0" in the floating-point range.

### Bit Structure Comparison

**FP32 Structure:**
* 1 Sign bit
* 8 Exponent bits
* 23 Mantissa (Fraction) bits

**FP16 Structure:**
* 1 Sign bit
* 5 Exponent bits
* 10 Mantissa (Fraction) bits



---
*Summary based on the presentation by Kish Naak.*
