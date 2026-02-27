# 🎵 DJing with Data: Validating the Normal Distribution

### *Part 3: Testing the Empirical Rule on Track Duration*

![Spotify Analysis](https://img.shields.io/badge/Project-Spotify%20Distribution%20Analysis-1DB954?style=for-the-badge&logo=spotify&logoColor=white)
![Statistics](https://img.shields.io/badge/Method-Empirical%20Rule%20(68--95--99.7)-orange?style=for-the-badge)
![Data](https://img.shields.io/badge/Data-Normality%20Testing-blue?style=for-the-badge)

---

## 📚 The Spotify Data Science Trilogy

This repository is the third and final installment of a comprehensive statistical deep-dive into Spotify track data.

  1. [Part 1: Descriptive Statistics](https://github.com/ayushi-gajendra/Spotify-hit-analysis_Descriptive-Statistics) - Summarizing the "DNA" of hit songs.
  2. [Part 2: Inferential Statistics](https://github.com/ayushi-gajendra/Spotify-hit-analysis_Inferential-Statistics) - Modeling track modality and predicting playlist outcomes.
  3. **Part 3: Distribution Analysis (This Repo)** - Validating mathematical models against industry data.

---

## 📌 Project Overview

Following my initial descriptive and inferential analyses, this project serves as the final validation layer. The objective is to determine if track duration follows a **Normal Distribution**, allowing for the use of parametric statistics to predict track lengths and identify operational outliers.

---

## 📂 Business Scenarios Modeled

  * **Quantifying Predictability:** Determining the exact probability of a track length before it is played.
  * **Operational Thresholds:** Statistically identifying "Outliers" (interludes vs. extended mixes) to automate curation logic.
  * **Resource Optimization:** Using the **68-95-99.7 Rule** to ensure track sets stay within a standard listener engagement window.

---

### 📂 Data Reference
* **Source:** [Spotify Tracks Dataset via Hugging Face](https://huggingface.co/datasets/maharshipandya/spotify-tracks-dataset)
* **Sample Size:** 3,000 tracks selected for granular statistical modeling.
* **Primary Fields:** `duration_ms` (Track length) and `acousticness`.

--- 

## ⚙️ Methodology: The Normality Stress Test


### 1. Visual Distribution Auditing (Intuition)

* I audited the visual "shape" of various features to identify potential models.
  
  <p align="center">
    <img src="Distribution of Acousticness.png" width="60%" />
  </p>
  
  <p align="center">
    <img src="Distribution of Track Duration (ms).png" width="60%" />
  </p>

* **Observation:** `Acousticness` followed a **Power Law Distribution**, where values are heavily skewed toward the lower end.

* **Selection:** `duration_ms` appeared symmetric and mound-shaped, making it the primary candidate for a **Normal Distribution** model.


### 2. Testing the Empirical Rule (Logic)

* I calculated **Sample Statistics** to see how closely the real-world data followed the theoretical **68-95-99.7 Rule**.

  | Range from Mean | Theoretical Normal | Actual Spotify Data | Result |
  | --- | --- | --- | --- |
  | **Within 1 σ (68% Rule)** | **68.00%** | **76.80%** | High Concentration |
  | **Within 2 σ (95% Rule)** | **95.00%** | **95.57%** | Strong Correlation |
  | **Within 3 σ (99.7% Rule)** | **99.70%** | **98.67%** | Standard Variance |

* **Logic:**
    The 76.8% result indicates that Spotify tracks are *more* concentrated around the average than a standard bell curve. This suggests an industry-wide "standard" for track length that exerts more pressure on the data than natural randomness alone.


### 3. Probability Density Function (PDF) Modeling

To compare the "Actual" data against a "Theoretical" ideal, I generated a PDF curve.

* **Calculating the X-Axis (Z-Values)**

  To create a smooth curve, I generated 50 equally spaced duration values (Z) across the histogram's range:

    * **Start Point:** $5,000$ ms
    * **Step Logic:** `=start_point or prev_cell + (600,000 - 5,000) / 50`
    * **Result:** A linear range from 5,000 to 600,000 ms.

* **Mapping the Density (PDF Values)**

    Using the `NORM.DIST` function, I calculated the probability density for each Z-value:
    
    **Formula:** `=NORM.DIST(z, 216299, 55427, FALSE)`


* 📊 **Normal Distribution & PDF Mapping (First 10 Values)**:
  
    Here is the distribution table for the first 10 calculated values of the PDF model.

  | Track Duration (Z-Value) | PDF Value (Density) | 
  | --- | --- | 
  | **5,000 ms** | 0.0000000050 | 
  | **16,900 ms** | 0.0000000111 | 
  | **28,800 ms** | 0.0000000236 | 
  | **40,700 ms** | 0.0000000476 | 
  | **52,600 ms** | 0.0000000919 | 
  | **64,500 ms** | 0.0000001692 | 
  | **76,400 ms** | 0.0000002977 | 
  | **88,300 ms** | 0.0000005002 | 
  | **100,200 ms** | 0.0000008025 | 
  | **112,100 ms** | 0.0000012296 | 

---

### 🔬 Analytical Logic behind these Values

  * **Calculation for X-Axis (Z-Values):** To ensure a smooth visualization, 50 points were generated.
 
  * **Note:** Only the **first 10 values** are displayed here to illustrate the rising slope of the left tail.
  
  * **Step Logic:** Starting at `5,000`, each subsequent value increases by `= prev_cell_value + (600,000 - 5,000) / 50`.
  
  * **The PDF Value:** This represents the height of the bell curve calculated via `=NORM.DIST(z, 216299, 55427, FALSE)`.
  
  * **The Strategic Narrative:** These first 10 values quantify why "short" songs are rare in a Normal Distribution—the density is nearly 1,500x lower at 5,000ms than it is at the 219,000ms peak.
  
  * **The Curve Alignment:** By setting the cumulative parameter to FALSE, I modeled the exact probability "height" for 50 equally spaced duration values, rather than the area under it. This allowed for a direct overlay against the actual histogram. By comparing the peaks of this table to the actual histogram, we can prove that while the "Ideal" peak is at ~219k, the "Actual" data is slightly skewed — a key insight for operational strategy.

---

* **Visualazition**

  <p align="center">
    <img src="PDF of Z ~ Normal(216299, 55427).png" width="60%" />
  </p>
  
  <p align="center">
    <img src="Distribution of Track Duration (ms).png" width="60%" />
  </p>

* **Logic:**
    

---

## 🔬 Analytical Deep Dive: Why Simulate & Model?

  We simulate and model to move from "Ideal" Population Parameters to "Actual" Sample Statistics.

  * **Quantifying Volatility:** Simulation quantifies the **Standard Error**—showing how much our sample mean might "wiggle" in a real-world batch of songs compared to the theoretical mean.
  
  * **The Outlier Effect:** While a Mean tells us the average, simulation reveals the **Tails**. Seeing an extreme outlier in a sample run forces a manager to plan for extremes (like a 10-minute song) that a standard average ignores.
  
  * **Defining Thresholds:** By simulating thousands of batches, we can set **Operational Thresholds**—knowing exactly when a result is a statistical fluke versus a systemic industry shift.

---

## 📈 Strategic Insights

  * **Normality Approximation:** The 95.57% correlation at 2-Standard Deviations proves that a Normal Distribution is a reliable approximation for 95% of the catalog.
  
  * **Outlier Management:** Tracks falling outside the 3-Sigma range (1.33% of the data) are statistically significant outliers. **Recommendation:** Automated playlist algorithms should flag these tracks for manual review as they likely represent non-standard content like ambient interludes or live jams.
  
  * **Mean vs. Median:** Because of slight skewness, the Mean and Median are not perfectly aligned. For high-precision operations, the **Median** should define "Typical" song length to avoid being pulled by extreme outliers.

---

## 🛠️ Summary of Statistical Logic

  | Function | Statistical Concept | Business Application |
  | --- | --- | --- |
  | **STDEV.S** | Sample Std. Deviation | Measuring the "Spread" and volatility of song lengths. |
  | **NORM.DIST** | PDF Modeling | Creating an "Ideal" mathematical baseline for comparison. |
  | **Sigma Rules** | Empirical Validation | Testing data predictability against a theoretical bell curve. |
  
---

## 🎓 Project Credits

Developed as part of the **Applied Statistics for Data Analytics** specialization by **DeepLearning.AI**.

---

## 👤 About the Author
**Ayushi Gajendra** 

*Data Analyst | Former EdTech Co-Founder*

* **7+ Years** of experience in business operations, strategic growth, and entrepreneurial leadership.
* I specialize in bridging the gap between raw data and **high-stakes business decisions**.
* My goal is to help organizations move beyond "gut feeling" to drive growth through evidence-based strategy.
