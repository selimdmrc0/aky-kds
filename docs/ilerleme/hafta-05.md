# Aşama 1 İlerleme Raporu — Hafta 5

**Odak:** Proje tanımı, veri erişimi, etiket stratejisi, mimari ve ER şeması

## Tamamlananlar

| İş | Durum | Çıktı |
| --- | --- | --- |
| Revize proje raporu | ✅ | Proje Raporu (Revize) |
| Veri erişimi | ✅ Karar verildi | NHANES 2017–Mart 2020 ana veri seti; MIMIC-IV Demo yalnızca pipeline testi |
| ICD/beyan → uzmanlık eşleme tablosu | 🟡 Taslak hazır, hekim doğrulaması bekliyor | [etiket-esleme.md](../etiket-esleme.md), [etiket-esleme-v1.xlsx](../etiket-esleme-v1.xlsx) |
| Sistem mimarisi şeması | ✅ | [mimari.md](../mimari.md) |
| ER diyagramı / Prisma şeması taslağı | ✅ (`prisma validate` geçti) | [er-diyagrami.md](../er-diyagrami.md), [schema.prisma](../../backend/prisma/schema.prisma) |
| Model servisi sözleşmesi | ✅ (ek) | [model-servisi-sozlesmesi.md](../model-servisi-sozlesmesi.md) |
| Repo ve klasör yapısı | ✅ | `model/`, `backend/`, `frontend/`, `docs/` |

## Alınan Kararlar

- **Sınıflar (v1):** Hematoloji, Endokrinoloji (diyabet), Nefroloji, Gastroenteroloji/Hepatoloji, Kardiyoloji, Göğüs Hastalıkları (isteğe bağlı), Sevk gerekmiyor.
- **Tiroid** v1'de ayrı sınıf değil (ayırt edici tahlil yok) — sınırlılık olarak raporlanacak.
- **"Sevk gerekmiyor"** etiketi yalnızca beyana göre tanımlanır; nihai normal sonuç kararı kural tabanlı kontrol + model güveniyle verilir.
- Veritabanı yalnızca klinik akışı saklar; tahlil yükleme modülü veritabanına yazmaz.

## Riskler / Açık Konular

- Eşleme tablosu hekim tarafından doğrulanmadı; doğrulanamazsa raporda "doğrulanmadı" olarak belirtilecek.
- Çoklu tanılıların çıkarılmasından sonra sınıf başına kalan kişi sayısı Aşama 2'de kontrol edilecek; yetersizse sınıf sayısı azaltılacak (ilk aday: Göğüs Hastalıkları).
- Kardiyoloji sınıfında tahlil sinyali dolaylı; başarı düşük çıkabilir.

## Sonraki Aşama (Hafta 7)

NHANES dosyalarının indirilmesi ve birleştirilmesi, etiketleme kodu, keşifsel analiz, hasta bazlı bölme, referans aralığı kontrolü, lojistik regresyon baseline.
