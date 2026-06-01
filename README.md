# Akıllı Müşteri Yönetim ve Analiz Sistemi

> Python Programlama Dönem Ödevi — Telco Senaryosu | Google Colab


## Hakkında

Bu proje, Python'un temel veri yapıları ve kontrol akışı mantığını gerçekçi bir Telco (telekomünikasyon) senaryosu üzerinden öğretmek amacıyla hazırlanmıştır. Sistem; müşteri kaydı, VIP tespiti, KDV'li fatura hesaplama ve churn (müşteri kaybı) risk analizi işlevlerini kapsamaktadır.

---

## İçindekiler

- [Kısım 1 — Veri Yapıları ve Temel Mantık](#kısım-1--veri-yapıları-ve-temel-mantık-hafta-1)
- [Kısım 2 — Fonksiyonlar, Döngüler ve Kütüphaneler](#kısım-2--fonksiyonlar-döngüler-ve-kütüphaneler-hafta-2)
- [Kullanılan Teknolojiler](#kullanılan-teknolojiler)
- [Teslim Edilecekler](#teslim-edilecekler)

---

## Kısım 1 — Veri Yapıları ve Temel Mantık (Hafta 1)

**Amaç:** Tek bir müşterinin verilerini Python veri yapılarıyla temsil etmek ve basit kararlar verdirmek.

### Görevler

**Görev 1 — Değişkenler ve Tipler**

Bir müşterinin aşağıdaki bilgilerini uygun veri tipleriyle tanımla:
- `ad_soyad` → `str`
- `aylik_ucret` → `float`
- `sadakat_ayi` → `int`
- `aktif_mi` → `bool`

**Görev 2 — Liste ve Sözlük Kullanımı**

Şirketin sunduğu 5 farklı hizmeti bir `list`'e, müşterinin tüm bilgilerini bir `dict`'e kaydet.

**Görev 3 — Koşullu İfadeler (If-Else)**

`aylik_ucret > 500` veya `sadakat_ayi > 24` koşulu sağlanıyorsa `"VIP Müşteri: İndirim Tanımla"`, aksi halde `"Standart Müşteri"` yazdır.

**Görev 4 — String Operasyonları ve Random ID**

Müşteri adını büyük harfe çevir, `IST-2026-XXXX` formatında `random` kütüphanesiyle benzersiz bir ID üret.

### Kritik Soru

> Müşteri bilgilerini neden `'Liste'` yerine `'Sözlük'` içinde saklamayı tercih ettik? Avantajı nedir?

---

## Kısım 2 — Fonksiyonlar, Döngüler ve Kütüphaneler (Hafta 2)

**Amaç:** Birden fazla müşteriyi yöneten ve analiz yapan bir sistem kurmak.

### Görevler

**Görev 1 — Ana Müşteri Listesi**

Her biri sözlük (`dict`) yapısında olan 5 farklı müşteriden oluşan `musteriler_listesi` adlı ana listeyi oluştur. Her müşteride şu alanlar bulunmalı: `ad_soyad`, `aylik_ucret`, `sadakat_ayi`, `aktif_mi`, `sehir`, `hizmetler`.

**Görev 2 — `tutar_hesapla()` Fonksiyonu**

`aylik_ucret` parametresi alan, üzerine %20 KDV ekleyip sonucu `return` ile döndüren bir fonksiyon yaz.

```python
def tutar_hesapla(aylik_ucret):
    kdv_orani = 0.20
    return aylik_ucret * (1 + kdv_orani)
```

**Görev 3 — Kütüphane Kullanımı**

- `math.ceil()` ile KDV'li tutarı yukarıya yuvarla
- `datetime.datetime.now()` ile fatura çıktısının altına günün tarihini ekle

**Görev 4 — Hata Denetimi ve Set Kullanımı**

Tüm müşterilerin hizmetlerini (tekrarlı) bir listeye topla, ardından `set()` ile benzersiz hizmet listesini elde et. Olası hataları `try-except` bloğuyla yakala.

**Görev 5 — Churn Analizi (AI Denetimli Kod)**

Kodu yapay zekaya yazdır, hatalı kısımları düzelt ve her satıra `# konu_adı` yorumu ekle (örn. `# Döngü Kullanımı`, `# Koşullu İfade`).

Churn tespiti için önerilen risk puanı mantığı:

| Kriter | Puan |
|---|---|
| `sadakat_ayi < 6` | +2 |
| `aktif_mi == False` | +3 |
| `aylik_ucret < 200` | +1 |

Toplam puan `>= 2` ise müşteri **Churn Riskli** olarak işaretlenir.

### Kritik Soru

> For döngüsü müşteri listesini gezerken, hangi müşterilerin Churn (ayrılma) riskinde olduğunu nasıl tespit ettiniz? Mantığınızı açıklayın.

---

## Kullanılan Teknolojiler

- `Python 3`
- `math` — fatura tutarı yuvarlama
- `datetime` — fatura tarihi
- `random` — benzersiz müşteri ID üretimi
- Google Colab — çalışma ortamı

---

## Teslim Edilecekler

### Kısım 1
- [ ] Colab linki veya PDF çıktısı
- [ ] Kritik Soru yazılı cevabı

### Kısım 2
- [ ] Fonksiyonları ve döngüleri içeren tam Python kodu
- [ ] Kritik Soru yazılı cevabı

---

*Karadeniz Teknik Üniversitesi · Bilgisayar Programcılığı · 2025–2026*
