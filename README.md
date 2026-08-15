# Smart Agri-Advisory Platform with Crop & Input Recommendation

Full source code for the CSE347 project — a role-based (RBAC) web platform
that recommends crops, fertilizer, market timing, and (optionally) detects
pest/disease from soil, weather, and historical data for Bangladeshi
farmers. Built against the 26-table normalized database schema
(users / climate_zones / crops / farm_profiles / recommendations / ...).

## Architecture

```
┌───────────────────────┐        HTTP/Axios        ┌─────────────────────────┐
│  Laravel 11 + Blade    │ ───────────────────────▶ │  Flask ML Microservice  │
│  (RBAC, MySQL, UI)     │ ◀─────────────────────── │  (Random Forest, k-NN,  │
│  laravel-backend/      │        JSON              │   sequence/LSTM model)  │
└───────────────────────┘                           │  ml-service/            │
                                                     └─────────────────────────┘
```

- **laravel-backend/** — the web app: RBAC auth, farm profiles (soil pH/N/P/K
  live directly on `farm_profiles`), recommendations, fertilizer/price/pest
  history, Extension Officer verification/override/advisory/alerts/training,
  Supplier products/orders/inquiries, Admin master-data/retrain/backup/
  analytics. Bengali UI. See `laravel-backend/README.md` for the full
  schema-to-code map.
- **ml-service/** — the Python Flask microservice: crop recommendation
  (Random Forest, trained & tested), fertilizer (rule-based + k-NN),
  price forecast (sequence model, LSTM-swappable), pest/disease CNN
  (architecture ready, needs real image data to train). See
  `ml-service/README.md`.

## Quick start

```bash
# Terminal 1 — ML service
cd ml-service
python3 -m venv venv && source venv/bin/activate
pip install -r requirements.txt
python generate_data.py && python train_crop_model.py && python price_forecast.py
python app.py                      # -> http://localhost:5000

# Terminal 2 — Laravel app
cd laravel-backend
composer install
cp .env.example .env && php artisan key:generate
php artisan migrate --seed
php artisan serve                  # -> http://localhost:8000
```

Log in with a seeded demo account (see `laravel-backend/README.md`) and try:
add a farm profile (zone + soil pH/N/P/K) → get a crop recommendation →
get a fertilizer recommendation using that recommendation's ID → check the
price forecast → (as Extension Officer) verify the farm profile and
override/advise → (as Admin) approve pending accounts and view analytics.

## What's real vs. what's a scaffold

- **Real & tested in this repo:** crop Random Forest model (~83% accuracy
  on synthetic data), fertilizer rule+kNN engine, price sequence
  forecaster — trained, and the full Flask API tested end-to-end with curl.
- **Scaffolded, needs your data:** the pest/disease CNN needs real labeled
  leaf images (none bundled); the literal Keras LSTM script
  (`ml-service/train_lstm_keras.py`) needs `pip install tensorflow`.
- **Needs `composer install` locally:** the Laravel backend, since this
  sandbox cannot reach Packagist.

## Next steps for your group

1. Replace `ml-service/data/crop_data.csv` with real BARI data, then
   re-run `train_crop_model.py` — no other code changes needed.
2. Collect a small leaf-disease image set and run `pest_cnn.py --train`.
3. Wire `OPENWEATHER_API_KEY` in `.env` and populate `weather_logs` on a
   schedule (e.g. `php artisan schedule` + a new `WeatherService`) so crop
   recommendations use live weather instead of the fallback defaults.
4. Add `php artisan make:test` coverage for the controllers if your course
   wants automated tests as part of grading.
