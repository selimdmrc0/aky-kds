# Veri

> `raw/` ve `processed/` klasörlerinin içeriği **git'e girmez** (bkz. `.gitignore`).

## Ana veri seti: NHANES 2017–Mart 2020 (pre-pandemic)

Kaynak: https://wwwn.cdc.gov/nchs/nhanes/continuousnhanes/default.aspx?Cycle=2017-2020

`raw/` klasörüne indirilecek `.XPT` dosyaları:

| Dosya | İçerik | Kullanım |
| --- | --- | --- |
| P_DEMO | Demografi (yaş, cinsiyet) | özellik + ≥20 yaş filtresi |
| P_MCQ | Tıbbi durumlar | etiket (anemi, karaciğer, kalp, tiroid, astım/KOAH, dışlama) |
| P_DIQ | Diyabet | etiket |
| P_KIQ_U | Böbrek | etiket |
| P_CBC | Tam kan sayımı | özellik |
| P_BIOPRO | Biyokimya profili | özellik |
| P_GHB | HbA1c | özellik |
| P_TCHOL, P_HDL, P_TRIGLY | Lipid paneli | özellik |

Etiket kuralları: [docs/etiket-esleme.md](../../docs/etiket-esleme.md)

## Geliştirme verisi: MIMIC-IV Demo

Yalnızca şema/pipeline testi içindir; başarı ölçümü için kullanılmaz.

## ⚠️ MIMIC-IV (erişim gelirse)

PhysioNet veri kullanım sözleşmesi gereği MIMIC-IV verisi ve türevleri **bu repoya commit edilmez ve harici yapay zeka servislerine (ChatGPT, Claude, Gemini vb.) yüklenmez.** Yalnızca yerel ortamda işlenir.
