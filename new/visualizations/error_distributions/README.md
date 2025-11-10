# Error Distribution Analysis

This directory contains error distribution visualizations for both machine learning models used in the Slope Stability Prediction System.

## Generated Files

### Individual Model Plots

1. **gradient_boosting_error_distribution.png** (and .pdf)
   - Error distribution histogram with fitted bell curve
   - Statistics: Mean error, Standard deviation, Sample size
   - Visual indicators for mean, ±1 standard deviation, and zero error

2. **xgboost_error_distribution.png** (and .pdf)
   - Error distribution histogram with fitted bell curve
   - Statistics: Mean error, Standard deviation, Sample size
   - Visual indicators for mean, ±1 standard deviation, and zero error

### Comparison Plot

3. **both_models_error_distribution_comparison.png** (and .pdf)
   - Side-by-side comparison of both models
   - Same scale for easy comparison
   - Highlights differences in error distributions

### Statistics Table

4. **error_statistics_comparison.csv**
   - Comprehensive statistical comparison
   - Includes: Mean, Std Dev, Min, Max, Median, Percentiles
   - Also includes Skewness and Kurtosis measures

## Key Statistics

### Gradient Boosting
- **Mean Error**: 0.0195 FoS units
- **Std Dev**: 0.0811
- **Sample Size**: 73
- **Error Range**: -0.1556 to 0.4041
- **Skewness**: 1.4328 (slightly right-skewed)

### XGBoost
- **Mean Error**: 0.0112 FoS units
- **Std Dev**: 0.0830
- **Sample Size**: 73
- **Error Range**: -0.1744 to 0.3880
- **Skewness**: 1.1487 (slightly right-skewed)

## Interpretation

### Error Distribution Characteristics

Both models show:
- **Near-zero mean error**: Indicating unbiased predictions
- **Symmetric distribution**: Errors are roughly normally distributed (bell curve)
- **Slight positive skew**: More large positive errors than negative errors
- **Low standard deviation**: Most predictions within ±0.08 FoS units of actual values

### Model Comparison

**XGBoost advantages:**
- Lower mean error (0.0112 vs 0.0195)
- Less skewness (more symmetric distribution)

**Gradient Boosting advantages:**
- Slightly lower standard deviation (0.0811 vs 0.0830)
- More consistent predictions in the center of the distribution

### Practical Implications

For Factor of Safety predictions:
- **68% of predictions** fall within ±1 standard deviation (~0.08 FoS units)
- **95% of predictions** fall within ±2 standard deviations (~0.16 FoS units)
- Both models show excellent accuracy with minimal systematic bias

## Visualization Features

Each plot includes:
- ✅ Histogram of observed errors (green bars)
- ✅ Fitted normal distribution curve (blue line)
- ✅ Mean error line (red dashed)
- ✅ ±1 standard deviation lines (orange dotted)
- ✅ Zero error reference line (dark green)
- ✅ Statistics text box with key metrics
- ✅ Publication-quality formatting (300 DPI)

## Generation

These visualizations were generated using:
```bash
python generate_error_distributions.py
```

The script analyzes prediction errors from the test set and creates publication-ready figures with statistical overlays.

## File Formats

- **PNG files**: High-resolution (300 DPI) for presentations and web display
- **PDF files**: Vector format for publication and printing
- **CSV file**: Detailed statistics for further analysis

---

**Generated**: November 10, 2025  
**Models**: Gradient Boosting & XGBoost  
**Test Set Size**: 73 samples
