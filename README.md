# Akıllı Müşteri Yönetim ve Analiz Sistemi

**Python Programlama Dönem Ödevi**  
Telco Senaryosu | Google Colab

---

## Proje Hakkında

Bu proje, bir telekomünikasyon (Telco) şirketinin müşterilerini Python ile yönetmek ve analiz etmek amacıyla geliştirilmiştir. Temel Python veri yapıları ve kontrol mekanizmaları kullanılarak; VIP müşteri tespiti, KDV'li fatura hesaplama, benzersiz hizmet listeleme ve çok kriterli Churn (müşteri kaybı) risk analizi gerçekleştirilmiştir.

---

## Ödev Yapısı

Ödev iki ana kısımdan oluşmaktadır. Her kısım, Python'un farklı temel konularını kapsamaktadır.

---

## 1. Kısım — Veri Yapıları ve Temel Mantık

### Amaç

Tek bir müşterinin verilerini Python veri yapılarıyla temsil etmek ve basit kararlar verdirmek.

### Yapılan İşlemler

**Görev 1 — Değişkenler ve Tipler**

Bir müşteriye ait dört temel bilgi, uygun Python veri tipleriyle tanımlanmıştır:

| Değişken | Tip | Örnek Değer |
|---|---|---|
| `ad_soyad` | `str` | `"Ahmet Yılmaz"` |
| `aylik_ucret` | `float` | `650.0` |
| `sadakat_ayi` | `int` | `30` |
| `aktif_mi` | `bool` | `True` |

**Görev 2 — Liste ve Sözlük Kullanımı**

Şirketin sunduğu 5 hizmet bir `list` içinde, müşterinin tüm bilgileri ise bir `dict` (sözlük) yapısında saklanmıştır:

```python
hizmetler_listesi = ["Fiber Internet", "Dijital TV", "Mobil Hat", "Bulut Depolama", "Siber Güvenlik"]

musteri = {
    "ad_soyad"    : "Ahmet Yılmaz",
    "aylik_ucret" : 650.0,
    "sadakat_ayi" : 30,
    "aktif_mi"    : True,
    "hizmetler"   : hizmetler_listesi
}
```

**Görev 3 — Koşullu İfadeler (If-Else)**

Aylık ücret 500 TL'yi aşıyorsa **veya** sadakat süresi 24 ayı geçiyorsa müşteri VIP olarak sınıflandırılır:

```python
if musteri["aylik_ucret"] > 500 or musteri["sadakat_ayi"] > 24:
    print("VIP Müşteri: İndirim Tanımla")
else:
    print("Standart Müşteri")

# Çıktı: VIP Müşteri: İndirim Tanımla
```

**Görev 4 — String Operasyonları ve Rastgele ID Üretimi**

`random` kütüphanesi ile `IST-2026-XXXX` formatında benzersiz bir müşteri ID'si üretilmiştir:

```python
import random

ad_buyuk   = musteri["ad_soyad"].upper()               # "AHMET YILMAZ"
musteri_id = f"IST-2026-{random.randint(1000, 9999)}"  # "IST-2026-4823"
musteri["musteri_id"] = musteri_id
```

### Kritik Soru — Neden Sözlük?

> **Soru:** Müşteri bilgilerini neden Liste yerine Sözlük içinde saklamayı tercih ettik?

Listelerde verilere indeks numarasıyla (örn. `musteri[0]`) erişilir; bu yaklaşım hangi indeksin hangi bilgiyi tuttuğunu hatırlamayı güçleştirir. Sözlükte ise anlamlı anahtarlarla erişim sağlanır: `musteri["ad_soyad"]`, `musteri["aylik_ucret"]`. Başlıca avantajlar şunlardır:

1. Okunabilirlik artar — kod kendini açıklar.
2. Yeni alan eklemek diğer alanların sırasını bozmaz.
3. Anahtar bilindiğinde erişim O(1) hızındadır.
4. Yanlış indeks sırasından kaynaklı hatalar önlenir.

---

## 2. Kısım — Fonksiyonlar, Döngüler ve Kütüphaneler

### Amaç

Birden fazla müşteriyi yöneten ve analiz yapan bir sistem kurmak.

### Müşteri Verileri

| Müşteri Adı | Aylık Ücret | Sadakat Ayı | Aktif Mi |
|---|---|---|---|
| Ahmet Yılmaz | 650.0 TL | 30 ay | Evet |
| Fatma Kaya | 299.0 TL | 6 ay | Evet |
| Mehmet Demir | 520.5 TL | 18 ay | Hayır |
| Zeynep Arslan | 180.0 TL | 3 ay | Evet |
| Can Öztürk | 890.0 TL | 48 ay | Evet |

### Yapılan İşlemler

**Görev 2 — Fonksiyon Tanımlama: `tutar_hesapla()`**

%20 KDV ekleyerek brüt fatura tutarını döndüren fonksiyon:

```python
def tutar_hesapla(aylik_ucret):   # Fonksiyon tanımlama
    kdv_orani      = 0.20
    kdv_tutari     = aylik_ucret * kdv_orani
    kdv_dahil_tutar = aylik_ucret + kdv_tutari
    return kdv_dahil_tutar        # Return ile döndür

# Örnek: tutar_hesapla(500) -> 600.0
```

**Görev 3 — Kütüphane Kullanımı: `math` ve `datetime`**

Fatura tutarı `math.ceil()` ile yukarıya yuvarlanmış, `datetime` ile tarihe eklenmiştir:

```python
import math, datetime

for musteri in musteriler_listesi:
    kdv_dahil    = tutar_hesapla(musteri["aylik_ucret"])
    yuvarlanmis  = math.ceil(kdv_dahil)                   # math kütüphanesi
    bugun        = datetime.datetime.now()
    tarih_format = bugun.strftime("%d/%m/%Y %H:%M")       # datetime kütüphanesi
```

**Görev 4 — Hata Denetimi ve Set Kullanımı**

Tüm hizmetler tekrarlı listeye eklenmiş, `set()` ile benzersiz küme oluşturulmuş, `try-except` ile olası hatalar yakalanmıştır:

```python
tum_hizmetler = []
for m in musteriler_listesi:
    for h in m["hizmetler"]:
        tum_hizmetler.append(h)     # Tekrarlı liste

benzersiz = set(tum_hizmetler)      # set() ile benzersizler

try:
    deger = int("abc")              # Hata oluşturma
except ValueError as hata:
    print(f"Hata: {hata}")          # try-except ile yakalama
```

**Görev 5 — Churn Analizi (AI Denetimli Kod)**

```python
def churn_analizi(musteriler):              # Fonksiyon tanımlama
    churn_riskli = []                       # Liste kullanımı
    guvenli      = []

    for musteri in musteriler:             # Döngü kullanımı
        risk_puani = 0                     # Değişken tanımlama
        risk_sebebi = []

        if musteri["sadakat_ayi"] < 6:     # Koşullu ifade
            risk_puani += 2                # Aritmetik işlem
            risk_sebebi.append("Düşük sadakat")  # Liste metodu

        if not musteri["aktif_mi"]:        # Boolean kontrolü
            risk_puani += 3
            risk_sebebi.append("Hesap pasif")

        if musteri["aylik_ucret"] < 200:   # Float karşılaştırma
            risk_puani += 1
            risk_sebebi.append("Düşük ücret")

        if risk_puani >= 2:                # Koşullu ifade — risk eşiği
            churn_riskli.append({"ad": musteri["ad_soyad"], "puan": risk_puani})
        else:
            guvenli.append(musteri["ad_soyad"])

    return churn_riskli, guvenli           # Return — Tuple dönüş
```

### Churn Analizi Sonuçları

| Müşteri Adı | Risk Durumu | Sebepler |
|---|---|---|
| Ahmet Yılmaz | Güvenli | — |
| Fatma Kaya | Güvenli | — |
| Mehmet Demir | RİSKLİ | Hesap pasif |
| Zeynep Arslan | RİSKLİ | Sadakat < 6 ay, Düşük ücret |
| Can Öztürk | Güvenli | — |

### Kritik Soru — Churn Tespiti Mantığı

> **Soru:** Döngü müşteri listesini gezerken, hangi müşterilerin Churn riskinde olduğunu nasıl tespit ettiniz?

Churn tespiti için ağırlıklı risk puanı mantığı kullanıldı. `for` döngüsü her müşteri için sırasıyla üç kriteri kontrol eder:

1. `sadakat_ayi < 6` ise **+2 puan** — Müşteri henüz yeterince bağlanmamış, rakibe geçme olasılığı yüksektir.
2. `aktif_mi == False` ise **+3 puan** — Müşteri zaten hizmeti kullanmıyor; bu en güçlü ayrılma sinyalidir.
3. `aylik_ucret < 200` ise **+1 puan** — Az hizmet alan müşteri, ek bir sorunla birlikte kolayca ayrılabilir.

Risk puanı **2 veya üzeri** olan müşteriler `churn_riskli` listesine eklenir. Bu yöntem tek kritere değil, birden fazla kriterin bir araya gelmesiyle daha isabetli bir tespit yapar.

---

## Kullanılan Teknolojiler ve Kütüphaneler

| Kütüphane | Kullanım Amacı |
|---|---|
| `random` | Rastgele müşteri ID üretimi |
| `math` | Fatura tutarını yukarıya yuvarlama (`math.ceil`) |
| `datetime` | Fatura tarih/saat damgası ekleme |

---

## Kullanılan Python Kavramları

- Değişkenler ve veri tipleri (`str`, `float`, `int`, `bool`)
- Liste (`list`), Sözlük (`dict`), Küme (`set`), Demet (`tuple`)
- Koşullu ifadeler (`if-else`)
- Döngüler (`for`)
- Fonksiyon tanımlama ve `return` kullanımı
- Hata yönetimi (`try-except`)
- String metotları (`.upper()`, `f-string`)
- Standart kütüphaneler (`random`, `math`, `datetime`)

---

## Çalıştırma

Proje Google Colab ortamında geliştirilmiştir. Çalıştırmak için:

1. `musteri_yonetim_sistemi.ipynb` dosyasını Google Colab'a yükleyin.
2. Hücreleri sırasıyla çalıştırın (`Çalışma Zamanı > Tümünü Çalıştır`).
3. Ek kütüphane kurulumu gerekmez; kullanılan tüm kütüphaneler Python standart kütüphanesine aittir.

---

## Proje Dosyaları

```
├── musteri_yonetim_sistemi.ipynb   # Ana notebook dosyası
└── README.md                       # Bu dosya
```

---

## Notlar

- Ödev en fazla 3 kişilik gruplar halinde yapılabilir.
- Ödev kontrolleri dönemin son 2 haftasında sınıfta gerçekleştirilecektir.
- 2. Kısım Görev 5 kapsamında yapay zeka tarafından üretilen kod, hatalı noktalar düzeltilerek ve her satıra konu yorumu eklenerek teslim edilmiştir.
