# Frontend (React + Bootstrap)

> Aşama 4 (hekim paneli) ve Aşama 5 (tahlil yükleme sayfası) kapsamında geliştirilecek.

## Rotalar

| Rota | Erişim | İçerik |
| --- | --- | --- |
| `/giris` | herkes | Hekim / yönetici girişi |
| `/hekim` | PHYSICIAN | Bekleyen vakalar listesi |
| `/hekim/vaka/:id` | PHYSICIAN | 3 modül: Objektif Veri Paneli · Yapay Zeka Karar ve Gerekçe Paneli · Onay ve Aksiyon Paneli |
| `/yonetici` | ADMIN | Onay/override oranları, denetim kaydı |
| `/tahlil-yukle` | herkes | Bilgilendirme modülü — klinik panelden ayrı, uyarı + onay kutusu zorunlu |

## Tasarım Kuralları

- Gerekçe paneli onay butonuyla **aynı ekranda** görünür.
- `MANUAL_REVIEW` vakalarda onay için gerekçe kodu seçimi zorunludur.
- Tahlil yükleme sayfasında yapay zeka uyarısı yüklemeden önce **ve** her sonuç ekranında görünür; tanı dili kullanılmaz ("başvurabileceğiniz bölümler").

## Kurulum (Aşama 4'te)

```bash
npm create vite@latest . -- --template react-ts
npm i bootstrap react-router-dom
```
