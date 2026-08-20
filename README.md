<div align="center">

# 🌾 Smart Agri-Advisory Platform 🌾
### AI-চালিত ফসল ও ইনপুট সুপারিশ সিস্টেম | বাংলাদেশি কৃষকদের জন্য

**একটি Role-Based (RBAC) ওয়েব প্ল্যাটফর্ম যা মাটি, আবহাওয়া ও ঐতিহাসিক ডেটা বিশ্লেষণ করে**
**ফসল, সার, বাজার-সময় এবং রোগ শনাক্তকরণের সুপারিশ দেয়**

🔗 **Live:** [agriadvisory.app](http://agriadvisory.app)

![Laravel](https://img.shields.io/badge/Laravel-11-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-ML%20Service-000000?style=for-the-badge&logo=flask&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-26%20Tables-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![Azure](https://img.shields.io/badge/Deployed%20on-Azure-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)
![License](https://img.shields.io/badge/License-Academic-9C27B0?style=for-the-badge)

</div>

---

## ✨ কেন এই প্রজেক্ট?

> বাংলাদেশের ছোট ও মাঝারি কৃষকদের জন্য সঠিক ফসল নির্বাচন, সঠিক সারের পরিমাণ আর সঠিক সময়ে বিক্রি করা — এই তিনটি সিদ্ধান্তই আয়ের সবচেয়ে বড় নির্ধারক। **Smart Agri-Advisory Platform** এই তিনটি সিদ্ধান্তকেই ডেটা-চালিত এবং স্বয়ংক্রিয় করে তোলে — একদম মাঠ পর্যায়ের কৃষক থেকে শুরু করে কৃষি সম্প্রসারণ কর্মকর্তা, সরবরাহকারী এবং অ্যাডমিন পর্যন্ত — সবার জন্য একটি সমন্বিত সিস্টেমে।

<table align="center">
<tr>
<td align="center">🌱<br><b>ফসল সুপারিশ</b><br><sub>Random Forest · ~৮৩% নির্ভুলতা</sub></td>
<td align="center">🧪<br><b>সার সুপারিশ</b><br><sub>Rule-based + k-NN</sub></td>
<td align="center">📈<br><b>বাজার পূর্বাভাস</b><br><sub>Sequence / LSTM Model</sub></td>
<td align="center">🍃<br><b>রোগ শনাক্তকরণ</b><br><sub>CNN (Scaffolded)</sub></td>
</tr>
</table>

---

## 🏗️ আর্কিটেকচার

```
┌────────────────────────────┐         HTTP / Axios        ┌──────────────────────────────┐
│   🖥️  Laravel 11 + Blade     │ ───────────────────────────▶ │   🧠  Flask ML Microservice   │
│   RBAC · MySQL · Bengali UI  │ ◀─────────────────────────── │   Random Forest · k-NN · LSTM │
│   laravel-backend/           │            JSON              │   ml-service/                 │
└────────────────────────────┘                              └──────────────────────────────┘
```

| স্তর | প্রযুক্তি | দায়িত্ব |
|---|---|---|
| 🌐 **Web App** | Laravel 11, Blade, MySQL | RBAC, ফার্ম প্রোফাইল (মাটির pH/N/P/K), সুপারিশ ম্যানেজমেন্ট, অ্যাডমিন প্যানেল |
| 🤖 **ML Service** | Python, Flask, scikit-learn | ফসল/সার প্রেডিকশন, মূল্য পূর্বাভাস, রোগ শনাক্তকরণ API |
| ☁️ **Hosting** | Azure App Service | দুটি সার্ভিসই আলাদাভাবে ডিপ্লয়েড, Custom domain + SSL |

---

## 🚀 কুইক স্টার্ট

### ১️⃣ ML Service চালু করা

```bash
cd ml-service
python3 -m venv venv && source venv/bin/activate
pip install -r requirements.txt

python generate_data.py         # 🔄 সিন্থেটিক ডেটাসেট তৈরি
python train_crop_model.py      # 🌾 Random Forest ট্রেইন
python price_forecast.py        # 📊 মূল্য মডেল ট্রেইন

python app.py                   # ➜ http://localhost:5000
```

### ২️⃣ Laravel App চালু করা

```bash
cd laravel-backend
composer install
cp .env.example .env
php artisan key:generate
php artisan migrate --seed

php artisan serve               # ➜ http://localhost:8000
```

> 📖 সিডেড ডেমো অ্যাকাউন্ট এবং সম্পূর্ণ schema-to-code ম্যাপের জন্য দেখুন → `laravel-backend/README.md`

### ৩️⃣ ফিচারগুলো এভাবে টেস্ট করুন

```
👤 ফার্ম প্রোফাইল যোগ করুন (জোন + মাটির pH/N/P/K)
        ↓
🌾 ফসল সুপারিশ নিন
        ↓
🧪 ওই সুপারিশ আইডি দিয়ে সার সুপারিশ নিন
        ↓
📈 বাজার মূল্যের পূর্বাভাস দেখুন
        ↓
👨‍🌾 Extension Officer হিসেবে যাচাই / override / পরামর্শ দিন
        ↓
🛡️ Admin হিসেবে পেন্ডিং অ্যাকাউন্ট অনুমোদন ও অ্যানালিটিক্স দেখুন
```

---

## 📦 কী আছে প্রজেক্টে (26-Table Normalized Schema)

<details>
<summary><b>🗂️ মূল মডিউলগুলো দেখতে ক্লিক করুন</b></summary>

| মডিউল | বিবরণ |
|---|---|
| `users` / RBAC | কৃষক, Extension Officer, Supplier, Admin — চার ধরনের রোল |
| `climate_zones` / `crops` | জলবায়ু অঞ্চল ও ফসলের মাস্টার ডেটা |
| `farm_profiles` | কৃষকের জমির তথ্য — মাটির pH, N, P, K সরাসরি টেবিলে |
| `recommendations` | ফসল, সার, মূল্য সংক্রান্ত সব সুপারিশের লগ |
| ফার্টিলাইজার / প্রাইস / পেস্ট হিস্ট্রি | ঐতিহাসিক ডেটা ট্র্যাকিং |
| Extension Officer টুলস | verification, override, advisory, alerts, training |
| Supplier মডিউল | products, orders, inquiries |
| Admin প্যানেল | master-data ম্যানেজমেন্ট, retrain ট্রিগার, backup, অ্যানালিটিক্স |

</details>

---

## ✅ কী সত্যিকারভাবে কাজ করছে vs 🚧 কী এখনো স্ক্যাফোল্ড

| স্ট্যাটাস | কম্পোনেন্ট | বিস্তারিত |
|:---:|---|---|
| ✅ **রেডি ও টেস্টেড** | ফসল Random Forest মডেল | সিন্থেটিক ডেটায় ~৮৩% নির্ভুলতা |
| ✅ **রেডি ও টেস্টেড** | সার সুপারিশ ইঞ্জিন | Rule-based + k-NN |
| ✅ **রেডি ও টেস্টেড** | মূল্য পূর্বাভাস মডেল | Sequence model, curl দিয়ে end-to-end টেস্টেড |
| 🚧 **স্ক্যাফোল্ড** | পেস্ট/ডিজিজ CNN | রিয়েল লিফ ইমেজ ডেটা দরকার |
| 🚧 **স্ক্যাফোল্ড** | Keras LSTM স্ক্রিপ্ট | `pip install tensorflow` প্রয়োজন |
| ⚙️ **লোকাল সেটআপ প্রয়োজন** | Laravel backend | `composer install` — sandbox থেকে Packagist অ্যাক্সেসযোগ্য নয় |

---

## 🎯 পরবর্তী ধাপ (Next Steps)

- [ ] `ml-service/data/crop_data.csv` কে রিয়েল BARI ডেটা দিয়ে প্রতিস্থাপন করে `train_crop_model.py` পুনরায় চালান
- [ ] একটি ছোট leaf-disease ইমেজ সেট সংগ্রহ করে `pest_cnn.py --train` চালান
- [ ] `.env`-এ `OPENWEATHER_API_KEY` যুক্ত করে `weather_logs`-এ শিডিউল অনুযায়ী রিয়েল আবহাওয়া ডেটা পপুলেট করুন
- [ ] গ্রেডিং-এর জন্য কন্ট্রোলারগুলোতে `php artisan make:test` কভারেজ যোগ করুন

---

<div align="center">

### 🌍 Live Deployment
**🔗 [agriadvisory.app](http://agriadvisory.app)**

Made with 💚 for Bangladeshi Farmers | CSE347 Project

![Bangladesh](https://img.shields.io/badge/Made%20for-🇧🇩%20Bangladesh-006A4E?style=for-the-badge)

</div>
