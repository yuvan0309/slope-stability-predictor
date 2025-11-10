# Multi-Layer Mineral Support

## Overview

The web application now supports **multi-layer predictions**, allowing you to model slopes with different mineral or soil layers, each with its own properties and pore pressure ratio (Ru).

## Features

### 🪨 Multi-Layer Mode
- Add multiple mineral/soil layers
- Each layer has independent properties:
  - **Cohesion (c)** - 0 to 100 kPa
  - **Friction Angle (φ)** - 0 to 45 degrees
  - **Unit Weight (γ)** - 15 to 25 kN/m³
  - **Ru (Pore Pressure Ratio)** - 0 to 1 (separate for each layer!)

### 📊 Simple Mode
- Traditional single-layer prediction
- All original features preserved
- Quick predictions for homogeneous slopes

## How to Use

### Method 1: Sample Data (Quick Start)

1. Click **"🪨 Multi-Layer Mode"** button
2. Click **"📋 Load Sample Data"** to load example layers:
   - **Laterite**: c=23.33 kPa, φ=23.07°, γ=22.35 kN/m³, Ru=0.25
   - **Phyllitic Clay**: c=15.73 kPa, φ=19.70°, γ=18.76 kN/m³, Ru=0.35
3. Click **"🎯 Predict Factor of Safety"**

### Method 2: Custom Layers

1. Switch to Multi-Layer Mode
2. Enter layer name (e.g., "Laterite", "Phyllitic Clay", "Sandstone")
3. Adjust properties using sliders or direct input
4. **Important**: Set appropriate Ru value for each layer
5. Click **"➕ Add Layer"** for additional layers
6. Click **"🎯 Predict Factor of Safety"**

## Calculation Method

The system uses a **weighted average approach**:

```
Overall FoS = Σ(FoS_i × γ_i) / Σ(γ_i)
```

Where:
- `FoS_i` = Factor of Safety for layer i
- `γ_i` = Unit weight of layer i

This method accounts for the relative contribution of each layer based on its unit weight.

## Results Display

### Multi-Layer Results Show:

1. **Overall Factor of Safety**
   - Weighted average across all layers
   - 95% confidence interval
   - Safety classification (SAFE/CAUTION/WARNING/CRITICAL)

2. **Individual Layer Analysis**
   - FoS for each layer
   - Properties of each layer
   - Visual cards with hover effects

3. **Model Performance**
   - R² Score
   - RMSE and MAE metrics
   - Model name (Gradient Boosting or XGBoost)

## Example Use Case

### Mining Slope with Two Layers

**Top Layer - Laterite (Weathered material)**
- Cohesion: 23.33 kPa
- Friction Angle: 23.07°
- Unit Weight: 22.35 kN/m³
- Ru: 0.25 (moderate pore pressure)

**Bottom Layer - Phyllitic Clay (Parent rock)**
- Cohesion: 15.73 kPa
- Friction Angle: 19.70°
- Unit Weight: 18.76 kN/m³
- Ru: 0.35 (higher pore pressure)

**Result**: The system predicts an overall FoS and shows individual FoS for each layer.

## API Endpoint

### POST /predict

#### Multi-Layer Request Format:

```json
{
  "layers": [
    {
      "name": "Laterite",
      "cohesion": 23.33,
      "friction_angle": 23.07,
      "unit_weight": 22.35,
      "ru": 0.25
    },
    {
      "name": "Phyllitic Clay",
      "cohesion": 15.73,
      "friction_angle": 19.70,
      "unit_weight": 18.76,
      "ru": 0.35
    }
  ],
  "model": "gradient_boosting"
}
```

#### Response Format:

```json
{
  "success": true,
  "prediction_type": "multi-layer",
  "prediction": {
    "fos": 1.2345,
    "confidence_interval": {
      "lower": 1.1500,
      "upper": 1.3190,
      "level": "95%"
    }
  },
  "layers": [
    {
      "name": "Laterite",
      "fos": 1.2567,
      "properties": {
        "cohesion": 23.33,
        "friction_angle": 23.07,
        "unit_weight": 22.35,
        "ru": 0.25
      }
    },
    {
      "name": "Phyllitic Clay",
      "fos": 1.2123,
      "properties": {
        "cohesion": 15.73,
        "friction_angle": 19.70,
        "unit_weight": 18.76,
        "ru": 0.35
      }
    }
  ],
  "calculation_method": "Weighted average by unit weight",
  "safety": {
    "status": "WARNING",
    "message": "Slope stability is marginal - review required",
    "color": "orange"
  },
  "model": {
    "name": "Gradient Boosting",
    "r2_score": 0.9426,
    "rmse": 0.0834,
    "mae": 0.0563
  }
}
```

## UI Features

### Layer Card
- **Editable name** - Click to change layer name
- **Number inputs** - Direct value entry
- **Sliders** - Visual adjustment of parameters
- **Ru highlighting** - Yellow background for critical parameter
- **Remove button** - Delete layers (minimum 1 required)

### Visual Indicators
- **First layer** - Blue border
- **Hover effect** - Blue glow on hover
- **Ru parameter** - Yellow highlighting with "CRITICAL" badge

### Buttons
- **Load Sample Data** - Green button with preset values
- **Add Layer** - Blue button to add more layers
- **Remove Layer** - Red X button (only if multiple layers exist)
- **Predict** - Purple gradient button with loading state

## Switching Modes

Toggle between modes using the buttons at the top:
- **🪨 Multi-Layer Mode** - For complex, stratified slopes
- **📊 Simple Mode** - For single-layer predictions

Results are cleared when switching modes.

## Validation

Each layer is validated:
- Cohesion: 0-100 kPa
- Friction Angle: 0-45°
- Unit Weight: 15-25 kN/m³
- Ru: 0-1

Invalid values will show an error message.

## Benefits

1. **More Accurate** - Models real-world stratified slopes
2. **Separate Ru Values** - Each layer can have different pore pressures
3. **Layer-Specific Results** - See which layers are critical
4. **Flexible** - Add as many layers as needed
5. **Intuitive UI** - Easy to use with visual feedback

## Technical Details

- **Backend**: Flask API with multi-layer prediction endpoint
- **Frontend**: Svelte component with reactive state management
- **Calculation**: Weighted average by unit weight
- **Models**: Same trained GB and XGBoost models
- **Scaling**: StandardScaler applied to each layer independently

---

**Last Updated**: November 10, 2025  
**Version**: 2.0 - Multi-Layer Support
