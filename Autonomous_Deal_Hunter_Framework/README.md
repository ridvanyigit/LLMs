# Autonomous Deal Hunter: A Multi-Agent Framework

Bu proje, internetteki RSS akışlarını otonom olarak tarayan, potansiyel ürün fırsatlarını belirleyen, bu ürünlerin değerini hibrit bir yapay zeka yaklaşımıyla tahmin eden ve önemli indirimleri kullanıcıya anlık bildirim olarak gönderen çok-ajanlı (multi-agent) bir yapay zeka sistemidir.

## Proje Mimarisi

Sistem, bir orkestra şefi (**Planning Agent**) tarafından yönetilen bir uzman ajanlar ekibinden oluşur. Her ajanın belirli bir görevi vardır:

- **Scanner Agent:** RSS akışlarından ham veriyi toplar ve bir LLM kullanarak en umut verici 5 fırsatı seçip özetler.
- **Ensemble Agent:** Fırsatları fiyatlandırmak için üç farklı uzmandan görüş alır:
    1.  **Specialist Agent:** Modal üzerinde dağıtılmış, özel olarak ince ayarlanmış bir LLM kullanır.
    2.  **Frontier Agent:** Geniş bir RAG veritabanı (400k ürün) ve GPT-4o gibi bir sınır (frontier) model kullanır.
    3.  **Random Forest Agent:** Klasik bir makine öğrenmesi modeliyle istatistiksel bir tahmin yapar.
- **Messaging Agent:** Belirlenen eşiği aşan indirimler için Pushover API'si üzerinden anlık bildirim gönderir.
- **Deal Agent Framework:** Tüm bu ajanları bir araya getiren, sistemin hafızasını ve genel durumunu yöneten ana çerçevedir.

<!-- Buraya bir mimari diyagramı ekleyebilirsiniz -->

## Teknoloji Stack'i

- **AI/ML:** OpenAI, Sentence Transformers, Scikit-learn, ChromaDB
- **Deployment & Altyapı:** Modal
- **Arayüz:** Gradio
- **Veri İşleme:** Python, Pandas, NumPy, Feedparser
- **Bildirim:** Pushover

## Kurulum

1.  Bu repoyu klonlayın:
    ```bash
    git clone https://github.com/ridvanyigit/LLMs.git
    cd LLMs/Autonomous_Deal_Hunter_Framework
    ```
2.  Gerekli Anaconda ortamını oluşturun ve aktive edin (`llms` ortamı):
    ```bash
    # Eğer environment.yml varsa:
    conda env create -f environment.yml 
    conda activate llms
    # Veya manuel kurulum:
    pip install -r requirements.txt
    ```
3.  `.env.example` dosyasını kopyalayarak `.env` adında yeni bir dosya oluşturun ve içine kendi API anahtarlarınızı girin.
    ```bash
    cp .env.example .env
    # .env dosyasını düzenleyin
    ```
4.  Modal CLI'ı kurun ve hesabınızı yapılandırın.
    ```bash
    modal setup
    ```
5.  Hugging Face token'ınızı Modal'a bir "secret" olarak ekleyin (isim: `hf-secret`).

## Kullanım

Bu sistemi iki farklı modda çalıştırabilirsiniz:

**1. Otonom Terminal Modu:**
Ajan çerçevesini doğrudan terminalden başlatarak arka planda otonom olarak çalışmasını sağlayın.

```bash
# Önce ince ayarlı model servisini Modal'a deploy edin
modal deploy deployment/pricer_service2.py

# Ardından ana ajan çerçevesini çalıştırın
python deal_agent_framework.py