# Backend (NestJS + Prisma)

> Aşama 4 (Hafta 11) kapsamında geliştirilecek. Hafta 5'te yalnızca veritabanı şeması taslağı hazırdır.

## İçerik

| Yol | Durum |
| --- | --- |
| `prisma/schema.prisma` | ✅ Taslak v1 (doğrulandı: `prisma validate`) — açıklama: [docs/er-diyagrami.md](../docs/er-diyagrami.md) |
| `.env.example` | ✅ |
| NestJS uygulaması | ⬜ Aşama 4 |

## Planlanan Modüller

| Modül | Görev |
| --- | --- |
| `auth` | JWT giriş, rol guard'ı (PHYSICIAN, ADMIN) |
| `cases` | Vaka ve tahlil listesi/detayı, referans aralığı kontrolü (`flag`) |
| `predictions` | Model servisi istemcisi (`POST /v1/predict`) — [sözleşme](../docs/model-servisi-sozlesmesi.md) |
| `decisions` | Onay / override / red, gerekçe kodu zorunluluğu |
| `notifications` | SMS simülasyonu (nötr şablonlar) |
| `audit` | Append-only, hash zincirli denetim kaydı |
| `public` | Bilgilendirme API (tahlil yükleme) — **veritabanına yazmaz**, rate limit, dosya kontrolü |

## Kurulum (Aşama 4'te)

```bash
npx @nestjs/cli new . --package-manager npm --skip-git
npm i prisma -D && npm i @prisma/client
cp .env.example .env
npx prisma migrate dev --name init
```

Şemayı şimdiden doğrulamak için:

```bash
npx prisma@6 validate --schema prisma/schema.prisma
```
