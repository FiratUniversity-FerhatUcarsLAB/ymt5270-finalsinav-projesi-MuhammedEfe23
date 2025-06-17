# YMT5270 Final Sınav Projesi: H2O ile Veri Analizi ve Makine Öğrenmesi

## Öğrenci Bilgileri
- **Ad Soyad**: Muhammed Efe DEVECİ
- **Öğrenci Numarası**: *241137130*
- **E-posta**: *devecimuhammedefe@gmail.com*

---

## Proje Özeti
Bu projede, H2O.ai platformu kullanılarak kalp hastalığı tahmini için sınıflandırma problemi çözülmüştür. Kullanılan veri seti açık kaynaklıdır ve bireylerin tıbbi özelliklerini içermektedir. Projede önce detaylı keşifsel veri analizi (EDA) gerçekleştirilmiş; eksik veriler, veri tipleri ve öznitelikler incelenmiştir. Hedef değişken olarak `HeartDisease` seçilmiş ve AutoML yöntemiyle en uygun model seçilmiştir. AutoML sürecinde birçok farklı algoritma denenmiş ve en yüksek başarıyı Gradient Boosting Machine (GBM) modeli göstermiştir. Modelin doğruluk oranı %88.8, AUC skoru ise 0.937 olarak ölçülmüştür. Öznitelik katkı analizi sonucunda `ST_Slope`, `ChestPainType` ve `ExerciseAngina` değişkenlerinin tahmin performansına büyük katkı sağladığı görülmüştür. Sonuç olarak geliştirilen model kalp hastalığı riskini başarılı şekilde sınıflandırabilmektedir.

---

## Veri Seti

### Veri Seti Bilgileri
- **Veri Seti Adı**: Heart Disease UCI Dataset
- **Kaynak**: [Kaggle - Heart Disease Dataset](https://www.kaggle.com/datasets/fedesoriano/heart-failure-prediction)
- **Lisans**: CC0: Public Domain
- **Veri Seti Boyutu**: 918 satır, 12 sütun

### Veri Seti Tanımı
Veri seti, bireylerin çeşitli tıbbi ölçümlerini ve kalp hastalığına sahip olup olmadıklarını göstermektedir. Değişkenler arasında yaş, kolesterol, kan basıncı, egzersize bağlı anjina, maksimum kalp atış hızı ve EKG sonuçları gibi bilgiler yer almaktadır. Hedef değişken olan `HeartDisease`, bireyin kalp hastalığı taşıyıp taşımadığını belirtmektedir (1 = hasta, 0 = sağlıklı).

### Öznitelik Açıklamaları

| Öznitelik Adı     | Veri Tipi  | Açıklama                                      | Örnek Değer |
|-------------------|------------|-----------------------------------------------|-------------|
| Age               | Sayısal    | Yaş                                           | 54          |
| Sex               | Kategorik  | Cinsiyet (1 = erkek, 0 = kadın)               | 1           |
| ChestPainType     | Kategorik  | Göğüs ağrısı tipi                             | ATA         |
| RestingBP         | Sayısal    | Dinlenme kan basıncı                          | 140         |
| Cholesterol       | Sayısal    | Serum kolesterol seviyesi                     | 240         |
| FastingBS         | Kategorik  | Açlık kan şekeri > 120 mg/dl (1 = evet)       | 0           |
| RestingECG        | Kategorik  | Dinlenme EKG sonucu                           | Normal      |
| MaxHR             | Sayısal    | Maksimum kalp atış hızı                       | 160         |
| ExerciseAngina    | Kategorik  | Egzersize bağlı anjina (1 = evet, 0 = hayır)  | 0           |
| Oldpeak           | Sayısal    | ST segmentinde depresyon                      | 1.5         |
| ST_Slope          | Kategorik  | ST segmentinin eğimi                          | Up          |
| HeartDisease      | Kategorik  | Hedef değişken (1 = hasta, 0 = sağlıklı)      | 1           |

---

## Keşifsel Veri Analizi (EDA)

### Temel İstatistikler
- Ortalama, min, max, standart sapma gibi temel istatistikler `data.describe()` ile incelendi.
- Hedef değişkenin dağılımı dengeli olup, `HeartDisease = 1` olanların oranı %55.3 civarındadır.

### Veri Ön İşleme
- `HeartDisease` değişkeni `asfactor()` ile kategorik tipe dönüştürüldü.
- Eksik veri bulunmadığı için herhangi bir silme ya da doldurma yapılmadı.
- Sayısal değişkenlerde histogramlar çizildi.
- Kategorik değişkenlerin dağılımları kontrol edildi.

### Görselleştirmeler
- `Age`, `Cholesterol`, `MaxHR` gibi sütunlara ait histogramlar incelendi.
- Özniteliklerin hedef değişkenle ilişkisi gözlendi (ST_Slope ve ChestPainType dikkat çekici).

### Öznitelik İlişkileri
- Korelasyon matrisi ve önem sıralaması AutoML sonunda çıkarılan variable importance tablosu ile yorumlandı.

---

## Makine Öğrenmesi Uygulaması

### Kullanılan Yöntem
- **Yöntem**: Sınıflandırma  
- **Modelleme Yaklaşımı**: H2O AutoML (otomatik model seçimi)
- **Hedef Değişken**: HeartDisease

### Modeller ve Parametreler
- `max_runtime_secs = 300` olacak şekilde AutoML süreci başlatıldı.
- GBM, XGBoost, DRF, GLM, DeepLearning modelleri otomatik olarak denendi.
- En iyi sonuç veren model: **Gradient Boosting Machine (GBM)**

### Model Değerlendirmesi

#### Öne Çıkan Metrikler

| Metrik     | Değer   |
|------------|---------|
| Accuracy   | 0.8888  |
| Precision  | 0.8765  |
| Recall     | 0.9328  |
| F1-score   | 0.9030  |
| AUC        | 0.9376  |
| MCC        | 0.7768  |

- Confusion Matrix: 63 yanlış negatif, 41 yanlış pozitif örnek
- ROC eğrisi alanı oldukça geniş (%93+ başarı)

---

## Sonuç ve Öneriler

Bu projede kalp hastalığı tahmini için H2O AutoML algoritmaları başarıyla uygulanmıştır. Gradient Boosting Machine modeli, %88.8 doğruluk ve %93.7 AUC skoru ile en iyi performansı göstermiştir. ST_Slope, ChestPainType ve ExerciseAngina özniteliklerinin modele katkısı yüksek çıkmıştır. Modelin recall oranının yüksek olması, gerçek hastaların büyük ölçüde doğru tahmin edildiğini göstermektedir. Gelecekte modelin farklı veri setleriyle genel başarımı test edilebilir. Ayrıca özelleştirilmiş hiperparametre arama süreçleriyle daha iyi sonuçlar elde edilebilir.

---

## Kaynaklar
https://www.kaggle.com/datasets/fedesoriano/heart-failure-prediction  
 

---

## Ekler

### ipynb Proje Dosyası
> `finalprojeymot.ipynb` dosyası bu repoda `project` klasörü altında yer almaktadır.

### Veri Seti Dosyası veya Bağlantısı
> [HeartDisease Dataset (Kaggle)](https://www.kaggle.com/datasets/fedesoriano/heart-failure-prediction)

