# Türkçe Film Yorumlarında Duygu Analizi / Yıldız Tahmini (1–5) — BERTurk Fine-tuning (LLM Projesi)

Bu proje, Türkçe film yorum metinlerinden **1–5 yıldız (puan)** tahmini yapan bir **çok sınıflı metin sınıflandırma** çalışmasıdır.  
Model olarak Türkçe için ön-eğitimli bir Transformer modeli (**BERTurk**) kullanılmış ve görev için **fine-tuning (ince ayar)** uygulanmıştır.

> Proje **Google Colab** üzerinde geliştirilmiştir ve **ücretli API kullanılmamıştır**.  
> Tüm süreç tek bir `.ipynb` dosyasında çalışır.

---

## Proje Ne Yapıyor?

- Türkçe film yorumlarını ve puanlarını içeren veri setini yükler
- Metinleri temizler ve etiketleri (puanları) **5 sınıfa (1–5)** dönüştürür
- Veriyi **train/validation/test** olarak ayırır (stratified split)
- **BERTurk** ile fine-tuning yapar
- Performansı nicel metriklerle değerlendirir:
  - **Accuracy**
  - **Macro F1**
  - **Confusion Matrix**
- (Opsiyonel) Hata analizi: yanlış sınıflanan örnek yorumları gösterir

---

## Neden Bu Proje?

- Yorumlar çok sayıda olduğunda manuel değerlendirme zorlaşır
- Duygu analizi / puan tahmini, kullanıcı geri bildirimlerini ölçeklenebilir hale getirir
- 5 sınıf (1–5 yıldız) problemi, 2 sınıflı sentiment’e göre daha gerçekçi ve zorludur

---

## Veri Seti (Dataset)

Bu projede Hugging Face üzerinden erişilen film yorum veri seti kullanılmıştır:

- **Dataset:** `TFLai/turkish_movie_sentiment`
- Örnek kolonlar:
  - `comment` (yorum metni)
  - `point` (puan/yıldız)
  - `film_name`

> Not: `point` alanında `"5,0"` gibi virgüllü ondalık değerler olabildiği için `"5,0" → 5.0` dönüşümü yapılmıştır.

---

## Model

- **Model:** `dbmdz/bert-base-turkish-cased` (BERTurk)
- **Yaklaşım:** Transformer tabanlı metin sınıflandırma (Sequence Classification) + Fine-tuning

### Embedding yaklaşımı (kısa)
Metin tokenizer ile subword token’lara ayrılır. Transformer katmanları bağlamsal temsilleri üretir.  
Sınıflandırma için genellikle **[CLS] temsili** kullanılır ve bu temsil üzerinden 5 sınıfa olasılık üretilir.

---

## Kullanılan Araçlar

- **Python**
- **Google Colab** (GPU desteği ile hızlı fine-tuning)
- **Hugging Face Transformers / Datasets / Evaluate**
- **scikit-learn** (confusion matrix / raporlama)

---

## Nasıl Çalıştırılır? (Google Colab)

1. `.ipynb` dosyasını Google Colab’a yükle / aç
2. Hücreleri **sırayla** çalıştır:
   - Kurulum (pip install)
   - Veri setini yükleme, kolon tespiti
   - Etiket (label) dönüşümü (1–5 → 0–4)
   - Train/Val/Test split
   - Tokenization
   - Model eğitimi (Trainer)
   - Değerlendirme (Accuracy, Macro F1, Confusion Matrix)

> ⚠️ Hücre sırasını atlamayın. Özellikle tokenization ve split tamamlanmadan eğitime geçmeyin.

---

## Değerlendirme (Metrikler)

Bu projede raporlanan metrikler:
- **Accuracy:** Doğru sınıfı tam tahmin etme oranı
- **Macro F1:** Sınıflar dengesiz olduğunda daha adil performans ölçümü
- **Confusion Matrix:** Hangi yıldız sınıflarının karıştığını gösterir

---

## Sonuçlar (kısa not)
Eğitim sırasında validation metrikleri epoch bazında takip edilmiştir.  
5 sınıflı yıldız tahmini problemlerinde komşu sınıfların (ör. 4 ve 5) karışması beklenir.  
Final raporda test seti sonuçları (Accuracy, Macro F1) ve confusion matrix görseli paylaşılmıştır.

---

## Future Work (Gelecek Çalışmalar)

- **Daha güçlü GPU ile daha hızlı fine-tuning:** Eğitim süresi kısalır, daha fazla hiperparametre denemesi yapılabilir.
- **Daha iyi/alternatif modeller:** XLM-R gibi farklı Transformer modelleri denenebilir.
- **Daha iyi veri dengesi:** Sınıf dengesizliğine yönelik stratejiler (ör. daha fazla veri, örnekleme, farklı label stratejileri) ile Macro F1 artırılabilir.
- **Label stratejisi:** 5 sınıf yerine 3 sınıf (negatif/nötr/pozitif) gibi alternatif etiketleme ile performans kıyaslanabilir.

---

## Dosyalar
- `<MUSTAFAKURT220212022_LLMPROJEasıl>.ipynb` — Projenin tamamı (eğitim + değerlendirme)

---

## Lisans
Bu proje ders kapsamında hazırlanmıştır.
