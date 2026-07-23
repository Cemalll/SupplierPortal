vertical slice'ları modülün içinde verticalslise klasöründe tutmaya karar verdim
her verticcal slice benim promptum olacak gibi düşünebilirim.













İşte modül klasörünüzdeki 4 sabit dosya ve her birinin ne içerdiği — bunlar Bölüm 1'deki (roadmap dosyasında) 5 Fazlı Şablonun çıktılarına karşılık geliyor:

1. 01-decision-log.md (artık sizde decisions.md olarak adlandı)

Hangi faza ait: Faz 1 — Karar Toplama

İçeriği:

Modülün ele aldığı her soru için: Context (neden bu karara ihtiyaç var), Decision Question (net soru), Options Considered (seçenekler), Decision (verilen karar), Rationale (neden bu seçildi), Consequences (sonuçları), Exceptions (istisnalar), MVP Classification (bu turda mı sonraki turda mı)
Amaç: "Ne yapacağız" sorusunun cevabı — henüz teknik değil, iş kararı seviyesinde
Örnek: Modül 18'de az önce çalıştığınız 45 karar tam olarak bu dosyanın içeriği
2. 02-conceptual-model.md

Hangi faza ait: Faz 2 — Kavramsal Model Çıkarımı

İçeriği:

Varlıklar (Entities): Bu modülde hangi ana nesneler var (örn. Invitation, SupplierProspect)
State Machine: Her varlığın durumları ve geçiş kuralları (Mermaid diyagramı olarak — metin, ama görsel render edilir)
Yetki matrisi taslağı: Rol × Aksiyon × Alan bazlı kim ne yapabilir
Entegrasyon notu: Bu verinin source-of-truth'u nerede, ERP'ye ne zaman/nasıl gidiyor
Amaç: Faz 1'deki kararları teknik bir dile çevirmek — ama hâlâ kod değil, kavram seviyesinde
Kontrol kriteri: Bu dosyayı okuduğunuzda "evet, kararlarımı doğru yansıtıyor" diyebilmelisiniz
3. 03-technical-design.md

Hangi faza ait: Faz 3 — Teknik Tasarım

İçeriği:

Veritabanı şeması: Tablolar, alanlar, tipler, foreign key'ler (DDL veya ORM entity formatında)
API sözleşmesi: Endpoint'ler, request/response şemaları (OpenAPI/YAML veya markdown tablo)
İş kuralı/validasyon listesi: Alan bazlı kurallar, workflow adımları
Amaç: Kod yazılmadan önceki son somut adım — bu dosyayı başka bir geliştirici okusa modülü nasıl kodlayacağını anlamalı
Not: Faz 4 (kod) burada üretilmiyor — bu dosya sadece kodun tasarımı, src/ klasörüne giden kod ayrı
4. 05-validation.md

Hangi faza ait: Faz 5 — Doğrulama ve Kapanış

İçeriği:

Faz 1'deki karar kaydıyla üretilen kodun karşılaştırılması — sapma var mı?
Ek B tarzı kontrol listesi (bu modüle özel işaretlenmiş hali)
Açık kalan konuların (varsa) bir sonraki slice'a devredilme notu
Amaç: Modülü "kapalı" ilan etmeden önceki son kontrol katmanı
Neden Faz 4 (kod) için ayrı bir 04-...md dosyası yok

Faz 4'ün çıktısı zaten çalışan koddur — bu src/ klasörüne gider, docs/ altına değil. Dosya numaralandırmasında 4 boşluk gibi görünüyor ama kasıtlı: 03-technical-design.md, kodun referans belgesi olarak kalıyor, kodun kendisi ayrı yaşıyor.
