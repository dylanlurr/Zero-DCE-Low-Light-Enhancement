# Zero-DCE: Low-Light Image Enhancement

Real-time low-light image enhancement using **Zero-DCE (Zero-Reference Deep Curve Estimation)** with hybrid supervised training on the LoLI-Street Dataset.

## 🌟 Features

- ✅ **Lightweight Architecture**: Only 79K parameters for real-time performance
- ✅ **Hybrid Supervised Training**: Combines SSIM + L1 losses with spatial regularization
- ✅ **Smart Data Augmentation**: RandomCrop preserves street texture details
- ✅ **Real-Time Webcam Support**: Live enhancement via OpenCV
- ✅ **ONNX Export**: Production-ready deployment format
- ✅ **Batch Processing**: Bulk image enhancement utility
- ✅ **Kaggle Ready**: Automatic environment detection and path configuration

## 📋 Requirements

- Python 3.8+
- PyTorch 2.0+
- CUDA-capable GPU (recommended)
- Webcam (for real-time demo)

## 🚀 Quick Start

### Option A: Run on Kaggle (Recommended for Beginners)

1. Upload notebook to Kaggle
2. Add "LoLI-Street Dataset" from Kaggle datasets
3. Enable GPU (T4 or P100)
4. Run all cells

**Detailed guide:** See [KAGGLE_SETUP.md](KAGGLE_SETUP.md)

### Option B: Run Locally

#### 1. Setup Virtual Environment

```powershell
# Create virtual environment
python -m venv venv

# Activate (Windows PowerShell)
.\venv\Scripts\python.exe

# Install dependencies
.\venv\Scripts\pip.exe install -r requirements.txt
```

#### 2. Dataset Structure

Ensure your LoLI-Street Dataset is organized as:

```
LoLI-Street Dataset/
├── Train/
│   ├── low/     # Low-light training images
│   └── high/    # Ground truth training images
├── Val/
│   ├── low/     # Low-light validation images
│   └── high/    # Ground truth validation images
└── Test/        # Test images (low-light only)
```

#### 3. Training

Open `low-light-image-enhancement-of-street.ipynb` and run all cells sequentially:

1. **Cell 1-4**: Setup and model initialization
2. **Cell 5**: Training loop (50 epochs, ~2-4 hours on GPU)
3. **Cell 6**: Visualize training history
4. **Cell 7**: Export to ONNX and test inference
5. **Cell 8**: Real-time webcam enhancement (optional)
6. **Cell 9**: Batch processing (optional)

## 📊 Model Architecture

### DCE-Net (Deep Curve Estimation Network)

```
Input (3, H, W)
    ↓
Conv1-6 (32 filters, ReLU)
    ↓
Conv7 (24 filters, Tanh) → Curve Parameters
    ↓
Iterative Enhancement (8 iterations)
    ↓
Output (3, H, W)
```

### Loss Function

**Hybrid Loss = Supervised + Regularization**

- **Supervised**: `10×L_SSIM + 5×L_L1` (guides towards ground truth)
- **Regularization**: `1×L_spa + 1×L_tv` (ensures smoothness)

## 🎯 Hyperparameters

| Parameter | Value | Description |
|-----------|-------|-------------|
| Epochs | 50 | Training iterations |
| Batch Size | 4 | Images per batch |
| Learning Rate | 1e-4 | Initial LR (Adam optimizer) |
| Weight Decay | 1e-4 | L2 regularization |
| Train Crop | 256×256 | Random crop size |
| Val Resize | 512×512 | Validation resize |
| Curve Iterations | 8 | Enhancement iterations |

## 📁 Output Files

- `best_model.pth` - PyTorch checkpoint (best validation SSIM)
- `zero_dce_model.onnx` - ONNX format for deployment
- `enhanced_outputs/` - Batch processed images (optional)

## 🎥 Real-Time Demo

To run webcam enhancement:

```python
# In the notebook, uncomment and run:
real_time_webcam_enhancement()
```

Press `q` to quit the video stream.

## 📦 Batch Processing

To enhance multiple images:

```python
batch_enhance_images(
    input_dir="./LoLI-Street Dataset/Test",
    output_dir="./enhanced_outputs",
    max_images=10  # None for all images
)
```

## 🔧 Troubleshooting

### CUDA Out of Memory
- Reduce `BATCH_SIZE` to 2 or 1
- Reduce `TRAIN_CROP_SIZE` to 128

### Webcam Not Working
- Check camera permissions
- Try different camera index: `cv2.VideoCapture(1)`

### Poor Enhancement Quality
- Ensure dataset pairing is correct (matching filenames)
- Increase training epochs (50 → 100)
- Adjust loss weights in `CombinedLoss`

## 📚 References

- [Zero-DCE Paper](https://arxiv.org/abs/2001.06826)
- [LoLI-Street Dataset](https://www.kaggle.com/datasets/soumikrakshit/loli-street-low-light-image-enhancement-of-street)

## 📄 License

This project is for educational and research purposes.

---

**Built with ❤️ using PyTorch**
