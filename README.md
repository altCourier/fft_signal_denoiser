# sdr-fft-denoiser

HackRF One ile kaydedilen ham IQ sinyallerinden gürültü temizleme aracı.

Şu an offline WAV işleme desteklenmektedir; APT (NOAA uydu) ve ADS-B (1090 MHz) sinyalleri için optimize edilmiştir.

> İleride gerçek zamanlı akış ve Python kütüphanesi olarak paketleme planlanmaktadır.

---

## Ne Yapar?

Kaydedilmiş bir WAV dosyasını alır, FFT tabanlı denoising uygular ve temizlenmiş sinyali yeniden yazar. İşlem öncesi ve sonrasına ait spektrum karşılaştırması görsel olarak üretilir.

Desteklenen sinyal tipleri:

- **APT** NOAA 15/18/19 hava durumu uyduları (137 MHz bandı)
- **ADS-B** Uçak transponder sinyalleri (1090 MHz bandı)

---

## Klasör Yapısı

```
sdr-fft-denoiser/
│
├── data/
│   ├── raw/              # Ham WAV dosyaları
│   └── processed/        # Denoising sonrası çıktılar
│
├── src/
│   ├── __init__.py
│   ├── io.py             # WAV okuma/yazma
│   ├── fft.py            # FFT + denoising algoritmaları
│   ├── filters.py        # Wiener filtresi, spectral subtraction
│   └── visualize.py      # Before/after spektrum grafikleri
│
├── signals/
│   ├── apt.py            # APT parametreleri (137 MHz, 40 kHz BW)
│   └── adsb.py           # ADS-B parametreleri (1090 MHz, 2+ MSPS)
│
├── tests/
│   └── test_fft.py
│
├── main.py               # CLI giriş noktası
├── requirements.txt
└── README.md
```

---

## Kurulum

```bash
git clone https://github.com/altCourier/sdr-fft-denoiser.git
cd sdr-fft-denoiser
pip install -r requirements.txt
```

---

## Kullanım

```bash
# APT sinyali için
python main.py --input data/raw/noaa19_recording.wav --signal apt

# ADS-B sinyali için
python main.py --input data/raw/adsb_recording.wav --signal adsb

# Çıktı klasörü belirtmek için
python main.py --input data/raw/noaa19_recording.wav --signal apt --output data/processed/
```

İşlem tamamlandığında `data/processed/` altında temizlenmiş WAV ve spektrum karşılaştırma görseli oluşturulur.

---

## Teknik Arka Plan

### Neden FFT Tabanlı Denoising?

APT ve ADS-B sinyalleri frekans domeninde belirgin yapılar içerir. APT'nin 2400 Hz sync tonu ve ADS-B'nin 1 µs PPM darbeleri, gürültüden spektral olarak ayrıştırılabilir. FFT tabanlı yöntemler bu yapıyı korurken gürültü tabanını baskılar.

### Kullanılan Yöntemler

- **Spectral Subtraction** — Gürültü tahmini çıkarma
- **Wiener Filtresi** — SNR tabanlı adaptif filtreleme

### Sinyal Parametreleri

| Sinyal | Merkez Frekans | Bant Genişliği | Örnekleme Hızı |
|--------|---------------|----------------|----------------|
| APT (NOAA) | 137 MHz | 40 kHz | 2 MSPS |
| ADS-B | 1090 MHz | ~2 MHz | 4 MSPS |

---

## Donanım

Bu proje aşağıdaki donanımla geliştirilmiş ve test edilmiştir:

- **HackRF One** RF ön uç
- **PortaPack H2 (Mayhem firmware)** Referans kayıt
- **SDR++ v1.3.0** WAV kayıt arayüzü

---

## Yol Haritası

- [x] Klasör yapısı ve proje iskeleti
- [ ] Offline WAV denoising (APT)
- [ ] Offline WAV denoising (ADS-B)
- [ ] Before/after spektrum görselleştirme
- [ ] CLI arayüzü
- [ ] Gerçek zamanlı HackRF akışı
- [ ] Python kütüphanesi olarak paketleme

---

## Katkı

Proje aktif geliştirme aşamasındadır.

Gerçek zamanlı akış modülü ve ek sinyal tipi desteği için katkı ve iş birliği önerileri memnuniyetle karşılanır.

Issue açabilir veya doğrudan PR gönderebilirsiniz.

---

## Lisans

MIT