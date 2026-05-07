# Lab 21 Submission - Nguyễn Đức Tiến (2A202600393)

## 📋 Submission Structure (Option A - Lightweight)

```
lab21_2A202600393_NguyenDucTien/
├── REPORT.md                          ← Evaluation report with 6 sections
├── notebook.ipynb                     ← Notebook (clear outputs before submission)
├── requirements.txt                   ← Package versions for reproducibility
├── adapters/
│   └── r16/                           ← Best rank adapter only
│       ├── adapter_model.safetensors
│       └── adapter_config.json
└── results/
    ├── rank_experiment_summary.csv    ← Metrics for all 3 ranks
    ├── qualitative_comparison.csv
    └── loss_curves.png
```

## 🚀 How to Use

### 1. Run the Notebook
1. Upload `notebook.ipynb` to Google Colab
2. Enable GPU runtime: `Runtime > Change runtime type > GPU`
3. Run all cells sequentially (0-31)
4. All outputs will be saved to `/content/lab21_lora_t4/`

### 2. Prepare Submission
1. Download all generated files from Colab
2. Copy files to the correct folders as shown above
3. Clear notebook outputs: `Kernel > Restart & Clear Output`
4. Zip the entire folder and submit

## 📊 Expected Results

After running the notebook, you should have:
- **3 trained LoRA adapters** (r=8, r=16, r=64)
- **Quantitative metrics**: training time, VRAM usage, perplexity
- **Qualitative comparison**: 5 test prompts with before/after results
- **Complete report**: 6 sections with analysis and conclusions

## 🎯 Grading Criteria

- **Functionality (40 pts)**: All 3 adapters trained successfully
- **Experiment Design (25 pts)**: Proper rank comparison with insights
- **Evaluation Quality (15 pts)**: Perplexity + qualitative examples
- **Report Quality (20 pts)**: All 6 sections complete with analysis

**Total**: 100 points possible

## 📝 Notes

- Notebook is optimized for T4 GPU (16GB VRAM)
- Uses QLoRA 4-bit quantization for memory efficiency
- Includes robust error handling for OOM situations
- Auto-generates report with actual experimental results
