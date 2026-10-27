# 🏦 LoanSense: Loan Approval Prediction using Neural Networks

LoanSense is a web app that predicts whether a loan application will be approved. You enter the applicant's details, and a trained neural network instantly shows the approval probability and a final decision (Approved or Rejected). The whole model runs inside your browser, so no server or backend is needed.

> **Note:** This is an educational project. Do not use it for real loan decisions.

---

## What does it do?

1. You fill in 11 details about the applicant.
2. The app builds extra useful numbers from them (like monthly EMI).
3. A neural network gives a probability between 0% and 100%.
4. If the probability is above a set threshold (0.54), the loan is **Approved**. Otherwise it is **Rejected**.
5. The app also explains *why* the decision was made and what could change it.

---

## Inputs (11 fields)

| Field | Meaning |
|---|---|
| Gender | Male or Female |
| Married | Yes or No |
| Dependents | 0, 1, 2 or 3+ |
| Education | Graduate or Not Graduate |
| Self Employed | Yes or No |
| Property Area | Urban, Semiurban or Rural |
| Credit History | Good (1) or Poor (0) |
| Loan Amount Term | Loan duration in months |
| Applicant Income | Monthly income in ₹ |
| Coapplicant Income | Monthly income of co-applicant in ₹ |
| Loan Amount | Loan amount in thousands |

---

## How it works (step by step)

**1. Data**
The model is trained on loan application data with the 11 inputs above. The data used in this project is **synthetic** (artificially generated), so the numbers will change if you train on real data such as the Kaggle loan dataset.

**2. Feature engineering**
New features are created automatically:
- **TotalIncome** = applicant income + coapplicant income
- **EMI** = loan amount ÷ loan term (monthly payment, simple division)
- **Loan-to-Income ratio**
- **Log transforms** of income and loan amount (to reduce the effect of very large values)

**3. Preprocessing**
- Missing values are filled
- Numbers are scaled (standardized)
- Categories (like Property Area) are one-hot encoded

**4. Neural network (MLP)**
- Input layer, then **two hidden layers (128 neurons, then 64 neurons)**, then 1 output neuron
- **ReLU** activation in hidden layers, **sigmoid** at the output to give a probability
- **Adam** optimizer and **early stopping** to avoid overfitting
- Hyperparameters tuned with **GridSearchCV**
- About **11,905 trainable weights**
- Trained in Python with scikit-learn, then the learned weights are placed inside the web page so it can run in JavaScript

**5. Decision**
The probability is compared with a threshold of **0.54**, which was chosen using cross-validation.

---

## Model results

Tested on a held-out test set (20% of the data):

| Model | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---|---|---|---|---|
| **Neural Network (MLP)** | **0.852** | 0.875 | 0.947 | 0.909 | 0.800 |
| Logistic Regression | 0.840 | 0.859 | 0.953 | 0.903 | 0.793 |
| Random Forest | 0.840 | 0.860 | 0.951 | 0.903 | 0.792 |
| Majority baseline | 0.785 | 0.785 | 1.000 | 0.880 | 0.500 |

The neural network performs slightly better than the other models. The gap is small, which is common for tabular data like this.

---

## Features of the web app

- **Live prediction:** approval probability shown as a ring with an Approved or Rejected badge
- **Two demo applicants:** "Strong applicant" and "Risky applicant" buttons for quick testing
- **6 AI agents view:** shows the pipeline step by step (Intake, Feature, Neural, Risk, Decision, Advisor) with a live log. These agents are modules in the pipeline (rule-based plus the neural network). They are **not** an LLM.
- **Decision explained:**
  - *Input impact chart:* shows how much each input pushed the result toward approval or rejection (an ablation-based estimate, not exact SHAP)
  - *What to change:* suggests how much to reduce the loan or increase income to get approved, or how far the loan can go while staying approved
  - *Fairness check:* changes only Gender or Marital status and shows whether the decision changes
- **Model results table** and **live training replay** (loss and validation accuracy curve)
- **Visuals:** animated 3D neural network background, light and dark mode support, mobile friendly layout

---

## Tech stack

- **Training:** Python, scikit-learn (MLPClassifier, GridSearchCV, Logistic Regression, Random Forest)
- **App:** HTML, CSS, vanilla JavaScript (the model's forward pass is written in JavaScript)
- **Visuals:** Three.js (3D background), Canvas (charts and confetti)

---

## How to run

1. Download the `.html` file.
2. Open it in any modern browser (Chrome, Edge, Safari, Firefox).
3. An internet connection is needed only to load the Three.js library for the 3D background.

No installation is required.

---

## Limitations

- The data is synthetic, so results do not reflect real-world lending.
- Gender and marital status are used as inputs. Real lenders generally avoid sensitive attributes, and the fairness check is included to show how much the model depends on them.
- EMI is calculated by simple division and does not include interest.
- The model is small and meant for learning. It is not a production credit scoring system.

---

## Author

Made by **Aryan Vimal**.
