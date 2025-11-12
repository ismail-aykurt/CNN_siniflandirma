# Proje 1: Keras ile CNN Görüntü Sınıflandırma

Bu proje, Makine Öğrenmesi dersi kapsamında Keras kütüphanesi kullanılarak geliştirilmiş bir görüntü sınıflandırma projesidir.

**Projenin Amacı:** Kendi topladığım özel bir veri seti (Mouse vs Havya) kullanarak üç farklı CNN modelini eğitmek, karşılaştırmak ve performanslarını analiz etmek.


## Veri Seti (Dataset)

Projede kullanılan veri seti tamamen tarafımca oluşturulmuştur.
* **Sınıf 1:** `mouse` (Bilgisayar Faresi) - 53 adet
* **Sınıf 2:** `havya` (Lehim Aleti) - 53 adet
* **Toplam Görüntü:** 106 adet
* **Boyutlandırma:** Tüm görüntüler kodlama aşamasında (ImageDataGenerator) 128x128 piksel olarak yeniden boyutlandırılmıştır.

Veri seti, `dataset/` klasörü altında `mouse/` ve `havya/` olarak iki alt klasörde yapılandırılmıştır.


## Modeller ve Sonuçlar

Proje kapsamında 3 farklı model eğitilmiş ve karşılaştırılmıştır.

### 1. Model (`model1.ipynb`): Transfer Öğrenme (VGG16)
* **Mimari:** ImageNet ağırlıklarıyla önceden eğitilmiş VGG16 modeli temel (base) olarak alınmış, son katmanları dondurulmuş (freeze) ve üzerine özel sınıflandırma katmanları (Dense, Dropout) eklenmiştir.
* **Amaç:** Transfer Öğrenmenin (Kalıtım yoluyla öğrenme) az veriyle ne kadar başarılı olabileceğini test etmek.
* **Sonuç (Test Doğruluğu):** %100.00
* **Analiz:** VGG16, milyonlarca görüntüden öğrendiği özellik bilgisi sayesinde, çok az veriyle (86 eğitim resmi) bile hızlı ve kararlı bir şekilde %100 doğruluğa ulaşmıştır.

### 2. Model (`model2.ipynb`): Basit CNN (Sıfırdan)
* **Mimari:** 3 adet `Conv2D` ve `MaxPooling2D` bloğundan oluşan, sıfırdan eğitilmiş basit bir CNN mimarisidir.
* **Amaç:** Sıfırdan eğitilen bir modelin, aynı veri seti üzerindeki temel performansını (baseline) ölçmek.
* **Sonuç (Test Doğruluğu):** %100.00
* **Analiz:** Model %100 doğruluğa ulaşsa da, eğitim grafikleri (Accuracy/Loss) çok kararsız (dalgalı) bir öğrenme süreci gösterdi. Bu durum, modelin az veri nedeniyle ezberlemeye (overfitting) yatkın olduğunu göstermektedir.

### 3. Model (`model3.ipynb`): Geliştirilmiş CNN (Hiperparametre Optimizasyonu)
* **Mimari:** `model2` mimarisi temel alınarak aşağıdaki iyileştirmeler yapılmıştır:
    1.  **Hiperparametre Değişikliği:**
        * `Batch Size`: 32'den 64'e çıkarıldı.
        * `Learning Rate`: 0.001'den (varsayılan) 0.0005'e düşürüldü (daha yavaş öğrenme).
        * `Dropout`: Overfitting'i engellemek için 0.4 oranında Dropout katmanı eklendi.
    2.  **Veri Artırımı (Data Augmentation):** `ImageDataGenerator` kullanılarak eğitim verilerine (döndürme, kaydırma, çevirme) işlemleri uygulandı.
* **Amaç:** `model2`'deki kararsızlığı ve ezberlemeyi (overfitting) azaltarak daha stabil ve güvenilir bir model elde etmek.
* **Sonuç (Test Doğruluğu):** %100.00
* **Analiz:** Yapılan iyileştirmeler sayesinde, `model3`'ün öğrenme grafikleri `model2`'ye kıyasla daha kararlı hale gelmiştir. Veri artırımı ve Dropout, modelin ezberlemesini zorlaştırarak daha iyi genelleme yapmasını sağlamıştır.

---

## Karşılaştırma Tablosu



<img width="828" height="190" alt="Ekran görüntüsü 2025-11-12 210927" src="https://github.com/user-attachments/assets/41c0e040-6ec2-43d6-bf32-979e716b6b6a" />
