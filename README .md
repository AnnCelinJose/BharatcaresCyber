# Malware Classification using Machine Learning (PE Header Features)

Project for the **IBM SkillsBuild Cyber Security BootCamp 2026**, conducted by **BharatCares**.

## Project Description
Malware is one of the most common threats to computer systems. This project builds a machine learning model that decides whether a Windows executable (PE file) is **benign** or **malware**, using only static features from the file's PE header. Because the file is never executed, the analysis is fast and safe.

Four classifiers are trained and compared: Logistic Regression, Decision Tree, Random Forest and Gradient Boosting. The best model is evaluated with accuracy, precision, recall, F1-score, ROC-AUC and a confusion matrix. Feature importance shows which header fields best separate malware from benign files.

## Dataset
- **Name:** ClaMP (Classification of Malware with PE headers)
- **Author:** Ajit Kumar (2020), Mendeley Data, DOI: [10.17632/xvyv59vwvz.1](https://doi.org/10.17632/xvyv59vwvz.1)
- **Also on Kaggle:** search for *Classification of Malwares (CLaMP)* by saurabhshahane
- **File used:** `ClaMP_Integrated-5184.csv` (5,210 rows as loaded: 2,488 benign and 2,722 malware; 69 feature columns. After removing 3 duplicate rows, 1 text column and 8 constant columns, 5,207 samples and 60 features are used)
- **Target column:** `class` (0 = benign, 1 = malware)

The dataset is not included in this repository. Download it and place the CSV in the project folder.

## Technologies Used
- Python 3.9+
- pandas, NumPy
- scikit-learn
- matplotlib, seaborn
- joblib
- Jupyter Notebook

## Project Structure
```
.
├── AnnCelinJose_MalwareClassification.ipynb   # main notebook
├── requirements.txt                       # dependencies
├── README.md                              # this file
├── AnnCelinJose_ProjectReport.docx            # project report
└── ClaMP_Integrated-5184.csv              # dataset (download separately)
```

## Setup and Run Instructions
1. Clone the repository
   ```bash
   git clone <your-repository-url>
   cd <repository-folder>
   ```
2. (Optional) create a virtual environment
   ```bash
   python -m venv venv
   source venv/bin/activate        # Windows: venv\Scripts\activate
   ```
3. Install dependencies
   ```bash
   pip install -r requirements.txt
   ```
4. Download `ClaMP_Integrated-5184.csv` from the dataset link above and put it in the project folder.
5. Start Jupyter and open the notebook
   ```bash
   jupyter notebook
   ```
6. Choose **Kernel > Restart & Run All**.

## Outputs
Running the notebook creates:
- `model_results.csv`: comparison table of all models
- `malware_classifier.joblib`: the saved best model
- `class_distribution.png`, `model_comparison.png`, `confusion_matrix.png`, `roc_curves.png`, `feature_importance.png`

## Results
Test-set results (20% hold-out, 1,042 of the 5,207 samples):

| Model | CV Accuracy | Test Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---|---|---|---|---|---|
| Random Forest | 0.9902 | 0.9894 | 0.9890 | 0.9908 | 0.9899 | 0.9991 |
| Gradient Boosting | 0.9866 | 0.9846 | 0.9818 | 0.9890 | 0.9853 | 0.9988 |
| Decision Tree | 0.9745 | 0.9674 | 0.9636 | 0.9743 | 0.9689 | 0.9671 |
| Logistic Regression | 0.9582 | 0.9616 | 0.9667 | 0.9596 | 0.9631 | 0.9904 |

Random Forest was the best model: 539 of 544 malware files were detected (5 missed) and 6 of 498 benign files were wrongly flagged. The most important feature was `FH_char12`, followed by `CheckSum`, `E_file`, `OH_DLLchar2` and `OH_DLLchar0`.

## Key Information
- Random seed is fixed (`42`) so results are reproducible.
- Data split: 80% training, 20% testing, stratified by class.
- Models are compared with 5-fold stratified cross-validation, then scored once on the held-out test set.
- **Limitations:** the dataset is relatively small, and attackers can alter header values, so a header-only model should support, not replace, other defences.

## Author
Ann Celin Jose, CSE, Sahrdaya College of Engineering and Technology (Autonomous), Kodakara
