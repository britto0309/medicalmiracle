# 🤖 AI Medical Prediction System

## Overview

An advanced machine learning system that predicts diseases, recommends medications, and suggests dosages based on patient symptoms and vital signs.

**⚠️ DISCLAIMER**: This system is for **educational and decision-support purposes only**. It is **NOT a substitute** for professional medical diagnosis or treatment. Always consult qualified healthcare professionals.

---

## 📊 Model Performance

Our trained models achieve the following accuracies:

| Model | Accuracy | Algorithm | Classes |
|-------|----------|-----------|---------|
| **Disease Prediction** | 87.6% | XGBoost | 8 diseases |
| **Medicine Prediction** | 33.9% | XGBoost | 16 medicines |
| **Dosage Prediction** | 88.2% | XGBoost | 6 dosage schedules |

### Supported Diseases

1. **Common Cold** - Viral infection, mild symptoms
2. **Influenza (Flu)** - Severe viral infection
3. **Bacterial Infection** - Requires antibiotics
4. **Dengue Fever** - Mosquito-borne disease
5. **Malaria** - Parasitic infection
6. **Typhoid** - Bacterial infection from contaminated food/water
7. **UTI (Urinary Tract Infection)** - Bacterial infection
8. **COVID-19** - Coronavirus infection

---

## 🚀 Quick Start

### 1. Train the Models

First, train the ML models using the provided dataset:

```bash
cd backend
python train_medical_model.py
```

This will:
- Generate a synthetic medical dataset (5000 samples)
- Preprocess data (scaling, encoding, handling missing values)
- Train 3 separate models (disease, medicine, dosage)
- Evaluate performance with cross-validation
- Save models to `backend/models/` directory

**Training time**: ~30-60 seconds

**Output files**:
```
backend/models/
├── disease_model.pkl        # Disease classification model
├── medicine_model.pkl       # Medicine recommendation model
├── dosage_model.pkl         # Dosage prediction model
├── disease_encoder.pkl      # Label encoder for diseases
├── medicine_encoder.pkl     # Label encoder for medicines
├── dosage_encoder.pkl       # Label encoder for dosages
└── feature_scaler.pkl       # Feature scaler for normalization
```

### 2. Start the Backend

The models will automatically load when you start the backend:

```bash
cd backend
python main.py
```

### 3. Access the API

**API Endpoints**:
- `POST /api/v1/medical-prediction/predict` - Get disease/medicine prediction
- `GET /api/v1/medical-prediction/model-status` - Check if models are loaded
- `GET /api/v1/medical-prediction/supported-diseases` - List all diseases
- `GET /api/v1/medical-prediction/supported-medicines` - List all medicines

**API Documentation**: http://localhost:8000/docs

### 4. Use the Frontend

1. Start frontend: `cd frontend && npm run dev`
2. Open: http://localhost:5173
3. Click **🤖 AI Predict** tab
4. Enter patient information
5. Get instant predictions

---

## 📋 Input Features

The model requires the following patient data:

### Vital Signs (Numerical)
- **Temperature** (35-42°C) - Body temperature
- **BP Systolic** (70-200 mmHg) - Upper blood pressure
- **BP Diastolic** (40-130 mmHg) - Lower blood pressure
- **Heart Rate** (40-180 BPM) - Beats per minute

### Symptoms (Binary: 0 or 1)
- **has_fever** - Elevated body temperature (≥37.5°C)
- **has_cough** - Persistent coughing
- **has_body_ache** - Muscle/joint pain
- **has_headache** - Head pain
- **has_fatigue** - Tiredness/weakness
- **has_rash** - Skin rash
- **has_pain** - Localized pain
- **symptom_count** - Total number of symptoms

---

## 💻 API Usage Examples

### cURL Example

```bash
curl -X POST "http://localhost:8000/api/v1/medical-prediction/predict" \
  -H "Content-Type: application/json" \
  -d '{
    "temperature": 39.5,
    "bp_systolic": 120,
    "bp_diastolic": 75,
    "heart_rate": 88,
    "has_fever": 1,
    "has_cough": 1,
    "has_body_ache": 1,
    "has_headache": 1,
    "has_fatigue": 1,
    "has_rash": 0,
    "has_pain": 0,
    "symptom_count": 5
  }'
```

### Python Example

```python
import requests

patient_data = {
    "temperature": 39.5,
    "bp_systolic": 120,
    "bp_diastolic": 75,
    "heart_rate": 88,
    "has_fever": 1,
    "has_cough": 1,
    "has_body_ache": 1,
    "has_headache": 1,
    "has_fatigue": 1,
    "has_rash": 0,
    "has_pain": 0,
    "symptom_count": 5
}

response = requests.post(
    "http://localhost:8000/api/v1/medical-prediction/predict",
    json=patient_data
)

result = response.json()
print(f"Disease: {result['primary_prediction']['disease']}")
print(f"Medicine: {result['primary_prediction']['medicine']}")
print(f"Confidence: {result['primary_prediction']['disease_confidence']:.2%}")
```

### JavaScript Example

```javascript
const patient = {
  temperature: 39.5,
  bp_systolic: 120,
  bp_diastolic: 75,
  heart_rate: 88,
  has_fever: 1,
  has_cough: 1,
  has_body_ache: 1,
  has_headache: 1,
  has_fatigue: 1,
  has_rash: 0,
  has_pain: 0,
  symptom_count: 5
};

const response = await fetch('http://localhost:8000/api/v1/medical-prediction/predict', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify(patient)
});

const result = await response.json();
console.log('Disease:', result.primary_prediction.disease);
console.log('Medicine:', result.primary_prediction.medicine);
```

---

## 📈 Model Training Pipeline

### Architecture

```
┌─────────────────────────────────────────────────────┐
│  STEP 1: Data Generation/Loading                   │
│  - Create synthetic dataset OR load from CSV       │
│  - Disease-symptom-medicine mappings                │
│  - 5000 patient records                             │
└─────────────────────────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────┐
│  STEP 2: Data Preprocessing                        │
│  - Handle missing values (median/mode imputation)   │
│  - Remove duplicates                                │
│  - Encode categorical variables (LabelEncoder)      │
│  - Scale numerical features (StandardScaler)        │
│  - Feature engineering                              │
└─────────────────────────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────┐
│  STEP 3: Train-Validation-Test Split               │
│  - Training: 60% (3000 samples)                     │
│  - Validation: 20% (1000 samples)                   │
│  - Testing: 20% (1000 samples)                      │
│  - Stratified sampling for balanced classes         │
└─────────────────────────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────┐
│  STEP 4: Model Training (3 separate models)        │
│  - XGBoost Classifier (200 trees, depth 8)         │
│  - Class balancing for imbalanced data             │
│  - 5-fold cross-validation                          │
│  - Hyperparameter optimization                      │
└─────────────────────────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────┐
│  STEP 5: Model Evaluation                          │
│  - Accuracy, Precision, Recall, F1-Score           │
│  - Classification reports                           │
│  - Confusion matrices                               │
└─────────────────────────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────┐
│  STEP 6: Model Persistence                         │
│  - Save models as pickle files                      │
│  - Save encoders and scalers                        │
│  - Version control                                  │
└─────────────────────────────────────────────────────┘
```

### Algorithms Used

1. **XGBoost (eXtreme Gradient Boosting)**
   - Ensemble learning method
   - Handles class imbalance
   - Feature importance analysis
   - Regularization to prevent overfitting

2. **Fallback: Random Forest**
   - Used if XGBoost not available
   - 200 decision trees
   - Class weighting for balance

---

## 🔧 Customization

### Using Your Own Dataset

Replace the `create_medical_dataset()` function in `train_medical_model.py`:

```python
def load_custom_dataset():
    """Load your own medical dataset"""
    # Load from CSV
    df = pd.read_csv('your_dataset.csv')
    
    # Ensure columns match:
    # - temperature, bp_systolic, bp_diastolic, heart_rate
    # - has_fever, has_cough, has_body_ache, etc.
    # - disease, medicine, dosage_schedule
    
    return df
```

### Supported Dataset Sources

1. **Kaggle Medical Datasets**
   - Disease Symptom Prediction
   - Medicine Recommendation
   - Clinical Decision Making

2. **UCI Machine Learning Repository**
   - Medical diagnostic datasets
   - Patient health records

3. **Public Health Databases**
   - CDC data
   - WHO health statistics

### Hyperparameter Tuning

Improve accuracy by tuning model parameters:

```python
from sklearn.model_selection import GridSearchCV

param_grid = {
    'n_estimators': [100, 200, 300],
    'max_depth': [5, 8, 10, 15],
    'learning_rate': [0.01, 0.1, 0.2]
}

grid_search = GridSearchCV(
    XGBClassifier(),
    param_grid,
    cv=5,
    scoring='accuracy'
)

grid_search.fit(X_train, y_train)
best_model = grid_search.best_estimator_
```

---

## 🎯 Improving Model Accuracy

### Current Limitations

- **Medicine Prediction: 33.9%** - Lower accuracy due to:
  - Complex medication interactions
  - Patient-specific factors (allergies, history)
  - Multiple valid treatment options
  - Limited training data

### Improvement Strategies

1. **Increase Training Data**
   ```python
   # Increase from 5000 to 50,000+ samples
   n_samples = 50000
   df = create_medical_dataset()
   ```

2. **Feature Engineering**
   - Add patient age, sex, medical history
   - Include lab test results
   - Add symptom severity scores

3. **Ensemble Methods**
   - Combine multiple models (voting/stacking)
   - Use different algorithms together

4. **Class Balancing**
   - SMOTE for minority classes
   - Weighted loss functions

5. **Real Medical Data**
   - Use actual electronic health records (EHRs)
   - Clinical trials data
   - Hospital databases (with permissions)

---

## 📊 Model Evaluation Metrics

### Classification Report Example

```
Disease Prediction:
                     precision    recall  f1-score   support
Bacterial Infection       0.76      0.81      0.78       124
           COVID-19       0.86      0.89      0.88       128
        Common Cold       0.90      0.91      0.91       122
       Dengue Fever       0.98      0.93      0.96       123
          Influenza       0.90      0.83      0.86       126
            Malaria       0.85      0.88      0.87       128
            Typhoid       0.95      0.85      0.90       124
                UTI       0.83      0.90      0.87       125

           accuracy                           0.88      1000
```

### What These Metrics Mean

- **Precision**: % of positive predictions that are correct
- **Recall**: % of actual positives identified
- **F1-Score**: Harmonic mean of precision and recall
- **Support**: Number of samples in each class

---

## 🔒 Safety & Ethics

### Important Considerations

1. **Not for Clinical Use**
   - This is a demonstration/learning tool
   - NOT approved for real medical decisions
   - NOT FDA-approved or clinically validated

2. **Data Privacy**
   - Do not use real patient data without consent
   - Comply with HIPAA, GDPR regulations
   - Anonymize all personal information

3. **Bias & Fairness**
   - Model trained on synthetic data
   - May not represent all demographics
   - Test across different populations

4. **Transparency**
   - Always show model confidence scores
   - Provide alternative diagnoses
   - Clear disclaimers to users

---

## 🐛 Troubleshooting

### Models Not Loading

**Error**: `ML models not loaded`

**Solution**:
```bash
cd backend
python train_medical_model.py
```

### Low Accuracy

**Causes**:
- Small training dataset
- Poor feature engineering
- Imbalanced classes

**Solutions**:
- Increase dataset size
- Add more relevant features
- Use SMOTE for class balancing

### Prediction Errors

**Error**: `Prediction failed`

**Checks**:
- Ensure all 12 features are provided
- Values within valid ranges
- Models trained and saved correctly

---

## 📚 Dependencies

```
pandas>=2.0.0
numpy>=1.24.0
scikit-learn>=1.3.0
xgboost>=2.0.0
fastapi>=0.104.0
pydantic>=2.0.0
```

Install all:
```bash
pip install pandas numpy scikit-learn xgboost fastapi pydantic
```

---

## 🎓 Educational Resources

- **Machine Learning**: [scikit-learn documentation](https://scikit-learn.org/)
- **XGBoost**: [XGBoost tutorials](https://xgboost.readthedocs.io/)
- **Medical AI**: [Healthcare ML Guide](https://github.com/topics/medical-ai)
- **Clinical Decision Support**: [CDSS Research](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6372467/)

---

## 📝 License & Disclaimer

### MIT License

Copyright (c) 2026 MedIntel

Permission is granted for educational and research purposes.

### Medical Disclaimer

This software is provided "AS IS" without warranty. The developers are NOT responsible for any medical decisions made using this tool. Always consult qualified healthcare professionals for medical advice, diagnosis, and treatment.

---

## 💡 Future Enhancements

- [ ] Deep learning models (CNN, RNN)
- [ ] Natural language processing for symptoms
- [ ] Multi-language support
- [ ] Integration with real EHR systems
- [ ] Explainable AI (SHAP/LIME)
- [ ] Mobile app deployment
- [ ] Real-time learning from feedback

---

## 🤝 Contributing

Want to improve the model? Contributions welcome!

1. Use real medical datasets
2. Implement advanced algorithms
3. Improve feature engineering
4. Add more diseases/medicines
5. Clinical validation studies

---

**Built with ❤️ for medical AI education**
