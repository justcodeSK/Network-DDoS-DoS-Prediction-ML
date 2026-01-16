# Network DoS & DDoS Prediction using ML
## Network Traffic Analysis & Machine Learning Pipeline

This project focuses on **analyzing, preprocessing, and classifying network traffic** using machine learning. The main training and evaluation were performed on **network traffic data captured from the Attack Net Zero project** using HPING3 in a controlled virtualized environment. The system also references **CIC-IDS-2017, CIC-IDS-2018, and CIC-DDoS-2019 datasets** as part of the testing and benchmarking phase. The complete pipeline performs **feature extraction, preprocessing, encoding, scaling, class balancing, hybrid feature selection, hyperparameter tuning, ensemble learning, evaluation, and visualization**.


---

## Features Extracted

The following features were used for analysis and modeling:

| Feature | Description |
|---------|-------------|
| FRAG    | Fragmented packets |
| RST     | TCP Reset flag |
| FIN     | TCP Finish flag |
| ACK     | TCP Acknowledgement flag |
| PSH     | TCP Push flag |
| DDOS    | Indicates DDoS attack traffic |
| UDP     | UDP protocol packets |
| SYN     | TCP Synchronize flag |
| BENIGN  | Normal traffic monitored from self virtualized environment |
| ICMP    | ICMP protocol packets |

---

## Data Preprocessing

Key preprocessing steps:

1. **Handling Null and Space Values** – Cleaned missing or empty entries.  
2. **Encoding** – Converted categorical labels into numeric values via a simple mapping function.  
3. **Scaling** – Standardized numeric features for models sensitive to feature magnitude.  
4. **Hybrid Feature Selection** – Combined filter, wrapper, and embedded methods to remove irrelevant or redundant features. Improved **accuracy, precision, and F1 score**.  
5. **Class Balancing (SMOTE)** – Synthetic oversampling of minority classes to reduce bias toward dominant classes.

---

## Machine Learning Models

| Model           | Description                               | Key Parameters / Notes |
|-----------------|-------------------------------------------|------------------------|
| XGBoost         | Gradient boosting ensemble                 | Tuned `n_estimators`, `max_depth`, `learning_rate` |
| SVM             | Support Vector Machine                     | Tuned `C`, `gamma`, `kernel` |
| KNN             | k-Nearest Neighbors                        | Tuned `n_neighbors`, `weights`, `p` |
| MLP             | Multi-layer Perceptron                      | Tuned `hidden_layer_sizes`, `activation`, `solver`, `learning_rate` |
| Random Forest   | Tree-based ensemble classifier             | Used as meta learner in stacking ensemble |

---

## Ensemble Learning

1. **Voting Ensemble (MLP-based)**  
   - Combined multiple MLP classifiers with soft voting.  
   - Improved stability and reduced variance of predictions.  

2. **Stacking Ensemble**  
   - Base learners: XGBoost, SVM, KNN, Voting-MLP.  
   - Meta-learner: Random Forest classifier.  
   - Leveraged predictions of base models to train a higher-level model, achieving **robust performance**.

---

## Model Training & Hyperparameter Tuning

- **RandomizedSearchCV** with StratifiedKFold for cross-validation.  
- Tuned hyperparameters for all base learners to maximize accuracy and F1 score.  
- **Voting ensemble** created by training three MLP models with different random seeds.  
- **Stacking ensemble** combines base models with a Random Forest meta-learner for final predictions.

---

## Evaluation & Visualization

- **Confusion Matrix** – Visualized with Seaborn heatmaps for all models.  
- **Classification Report** – Accuracy, Precision, Recall, F1 Score for each model.  
- **ROC Curve** – Multi-class ROC and AUC for ensemble model.  
- **Feature Importance** – Random Forest feature importances plotted for top features.  
- **Borderline Sample Analysis** – Identified samples where models are uncertain (probability between 0.4–0.6).  

Example metrics for Stacking Ensemble:

| Metric       | Score  |
|--------------|--------|
| Accuracy     | 99.4%  |
| Precision    | 99.3%  |
| Recall       | 99.4%  |
| F1 Score     | 99.4%  |

---

## Prediction Examples

- Used **real samples** from benign traffic and various attacks (PortScan, DDoS, DoS Hulk, DoS GoldenEye).  
- Predicted using **Random Forest, Logistic Regression, and Neural Network** models.  
- Plotted **probabilities for each class** to visualize confidence.  
- Correctly distinguished **BENIGN vs. attack traffic**.

---

## Model & Scaler Saving

- Models saved using `joblib` for later inference:  
  - Random Forest: `random_forest_model.pkl`  
  - Logistic Regression: `logistic_regression_model.pkl`  
  - Neural Network: `neural_network_model.pkl`  
- Feature scaler saved: `scaler.pkl`  
- Loaded models reproduce predictions and probabilities for new data.

---

## Visualization

- **Matplotlib & Seaborn** used for:  
  - Confusion matrices  
  - Feature importance plots  
  - Probability bar charts for sample predictions  
  - ROC curves
---

## Features Extracted

The following features were used for analysis and modeling:

| Feature             | Description |
|--------------------|-------------|
| Source IP           | Source IP address of the packet |
| Destination IP      | Destination IP address of the packet |
| Protocol            | Protocol type (TCP, UDP, ICMP, etc.) |
| Src Port            | Source port number |
| Dst Port            | Destination port number |
| TTL                 | Time to Live value |
| Length              | Packet length |
| Payload             | Packet payload size |
| Flow Duration       | Duration of the flow |
| PPS                 | Packets per second |
| BPS                 | Bytes per second |
| Inter-arrival       | Inter-arrival time between packets |
| IP Header Len       | IP header length |
| Packet ID           | IP packet ID |
| Frag Offset         | Fragment offset |
| TCP Window          | TCP window size |
| TCP Seq             | TCP sequence number |
| TCP Ack             | TCP acknowledgment number |
| TCP Urg Ptr         | TCP urgent pointer |
| ICMP Type           | ICMP message type |
| ICMP Code           | ICMP message code |
| SYN                 | TCP SYN flag |
| ACK                 | TCP ACK flag |
| RST                 | TCP RST flag |
| FIN                 | TCP FIN flag |
| PSH                 | TCP PSH flag |
| URG                 | TCP URG flag |
| Total Packets       | Total number of packets in the flow |
| Total Bytes         | Total bytes in the flow |
| Packet Size Std     | Standard deviation of packet sizes |
| Burstiness          | Measure of traffic bursts |
| Unique Dst Ports    | Number of unique destination ports |
| Label               | Traffic label: BENIGN or attack type |

---

## Attack Net Zero - Network-Level DoS & DDoS Detection System

This sub-project, **Attack Net Zero**, is a real-time network monitoring and intrusion detection system designed to detect, analyze, and mitigate Denial of Service (DoS) and Distributed Denial of Service (DDoS) attacks. It leverages machine learning, ethical hacking techniques, and virtualization to provide a practical and extensible cybersecurity solution.

### Key Features & Contributions

- **Real-Time Threat Detection:** Utilized ensemble machine learning models (Random Forest, SVM, KNN, MLP, XGBoost) to detect multiple types of DoS and DDoS attacks (SYN, ACK, RST, FIN, PSH, UDP, ICMP, Frag floods) with high accuracy using live network traffic.  
- **Integrated Packet Monitoring Engine:** Built a system using Scapy and PyShark to capture, analyze, and store network packets in real-time. Metrics like packets per second (PPS), bytes per second (BPS), and packet sizes are tracked and displayed interactively.  
- **Smart Feature Selection:** Reduced over 80 network traffic features to the most critical 28–29 using Random Forest feature importance to enhance model performance and efficiency.  
- **Attack Visualization & UI:** Developed a cyber-themed, responsive interface with Tailwind CSS, visualizing attack metrics, detection summaries, and traffic charts. Mobile-friendly with sidebar navigation and smooth transitions.  
- **Stacked Ensemble Learning:** Implemented a stacking ensemble with XGBoost, SVM, KNN, and MLP as base learners, and Random Forest as a meta-learner, achieving over 99.33% accuracy across multi-class attack scenarios.  
- **IP Blocking & Mitigation:** Enabled dynamic IP blocking at Layer 2 (ebtables) and Layer 3 (iptables) through a Django frontend. Users can block/unblock IPs in real-time with history stored persistently.  
- **Manual & Live Input Testing:** Supports manual data entry and live input from external systems for flexible evaluation and realistic attack simulation.  
- **Virtualized Multi-VM Testbed:** Created a scalable testbed with Windows XP (victim), Kali Linux (attacker), and monitor/controller nodes (Kali/Ubuntu/Windows) to emulate real-world DoS and DDoS scenarios.  
- **JSON-Based Reporting & Export:** Integrated structured reporting and advanced summarization of traffic anomalies using JSON and Gemini AI API.  
- **Educational Impact:** Serves as a hands-on learning tool for cybersecurity, traffic analysis, and mitigation strategies, aligned with industry practices.  

---

## Conclusion

- Implemented **end-to-end pipeline** for network traffic classification: **data preprocessing → feature engineering → model training → ensemble learning → evaluation → deployment**.  
- Achieved **high accuracy (99.4%)** using **hybrid feature selection, SMOTE balancing, and stacking ensemble**.  
- Workflow is **fully reproducible**, with models and scaler saved for inference on new traffic data.

---

This repository is ideal for researchers and developers aiming to **analyze network traffic and detect attacks using machine learning**, providing a **robust and explainable solution**.
