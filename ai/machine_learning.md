# Machine Learning Cheat Sheet

## Glossary

* 人工智能: 使机器具有模仿人类行为的能力

* 机器学习: 利用统计技术来训练机器，从而让机器从数据中学习

* 深度学习: 采用深层的多层神经网络网络进行预测, 在计算机视觉, 语音识别, 自然语言等方面表现优异

---

## **1. Machine Learning Types**  
- **Supervised Learning:** Labeled data → Predict output  
  - Regression: Predict continuous values (e.g., Linear Regression, Decision Trees)  
  - Classification: Predict discrete labels (e.g., Logistic Regression, SVM, Random Forest, Neural Networks)  
- **Unsupervised Learning:** No labeled data → Find structure  
  - Clustering: (e.g., K-Means, DBSCAN)  
  - Dimensionality Reduction: (e.g., PCA, t-SNE)  
- **Reinforcement Learning:** Agent learns by interacting with the environment (e.g., Q-learning, Deep Q Networks)  

---

## **2. Data Preprocessing**  
- **Feature Scaling:**  
  - Standardization: \( X' = \frac{X - \mu}{\sigma} \)  
  - Normalization: \( X' = \frac{X - X_{\min}}{X_{\max} - X_{\min}} \)  
- **Feature Engineering:**  
  - Encoding categorical variables (One-Hot Encoding, Label Encoding)  
  - Handling missing values (Mean/Median Imputation, Dropping)  
- **Data Splitting:**  
  - Train/Test Split (e.g., 80/20)  
  - Cross-Validation (e.g., K-Fold CV)  

---

## **3. Key Algorithms**  
- **Regression:** Linear Regression, Ridge, Lasso, Decision Trees  
- **Classification:** Logistic Regression, SVM, Random Forest, KNN, Neural Networks  
- **Clustering:** K-Means, DBSCAN, Hierarchical Clustering  
- **Dimensionality Reduction:** PCA, t-SNE  
- **Neural Networks:** MLP, CNNs (for images), RNNs (for sequences)  
- **Ensemble Methods:** Bagging (Random Forest), Boosting (XGBoost, AdaBoost)  

---

## **4. Model Evaluation Metrics**  
- **Regression:** MSE, RMSE, MAE, R²  
- **Classification:** Accuracy, Precision, Recall, F1-score, ROC-AUC  
- **Clustering:** Silhouette Score, Davies-Bouldin Index  

---

## **5. Model Optimization**  
- **Hyperparameter Tuning:**  
  - Grid Search  
  - Random Search  
  - Bayesian Optimization  
- **Regularization:**  
  - L1 (Lasso)  
  - L2 (Ridge)  
- **Handling Overfitting:**  
  - More data, Regularization, Dropout (NNs), Early Stopping  

---

## **6. Deep Learning Basics**  
- **Neural Network Layers:**  
  - Input → Hidden → Output  
  - Activation Functions: ReLU, Sigmoid, Softmax  
- **Optimizers:** SGD, Adam  
- **Loss Functions:** Cross-Entropy (Classification), MSE (Regression)  

---

## **7. Tools & Libraries**  
- **Python:** NumPy, Pandas, Scikit-Learn, TensorFlow, PyTorch  
- **Data Visualization:** Matplotlib, Seaborn  

