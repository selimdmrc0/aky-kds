# Akıllı Klinik Yönlendirme ve Karar Destek Sistemi (AKY-KDS)

> ⚠️ Bu çalışma bir **araştırma prototipidir**. Klinik kullanıma hazır değildir ve gerçek hastalarda kullanılamaz.

Laboratuvar sonuçlarındaki anormal değerleri vurgulayan, en olası üç uzmanlık bölümünü (Top-3) kalibre güven skoru ve SHAP gerekçesiyle öneren ve nihai kararı **hekim onayına** sunan klinik karar destek sistemi (CDSS) prototipi.

## Repo Yapısı

```
aky-kds/
├── docs/                     # Rapor ekleri ve tasarım belgeleri
│   ├── mimari.md             # Sistem mimarisi ve akışlar
│   ├── er-diyagrami.md       # ER diyagramı + tablo açıklamaları
│   ├── model-servisi-sozlesmesi.md  # NestJS ↔ FastAPI sözleşmesi
│   ├── etiket-esleme.md      # Tanı → uzmanlık eşleme tablosu (okunabilir kopya)
│   ├── etiket-esleme-v1.xlsx # Eşleme tablosu + hekim doğrulama sayfası
│   ├── ilerleme/             # Aşama sonu ilerleme raporları
│   └── img/
├── model/                    # Python: veri hazırlama, eğitim, model servisi
│   ├── data/                 # raw/ ve processed/ git'e girmez
│   ├── notebooks/            # Keşifsel analiz
│   ├── src/                  # Veri hazırlama, eğitim, değerlendirme kodu
│   └── service/              # FastAPI tahmin servisi (Aşama 3)
├── backend/                  # NestJS + Prisma (Aşama 4)
│   └── prisma/schema.prisma  # Veritabanı şeması (taslak v1)
├── frontend/                 # React: hekim paneli + tahlil yükleme sayfası (Aşama 4–5)
└── docker-compose.yml        # Geliştirme için PostgreSQL
```

## Teknolojiler

| Katman | Teknoloji |
| --- | --- |
| Önyüz | React, Bootstrap |
| Arka yüz | NestJS, TypeScript, Prisma |
| Veritabanı | PostgreSQL |
| Model servisi | Python, FastAPI |
| ML | XGBoost, scikit-learn, SHAP |

## Takvim

| Aşama | Hafta | Odak | Durum |
| --- | --- | --- | --- |
| 1 | 5 | Proje tanımı, veri erişimi, etiket stratejisi, mimari, ER | 🟡 Devam ediyor |
| 2 | 7 | Veri hazırlama, baseline (lojistik regresyon) | ⬜ |
| 3 | 9 | XGBoost, kalibrasyon, SHAP, FastAPI servisi | ⬜ |
| 4 | 11 | Backend, veritabanı, hekim paneli | ⬜ |
| 5 | 13 | Tahlil yükleme modülü, entegrasyon, test | ⬜ |
| 6 | 14 | Final değerlendirme ve gösterim | ⬜ |

## Veri

Ana veri seti: **NHANES 2017–Mart 2020** (açık erişim). MIMIC-IV erişimi sağlanırsa değerlendirilir; MIMIC-IV verisi ve türevleri **hiçbir koşulda** bu repoya veya harici yapay zeka servislerine yüklenmez (PhysioNet DUA). Ayrıntılar: [model/data/README.md](model/data/README.md).

## Geliştirme (hızlı başlangıç)

```bash
# PostgreSQL
docker compose up -d db

# Model ortamı
cd model
python -m venv .venv
.venv\Scripts\activate        # Windows
pip install -r requirements.txt
```

Backend ve frontend kurulumu ilgili aşamalarda eklenecek (bkz. klasör README'leri).
