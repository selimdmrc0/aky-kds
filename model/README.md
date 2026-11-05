# Model (Python)

| Klasör | İçerik | Aşama |
| --- | --- | --- |
| `data/` | Ham ve işlenmiş veri (git'e girmez) — [data/README.md](data/README.md) | 2 |
| `notebooks/` | Keşifsel analiz (sınıf dağılımı, eksik veri, tahlil dağılımları) | 2 |
| `src/` | Veri hazırlama, etiketleme, eğitim, kalibrasyon, SHAP, değerlendirme | 2–3 |
| `service/` | FastAPI tahmin servisi — [sözleşme](../docs/model-servisi-sozlesmesi.md) | 3 |
| `artifacts/` | Eğitilmiş model dosyaları (git'e girmez) | 3 |

## Ortam

```bash
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

> Not: XGBoost/SHAP için Python 3.11 veya 3.12 önerilir; en yeni Python sürümlerinde hazır paket (wheel) bulunmayabilir.

## Kurallar

- Veri bölme **hasta (SEQN) bazında**; test kümesine model seçiminde dokunulmaz.
- Sentetik veride elde edilen doğruluk model başarısı olarak raporlanmaz.
- Rastgelelik için sabit `random_state = 42`.
