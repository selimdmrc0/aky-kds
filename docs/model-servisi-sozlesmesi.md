# Model Servisi Giriş-Çıkış Sözleşmesi (Taslak v1)

> Rapor §4: "Servisin giriş-çıkış sözleşmesi sabit tutulur, böylece veri seti veya model değişse bile diğer katmanlar etkilenmez."
> Bu belge NestJS (Klinik API + Bilgilendirme API) ile FastAPI model servisi arasındaki sözleşmedir. Servis Aşama 3'te (Hafta 9) yazılacak; sözleşme şimdiden sabitlenir.

## `POST /v1/predict`

### İstek

```json
{
  "age": 54,
  "sex": "FEMALE",
  "labs": [
    { "code": "hgb",  "value": 9.8,  "unit": "g/dL" },
    { "code": "mcv",  "value": 71.0, "unit": "fL" },
    { "code": "glu",  "value": 96.0, "unit": "mg/dL" },
    { "code": "crea", "value": 0.8,  "unit": "mg/dL" }
  ]
}
```

| Alan | Tür | Kural |
| --- | --- | --- |
| `age` | int | 20–120 (eğitim verisi ≥20 yaş; altı için `out_of_scope` uyarısı) |
| `sex` | `"MALE"` \| `"FEMALE"` | zorunlu |
| `labs[].code` | string | `GET /v1/features` listesindeki kodlardan biri |
| `labs[].value` | number | sonlu sayı |
| `labs[].unit` | string | standart birim; farklıysa servis 422 döner (birim çevirisi backend'de) |

Eksik tahliller gönderilmez; XGBoost eksik değeri kendisi yönetir.

### Yanıt (200)

```json
{
  "model_version": "xgb-nhanes-v1.0",
  "status": "AUTO_SUGGESTION",
  "threshold": 0.75,
  "top3": [
    { "specialty": "HEMATOLOGY",    "probability": 0.81 },
    { "specialty": "NO_REFERRAL",   "probability": 0.09 },
    { "specialty": "NEPHROLOGY",    "probability": 0.04 }
  ],
  "rule_check": {
    "all_normal": false,
    "missing_required": [],
    "abnormal": [
      { "code": "hgb", "value": 9.8, "low": 12.0, "high": 16.0, "flag": "LOW" }
    ],
    "critical": []
  },
  "explanations": [
    {
      "rank": 1,
      "feature": "hgb",
      "value": 9.8,
      "shap": 1.42,
      "text": "Hemoglobin referans aralığının altında (9,8 g/dL; referans 12–16) ve bu değer Hematoloji önerisini en çok artıran özelliktir."
    }
  ],
  "warnings": []
}
```

| Alan | Açıklama |
| --- | --- |
| `status` | `AUTO_SUGGESTION` veya `MANUAL_REVIEW` (Top-1 güven < eşik **veya** normal-sonuç koşullarından biri sağlanmadı) |
| `top3[].probability` | **Kalibre edilmiş** olasılık (isotonic/Platt) |
| `rule_check.all_normal` | Kural tabanlı kontrol: zorunlu tahliller eksiksiz **ve** hepsi referans aralığında |
| `rule_check.critical` | Kritik derecede anormal değerler (bilgilendirme modülünde "acilen başvurun" uyarısı) |
| `explanations` | Yalnızca Top-1 sınıf için SHAP; `text` şablon katmanının çıktısı |
| `warnings` | ör. `out_of_scope_age`, `few_labs` |

**Normal sonuç kuralı:** Top-1 = `NO_REFERRAL` ise yalnızca `rule_check.all_normal = true` **ve** güven ≥ eşik olduğunda `AUTO_SUGGESTION` döner; aksi halde `MANUAL_REVIEW`.

### Hatalar

| Kod | Durum |
| --- | --- |
| 422 | Geçersiz girdi (bilinmeyen kod, birim uyuşmazlığı, sayı dışı değer) |
| 503 | Model yüklenmedi |

## `GET /v1/features`

Modelin beklediği tahlil kodları, standart birimleri ve referans aralıkları. Backend `lab_tests` / `reference_ranges` seed'ini buradan doğrular.

## `GET /v1/specialties`

Sınıf kodları ve Türkçe adları — [etiket-esleme.md](etiket-esleme.md) ve `specialties` tablosuyla aynı.

## `GET /health`

`{ "status": "ok", "model_version": "..." }`

## Sınıf Kodları (v1)

| Kod | Ad | Not |
| --- | --- | --- |
| `HEMATOLOGY` | Hematoloji | kritik |
| `ENDOCRINOLOGY` | Endokrinoloji | |
| `NEPHROLOGY` | Nefroloji | kritik |
| `GASTROENTEROLOGY` | Gastroenteroloji / Hepatoloji | |
| `CARDIOLOGY` | Kardiyoloji | kritik |
| `PULMONOLOGY` | Göğüs Hastalıkları | isteğe bağlı — veri yetersizse çıkarılır |
| `NO_REFERRAL` | Sevk gerekmiyor | `is_referral = false` |

> Tiroid (MCQ160m) v1'de ayrı sınıf değildir (bkz. eşleme tablosu satır 3).
