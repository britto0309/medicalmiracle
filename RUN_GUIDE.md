# 🚀 QUICK RUN GUIDE - MedIntel

## ✅ CURRENT STATUS (2025-02-07)

**ALL SYSTEMS RUNNING - READY TO USE!**

- ✅ Backend: http://localhost:8000 (PID 19052)
- ✅ Frontend: http://localhost:5173 (PID 28388)
- ✅ ML Models: LOADED (7 models)
- ✅ API: ALL ENDPOINTS WORKING

---

## 🎯 ACCESS APPLICATION NOW

### Option 1: Direct Browser Access
1. Open your browser
2. Go to: **http://localhost:5173**
3. Click "🤖 AI Predict" tab
4. Start making predictions!

### Option 2: VS Code Simple Browser
- Already opened at http://localhost:5173

---

## 🧪 TEST THE AI PREDICT FEATURE

### Step-by-Step Test
1. Click "**🤖 AI Predict**" in the navigation
2. Enter patient data:
   - **Age:** 35
   - **Sex:** Male (Select from dropdown)
   - **Temperature:** 38.5°C (use slider)
   - **Blood Pressure:** 120/80
   - **Heart Rate:** 85 BPM
3. Toggle symptoms:
   - ✅ Fever
   - ✅ Cough
   - ✅ Body Ache
   - ✅ Fatigue
4. Click "**Get Prediction**" button
5. View results!

### Expected Results
- **Disease:** Influenza (99.9% confidence)
- **Medicine:** Paracetamol
- **Dosage:** 650mg every 6 hours
- **Risk Level:** MEDIUM - Monitor symptoms

---

## 🔧 IF SERVERS ARE NOT RUNNING

### Start Backend
```bash
cd backend
python main.py
```
Server will start on http://localhost:8000

### Start Frontend
```bash
cd frontend
npm run dev
```
Server will start on http://localhost:5173

---

## ⚠️ TROUBLESHOOTING

### Gender Dropdown Not Showing Options?
**Solution:** Clear browser cache
- Press `Ctrl + Shift + R` (Hard Refresh)
- OR Press `Ctrl + F5` (Force Reload)

The dropdown should show:
- Select
- Male
- Female
- Others

### "Models Not Found" Error?
**Check:** Models should be in `backend/models/`
```bash
dir backend\models\*.pkl
```
Should show 7 .pkl files

### Port Already in Use?
**Backend (8000):**
```powershell
netstat -ano | findstr ":8000"
Stop-Process -Id <PID>
```

**Frontend (5173):**
```powershell
netstat -ano | findstr ":5173"
Stop-Process -Id <PID>
```

---

## 📊 VERIFY EVERYTHING IS WORKING

### Quick Health Check
```bash
# Backend health
curl http://localhost:8000/health

# Model status
curl http://localhost:8000/api/v1/medical-prediction/model-status

# Frontend
curl http://localhost:5173
```

All should return 200 OK

---

## 🎉 FEATURES TO TRY

### 1. AI Medical Prediction (NEW!)
- Interactive vitals sliders
- Symptom toggles
- Real-time ML predictions
- Confidence scores
- Alternative diagnoses
- Risk assessment
- Personalized recommendations

### 2. Chat
- Medical chatbot
- Symptom discussion
- Health questions

### 3. Fever Tracker
- Temperature monitoring
- Trend analysis

### 4. Nearby
- Find nearby medical facilities
- Location-based services

---

## 📈 MODEL INFORMATION

### Trained Models
- **Disease Model:** 87.6% accuracy (8 diseases)
- **Medicine Model:** 33.9% accuracy (16 medicines)
- **Dosage Model:** 88.2% accuracy (6 dosage schedules)

### Supported Diseases
1. Influenza
2. COVID-19
3. Common Cold
4. Malaria
5. Dengue
6. Typhoid
7. Pneumonia
8. Gastroenteritis

### Dataset
- **Type:** Synthetic (computer-generated)
- **Samples:** 5,000 patient records
- **Safety:** HIPAA-compliant (no real patient data)

---

## ⚡ QUICK COMMANDS

### Check Running Processes
```powershell
# Backend
netstat -ano | findstr ":8000"

# Frontend
netstat -ano | findstr ":5173"
```

### Restart Servers
```powershell
# Kill backend
Stop-Process -Id 19052

# Kill frontend
Stop-Process -Id 28388

# Start again (see "IF SERVERS ARE NOT RUNNING" above)
```

### View Logs
```bash
# Backend logs
tail -f backend/backend_log.txt

# Frontend logs (in terminal where npm run dev is running)
```

---

## 🛡️ IMPORTANT DISCLAIMERS

⚠️ **FOR DECISION SUPPORT ONLY**
- This is NOT a medical diagnostic device
- Always consult qualified healthcare professionals
- AI predictions are probabilistic, not definitive
- Use as supplementary information only

---

## 📞 NEED HELP?

1. **Check:** [DEBUG_REPORT.md](DEBUG_REPORT.md) for comprehensive details
2. **Read:** [ML_MODEL_README.md](ML_MODEL_README.md) for model documentation
3. **Review:** Console logs (F12 in browser)
4. **Verify:** Both servers are running (see Quick Commands)

---

## ✅ CURRENT STATUS: **READY TO USE!**

**Everything is working. Start testing the AI Predict feature!**

**URL:** http://localhost:5173 → Click "🤖 AI Predict"

---

*Last Updated: 2025-02-07*  
*Status: ALL SYSTEMS OPERATIONAL*
