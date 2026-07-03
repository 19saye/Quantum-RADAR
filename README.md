# Quantum-RADAR
# Kuantum Aydınlatma Tabanlı Radar Simülasyon Sistemi ve Hayalet Hedef Analizi
projenin temel hedefi, hayalet uçak tespitinde kuantum radar sistemlerinin teorik önemini ve potansiyelini incelemektir.Düşük radar kesit alanına ($RCS = 0.0800 \, \text{m}^2$) sahip optimize edilmiş hayalet (stealth) askeri hava platformu, **Klasik Monostatik Radar** ile **Kuantum Aydınlatma (Quantum Illumination) Tabanlı Kuantum Radar** mimarileri altındaki tespit başarımlarını sayısal, analitik ve simülasyon temelli olarak karşılaştıran araştırma ve yazılım kütüphanesidir.
Sistem, ekstrem düşük sinyal-gürültü oranı ($SNR$) rejimlerinde kuantum dolanıklığın (quantum entanglement) ve kuantum dekoherans dinamiklerinin sinyal işleme algoritmaları ile nasıl optimize edilebileceğini modellemektedir.

---

##  Teknik Altyapı ve Matematiksel Modeller

### 1. Klasik Monostatik Radar Denklem Modeli
Klasik radarlar için hedeften dönen sinyal gücü ($P_r$), uzaklığın dördüncü kuvvetiyle ($R^4$) ters orantılı bir sönümlenmeye tabidir:

$$P_r = \frac{P_t \cdot G^2 \cdot \lambda^2 \cdot \sigma}{(4\pi)^3 \cdot R^4 \cdot L}$$

Burada:
- $P_t$: Verici gücü (Transmitter Power)
- $G$: Anten kazancı (Antenna Gain)
- $\lambda$: Çalışma dalga boyu (Wavelength)
- $\sigma$: Radar Kesit Alanı (RCS - $\text{m}^2$)
- $R$: Hedef menzili (Range - $\text{m}$)
- $L$: Sistem ve atmosferik kayıplar (Losses)

### 2. Kuantum Aydınlatma ve Korelasyon Mekanizması
Kuantum radar mimarisinde, kuantum kaynağından çıkan sinyal ($S$) ve avare ($I$ - idler) foton çiftleri üretilir. Sinyal fotonu hedefe gönderilirken, avare fotonu alıcıda referans olarak saklanır. Başlangıçtaki dolanık durumun pozisyon/momentum korelasyon matrisi ve sinyal üretimi şu şekilde modellenmiştir:

$$\text{signal} = \mathcal{N}(\mu_s, \sigma_s^2) + \text{noise}$$
$$\text{idler} = \rho \cdot \text{signal} + \mathcal{N}(\mu_i, \sigma_i^2)$$

Alıcı aşamasında, yansıyan gürültülü sinyal ile saklanan avare foton arasındaki faz korelasyonu ve kovaryans analizi yapılarak arka plan termal gürültüsü baskılanır:

## 3D Model ve Geometrik Analiz

Proje kapsamında incelenen hayalet (stealth) uçak geometrisi, kaplamaları ve gövde açılarından kaynaklanan yansıma kayıplarını simüle etmek amacıyla modellenmiştir.
<img width="938" height="770" alt="resim" src="https://github.com/user-attachments/assets/1ad35212-c4d4-4069-aea0-a1c7f2894b3e" />
Teknik altyapı çalışmaları iCADFEKO ortamında hazırlanmış orijinal uçak mesh dosyasının detaylı işlenmesiyle başlamıştır. Geliştirilen robust Python parser’lar ile dosya içinden DR (Veri Satırı) satırları ve vertex koordinatları çıkarılarak 63.160 üçgen ve yaklaşık 190.498 nokta içeren ham model temizlenmiştir.


<img width="1254" height="1422" alt="resim" src="https://github.com/user-attachments/assets/a40f518a-db54-4456-bd7f-c6f6d8962721" />

Hava platformunun yüksek frekans asimptotik saçılma karakteristikleri, Fiziksel Optik (PO) yöntemi kullanılarak elektromanyetik simülasyonlar vasıtasıyla Şekil2 deki gibi analiz edilmiştir. Temel geometrik konfigürasyona sahip orijinal modelin ortalama Radar Kesit Alanı (RCS), referans düzlemde  olarak hesaplanmıştır.

<img width="944" height="1014" alt="resim" src="https://github.com/user-attachments/assets/ab8f69e7-4f57-4ae5-8e2e-2a33c7acaea8" />

<img width="1104" height="706" alt="resim" src="https://github.com/user-attachments/assets/61827abf-e2b8-48f3-afec-37060b511eea" />

Uygulanan pasif imza yönetimi metodolojileri sayesinde, orijinal modele kıyasla toplamda  değerinde kayda değer bir saçılma bastırması elde edilmiştir.
---

## Simülasyon Çıktıları ve Performans Analizi

### Klasik Monostatik Radar Performansı
Yapılan simülasyonda kritik tespit eşiği ($\text{Threshold}$) **12 dB** olarak belirlenmiştir. 
- **1200 m Menzilde:** Sistem $12.40 \, \text{dB}$ SNR değeri ile sınırda başarılı bir tespit gerçekleştirmektedir (`EVET`).
- **5000 m Menzilde:** Hedefin küçük RCS değeri ($0.0800 \, \text{m}^2$) ve $R^4$ sönümlemesi nedeniyle sinyal gücü $5.80 \times 10^{-16} \, \text{W}$ seviyesine, sinyal-gürültü oranı ise $-12.39 \, \text{dB}$'e gerilemektedir. Bu durumda klasik radar hedefi **kesinlikle tespit edememektedir** (`HAYIR`).
 <img width="1688" height="1030" alt="resim" src="https://github.com/user-attachments/assets/c989b25e-90e6-4727-a84e-3e5c18e1b82f" />


| Parametre | 1200 m Menzil | 5000 m Menzil |
| :--- | :---: | :---: |
| **RCS ($\sigma$)** | $0.0800 \, \text{m}^2$ | $0.0800 \, \text{m}^2$ |
| **Alınan Güç ($P_r$)** | Sınırda | $5.80 \times 10^{-16} \, \text{W}$ |
| **SNR (dB)** | $+12.40 \, \text{dB}$ | $-12.39 \, \text{dB}$ |
| **Tespit Durumu** | **EVET** | **HAYIR** |

###  Kuantum Radar
<img width="1368" height="524" alt="resim" src="https://github.com/user-attachments/assets/ecb2922c-82f3-4cb3-8ec3-4d2c985650bc" />
Kuantum radarın ekstrem gürültülü ortamlardaki performansı, geliştirilen farklı sinyal işleme katmanları ile test edilmiştir:
<img width="1434" height="540" alt="resim" src="https://github.com/user-attachments/assets/7ddf3408-27bc-4192-9e92-fc3dc67bee9c" />

#### Gelişmiş Gürültü Altında
<img width="1226" height="618" alt="resim" src="https://github.com/user-attachments/assets/6d3f5390-233c-4f7f-967e-179e70c88e46" />

Dinamik faz düzeltmesi yapılmadığında veya gürültü bastırma aşaması yetersiz kaldığında, ham korelasyon $0.2113$ seviyesinde başlasa bile, dolaşıklık filtreleme hassasiyet kaybı nedeniyle işlenmiş korelasyon $0.0365$'e düşmekte ve Tespit Skoru $0.0013$ seviyesinde kalarak **başarısızlıkla** sonuçlanmaktadır (`Hedef Tespit Edildi mi? -> HAYIR`).


####  `advanced_signal_processor` ve `ultra_signal_processor` Başarımları
<img width="1514" height="776" alt="resim" src="https://github.com/user-attachments/assets/56e48dd5-42b9-4e34-b8ac-9c772e8cbfad" />

İleri düzey faz düzeltme ($\text{phase correction}$) ve dinamik eşikleme algoritmaları devreye alındığında, kuantum aydınlatma avantajı görünür kılınmıştır:
- **Üretilen Dolanık Çift:** 10,000 Foton Çifti
- **Son Korelasyon:** $0.4760$
- **Sistem SNR Değeri:** $1.055$
- **Hesaplanan Tespit Skoru:** $0.2455$
- **Nihai Karar:** `Hedef Tespit Edildi mi? -> EVET`
<img width="1036" height="308" alt="resim" src="https://github.com/user-attachments/assets/3529f33d-beb6-438a-8be2-25ebcc5a6460" />

<img width="1362" height="694" alt="resim" src="https://github.com/user-attachments/assets/c22f9aab-daeb-420b-9bad-37ab580e4713" />

Bu süreçte arka plan termal gürültüsü başarıyla ayıklanmış, saçılım grafiklerinde idler ve sinyal fotonları arasındaki kuantum bağı korelasyon yoğunluğu matrisi üzerinde matematiksel olarak kanıtlanmıştır.

---
Lisans: MIT
