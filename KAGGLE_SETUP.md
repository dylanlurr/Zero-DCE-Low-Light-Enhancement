# 🌐 Kaggle Setup Guide

## Quick Start for Kaggle

### 1. Upload Notebook
- Go to Kaggle.com → Click **"Code"** → **"New Notebook"**
- Click **"File"** → **"Upload Notebook"**
- Select `low-light-image-enhancement-of-street.ipynb`

### 2. Add Dataset
The notebook needs the **LoLI-Street Dataset**. To add it:

**Option A - Search by Name:**
1. Click **"+ Add Data"** (right sidebar)
2. Search: `loli-street-low-light-image-enhancement-of-street`
3. Click **"Add"** on the dataset by **soumikrakshit**

**Option B - Direct Link:**
- Dataset URL: https://www.kaggle.com/datasets/soumikrakshit/loli-street-low-light-image-enhancement-of-street
- Click "Add to Notebook" while viewing the dataset

### 3. Enable GPU Acceleration
1. Click **"Settings"** (right sidebar)
2. Under **"Accelerator"**, select:
   - **GPU T4 x2** (recommended) or
   - **GPU P100** (faster but limited availability)
3. Click **"Save"**

### 4. Verify Environment
Run cell 3 (Imports & Setup). You should see:
```
🌐 Running on Kaggle
🔧 Using device: cuda
📁 Dataset paths configured:
   Train Low:  /kaggle/input/loli-street-low-light-image-enhancement-of-street/LoLI-Street Dataset/Train/low
   ...
```

### 5. Run Training
- Click **"Run All"** or run cells sequentially
- Training takes ~2-4 hours on GPU
- Model saves automatically to `/kaggle/working/best_model.pth`

---

## What Happens Automatically

The notebook **auto-detects** the environment and adjusts:

| Setting | Local | Kaggle |
|---------|-------|--------|
| Dataset Root | `./LoLI-Street Dataset/` | `/kaggle/input/...` |
| Output Directory | `./` | `/kaggle/working/` |
| Num Workers | 0 | 2 |
| Webcam Support | ✅ Yes | ❌ No (cell 16 disabled) |

---

## Downloading Results

After training completes:

1. **Best Model:**
   - Path: `/kaggle/working/best_model.pth`
   - Click **"Output"** tab → Download

2. **ONNX Model:**
   - Path: `/kaggle/working/zero_dce_model.onnx`
   - Click **"Output"** tab → Download

3. **Enhanced Images** (if batch processing was run):
   - Path: `/kaggle/working/enhanced_outputs/`
   - Click folder → Download as ZIP

---

## Troubleshooting

### Dataset Not Found Error
**Error:** `FileNotFoundError: [Errno 2] No such file or directory`

**Fix:**
1. Verify dataset is added (check right sidebar under "Input")
2. Update cell 3 if the dataset path differs:
   ```python
   DATA_ROOT = '/kaggle/input/YOUR-ACTUAL-DATASET-SLUG/LoLI-Street Dataset'
   ```

### CUDA Out of Memory
**Error:** `RuntimeError: CUDA out of memory`

**Fix:** Reduce batch size in cell 3:
```python
BATCH_SIZE = 2  # Instead of 4
```

### Session Timeout
**Issue:** Session ends during training

**Fix:** 
- Enable "Persistent Sessions" (requires Kaggle verification)
- Or save checkpoints more frequently
- Or reduce `NUM_EPOCHS` to 25-30

---

## Performance Comparison

| Hardware | Training Time (50 epochs) |
|----------|---------------------------|
| Kaggle GPU T4 | ~2-3 hours |
| Kaggle GPU P100 | ~1.5-2 hours |
| Local CPU | ~20-30 hours |
| Local GPU (RTX 3060) | ~2-3 hours |

---

## Cost Comparison

| Platform | GPU Access | Storage | Time Limit | Cost |
|----------|------------|---------|------------|------|
| **Kaggle** | Free T4/P100 | 20GB temp | 12 hrs/session | **FREE** |
| **Local** | Your GPU | Unlimited | Unlimited | Hardware cost |
| **Google Colab** | Free T4 | 15GB temp | 12 hrs/session | FREE |
| **AWS SageMaker** | Various | Unlimited | Pay-per-use | ~$1-3/hour |

---

## Next Steps After Training

1. **Download the model** (`best_model.pth`)
2. **Test locally** with real-time webcam (cell 16)
3. **Deploy with ONNX** for production use
4. **Share your results** on Kaggle discussion forums!

---

**Happy Training! 🚀**
