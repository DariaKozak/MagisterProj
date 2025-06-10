# Liver Tumor Segmentation with nnU-Net v2

This project demonstrates how to train and evaluate a liver tumor segmentation model using **nnU-Net v2** on the [MSD Task 03 Liver](http://medicaldecathlon.com/) dataset.

⚠️ **Note:** Due to GitHub file size limitations, the original raw medical image files used for inference have been removed from the repository. You will need to download and prepare the dataset manually (instructions below).

---

## 📦 Project Structure

```
├── nnUNet_raw/
│   └── Dataset503_Liver/
│       ├── imagesTr/              ← training images
│       ├── labelsTr/              ← ground truth segmentations
│       └── imagesTs/              ← test images (optional)
├── predictions_selected_demo/     ← prediction outputs for selected files
├── selected_images/               ← directory with a few test images for inference demo
├── evaluate_liver_metrics.py      ← script to evaluate predictions against ground truth
├── predict_evaluate_liver_metrics.py ← script to predict + evaluate demo images
└── nnUNet_preprocessed/           ← output of preprocessed data
```

---

## 🚀 Steps to Run

### 1. Download Dataset

Download the LiTS dataset from the official [MSD Task 03 Liver](http://medicaldecathlon.com/). Organize the files as follows:

```
nnUNet_raw/
└── Dataset503_Liver/
    ├── imagesTr/
    ├── labelsTr/
    └── imagesTs/
```

Ensure that the `.nii.gz` files are properly named, e.g., `liver_0_0000.nii.gz`, `liver_0.nii.gz`, etc.

---

### 2. Prepare Data

Run the dataset setup script:

```bash
python setup_liver_nnunet.py
```

This will:
- Create the correct folder structure
- Rename files to match nnU-Net conventions
- Generate the necessary `dataset.json`

---

### 3. Start Training

```bash
nnUNetv2_train -d 503 -c 2d -f 0
```

This will begin training a 2D nnU-Net model for liver segmentation on CPU or GPU (depending on your setup).

You can stop training anytime — progress is saved.

---

### 4. Run Inference + Evaluation

After training completes or checkpoint is available, run:

```bash
python predict_evaluate_liver_metrics.py
```

This will:
- Use your trained model to predict on a few selected test images
- Compare predictions with ground truth
- Output Dice, Precision, Recall, and F1 scores in a CSV
- Display visual overlays of results

---

## 🧪 Evaluation Metrics

Metrics are saved to `liver_segmentation_metrics.csv` and include:

- Dice Coefficient
- Precision
- Recall
- F1-score

---

## 📌 Requirements

- Python 3.10 or 3.11
- PyTorch
- nnU-Net v2 installed in editable mode
- nibabel, numpy, pandas, scikit-learn, matplotlib

Use a virtual environment and activate it before running scripts.

# 🧠 Liver Segmentation with MONAI (2D UNet)

This project trains a 2D U-Net model using [MONAI](https://monai.io/) for liver and tumor segmentation based on sagittal slices of medical CT scans (LiTS dataset).

> ⚠️ **Note:**  
> Due to [GitHub file size limitations](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-large-files-on-github), the raw `.nii.gz` files used for inference and training have been **removed from the repository**.

## 📁 Project Structure

```
monai-liver/
├── data/
│   ├── raw/               # raw .nii.gz files (you must re-download)
│   ├── imagesTr/          # 2D training images
│   ├── labelsTr/          # 2D training masks
│   └── ...
├── scripts/
│   ├── prepare_data.py    # convert .nii.gz to PNG slices
│   ├── train.py           # model training
│   ├── infer.py           # prediction / visualization
├── models/
├── utils/
├── outputs/
│   ├── checkpoints/       # saved weights
│   └── progress.png       # training curve
```

## 🚀 How to Run

1. **Download Dataset**  
   Place `.nii.gz` files into:
   ```
   data/raw/imagesTr/
   data/raw/labelsTr/
   ```

2. **Prepare Data**  
   Convert `.nii.gz` files into 2D sagittal PNG slices:
   ```bash
   python scripts/prepare_data.py
   ```

3. **Start Training**  
   ```bash
   python scripts/train.py
   ```

4. **Run Inference**
   ```bash
   python scripts/infer.py
   ```