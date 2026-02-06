# 🌡️ Fever Diagnosis Feature

## Overview

An interactive fever diagnosis system that acts like a real doctor, asking questions step-by-step to understand your symptoms and provide a preliminary diagnosis with treatment recommendations.

## Features

### ✅ What It Does

1. **Interactive Consultation** - Asks questions like a real doctor
2. **9 Common Fever Conditions** - Covers major fever-related illnesses
3. **Personalized Diagnosis** - Based on your specific symptoms
4. **Medicine Recommendations** - Suggests appropriate medications
5. **Home Remedies** - Provides natural care tips
6. **Doctor Consultation Advice** - Always recommends seeing a doctor
7. **Severity Assessment** - Indicates urgency level

### 📊 Covered Conditions

1. **Common Cold** (Mild)
2. **Influenza/Flu** (Moderate)
3. **Viral Infection** (Mild)
4. **Bacterial Infection** (Moderate-High)
5. **Dengue Fever** (High) ⚠️
6. **Malaria** (High) ⚠️
7. **Typhoid Fever** (High) ⚠️
8. **Urinary Tract Infection** (Moderate)
9. **COVID-19** (Mild-Severe) ⚠️

## How It Works

### Step-by-Step Flow

```
1. User clicks "Start Diagnosis"
   ↓
2. System asks 7 doctor-like questions:
   - Body temperature
   - Duration of fever
   - Fever pattern (continuous/cyclic)
   - Symptoms (multiple choice)
   - Severity level
   - Exposure history
   - Pre-existing conditions
   ↓
3. AI analyzes answers against dataset
   ↓
4. Provides diagnosis with:
   - Condition name & severity
   - Expected duration
   - Recommended medicines
   - Home remedies
   - Doctor consultation advice
   ↓
5. Always recommends consulting a doctor
```

## Usage

### Frontend (Web Interface)

1. **Start the application**
   ```bash
   # Backend
   cd backend
   python main.py

   # Frontend
   cd frontend
   npm run dev
   ```

2. **Navigate to Fever tab**
   - Open http://localhost:5173
   - Click the "🌡️ Fever" tab in navigation

3. **Follow the consultation**
   - Click "Start Diagnosis"
   - Answer each question honestly
   - View your diagnosis and recommendations

### API Endpoints

#### 1. Start/Continue Diagnosis
```http
POST /api/v1/fever/diagnose
Content-Type: application/json

{
  "session_id": "unique_session_id",
  "user_response": "39.5",
  "start_new": false
}
```

**Response:**
```json
{
  "session_id": "unique_session_id",
  "question": "How many days have you had the fever?",
  "options": ["Less than 2 days", "2-4 days", "5-7 days", "More than 7 days"],
  "question_type": "choice",
  "completed": false,
  "current_step": 2,
  "total_steps": 7
}
```

#### 2. Get All Conditions
```http
GET /api/v1/fever/conditions
```

**Response:**
```json
{
  "conditions": [
    {
      "id": "common_cold",
      "name": "Common Cold",
      "severity": "mild",
      "common_symptoms": ["mild fever", "runny nose", "sneezing"]
    }
  ]
}
```

#### 3. Get Condition Details
```http
GET /api/v1/fever/condition/{condition_id}
```

#### 4. Clear Session
```http
DELETE /api/v1/fever/session/{session_id}
```

## Testing

### Run Test Script

```bash
cd backend
python test_fever_diagnosis.py
```

This will:
- Show all available conditions
- Run a simulated diagnosis (COVID-19 case)
- Display the complete diagnosis output

### Manual Testing with curl

```bash
# Start diagnosis
curl -X POST http://localhost:8000/api/v1/fever/diagnose \
  -H "Content-Type: application/json" \
  -d '{"session_id": "test123", "start_new": true}'

# Submit answer
curl -X POST http://localhost:8000/api/v1/fever/diagnose \
  -H "Content-Type: application/json" \
  -d '{"session_id": "test123", "user_response": "39.5"}'

# Get all conditions
curl http://localhost:8000/api/v1/fever/conditions
```

## Sample Diagnosis Output

```
📋 DIAGNOSIS RESULTS
============================================================

Condition: COVID-19
Severity: mild to severe
Duration: 7-14 days for mild cases
Temperature: 39.5°C

💊 Recommended Medicines:
  • Paracetamol - 500mg every 6 hours
  • Vitamin D - 60,000 IU weekly
  • Vitamin C - 500mg twice daily
  • Zinc - 50mg once daily

🏠 Home Remedies:
  ✓ Complete isolation at home
  ✓ Monitor oxygen saturation (SpO2)
  ✓ Steam inhalation
  ✓ Drink warm water regularly
  ✓ Prone positioning if breathing difficulty

👨‍⚕️ Doctor Consultation:
🏥 URGENT CONSULTATION REQUIRED

You should consult a doctor IMMEDIATELY. This condition requires:
- Professional medical diagnosis
- Prescription medications
- Regular monitoring
- Possible lab tests

Do not delay medical attention.

⚠️ This is an AI-based preliminary assessment. Please consult 
a qualified doctor for proper diagnosis and treatment.
```

## Dataset Structure

### Each Condition Contains:

```python
{
  "name": "Condition Name",
  "symptoms": ["symptom1", "symptom2", ...],
  "temperature_range": {"min": 37.5, "max": 39.0, "unit": "°C"},
  "medicines": [
    {
      "name": "Medicine Name",
      "dosage": "500mg twice daily",
      "type": "fever reducer",
      "prescription": False  # Optional
    }
  ],
  "home_remedies": ["remedy1", "remedy2", ...],
  "severity": "mild/moderate/high",
  "duration": "3-7 days",
  "warning": "Optional urgent warning"  # For serious conditions
}
```

## Important Notes

### ⚠️ Medical Disclaimer

- This is an **AI-based preliminary assessment** only
- **NOT a replacement** for professional medical diagnosis
- **Always consult a qualified doctor** for proper treatment
- For emergencies, call emergency services immediately

### 🚨 Red Flags (Seek Immediate Care)

The system identifies high-severity conditions and urges immediate medical attention for:
- High fever with severe symptoms
- Difficulty breathing
- Suspected dengue, malaria, or typhoid
- COVID-19 symptoms with low oxygen levels

### 🔒 Privacy

- Uses session-based storage only
- No personal data stored permanently
- Sessions cleared after diagnosis
- HIPAA compliance considerations for production use

## Customization

### Adding New Conditions

Edit `backend/knowledge_base/fever_diagnosis_dataset.py`:

```python
FEVER_DIAGNOSIS_DATASET["new_condition"] = {
    "name": "New Condition Name",
    "symptoms": ["symptom1", "symptom2"],
    "temperature_range": {"min": 37.0, "max": 38.0, "unit": "°C"},
    "medicines": [...],
    "home_remedies": [...],
    "severity": "mild",
    "duration": "3-5 days"
}
```

### Modifying Questions

Edit `DIAGNOSTIC_QUESTIONS` in the same file to change or add questions.

## Architecture

```
Frontend (React)
    ↓
API Gateway (FastAPI)
    ↓
Fever Diagnosis Router
    ↓
Diagnosis Engine
    ↓
Dataset Matching
    ↓
Response Generation
```

## Future Enhancements

- [ ] Integration with Electronic Health Records (EHR)
- [ ] Multi-language support
- [ ] Voice-based consultation
- [ ] Image upload for rash/symptoms
- [ ] Doctor appointment booking
- [ ] Prescription delivery integration
- [ ] Follow-up reminders
- [ ] Symptom tracking over time

## Support

For issues or questions:
1. Check API documentation at http://localhost:8000/docs
2. Run test script for debugging
3. Review logs in backend console

---

**Built with ❤️ for better healthcare access**
