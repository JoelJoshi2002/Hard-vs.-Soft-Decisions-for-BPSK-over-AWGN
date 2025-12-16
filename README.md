# Hard vs. Soft Decision Decoding for BPSK with Repetition Coding

**Author:** Joel Joshi  
**Date:** 16-12-2025  
**Project Artifacts:** `IT_Project.ipynb` (Code), `IT_Project.pdf` (Report)

## 📌 Project Overview
This project evaluates the performance differences between two fundamental Forward Error Correction (FEC) decoding strategies: **Hard Decision Decoding** and **Soft Decision Decoding**. 

Using **Binary Phase Shift Keying (BPSK)** modulation over an **Additive White Gaussian Noise (AWGN)** channel, the project simulates a communication system protected by a simple **Repetition Code (3,1)**. The study validates that Soft Decision decoding offers superior error performance compared to Hard Decision decoding, particularly in noisy environments.

The project is divided into two key components:
1.  **Theoretical Verification:** A Monte Carlo simulation generating Bit Error Rate (BER) curves to quantify "Soft Decision Gain" and analyze the impact of LLR quantization (3-bit and 4-bit).
2.  **Practical Application:** A real-world demonstration transmitting medical **Electrocardiogram (ECG)** data through a noisy channel to visualize how Soft Decision decoding preserves critical signal features.

---

## 🛠️ Features
* **BPSK Modulation & AWGN Channel:** Simulates physical layer transmission with tunable Signal-to-Noise Ratio (SNR).
* **Repetition Code (3,1):** Implements a robust redundancy coding scheme where every bit is repeated three times.
* **Decoding Algorithms:**
    * **Hard Decision:** Uses a simple majority vote mechanism on thresholded bits.
    * **Soft Decision:** Uses Log-Likelihood Ratios (LLR) to aggregate "confidence" levels for higher accuracy.
* **Hardware Quantization Simulation:** Simulates limited hardware precision by quantizing LLR values to 3-bit and 4-bit integers.
* **Soft Decision Gain Calculation:** Automatically calculates the dB gain achieved by Soft Decision at a target BER (e.g., $10^{-3}$).
* **ECG Signal Reconstruction:** Visual comparison of an original ECG signal against reconstructed versions from Hard and Soft decoders.

---

## 💻 Methodology & Code Structure

The implementation is contained within `IT_Project.ipynb`.

### 1. The Encoder
* **Function:** `encode_repetition(bits)`
* **Logic:** Repeats every input bit three times (e.g., `1` $\rightarrow$ `[1, 1, 1]`).

### 2. The Channel
* **Function:** `run_simulation_repetition`
* **Modulation:** Maps `0` to `+1.0` and `1` to `-1.0` (BPSK).
* **Noise:** Adds Gaussian noise calculated based on the target $E_b/N_0$.

### 3. The Decoders
* **Hard Decoder (`decode_hard_repetition`):** * Thresholds received signal ($>0 \to 0, <0 \to 1$).
    * Performs a **Majority Vote** on triplets.
* **Soft Decoder (`decode_soft_repetition`):** * Calculates LLRs ($2y/\sigma^2$).
    * Sums LLRs for the triplet; a positive sum indicates `0`, negative indicates `1`.

### 4. Quantization
* **Function:** `quantize_LLR`
* **Purpose:** Maps continuous LLR values to discrete levels to study hardware trade-offs (e.g., 3-bit quantization = 8 levels).

---

## 📊 Results

### Theoretical Verification (BER vs SNR)
The simulation compared Hard Decision against Soft Decision (Float, 3-bit, and 4-bit). 
* **Target BER:** $10^{-3}$
* **Hard Decision Requirement:** ~8.15 dB
* **Soft Decision Requirement:** ~6.77 dB
* **Soft Decision Gain:** **~1.38 dB**

*Note: Theoretical gain for simple codes is typically around 2dB; the quantization analysis shows that 4-bit precision achieves near-optimal performance.*

### Practical Application (ECG)
The ECG demonstration highlights that at low SNR, Hard Decision decoding frequently corrupts the medical waveform, making diagnosis impossible. In contrast, Soft Decision decoding effectively reconstructs the signal, preserving the QRS complex and other vital features.

![ECG Signal Reconstruction](images/ECG-2.png)
---

## ⚙️ Installation & Usage

### Prerequisites
The project requires Python and the following libraries:
* `numpy`
* `matplotlib`
* `scipy`
* `wfdb` (Waveform Database package for ECG data)
* `pandas`

### Setup
1.  Install the required packages:
    ```bash
    pip install numpy matplotlib scipy wfdb pandas
    ```
2.  Open the Jupyter Notebook:
    ```bash
    jupyter notebook IT_Project_Final.ipynb
    ```

### Running the Simulation
1.  **Run All Cells:** Execute the cells sequentially.
2.  **View Results:**
    * The console will output the BER table for different SNR levels ($0-11$ dB).
    * The "Gain Calculation" block will compute the specific dB gain.
    * A plot will generate showing the BER curves (log scale) for all decoding methods.
    * (If ECG data is available) A waveform plot will compare Original, Hard-Decoded, and Soft-Decoded ECG signals.

---

## 📚 References
1.  **FEC Definition:** GeeksforGeeks, "Forward Error Correction in Computer Networks."
2.  **Hard vs Soft Decisions:** GaussianWaves, "Hard and Soft Decision Decoding."
3.  **Simulation & Algorithms:** Proakis, J. G., & Salehi, M. (2008). *Digital Communications*. McGraw-Hill Education.
