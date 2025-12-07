# Zero-DCE Real-Time Enhancement - Full Stack Setup

## 📋 Project Structure

```
Zero-DCE-Low-Light-Enhancement/
├── backend/
│   ├── model.py           # DCENet architecture + loading utilities
│   ├── main.py            # FastAPI WebSocket server
│   ├── requirements.txt    # Python dependencies
│   └── __init__.py
├── frontend/
│   ├── app/
│   │   ├── page.tsx       # Main cyberpunk UI component
│   │   ├── layout.tsx     # Next.js layout
│   │   └── globals.css    # Tailwind + custom styles
│   ├── components/
│   ├── package.json
│   ├── tsconfig.json
│   ├── tailwind.config.js
│   ├── next.config.js
│   └── postcss.config.js
├── best_model.pth         # Pre-trained Zero-DCE weights
├── lol_dataset_eval15/    # Test dataset
├── SETUP.md               # This file
└── README.md
```

## 🚀 Quick Start

### Phase 1: Backend Setup (FastAPI + PyTorch)

#### 1.1 Create Python Virtual Environment
```bash
# Navigate to backend directory
cd backend

# Create virtual environment (Python 3.10+)
python -m venv venv

# Activate virtual environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate
```

#### 1.2 Install Dependencies
```bash
pip install -r requirements.txt
```

**Note on GPU Support:**
- If you have NVIDIA GPU: `pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118`
- The server will automatically detect and use CUDA if available

#### 1.3 Start Backend Server
```bash
python main.py
```

Expected output:
```
Starting Zero-DCE Server | Device: cuda (or cpu)
✓ Model loaded successfully on cuda
Uvicorn running on http://0.0.0.0:8000
```

**Health Check:**
```bash
curl http://localhost:8000/health
```

---

### Phase 2: Frontend Setup (Next.js + Tailwind)

#### 2.1 Install Dependencies
```bash
cd frontend
npm install
```

#### 2.2 Development Server
```bash
npm run dev
```

The frontend will start at: **http://localhost:3000**

#### 2.3 Production Build
```bash
npm run build
npm start
```

---

## 🎮 Usage

1. **Open Frontend:** Navigate to `http://localhost:3000`
2. **Wait for Connection:** Green status indicator appears when connected to backend
3. **Start Camera:** Click `START STREAM` to begin real-time enhancement
4. **Monitor Stats:** Watch FPS, latency, and frame count
5. **Hold to Compare:** Press and hold `COMPARE` button to see original feed

---

## 🏗️ Architecture

### Backend Flow
```
Client (WebSocket) 
    ↓
FastAPI Server (main.py)
    ↓
base64_to_image() → Convert to Tensor
    ↓
DCENet Model (model.py)
    ↓
Inference with torch.no_grad()
    ↓
image_to_base64() → Send back
    ↓
Client Canvas Display
```

### Frontend Flow
```
Camera Stream (getUserMedia)
    ↓
Hidden Canvas (frame capture)
    ↓
Convert to base64
    ↓
WebSocket Send
    ↓
Receive Enhanced Image
    ↓
Display Canvas Render
    ↓
requestAnimationFrame loop
```

---

## 📊 API Endpoints

### REST Endpoints

#### GET `/health`
Check server status
```json
{
  "status": "healthy",
  "device": "cuda",
  "model_loaded": true,
  "torch_version": "2.0.0",
  "cuda_available": true
}
```

#### GET `/info`
Get model information
```json
{
  "model_name": "Zero-DCE",
  "description": "...",
  "input_size": [512, 512],
  "parameters": 1058976
}
```

### WebSocket Endpoint: `/ws/enhance`

**Client → Server (Frame):**
```json
{
  "type": "frame",
  "image": "data:image/jpeg;base64,/9j/4AAQSkZJR..."
}
```

**Server → Client (Enhanced):**
```json
{
  "type": "enhanced",
  "image": "data:image/jpeg;base64,/9j/4AAQSkZJR...",
  "processing_time_ms": 15.2,
  "frame_count": 1234
}
```

---

## 🎨 Customization

### Cyberpunk Styling
Edit `frontend/tailwind.config.js` to adjust colors:
```js
colors: {
  cyberpunk: {
    black: '#000000',    // Background
    cyan: '#00D9FF',     // Primary accent
    blue: '#0080FF',     // Secondary accent
    purple: '#7000FF',   // Tertiary
    pink: '#FF0080',     // Highlight
  }
}
```

### Model Parameters
In `backend/main.py`, change:
```python
PROCESS_SIZE = 512  # Change inference resolution
```

In `backend/model.py`:
```python
model = DCENet(n_filters=32, n_iterations=8)  # Adjust model capacity
```

---

## ⚙️ Performance Tuning

### For Better FPS (CPU/GPU):
1. **Reduce Frame Resolution:** Modify video constraints in `frontend/app/page.tsx`
   ```typescript
   width: { ideal: 640 },
   height: { ideal: 480 },
   ```

2. **Reduce JPEG Quality:** In `backend/main.py`
   ```python
   [cv2.IMWRITE_JPEG_QUALITY, 70]  # Lower from 90
   ```

3. **Use Half-Precision (GPU only):** In `backend/model.py`
   ```python
   model.half()  # Convert to FP16
   ```

### For Better Quality:
1. Increase `PROCESS_SIZE` to 768 or 1024 (slower)
2. Increase `n_iterations` in model to 16 (slower, better enhancement)

---

## 🔧 Troubleshooting

### Backend Issues

**"Model file not found"**
- Ensure `best_model.pth` is in the project root
- Check path: `MODEL_PATH = os.path.join(os.path.dirname(__file__), '..', 'best_model.pth')`

**"CUDA out of memory"**
- Fall back to CPU: Change `DEVICE = 'cpu'` in `backend/main.py`
- Reduce batch processing or model size

**"Connection refused"**
- Backend not running on port 8000
- Check firewall settings
- Ensure no other service uses port 8000: `netstat -ano | findstr :8000` (Windows)

### Frontend Issues

**"Connection error" message**
- Backend is not running
- Backend is on different port/host
- Check browser console for WebSocket errors

**Low FPS**
- Reduce video resolution in constraints
- Lower JPEG quality in backend
- Check CPU/GPU usage: Task Manager (Windows) or `top` (Linux)

**Webcam permission denied**
- Enable camera in browser settings
- Check OS camera permissions

---

## 📦 Deployment

### Docker Setup (Backend)

Create `backend/Dockerfile`:
```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["python", "main.py"]
```

Build and run:
```bash
docker build -t zero-dce-backend .
docker run -p 8000:8000 zero-dce-backend
```

### Cloud Deployment

**Backend:** Deploy FastAPI on:
- AWS ECS (with GPU support)
- Azure Container Instances
- Google Cloud Run
- Railway
- Render

**Frontend:** Deploy Next.js on:
- Vercel (recommended)
- Netlify
- AWS Amplify
- Azure Static Web Apps

---

## 📚 References

- **Zero-DCE Paper:** [Zero-Reference Deep Curve Estimation](https://openreview.net/forum?id=jmh0nDiYmF4)
- **FastAPI:** https://fastapi.tiangolo.com/
- **Next.js:** https://nextjs.org/
- **Tailwind CSS:** https://tailwindcss.com/
- **PyTorch:** https://pytorch.org/

---

## 📄 License

This project uses the Zero-DCE architecture. Ensure compliance with original paper's license.

---

## 💡 Future Enhancements

- [ ] Add preset enhancement levels (light, medium, heavy)
- [ ] Implement batch frame processing for higher throughput
- [ ] Add recording/export functionality
- [ ] Create mobile-responsive UI (PWA)
- [ ] Implement model quantization (ONNX)
- [ ] Add stats dashboard (latency history, etc.)
- [ ] Support multiple camera feeds
- [ ] Add effects/filters overlay
