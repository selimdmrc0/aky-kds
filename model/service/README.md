# Model Servisi (FastAPI)

> Aşama 3 (Hafta 9) kapsamında yazılacak.

Giriş-çıkış sözleşmesi şimdiden sabittir: [docs/model-servisi-sozlesmesi.md](../../docs/model-servisi-sozlesmesi.md)

Uçlar: `POST /v1/predict`, `GET /v1/features`, `GET /v1/specialties`, `GET /health`

```bash
uvicorn service.main:app --reload --port 8000   # model/ klasöründen
```
