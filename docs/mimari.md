# Sistem Mimarisi (Taslak v1)

> Aşama 1 / Hafta 5 teslimi. Rapor §4 ile uyumludur.

![AKY-KDS mimari şeması](img/mimari.png)

## Bileşenler

```mermaid
flowchart TD
    subgraph FE["Önyüz (React + Bootstrap) — frontend/"]
        HP["Hekim paneli<br/>/hekim — 3 modül"]
        TY["Tahlil yükleme sayfası<br/>/tahlil-yukle — ayrı rota"]
    end

    subgraph BE["Arka yüz (NestJS) — backend/"]
        KA["Klinik API<br/>JWT, rol, karar, SMS simülasyonu"]
        BA["Bilgilendirme API<br/>ayrı uç, veritabanına yazmaz"]
    end

    DB[("PostgreSQL<br/>Prisma, denetim logu")]
    MS["Model servisi (FastAPI) — model/service/<br/>Top-3, güven skoru, SHAP"]

    HP -->|"HTTPS + JWT"| KA
    TY -->|"HTTPS, rate limit"| BA
    KA --> DB
    KA -->|"POST /v1/predict"| MS
    BA -->|"POST /v1/predict"| MS
```

| Bileşen | Klasör | Teknoloji | Sorumluluk |
| --- | --- | --- | --- |
| Hekim paneli | `frontend/` | React, Bootstrap | Objektif veri paneli, yapay zeka karar ve gerekçe paneli, onay ve aksiyon paneli |
| Tahlil yükleme sayfası | `frontend/` | React | Uyarı + onay kutusu, form / CSV-PDF yükleme, değerlerin kullanıcıya onaylatılması |
| Klinik API | `backend/` | NestJS, TypeScript, Prisma | Kimlik doğrulama, rol, vaka/tahlil, model çağrısı, karar kaydı, SMS simülasyonu, denetim logu |
| Bilgilendirme API | `backend/` | NestJS (ayrı modül) | Girdi doğrulama, dosya tür/boyut kontrolü, istek sınırlama; **veritabanına yazmaz** |
| Veritabanı | `backend/prisma/` | PostgreSQL | Bkz. [er-diyagrami.md](er-diyagrami.md) |
| Model servisi | `model/service/` | Python, FastAPI, XGBoost, SHAP | Tahmin, kalibrasyon, SHAP gerekçesi. Sözleşme: [model-servisi-sozlesmesi.md](model-servisi-sozlesmesi.md) |

## Akışlar

### Klinik akış (hekim onaylı)

```mermaid
sequenceDiagram
    autonumber
    actor H as Hekim
    participant FE as Hekim paneli
    participant API as Klinik API (NestJS)
    participant MS as Model servisi (FastAPI)
    participant DB as PostgreSQL

    Note over API,DB: Tahliller sisteme düşer (prototipte sentetik seed)
    API->>DB: LabCase + LabResult (flag hesaplanır)
    API->>MS: POST /v1/predict (yaş, cinsiyet, tahliller)
    MS-->>API: Top-3, kalibre güven, SHAP gerekçesi, status
    API->>DB: Prediction + Items + Explanations, AuditLog
    H->>FE: Vakayı açar
    FE->>API: GET /cases/:id (JWT)
    API-->>FE: Anormal değerler + öneri + gerekçe
    H->>FE: Onay / farklı bölüm / red (+ gerekçe kodu)
    FE->>API: POST /cases/:id/decision
    API->>DB: PhysicianDecision, AuditLog
    API->>DB: Notification (SMS_SIMULATED, nötr metin)
```

### Bilgilendirme akışı (onaysız)

```mermaid
sequenceDiagram
    autonumber
    actor K as Kullanıcı
    participant FE as Tahlil yükleme sayfası
    participant BA as Bilgilendirme API
    participant MS as Model servisi

    FE->>K: Yapay zeka uyarısı + onay kutusu
    K->>FE: Değer girişi veya CSV/PDF
    FE->>BA: (dosya ise) POST /public/parse
    BA-->>FE: Okunan değerler + birimler
    K->>FE: Değerleri onaylar
    FE->>BA: POST /public/evaluate
    BA->>MS: POST /v1/predict
    MS-->>BA: Top-3, güven, gerekçe
    BA-->>FE: "Başvurabileceğiniz bölümler" (veri saklanmaz)
```

## Temel İlkeler

- **ML kodu NestJS içinde çalışmaz.** Model ayrı FastAPI servisidir; NestJS HTTP ile çağırır. Giriş-çıkış sözleşmesi sabittir, veri seti/model değişse de diğer katmanlar etkilenmez.
- **Hekim onayı olmadan hiçbir işlem yapılmaz**; SMS yalnızca karar sonrası ve yalnızca simüle edilir, tanı/tahlil bilgisi içermez.
- **Bilgilendirme modülü klinik akıştan ayrıdır**: ayrı rota, ayrı API ucu, denetim loguna ve hasta kayıtlarına yazmaz, veriyi varsayılan olarak saklamaz.
- **Güvenlik**: JWT + rol (PHYSICIAN, ADMIN), TLS, append-only denetim kaydı, istek sınırlama, dosya tür/boyut kontrolü.

## Geliştirme Ortamı Portları (öneri)

| Servis | Port |
| --- | --- |
| React (Vite) | 5173 |
| NestJS | 3000 |
| FastAPI | 8000 |
| PostgreSQL (docker compose) | 5432 |
