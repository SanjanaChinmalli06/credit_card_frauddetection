# Quick Reference Guide: Fraud Detection Model

## 📊 Quick Stats

- **Dataset Size**: 284,807 transactions
- **Fraud Rate**: 0.17% (492 frauds)
- **Best Model**: XGBoost + SMOTE
- **Optimal Threshold**: 0.9
- **Best ROC-AUC**: 0.9840 (RF + Undersampling)
- **Recommended ROC-AUC**: 0.9343 (XGB + SMOTE)

---

## 🎯 Model Selection Criteria

| Aspect | XGB + SMOTE | RF + Undersampling |
|--------|-------------|-------------------|
| **ROC-AUC** | 0.9343 | 0.9840 (best) |
| **Training Data** | 341K | 1.2K (loss) |
| **Recall** | 77.6% | 98.9% |
| **False Positives** | 5,689 | 11,664 |
| **Recommended** | ✅ **YES** | ⚠️ Too aggressive |

**Why XGB + SMOTE?** Better balance between fraud detection and false positive rate. Doesn't lose data.

---

## 🔧 Threshold Selection Guide

```
├─ Aggressive (0.1-0.2)
│  └─ ~98% fraud caught, ~30% false positives
│  └─ Cost: $127K (mostly FP costs)
│  └─ Use: High-risk scenarios
│
├─ Balanced (0.5)
│  └─ ~78% fraud caught, ~10% false positives  
│  └─ Cost: $30K
│  └─ Use: Mid-tier merchants
│
└─ Conservative (0.7-0.9)  ⭐ RECOMMENDED
   └─ ~38% fraud caught, ~1% false positives
   └─ Cost: $8,545 (OPTIMAL)
   └─ Use: Production deployment
```

**Decision Rule**: Start at 0.9, monitor fraud leakage, adjust based on incident data.

---

## 📈 Sampling Techniques Comparison

| Technique | Size | Fraud% | Best For |
|-----------|------|--------|----------|
| **Original** | 228K | 0.17% | ROC-AUC baseline |
| **Undersampling** | 1.2K | 33% | Maximum recall (loses data) |
| **Oversampling** | 341K | 33% | Good balance (overfitting risk) |
| **SMOTE** | 341K | 33% | ✅ **Recommended** (synthetic) |

---

## ⚖️ Precision vs Recall

```
              Precision  Recall  Best For
Low Threshold   1%       98%     Catch ALL fraud
Mid Threshold  10%       78%     Balanced (RECOMMENDED)
High Threshold 70%       20%     Minimize false alarms
```

**Business Context**: When fraud loss ($100) >> FP cost ($5), prioritize **Recall**.

---

## 🛠️ Implementation Checklist

- [ ] Use XGBoost + SMOTE model
- [ ] Set classification threshold to 0.9
- [ ] Flag transactions with fraud_probability ≥ 0.9
- [ ] Route flagged transactions to review team
- [ ] Monitor false positive rate weekly
- [ ] Retrain model monthly with new data
- [ ] Optimize threshold quarterly based on incident data

---

## 📊 Metrics to Monitor (Weekly Reports)

1. **Detection Metrics**
   - True Positive Rate (Recall): Target ~38%+
   - False Positive Rate: Target <2%
   
2. **Business Metrics**
   - Fraud caught ($): Actual savings
   - False positive complaints: Customer impact
   - Time-to-resolution: Process efficiency
   
3. **Model Health**
   - ROC-AUC: Should stay >0.93
   - Prediction distribution: Look for shifts
   - Class balance in recent data: Monitor fraud trend

---

## 🚨 Alert Thresholds

| Metric | Alert Level | Action |
|--------|------------|--------|
| ROC-AUC drops < 0.90 | 🔴 Critical | Retrain immediately |
| False positive rate > 5% | 🟡 Warning | Review threshold |
| Fraud leakage > 70% | 🔴 Critical | Lower threshold |
| No retraining > 4 weeks | 🟡 Warning | Schedule retraining |

---

## 💡 Key Learnings

### ✅ What Worked
1. **SMOTE** beats simple over/undersampling
2. **Ensemble models** (XGB, RF) outperform linear baselines
3. **Threshold optimization** based on business costs, not just accuracy
4. **ROC-AUC** is the right metric for imbalanced data

### ❌ What Didn't
1. Using raw accuracy (99.83% is misleading!)
2. Keeping default 0.5 threshold
3. Undersampling (loses too much data)
4. Treating precision and recall equally

### 📌 Critical Insight
**The precision-recall tradeoff is a business decision, not just a technical one.**

---

## 📞 Support & Questions

**Notebook Location**: `Fraud_Detection_Analysis.ipynb`

**Key Sections**:
1. EDA: Class imbalance visualization
2. Sampling Techniques: Compare all three approaches
3. Model Training: RF and XGBoost
4. Evaluation: Metrics and confusion matrices
5. ROC/PR Curves: Visual tradeoff analysis
6. Threshold Analysis: Cost-based optimization

**To adjust threshold**: Go to Section 10, modify threshold parameter, re-run.

---

**Last Updated**: April 21, 2026
**Status**: ✅ Production Ready
