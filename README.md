# cxr-foundation-model-shift-robustness
Cross-hospital generalization, image-corruption robustness, and uncertainty of chest X-ray foundation-model features for TB screening (Shenzhen to Montgomery).
## Structure
```bash
cxr-foundation-model-shift-robustness/
├── README.md
├── requirements.txt
├── .gitignore            # data/, features/, *.npy, kaggle.json
├── notebooks/
│   ├── 01_data_and_eda.ipynb
│   ├── 02_feature_extraction.ipynb
│   ├── 03_linear_probe_and_cross_hospital.ipynb
│   ├── 04_corruption_robustness.ipynb
│   ├── 05_calibration_and_selective_prediction.ipynb
│   └── 06_hospital_shortcut_check.ipynb
├── results/
│   ├── tables/           # CSV result tables
│   └── figures/
├── reports/
│   └── report.md         # or PDF
└── metadata.csv          # file names and labels only, no images
```
