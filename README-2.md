# Explainable and Privacy-Aware AI Threat Alert

**ENSP355 – AI in Cyber Security | Lab 4** · K.R. Mangalam University, School of Engineering & Technology

A small Decision Tree that flags suspicious login activity, then makes the result **explainable** (confidence, feature importance, sensitivity test) and **privacy-aware** (masked IPs and account IDs) before producing a **responsible alert**: severity, reason, recommended action and human-review status.

## What each task does

| Task | What it shows |
|---|---|
| Q1 | Predictions for 5 sample logins with **confidence** (max class probability) |
| Q2 | **Feature importance** table + chart and plain-language explanations |
| Q3 | **Sensitivity test**: change only `failed_logins` 2 → 8 and compare output |
| Q4 | **Masking**: `192.168.10.24 → 192.168.xxx.xxx`, `ACC-1002 → ACC-***02` + 4 privacy rules |
| Q5 | **Responsible alert** with severity, reason, action and human-review flag |

## Run it

**Google Colab:** upload the notebook and `data/login_activity.csv`, then *Runtime → Run all*. After pushing to GitHub you can open it with
`https://colab.research.google.com/github/<your-username>/<your-repo>/blob/main/notebooks/Lab4_Explainable_Privacy_Aware_Alert.ipynb`

**Locally:**
```bash
pip install -r requirements.txt
python src/lab4_alert.py          # run from the repo root
jupyter notebook notebooks/Lab4_Explainable_Privacy_Aware_Alert.ipynb
```

## Key results

- **Lowest-confidence record:** ACC-1003 → Suspicious, confidence 0.667
- **Top feature:** `failed_logins` (≈ 0.86), then `download_mb` (≈ 0.14); `login_hour` and `new_device` are unused by this shallow tree
- **Sensitivity:** ACC-1005 changes from **Normal → Suspicious** when `failed_logins` goes 2 → 8 (the flip happens between 3 and 4)

| Account | Prediction | Confidence | Severity | Reason | Human review |
|---|---|---|---|---|---|
| ACC-***01 | Normal | 1.000 | Low | no strong warning | No |
| ACC-***02 | Suspicious | 1.000 | High | many failed logins, new device, unusual login time | Yes |
| ACC-***03 | Suspicious | 0.667 | High | large data download | Yes |
| ACC-***04 | Suspicious | 1.000 | High | new device, unusual login time | Yes |
| ACC-***05 | Normal | 1.000 | Low | no strong warning | No |

![Feature importance](results/feature_importance.png)

## Limitations & responsible use

- Feature importance explains the **model**, it does not prove **cause**; an importance of 0 does not mean a feature is harmless.
- The alert *reasons* use fixed thresholds that are separate from the model's own splits, so they can disagree.
- Confidence is not certainty; it only reflects the training data in a tree leaf.
- Tiny training data (38 records): not suitable for real security decisions.
- Rules: mask identifiers before display/export · use sanitized data · restrict access to unmasked records · require human review for high-risk action.

## Author

_Your Name_ · _Roll No._ · _Section_
