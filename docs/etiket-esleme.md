# Beyan Edilen Tanı → Uzmanlık Grubu Eşleme Tablosu (Taslak v1)

> Kaynak dosya: [etiket-esleme-v1.xlsx](etiket-esleme-v1.xlsx) (hekim doğrulama sayfası dahil). Bu dosya Excel'in okunabilir kopyasıdır; çelişki olursa Excel esas alınır.
>
> **Durum: DOĞRULANMADI** — 8 satır hekim / tıp öğrencisi doğrulaması bekliyor.

**Veri:** NHANES 2017–Mart 2020 (pre-pandemic) birleşik dosyaları, yalnızca yetişkinler (≥20 yaş). Etiketler hekim tanısı değil, kişi beyanıdır.

| # | Sınıf (kod) | Dosya | Değişken | Etiket kuralı | Beklenen tahlil sinyali | Not / risk |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | Hematoloji (`HEMATOLOGY`) | P_MCQ | MCQ053 | MCQ053 = 1 (son 3 ayda anemi tedavisi) | Hb, Hct, MCV, ferritin | Tanı değil tedavi beyanı; kan sayımı etiketi doğrudan yansıttığı için başarı yüksek çıkabilir |
| 2 | Endokrinoloji (`ENDOCRINOLOGY`) | P_DIQ | DIQ010 | DIQ010 = 1 (3 = prediyabet ayrı tutulur) | Glukoz, HbA1c (P_GHB) | Prediyabet v1'de dışlanır |
| 3 | Endokrinoloji — tiroid (isteğe bağlı) | P_MCQ | MCQ160m | MCQ160m = 1 (**v1'de dahil edilmez**) | TSH/fT4 (yok) | Ayırt edici tahlil yok; sınırlılık olarak raporlanır |
| 4 | Nefroloji (`NEPHROLOGY`) | P_KIQ_U | KIQ022 | KIQ022 = 1 | Kreatinin, BUN, eGFR, ürik asit | Yalnızca ≥20 yaş |
| 5 | Gastroenteroloji / Hepatoloji (`GASTROENTEROLOGY`) | P_MCQ | MCQ160l | MCQ160l = 1 | ALT, AST, ALP, GGT, bilirubin, albümin | Safra taşı (MCQ550) dahil değil |
| 6 | Kardiyoloji (`CARDIOLOGY`) | P_MCQ | MCQ160b/c/d/e | herhangi biri = 1 | Lipid paneli, kreatinin, glukoz (dolaylı) | Sinyal dolaylı; başarı düşük çıkabilir |
| 7 | Göğüs Hastalıkları (`PULMONOLOGY`, isteğe bağlı) | P_MCQ | MCQ010+MCQ035, MCQ160p | (MCQ010=1 VE MCQ035=1) VEYA MCQ160p=1 | Eozinofil, WBC (zayıf) | Sınıf azaltılacaksa ilk çıkarılacak |
| 8 | Sevk gerekmiyor (`NO_REFERRAL`) | birden çok | – | 1–7'deki tüm koşullar = 0 ve kanser/inme/artrit beyanı yok | – | Etiket yalnızca beyana göre; tahlil normalliğine göre DEĞİL |

## Genel Kurallar

1. Yalnızca yaş ≥ 20.
2. Cevap 7 (Refused) / 9 (Don't know) → ilgili beyan belirsiz, kişi eğitimden çıkarılır.
3. Çoklu sınıfa giren kişiler v1'de çıkarılır; sayı yetersizse öncelik sırası kuralına geçilir (Aşama 2'de karar).
4. Kanser (MCQ220), inme (MCQ160f), artrit (MCQ160a) beyanı olanlar hem sınıflardan hem `NO_REFERRAL`'dan çıkarılır.
5. Tahliller P_CBC, P_BIOPRO, P_GHB, P_TCHOL, P_HDL, P_TRIGLY dosyalarından SEQN ile birleştirilir.
6. Değişken kodları 2017–Mart 2020 dosyalarından doğrulanmıştır; döngü değişirse yeniden doğrulanır.
