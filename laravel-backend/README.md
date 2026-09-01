<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:2E7D32,100:1B5E20&height=200&section=header&text=Laravel%20Backend&fontSize=50&fontColor=ffffff&animation=fadeIn&fontAlignY=35&desc=Smart%20Agri-Advisory%20Platform&descAlignY=55&descSize=20" width="100%"/>

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=22&duration=3000&pause=800&color=2E7D32&center=true&vCenter=true&width=600&lines=RBAC+%C2%B7+MySQL+%C2%B7+Bengali+UI;AI-Powered+Crop+%26+Fertilizer+Advisory;Built+for+Bangladeshi+Farmers+%F0%9F%8C%BE" alt="Typing SVG" />

![Laravel](https://img.shields.io/badge/Laravel-11-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)
![PHP](https://img.shields.io/badge/PHP-8.2+-777BB4?style=for-the-badge&logo=php&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-26%20Tables-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![RBAC](https://img.shields.io/badge/Auth-RBAC-9C27B0?style=for-the-badge)
![Blade](https://img.shields.io/badge/UI-Bengali%20%F0%9F%87%A7%F0%9F%87%A9-006A4E?style=for-the-badge)

</div>

---

## 📌 Overview

This folder contains the complete web application built with **Laravel 11**, based on a **26-table normalized database schema** (see `docs/Database_Schema.md`). It includes RBAC authentication, farm profiles, recommendation management, Extension Officer tools, a Supplier module, and an Admin panel — all in one place.

> ⚠️ `vendor/` and `composer.lock` are not included, since Packagist access isn't available in this sandbox. Run `composer install` locally the first time.

---

The seeder also generates reference rows: **6 `climate_zones`** and **15 `crops`** — the crop names match exactly with `ml-service/generate_data.py`, so `Crop::findByNameOrCreate()` in `app/Models/Crop.php` can directly resolve every crop name coming from the ML service to a `crops.id`.

---

## 🗂️ Schema → Code Map (26 Tables)

<details open>
<summary><b>Click to expand / collapse</b></summary>

| Group | Tables | Status |
|---|---|:---:|
| 🔐 **Core/Auth** | `users` | ✅ Complete (RBAC, `EnsureRole` middleware) |
| 📚 **Reference** | `climate_zones`, `crops` | ✅ Complete + seeded; `crop_calendar` migration exists, no UI (optional) |
| 🌱 **Farmer** | `farm_profiles` (pH/N/P/K live here), `weather_logs` | ✅ Complete; `weather_logs` used opportunistically, no OpenWeather cron yet |
| 🤖 **ML Output** | `recommendations`, `fertilizer_recommendations`, `price_forecasts`, `disease_detections` | ✅ Complete — every ML call in `FarmerController` is saved here |
| 💬 **Feedback** | `recommendation_feedback` | ✅ Complete |
| 👨‍🌾 **Extension Officer** | `officer_verifications`, `officer_zone_assignments`, `officer_overrides`, `advisory_messages`, `alerts`, `training_sessions`, `training_attendees` | ✅ Mostly complete; registration UI for `training_attendees` is pending |
| 🚚 **Supplier** | `suppliers`, `products`, `orders`, `order_items`, `inquiries`, `demand_forecasts` | ✅ Complete |
| 🛡️ **Admin** | `admin_logs`, `model_retraining_jobs`, `system_backups`, `analytics_snapshots` | ✅ Complete — every admin action is logged to `admin_logs` |

</details>

---

## 🛡️ RBAC (Role-Based Access Control)

`users.role` can take four values:

```
farmer  │  extension_officer  │  supplier  │  admin
```

`app/Http/Middleware/EnsureRole.php` enforces these roles (aliased as `role` in `bootstrap/app.php`).

> 🔔 New `extension_officer` / `supplier` signups start with `status = pending` (a `suppliers` row is also created, `verified = false`) — admin approval is required:
> `POST /admin/users/{user}/approve`

---

## 🔌 Connection to the ML Service

```
Laravel (FarmerController)
        │
        ▼
app/Services/MlService.php  ──HTTP──▶  ml-service/app.py (Flask)
        │
        ▼
Crop::findByNameOrCreate()  →  resolves crops.id
        │
        ▼
recommendations / fertilizer_recommendations
/ price_forecasts / disease_detections  ← stored as history
```

Extension Officers can later review and override this data.

---

## 📁 Folder Map

```
app/Models/                 28 Eloquent models — one per migration
app/Http/Controllers/       Auth, Farmer, ExtensionOfficer, Supplier, Admin
app/Http/Middleware/        EnsureRole.php  (RBAC)
app/Services/MlService.php  HTTP client for the Flask ML microservice
database/migrations/        28 migration files for 26 tables, in FK-safe order
database/seeders/           DatabaseSeeder.php — zones, crops, demo users
routes/web.php               All routes, grouped by role
resources/views/             Blade templates (Bengali UI), grouped by role
public/css/app.css           Styling
public/js/app.js             Axios helper functions
```

---

<div align="center">

🔗 **Live App:** [agriadvisory.app](http://agriadvisory.app)  ·  📦 **Repo:** [Agri_Advisory](https://github.com/Marjan15H/Agri_Advisory)

Made with for Bangladeshi Farmers

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:1B5E20,100:2E7D32&height=100&section=footer" width="100%"/>

</div>
