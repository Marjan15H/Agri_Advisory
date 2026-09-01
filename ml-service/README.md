<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:F7931E,100:2E7D32&height=200&section=header&text=ML%20Microservice&fontSize=50&fontColor=ffffff&animation=fadeIn&fontAlignY=35&desc=Smart%20Agri-Advisory%20Platform&descAlignY=55&descSize=20" width="100%"/>

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=22&duration=3000&pause=800&color=F7931E&center=true&vCenter=true&width=650&lines=Flask+%C2%B7+Random+Forest+%C2%B7+Real+Kaggle+Dataset;Crop+%2B+Fertilizer+%2B+Price+Forecast+API;99.3%25+Test+Accuracy+%F0%9F%8E%AF" alt="Typing SVG" />

![Flask](https://img.shields.io/badge/Flask-ML%20API-000000?style=for-the-badge&logo=flask&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Accuracy](https://img.shields.io/badge/Crop%20Model-99.3%25%20Accuracy-2ECC71?style=for-the-badge)
![Dataset](https://img.shields.io/badge/Dataset-2200%20rows%20%C2%B7%2022%20crops-orange?style=for-the-badge)
![scikit--learn](https://img.shields.io/badge/scikit--learn-Random%20Forest-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white)

</div>

---

## 📌 Overview

This folder contains the **Flask ML microservice** — it receives HTTP calls from the Laravel backend and returns predictions for crops, fertilizer, market prices, and (later) pest/disease detection.

> 🎉 **Update:** The crop recommendation model is now trained on the real **Kaggle "Crop Recommendation Dataset"** — 2,200 rows, 22 crops (N, P, K, temperature, humidity, ph, rainfall). It has moved on from synthetic data and now works on genuine agricultural data.

<div align="center">

| 📊 Metric | Value |
|:---:|:---:|
| **Test Accuracy** | 🎯 **99.3%** |
| **Dataset Size** | 2,200 rows |
| **Number of Crops** | 22 labels |
| **Features** | 7 (N, P, K, temperature, humidity, pH, rainfall) |

</div>

**Data files:**
- `data/Crop_recommendation.csv` — the original Kaggle dataset
- `data/crop_data.csv` — renamed to match this service's column names

> ⚠️ **Note:** The real dataset has no `climate_zone` column, so `climate_zone` is no longer a required field in `/api/predict/crop` — the model relies only on the 7 soil/weather features. Crop names for fertilizer and price-forecast have also been aligned with the real dataset's 22 labels (rice, maize, chickpea, banana, mango, cotton, jute, coffee, ...) instead of the earlier Bangladesh-style names. See the `BASE_DOSAGE` dict in `fertilizer.py` for the full list.

---

## 🚀 Setup

```bash
cd ml-service
python3 -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

## 🏋️ Training the Models (first run)

```bash
python train_crop_model.py      # 🌾 Train + save Random Forest on the real dataset
python price_forecast.py        # 📈 Train + save the price forecast model
```

## ▶️ Starting the API

```bash
python app.py
# ➜ http://localhost:5000 (or the port you've configured)
```

---

## 🔌 API Endpoints

<div align="center">

| Method | Path | Body |
|:---:|---|---|
| 🟢 `GET` | `/api/health` | – |
| 🌾 `POST` | `/api/predict/crop` | `soil_ph, nitrogen, phosphorus, potassium, rainfall_mm, temperature_c, humidity_pct` |
| 🧪 `POST` | `/api/predict/fertilizer` | `crop, soil_ph, nitrogen, phosphorus, potassium` |
| 📈 `POST` | `/api/predict/price` | `crop, months_ahead` |
| 🍃 `POST` | `/api/predict/pest` | multipart form field `image` |

</div>

---

## 🧪 Test Data

`data/test_samples.csv` contains one representative row per crop from the real dataset — use these to quickly verify predictions via the farmer dashboard or curl/Postman, without needing to sign up:

```csv
crop,soil_ph,nitrogen,phosphorus,potassium,rainfall_mm,temperature_c,humidity_pct
rice,6.5,90,42,43,202.94,20.88,82.0
maize,5.75,71,54,16,87.76,22.61,63.69
chickpea,7.49,40,72,77,88.55,17.02,16.99
...
```

**Example curl call:**

```bash
curl -X POST http://localhost:5000/api/predict/crop \
  -H "Content-Type: application/json" \
  -d '{
    "soil_ph": 6.5,
    "nitrogen": 90,
    "phosphorus": 42,
    "potassium": 43,
    "rainfall_mm": 202.94,
    "temperature_c": 20.88,
    "humidity_pct": 82.0
  }'
```

---

## 🔄 Data Flow

```
Laravel FarmerController
        │  (soil pH/N/P/K + weather)
        ▼
Flask /api/predict/crop  ──▶  🌾 Random Forest (99.3% accuracy)
        │
        ▼
Returns crop name  ──▶  resolved to crops.id in Laravel
        │
        ▼
/api/predict/fertilizer  ──▶  🧪 Rule-based + k-NN
        │
        ▼
/api/predict/price  ──▶  📈 Sequence forecast model
```

---

<div align="center">

🔗 **Live App:** [agriadvisory.app](http://agriadvisory.app)  ·  📦 **Repo:** [Agri_Advisory](https://github.com/Marjan15H/Agri_Advisory)

Made with for Bangladeshi Farmers

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:2E7D32,100:F7931E&height=100&section=footer" width="100%"/>

</div>
