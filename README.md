# STM32 Nucleo ADC ile Voltaj Ölçümü ve I2C LCD HMI

Bu proje, STM32 Nucleo-F072RB geliştirme kartı kullanılarak analog bir sinyalin (Potansiyometre) mikrodenetleyici tarafından okunması (ADC), dijital veriye dönüştürülmesi ve **I2C protokolü** kullanan 16x2 LCD ekran üzerinde görselleştirilmesi amacıyla geliştirilmiştir[cite: 9].

Çalışma, endüstriyel sensör okuma ve veri gösterme (HMI) uygulamalarının temelini oluşturmakta olup, analog dünyadaki fiziksel büyüklüklerin (voltaj) dijital verilere nasıl dönüştürüldüğünü uygulamalı olarak göstermektedir[cite: 9].

## 🚀 Öne Çıkan Özellikler (Highlights)

*   **Yüksek Çözünürlüklü Analog Okuma:** STM32'nin sahip olduğu 12-bit çözünürlüklü ADC birimi kullanılarak, 0-3.3V arasındaki giriş gerilimi 4096 (0-4095) farklı seviyede dijital değere dönüştürülmüştür[cite: 9].
*   **Kesintisiz Ölçüm (Continuous Conversion):** ADC konfigürasyonunda `Continuous Conversion Mode` (Sürekli Okuma) aktif edilerek, sensörden gelen veriler eş zamanlı olarak işlenmiştir[cite: 9].
*   **Özel I2C LCD Sürücüsü:** Standart HAL kütüphanelerinde bulunmayan I2C LCD ekran kontrolü için, pin seviyesinde (`i2c-lcd.h` ve `i2c-lcd.c`) özel bir donanım soyutlama katmanı yazılarak ekran sürülmüştür[cite: 9].
*   **Dinamik HMI Ekranı:** Okunan 12-bitlik ham veri ve gerçek voltaj ($V = ADC \times 3.3 / 4095$) değeri aynı anda I2C LCD ekran üzerinde dinamik olarak gösterilmektedir[cite: 9].

## 🛠️ Donanım ve Pin Yapılandırması

Sistemdeki dış birimlerin STM32 Nucleo-F072RB üzerindeki pin atamaları ve yapılandırmaları aşağıdaki gibidir[cite: 9]:

| Bileşen | Pin Kodu | Kullanım Modu | Açıklama |
| :--- | :--- | :--- | :--- |
| **Potansiyometre** | `PA0` | ADC_IN0 | 0-3.3V arası gerilim bölücü analog giriş[cite: 9]. |
| **I2C SDA** | `PB7` | I2C1_SDA | LCD ekran veri iletişimi hattı[cite: 9]. |
| **I2C SCL** | `PB6` | I2C1_SCL | LCD ekran saat senkronizasyon hattı[cite: 9]. |

## 📂 Yazılım Mimarisi ve Matematiksel Model

*   **Matematiksel Model:** ADC'den okunan 12-bitlik (0-4095) ham değer, aşağıdaki formül kullanılarak anlamlı bir gerilim (Volt) değerine dönüştürülür[cite: 9].
    ```c
    voltage = (adc_val * 3.3) / 4095.0;
    ```
*   **Ana Döngü (Main Loop):** İşlemci, `HAL_ADC_PollForConversion` fonksiyonu ile ADC çevriminin bitmesini bekler, ardından `HAL_ADC_GetValue` ile ham veriyi okur[cite: 9]. Okunan veri hesaplamalardan geçirilerek `sprintf` ile formatlanır ve I2C LCD'nin 1. ve 2. satırlarına bastırılır[cite: 9]. Görüntü titreşimlerini engellemek için döngü sonunda 200ms gecikme (`HAL_Delay`) uygulanmıştır[cite: 9].

## 💻 Nasıl Çalıştırılır?

1.  Projeyi `STM32CubeIDE` ile açın.
2.  Kodu derleyin (`Build`) ve STM32 Nucleo-F072RB kartına yükleyin[cite: 9].
3.  Sistem başlatıldığında LCD ekranda "Sivas CU Müh. Fakultesi" mesajı görüntülenecektir[cite: 9].
4.  Breadboard üzerindeki potansiyometreyi çevirerek 12-bitlik ham değerin (0-4095) ve gerçek voltaj değerinin (0-3.3V) eş zamanlı olarak ekranda nasıl değiştiğini gözlemleyebilirsiniz[cite: 9].

<img width="1456" height="1020" alt="dfh" src="https://github.com/user-attachments/assets/4755b1fa-35ce-4256-a01f-8bcd92a73ee6" />
