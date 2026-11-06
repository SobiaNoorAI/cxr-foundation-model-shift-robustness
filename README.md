# Chest X-ray Foundation-Model Features under Hospital Shift and Image Corruption

A reproducible study of tuberculosis (TB) screening from chest X-rays. Do features from a chest-specific foundation model (RAD-DINO) transfer better across hospitals, survive image degradation, and give more trustworthy probabilities than a general vision model (DINOv2) or an ImageNet ResNet-50?

## Research questions

1. Do chest-specific features transfer across hospitals better than general or ImageNet features?
2. How does performance change under blur, noise, JPEG compression, low contrast and low resolution?
3. Are predicted probabilities calibrated across hospitals, and does abstaining on uncertain cases help?
4. Can the hospital be identified from the image, and do border cues explain the TB results?

## Main results

Cross-hospital AUROC (95% bootstrap CI), linear probe on frozen features. S to M means trained on Shenzhen and tested on Montgomery.

| Encoder | S to M | M to S |
|---|---|---|
| ResNet-50 (ImageNet) | 0.557 (0.453-0.650) | 0.675 (0.637-0.717) |
| DINOv2-base | 0.836 (0.762-0.900) | 0.746 (0.708-0.781) |
| RAD-DINO | 0.970 (0.944-0.989) | 0.938 (0.920-0.956) |

Key findings (exploratory; two small datasets):

- RAD-DINO had the highest AUROC in every setting, and paired bootstrap differences against DINOv2 and ResNet-50 excluded zero on clean images.
- RAD-DINO had the lowest Brier score in both directions, and answering only its most confident cases raised accuracy.
- Its lead over DINOv2 held under blur, but not clearly under strong noise or JPEG compression, where its AUROC fell by up to 0.28.
- Hospital identity was perfectly predictable from the images (even from 32 x 32 pixels), but cropping the borders did not change DINOv2 or RAD-DINO results.

See the report for tables, confidence intervals and limitations.

## Repository structure

```
.
├── README.md
├── requirements.txt
├── .gitignore
├── metadata.csv                  # file names, labels, hospital (no images)
├── notebooks/
│   ├── 01_data_and_features.ipynb
│   ├── 02_linear_probe_and_cross_hospital.ipynb
│   ├── 03_corruption_robustness.ipynb
│   ├── 04_calibration_and_paired_tests.ipynb
│   └── 05_hospital_shortcut_check.ipynb
├── results/
│   ├── 02_probe_results.csv, 02_auroc.png
│   ├── 03_corruption_results.csv, 03_corruption_curves.png, 03_corruption_examples.png
│   ├── 04_calibration.csv, 04_reliability.png, 04_selective.csv, 04_selective.png, 04_paired_tests.csv
│   └── 05_hospital_predictability.csv, 05_crop_ablation.csv, 05_crop_examples.png
└── reports/
    └── report.md
```

## How to run

1. Open the notebooks in Google Colab in numeric order. Notebooks 01, 03 and 05 need a T4 GPU runtime; 02 and 04 run on CPU.
2. Notebook 01 downloads the data from Kaggle. Add your Kaggle credentials in Colab Secrets as `KAGGLE_USERNAME` and `KAGGLE_KEY`. Never paste keys into notebook cells.
3. Features and results are saved to Google Drive under `MyDrive/phd_projects/cxr_shift/` (change the `BASE` variable to use another folder). Notebooks 02 to 05 read from there.
4. Model weights are downloaded automatically from Hugging Face (`facebook/dinov2-base`, `microsoft/rad-dino`) and from timm (ResNet-50).

## Data

The images are **not** included in this repository.

- Shenzhen and Montgomery tuberculosis chest X-ray datasets, released by the US National Library of Medicine. Obtained from Kaggle mirrors (`raddar/tuberculosis-chest-xrays-shenzhen` and `raddar/tuberculosis-chest-xrays-montgomery`). The license field on those Kaggle pages was shown as "unknown"; please check the original source terms before reuse.
- Reference: Jaeger et al. (2014), Two public chest X-ray datasets for computer-aided screening of pulmonary diseases, Quantitative Imaging in Medicine and Surgery.

## Models and licenses

- RAD-DINO (`microsoft/rad-dino`): **[TODO: add the license name from the model card and confirm Shenzhen/Montgomery are not in its pretraining data.]**
- DINOv2 (`facebook/dinov2-base`): see its Hugging Face model card.
- ResNet-50 weights from timm: see the timm documentation.

## Limitations

Only two small datasets (Montgomery has 138 images); intervals reflect test-set sampling only; corruption severities are author-chosen stress tests; the border crop also zooms the lungs; calibration metrics have no confidence intervals. This is a research study, not a clinical tool. See the report for the full list.

## Citation

If you use this work, please cite the repository and contact the author through GitHub.
