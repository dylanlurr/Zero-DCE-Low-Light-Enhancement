# ⚡ QUICK REFERENCE CARD

## 🚀 Start Here (3 Steps)

### Step 1: Backend
```bash
cd backend
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
python main.py
```
✅ Running on `http://localhost:8000`

### Step 2: Frontend  
```bash
cd frontend
npm install
npm run dev
```
✅ Running on `http://localhost:3000`

### Step 3: Use It
1. Open `http://localhost:3000`
2. Wait for green "CONNECTED" indicator
3. Click `START STREAM`
4. Watch real-time enhancement!

---

## 📋 File Locations

| What | Where |
|------|-------|
| Backend Server | `backend/main.py` |
| Model Architecture | `backend/model.py` |
| Backend Dependencies | `backend/requirements.txt` |
| Frontend UI | `frontend/app/page.tsx` |
| Frontend Styles | `frontend/app/globals.css` |
| Tailwind Config | `frontend/tailwind.config.ts` |
| Setup Guide | `SETUP.md` |
| Full Docs | `README_FULLSTACK.md` |
| Pre-trained Model | `best_model.pth` |

---

## 🔗 API Reference

### REST Endpoints
```bash
# Health check
curl http://localhost:8000/health

# Model info
curl http://localhost:8000/info
```

### WebSocket
```
ws://localhost:8000/ws/enhance

Send: {"type": "frame", "image": "data:image/jpeg;base64,..."}
Receive: {"type": "enhanced", "image": "...", "processing_time_ms": 14.5}
```

---

## ⚙️ Key Settings

### Backend (`backend/main.py`)
```python
DEVICE = 'cuda'  # or 'cpu'
PROCESS_SIZE = 512
MODEL_PATH = '../best_model.pth'
```

### Model (`backend/model.py`)
```python
DCENet(n_filters=32, n_iterations=8)
```

### Frontend (`frontend/app/page.tsx`)
```typescript
video: { width: 1280, height: 720 }  // Camera resolution
toDataURL('image/jpeg', 0.9)  // JPEG quality
```

---

## 📊 Performance Targets

| Metric | Target |
|--------|--------|
| **Latency (GPU)** | 12-15ms |
| **FPS** | 40-50 |
| **Throughput** | 4200 img/s |
| **VRAM** | ~1.2 GB |

---

## 🐛 Common Issues

### Backend Won't Start
```
Error: Model file not found
Fix: Verify best_model.pth in root directory
```

### "CONNECTING..." Forever
```
Error: Cannot reach backend
Fix: Ensure backend is running on port 8000
```

### Low FPS
```
Error: Slow inference
Fix: Use GPU, reduce video resolution, lower JPEG quality
```

### Webcam Permission Denied
```
Error: Camera access blocked
Fix: Check browser/OS camera permissions
```

---

## 📦 Dependencies

### Backend
- torch, fastapi, uvicorn, opencv-python, numpy

### Frontend
- react, next.js, tailwindcss

### Install All
```bash
# Backend
pip install -r backend/requirements.txt

# Frontend
npm install
```

---

## 🎨 Color Palette

```css
Black:    #000000
Cyan:     #00D9FF  ← Primary accent
Blue:     #0080FF
Purple:   #7000FF
Pink:     #FF0080
```

---

## 🚀 Deploy

### Docker
```bash
docker build -t zero-dce-backend backend/
docker run -p 8000:8000 --gpus all zero-dce-backend
```

### Cloud
- **Frontend:** Vercel `vercel deploy`
- **Backend:** Railway, Render, AWS

---

## 📚 Documentation

| Doc | Purpose |
|-----|---------|
| `START_HERE.md` | Quick start |
| `SETUP.md` | Detailed setup |
| `README_FULLSTACK.md` | Full docs |
| `DELIVERABLES.md` | Checklist |

---

## 🎯 Architecture

```
Frontend (Next.js)
    ↓ (WebSocket)
Backend (FastAPI)
    ↓ (PyTorch)
DCENet Model
    ↓ (GPU)
Enhanced Image
    ↓ (WebSocket)
Display Canvas
```

---

## 📞 Support

- Check `SETUP.md` troubleshooting section
- Review code comments for details
- Check browser console for errors
- Verify backend: `curl http://localhost:8000/health`

---

## ✅ Quality Checklist

- [x] Model extracted from notebook
- [x] FastAPI server running
- [x] WebSocket streaming working
- [x] Frontend UI responsive
- [x] Real-time enhancement <50ms
- [x] Error handling complete
- [x] Documentation comprehensive
- [x] Production-ready code

---

**Ready to go! 🚀**
