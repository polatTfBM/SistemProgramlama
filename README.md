# BMT-408 Dersi: AWS Üzerinde Konteyner Mimarili Web Sunucusu

Bu proje kapsamında, gerçek dünya gereksinimlerine uygun olarak AWS bulut ortamında güvenli, ölçeklenebilir ve otomatik yedekleme sistemine sahip bir web sunucusu altyapısı kurulmuştur.

## Öğrenci Bilgileri
Ad Soyad: Polat Ashyrov
Öğrenci Numarası: 22181616xxx
Üniversite/Bölüm: Gazi Üniversitesi Teknoloji Fakültesi Bilgisayar Mühendisliği

## Sistem Mimarisi ve Kullanılan Teknolojiler
- Bulut Sağlayıcı: Amazon Web Services (AWS EC2 - Free Tier)
- İşletim Sistemi: Ubuntu Server 24.04 LTS
- Konteyner Yönetimi: Docker ve Docker Compose
- Web Katmanı: Nginx (Reverse Proxy)
- Backend: Python FastAPI
- Veritabanı: PostgreSQL
- Güvenlik: AWS Security Group ve nftables (Sunucu içi firewall)
- Otomasyon: Cron (Zamanlanmış görevler)

## Kurulum ve Yapılandırma Adımları

Sistemi ayağa kaldırmak için aşağıdaki adımlar izlenmiştir:

1. Sunucu Temini: AWS üzerinde Ubuntu 24.04 LTS işletim sistemli bir EC2 sunucusu oluşturuldu. Ağ ayarlarında sadece SSH (22) ve HTTP (80) portları dışarıya açıldı. Veritabanı güvenliği için 5432 portu dış erişime kapalı tutuldu.
2. Kullanıcı ve Güvenlik Ayarları: Sunucuda sudo yetkilerine sahip "gazi" kullanıcısı oluşturuldu. Root ile giriş ve parola ile SSH bağlantısı kapatılarak sadece anahtar (.pem) ile giriş yapılması sağlandı.
3. Altyapı Hazırlığı: Sistem güncellemeleri yapıldı ve istenen nmap, rkhunter, clamav, docker, nftables gibi paketler sisteme kuruldu.
4. Servislerin Başlatılması: Nginx, PostgreSQL ve FastAPI servisleri docker-compose kullanılarak izole konteynerlar halinde tek bir komutla ayağa kaldırıldı.
5. Güvenlik Duvarı: nftables kullanılarak sunucu içi firewall yapılandırıldı. Gelen bağlantılar varsayılan olarak engellendi, sadece yerel ağ trafiğine ve 22 ile 80 portlarına izin verildi.
6. Otomatik Yedekleme: crontab üzerinden her gece saat 04:00'te veritabanının yedeğini alan ve 7 günden eski yedekleri silen bir bash betiği zamanlandı.

## API Endpoint Listesi

FastAPI uygulaması üzerinden sunulan ve veritabanı ile bağlantılı çalışan endpoint'ler şu şekildedir:

- GET /health : Sistemin ayakta olup olmadığını kontrol eder. {"status": "ok"} yanıtını döner.
- POST /items : Veritabanına yeni bir kayıt ekler. İstek gövdesinde baslik ve icerik bilgilerini alır.
- GET /items : Veritabanında bulunan kayıtları JSON dizisi olarak listeler.

Tüm bu endpoint'ler sunucu adresi üzerinden /docs yoluna gidilerek (Swagger UI) test edilebilir.

## Değerlendirme Kanıtları (Test ve Ekran Görüntüleri)

Hoca tarafından proje değerlendirmesi için istenen tüm kanıt dosyaları ve ekran görüntüleri proje dizini altındaki "resimler" klasörüne yüklenmiştir. Bu klasörde aşağıdaki adımların kanıtları bulunmaktadır:

- AWS Security Group ve bütçe takibi (Budget) ekran görüntüleri
- Konteynerların çalıştığını gösteren "docker compose ps" çıktısı
- FastAPI /docs arayüzünün ekran görüntüsü
- Açık portları gösteren "ss -tulpn" ve firewall kurallarını gösteren "nft list ruleset" çıktıları
- Veritabanı yedek dosyaları ve başarılı restore işleminin kanıtı
- SFTP ile sunucuya dosya aktarımını gösteren kanıt görseli
