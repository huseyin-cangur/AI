# Yazılım Ekipleri için AI Araç Rehberi

## Genel Bakış

Yazılım ekipleri için iki temel AI aracı öne çıkıyor: Cursor ve Claude Code (Team + API). Her biri farklı bir kullanım senaryosuna hitap ediyor.

---

## 1. Cursor

Kategori: AI-first kod editörü
Fiyat: $40/kişi/ay (Business plan)

VS Code tabanlı bir editör. Tüm kod tabanını indeksleyip anlıyor, bu sayede proje genelinde bağlamsal yardım sunabiliyor. Tab autocomplete, inline düzenleme ve Composer özelliğiyle büyük refactor işlemleri yapılabiliyor. GPT-4o ve Claude modelleriyle çalışıyor.

Background Agent özelliğiyle arka planda görev yürütebiliyor; ancak kritik adımlarda (commit, push gibi) genellikle kullanıcı onayı istiyor. CI/CD entegrasyonu doğrudan mevcut değil.

Yazılım ekipleri için en popüler seçim. VS Code'dan geçiş neredeyse sıfır sürtünmeli.

---

## 2. Claude Code (Team + API)

Kategori: Agentic terminal CLI aracı (Team + API)
Fiyat:
- Claude Code Team (seat):
  - Standard: $25/kişi/ay
  - Premium: $125/kişi/ay (yıllık, minimum 5 kullanıcı)
- Claude Code API (örnek model fiyatı):
  - Sonnet 4.6: $3/MTok input, $15/MTok output

Claude Code iki farklı şekilde konumlanır:

### Team plan (seat)
Ekip bazında kullanım için Claude Code Team (seat) tercih edilir. Claude Code Team; merkezi fatura yönetimi, SSO, rol bazlı izinler ve öncelikli model erişimi gibi kurumsal ihtiyaçlara yöneliktir.

Ekip içinde Standard ve Premium koltuğu karıştırmak mümkündür. Yani sadece geliştiriciler Premium seat alırken diğer ekip üyeleri daha uygun fiyatlı Standard seat kullanabilir.

Bu modelde Claude Code genellikle geliştiricinin terminalinden çalışır: dosyaları okuyup düzenler, testleri çalıştırır, hataları görüp düzeltir, commit atar ve PR açar (repo izinleri ve korumalı branch kurallarına bağlı olarak).

### API (otomasyon)
CI/CD içinde Claude Code’u “bot” gibi otomatik çalıştırmak istiyorsanız Claude Code API kullanılır. Claude Code Team (seat) ise daha çok geliştiricilerin yerelde/terminalde kullanımı içindir; Team kullanıyor olsanız bile pipeline otomasyonları için genellikle API tarafı tercih edilir. API modeli sabit aylık ücret yerine kullanılan kadar ödeme sağlar ve otomasyon senaryolarında maliyet takibi ile limit koyma daha nettir.

Örnek otomasyon hedefleri:
- Sonar/SAST taramalarını tetiklemek ve çıktılara göre düzeltme PR’ı açmak
- Unit ve integration testlerini yazıp çalıştırmak
- Kod değişikliğini PR/commit olarak üretmek (tercihen PR üzerinden)
- Belirli koşullarda release/deploy adımlarını hazırlamak

Otomasyonda güvenli ve sürdürülebilir kurulum için guardrail önerileri:
- **Branch protection**: Ana dala doğrudan push yerine PR açtırma + review/merge policy
- **Deploy güvenliği**: Prod environment için manuel onay (environment protection) veya koşullu otomasyon
- **Secret yönetimi**: API key, Sonar token, deploy anahtarları CI secret store’da; loglara sızmayı engelleyecek maskeleme
- **Maliyet kontrolü**: API tarafında budget/limit ve job bazlı kullanım sınırları

---

## Otonom Görevler İçin Araç Sıralaması

CI/CD entegrasyonu, test yazma ve commit atma gibi otonom görevler için araçlar şu şekilde sıralanıyor:

**1. Claude Code** — Gerçek anlamda otonom. Dosya okur/yazar, test çalıştırır, hataları düzeltir, git commit/push atar, PR açar, CI pipeline tetikler. Zincir görevleri tek komutla yürütebilir.

**2. Cursor (Background Agent)** — Kısmi otonom. Dosya okur/yazar, terminal komutları çalıştırır, test yazar. Ancak kritik adımlarda onay ister, CI/CD entegrasyonu doğrudan mevcut değil.

Örnek Claude Code kullanımı:

```
claude "auth modülüne rate limiting ekle, unit testlerini yaz ve conventional commit ile push et"
```

Bu komutla Claude Code ilgili dosyaları okur, kodu yazar, testleri çalıştırır, hata varsa düzeltir ve ardından commit atıp push eder.

---

## Cursor + Claude Code Birlikte Kullanım

Pratikte en verimli kurulumlardan biri, Cursor’ı günlük geliştirme için; Claude Code’u ise agentic işler ve otomasyon için birlikte kullanmaktır.

### Ne sağlar?
- Cursor ile IDE içinde hızlı iterasyon (autocomplete, refactor, proje bağlamıyla yardım)
- Claude Code ile çok adımlı görevlerin tek akışta yürütülmesi (test yaz/çalıştır, hata düzelt, PR/commit üret)
- CI/CD tarafında Claude Code API ile tekrar eden işleri otomatikleştirme (Sonar/test/PR hazırlığı gibi)

### Nasıl konumlanır?
- Geliştirici: Cursor + Claude Code Team (seat)
- Pipeline/bot: Claude Code API (budget ve limitlerle)

### Maliyet yaklaşımı
Toplam Aylık Maliyet ≈ (Cursor seat toplamı) + (Claude Code Team seat toplamı) + (Claude Code API kullanım)

---

## Fiyatlandırma Özeti (Tüm Araçlar)

Not: Fiyatlar sağlayıcıya, bölgeye, kampanyalara ve sözleşme tipine göre değişebilir. Aşağıdaki rakamlar dokümandaki referans fiyatlardır; kesin satın alma öncesi resmi fiyat sayfası kontrol edilmelidir.

### Cursor
- Model: Seat bazlı
- Business: $40/kişi/ay
- Aylık hesap: Cursor kullanan kişi sayısı x kişi başı plan ücreti

### Claude Code (Team + API)
- Model 1 (Team): Seat bazlı (Standard/Premium dağılımı)
- Model 2 (API): Kullanım bazlı (token)
- Claude Code Team (seat):
  - Standard: $25/kişi/ay
  - Premium: $125/kişi/ay (yıllık, minimum 5 kullanıcı)
- Claude Code API (örnek model fiyatı):
  - Sonnet 4.6: $3/MTok input, $15/MTok output
- Aylık hesap: (Claude Code Team seat toplamı) + (Claude Code API input/output token maliyeti)

### Hızlı Toplam Maliyet Formülü
Toplam Aylık Maliyet = Cursor seat + Claude Code Team seat + Claude Code API kullanım

---

## Claude Code Agent Özeti (Otomatikleştirme)

Claude Code, yazılım süreçlerinde bir "agent" gibi davranarak çok adımlı işleri uçtan uca yürütebilir. Özellikle CI/CD ve tekrar eden geliştirme görevlerinde hız kazandırır.

### Neler yapabilir?
- Repo içeriğini okuyup bağlamı çıkarır, ilgili dosyaları bulur.
- Kod üretir/düzenler, refactor yapar ve değişiklikleri uygular.
- Unit ve integration testleri yazıp çalıştırır.
- Lint, test, build ve kalite adımlarını (ör. Sonar/SAST) pipeline içinde tetikler.
- Hata çıktılarından iteratif düzeltme yapar ve yeniden test eder.
- Değişiklikleri commit/PR akışına taşır (repo izinleri ve koruma kurallarına bağlı).

### Nasıl çalışır?
- Yerel kullanımda: Geliştirici terminalden görev verir; agent dosya düzenleme + komut çalıştırma + doğrulama adımlarını zincirler.
- CI/CD kullanımında: API üzerinden job içinde tetiklenir; verilen talimata göre kod, test ve kalite adımlarını yürütür.
- Sonuç üretimi: Çalıştırdığı adımların çıktısına göre değişiklik seti, test sonucu ve PR/commit çıktısı üretir.

### CI/CD içinde örnek otomasyon senaryoları
- Sonar/kalite kapısı kırılırsa: raporu analiz eder, ilgili modülde düzeltmeyi yapar, testleri çalıştırır ve “Fix Sonar issues” PR’ı açar.
- Yeni feature branch açıldığında: ilgili değişikliklere göre eksik unit testleri tamamlar, testleri koşar ve PR’a ek commit üretir.
- Integration test flakiness: başarısız test loglarını analiz eder, flaky testi stabilize edecek iyileştirmeyi yapar (timeout, mock, test data) ve PR üretir.
- Dependency güncellemesi: lockfile değişikliklerini uygular, breaking change notlarını özetler, gerekli kod uyarlamasını yapar, regresyon testlerini koşar ve PR açar.
- Güvenlik taraması bulguları: SAST/Dependency scan çıktısına göre CVE fix PR’ı üretir ve risk/etki özetini PR açıklamasına ekler.
- Release hazırlığı: changelog taslağı üretir, versiyon bump (semver) önerir, release branch için PR hazırlar.
- Deploy öncesi smoke: staging’e deploy öncesi smoke test senaryolarını koşar; başarısızlıkta loglardan kök neden analizi çıkarıp düzeltme PR’ı önerir/üretir.

### Güvenli ve sürdürülebilir kullanım önerisi
- Ana dala doğrudan push yerine PR zorunluluğu kullanın.
- Prod deploy için environment approval veya manuel onay adımı ekleyin.
- Secret'ları sadece CI secret store'da tutun (API key, Sonar token, deploy key).
- Agent görevlerini küçük ve doğrulanabilir parçalara bölün; her adımda kalite kapısı çalıştırın.

---

## Öneri Özeti

Öncelik otomasyon (Sonar, test üretimi/çalıştırma, commit/PR, deploy akışları) ise birincil tercih Claude Code (Team + API) olmalıdır. Geliştiriciler günlük işlerde Claude Code Team (seat) kullanırken, CI/CD içinde bot benzeri işler Claude Code API ile güvenli ve ölçeklenebilir şekilde otomatikleştirilebilir.

Otomasyon ağırlıklı bir ihtiyaç varsa Claude Code (Team + API) en kapsamlı seçenek. Geliştiriciler günlük kullanımda Claude Code Team (seat) ile ilerleyebilir; CI/CD içinde bot gibi çalışacak otomasyonlar içinse Claude Code API bazlı kurulum daha doğru olur. Ekipte sadece geliştiriciler Premium seat alırken diğerleri Standard seat kullanabilir; API tarafında da kullanım limiti ve maliyet takibi merkezi yönetilebilir.

Cursor Business, günlük geliştirme hızında ve düşük geçiş maliyetinde hala güçlü bir seçenek olsa da tam otonom CI/CD görevlerinde Claude Code kadar uygun değildir; bu nedenle otomasyon öncelikli ekiplerde ikincil konumda değerlendirilmelidir.

