# 🧠 EEG-Based Music Preference Detection

This project uses real-time EEG data to classify whether a person likes or dislikes a song. It integrates a live Spotify playback system with an OpenBCI Cyton board, performs deep signal processing, extracts time-frequency features, and trains multiple machine learning and deep learning models — including a Transformer architecture.

---

## 🎧 Real-Time System Overview

- ✅ **Spotify API integration**: Plays random songs from “liked” and “disliked” playlists.
- ✅ **Live EEG acquisition**: Records brain activity using 8 OpenBCI Cyton channels.
- ✅ **Automated labeling**: Saves each EEG trial with track metadata and user feedback.
- ✅ **Impedance control**: Swim cap + gel + alcohol prep for impedance < 45 kΩ.

---

## 📈 Data Collection

- Total recordings: **600+ EEG trials**
- Trial length: **20 seconds**
- First 2.5 seconds cut (dongle noise)
- Rest periods: **15 seconds** between songs
- 8 electrodes: F1, F2, FT7, FT8, T7, T8, P3, P4 (varies with configuration)
- One subject, multiple recording sessions

---

## 🧼 Preprocessing Pipeline

- **High-pass filter** at 1 Hz (removes drift)
- **Notch filter** at 50 Hz (removes electrical noise)
- **Butterworth bandpass filter**: 4–40 Hz
- **Wavelet denoising**: `db4`, level 2
- **Windowing**: 3s segments, 1s stride

---

## 🧬 Feature Extraction

Extracted per window & per channel:
- Welch PSD (theta, alpha, beta, gamma)
- Hjorth parameters (mobility, complexity)
- Shannon Entropy
- Zero Crossing Rate (ZCR)
- Spectral Entropy
- Wavelet coefficients
- Fractal Dimension
- PLV (phase synchronization)

---

## 🧠 Classical ML Results (RFE + PCA + Stratified K-Fold)

| Model           | Train Acc | Test Acc | Kappa | Overfit Gap |
|----------------|-----------|----------|--------|--------------|
| **KNN**         | 72%       | **65%**  | 0.30   | 0.07         |
| SVM             | 63%       | 60%      | 0.20   | 0.03         |
| Random Forest   | 76%       | 64%      | 0.27   | 0.13         |
| XGBoost         | 68%       | 64%      | 0.28   | 0.04         |
| MLP             | 70%       | 62%      | 0.24   | 0.08         |

All models trained using:
- 🔁 5-Fold Stratified Cross-Validation  
- 🔍 RFE (Recursive Feature Elimination)  
- 📉 PCA (95% explained variance retained)

---

## 🤖 Deep Learning (Transformer Model)

A Transformer model was trained on time-windowed EEG feature sequences.

| Metric                | Result            |
|-----------------------|-------------------|
| Mean Test Accuracy    | **68.6% ± 9.2%**   |
| Mean Train Accuracy   | 72.7% ± 10.2%      |
| Input Shape           | (windows × features) |
| Optimizer             | Adam (lr=5e-4)     |
| Regularization        | Dropout + L2       |

---

## 🔬 Signal Quality

- SNR analysis done using baseline recordings
- Wavelet-based power comparison with no-stimulus baseline
- Final configuration showed better separation between “liked” and “disliked” responses (especially in alpha/theta bands)

---

## 📚 Tools & Tech Stack

- Python, NumPy, SciPy, MNE, PyWavelets  
- TensorFlow / Keras, Scikit-learn, XGBoost  
- Spotify API (Spotipy)  
- BrainFlow SDK (OpenBCI)  
- Matplotlib, Seaborn

---

## 💡 Conclusions

- Classical models perform reasonably well, with **KNN reaching 65% accuracy**
- Transformer-based models show promising results with more data
- Signal quality, filtering, and electrode placement are crucial
- Project will benefit from continued training and expansion to multiple users

---


## 📂 Files and Directory Structure

This repository contains various scripts, datasets, and visualization tools related to EEG-based music preference analysis.

| File Name | Description |
|-----------|------------|
| **20_martie.zip** | EEG data from a single recording session (npy format, 20-minute session). |
| **Deep_Learning_approach.py** | Implements a deep learning model using Transformer blocks and CNN layers. |
| **Electrodes_first_placement_setup.jpeg** | Image of the first electrode placement. |
| **Electrodes_second_placement_setup.jpeg** | Image of the second electrode placement (8-electrode configuration). |
| **OpenBCI_Cyton_Board.jpeg** | Image of the OpenBCI Cyton board used for EEG recording. |
| **PSD_plot_visualization.py** | Script for visualizing Power Spectral Density (PSD) features extracted from EEG data. |
| **README.md** | The project documentation and instructions. |
| **Recorded_data_visualization.py** | Script for visualizing recorded EEG data . |
| **SNR_check.py** | Script for checking Signal-to-Noise Ratio (SNR) of EEG recordings. |
| **cohen_kappa_sklearn_models_accuracy.py** | Script for evaluating machine learning model performance using Cohen's Kappa score. |
| **recording_data_script.py** | EEG recording script that does not include arousal metric calculations. |
| **recording_npy_data+arousal_metric.py** | EEG recording script with arousal metric calculations included. |

This directory structure ensures a clear separation between data collection, preprocessing, feature extraction, model training, and evaluation.



