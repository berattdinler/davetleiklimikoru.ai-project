# 🌱 Davet Ağacı

**Davet et. Puan kazan. Sponsor desteğiyle fidan dik.**

**Davet Ağacı**, özellikle lise öğrencilerinin çevre farkındalığını ve aktif katılımını artırmak amacıyla tasarlanmış, sıfır dış bağımlılığa sahip tek dosyalık bir etkileşimli web prototipidir. Kullanıcıların cebinden gerçek para çıkmadan; arkadaşlarını davet ederek kazandıkları puanları sponsor bütçeleri aracılığıyla sembolik fidanlara dönüştürmesini simüle eder.

---

## 🎯 Projenin Amacı ve Temel Değer Döngüsü

Proje, sosyal etki ile oyunlaştırmayı (gamification) birleştirerek sürdürülebilir bir referans zinciri kurar:

1. **Davet Gönderimi:** Kullanıcı kendisine atanan davet kodunu (`FIDAN-XXXX`) arkadaşlarıyla paylaşır.
2. **Katılım ve Puan Kazanımı:** Davet kabul edildiğinde (simülasyonda `%70` olasılık) kullanıcı **+10 puan** kazanır.
3. **Fidan Talebi:** Kullanıcı **30 puana** (3 başarılı davet) ulaştığında 1 sembolik fidan dikme hakkı elde eder.
4. **Sponsor Havuzu:** Fidan maliyeti kullanıcıdan değil, sisteme bütçe tanımlayan sponsor kuruluşlardan karşılanır.
5. **Görsel Geri Bildirim (`--gr`):** Dikilen her fidan ve kabul edilen davet, sayfanın arka planını ve tepe grafiğini (`Math.min(1, pl * 0.085 + acc * 0.012)`) kademeli olarak yeşertir.

### ⚠️ Sistem Riski: "Tavuk-Yumurta" Darboğazı
Kullanıcılar hızla puan biriktirip fidan dikmek istediğinde sponsor bütçesi tükenmişse sistem işlemi iptal etmez; fidanı **bekleme kuyruğuna (`q`)** alır. Yeni bir sponsor havuza bütçe eklediğinde (`drain()`), sırada bekleyen fidanlar otomatik olarak dikilir.

---

## 🛡️ Güvenlik ve Anti-Suistimal Mekanizması

Referans tabanlı sistemlerde sıkça görülen sahte hesap açarak puan kasma (*self-referral*) girişimlerini engellemek için iki katmanlı bir koruma kurgulanmıştır:

* **Günlük Hız Sınırı (Rate Limiting):** Her kullanıcı günde en fazla **5 davet hakkına** sahiptir. Hak dolduğunda butonlar kilitlenir ve ertesi güne kadar yeni davet gönderilemez.
* **Olasılıksal Tespit ve İnceleme:** Aynı cihaz veya oturum üzerinden açılan şüpheli hesaplar `%80` ihtimalle anında engellenip puan dışı bırakılır; `%20` ihtimalle ise inceleme kuyruğuna alınır.

---

## 📱 Mobil Prototip Tasarımı (3 Temel Ekran)

Uygulamanın mobil arayüz eskizleri gizlilik ve sade kullanıcı deneyimi (UX) odaklı 3 ekrandan oluşur:

1. **Kayıt Ekranı:** Ad-soyad, telefon veya e-posta gibi kişisel veriler toplanmaz. Yalnızca **takma ad**, yaş aralığı ve 18 yaş altı kullanıcılar için **veli onayı** istenir.
2. **Ana Ekran:** Kullanıcının mevcut puanı, 30 puana kalan ilerleme çubuğu, kişisel davet kodu ve kalan günlük davet hakkı tek bakışta sunulur.
3. **Liderlik Tablosu:** İlk 3 sıradaki takma ad öne çıkarılır ve kullanıcının kendi sıralaması alt kısımda sabit gösterilerek akran motivasyonu desteklenir.

---

## ✋ Harici Model Kullanmadan Kamera ile El Kontrolü

Proje, erişilebilirlik ve temassız etkileşim sunmak amacıyla TensorFlow veya MediaPipe gibi harici yapay zeka kütüphaneleri indirmeden **%100 cihaz içi** çalışan hafif bir bilgisayarlı görü algoritması içerir:

1. **Düşük Çözünürlüklü Örnekleme (`80×60` px):** Kamera görüntüsü saniyede 20 kare (`50 ms` aralıkla) `4.800` piksellik bir tuvale aktarılır.
2. **YCbCr Ten Rengi Filtresi:** Işık değişimlerinden daha az etkilenmek için RGB pikselleri `YCbCr` renk uzayına çevrilir ve kalibre edilen ten rengi aralığı (`Cb`, `Cr`) filtrelenir.
3. **Hareket Geçmişi (`mh`):** Arka plandaki sabit nesneleri elemek için yalnızca son ~2 saniye içinde parlaklık değişimi (`|Y - prev| > 10`) gösteren pikseller aktif tutulur.
4. **Parmak Ucu Tespiti:** Bağlantılı bileşen analizi (*Flood-Fill*) ile en büyük el bölgesi bulunur ve bu bölgenin en üst noktası (`min Y`) işaret parmağı ucu olarak izlenir.
5. **Dwell-Click (1 Saniye Bekleme):** İmleç bir butonun üzerinde **1 saniye** sabit tutulduğunda tıklama tetiklenir; ekranın en üst veya en alt kenarına gidildiğinde ise sayfa otomatik kaydırılır.

---

## 🎓 Bilimsel Araştırma Tasarımı ve Etik İlkeler

* **Araştırma Sorusu:** *Referans tabanlı, puanlı bir dijital platform; klasik bilgilendirmeye (afiş/broşür) kıyasla lise öğrencilerinin çevre farkındalığını ve katılımını daha fazla artırır mı?*
* **Deneysel Yöntem:** Kontrol grubu (afiş/broşür) ve deney grubu (platform kullanımı) arasında ön-test ve son-test anketleriyle karşılaştırma yapılır.
* **Gizlilik ve Etik:**
  * Gerçek para toplanmaz; tüm finansal akış sembolik simülasyondur.
  * Kamera görüntüsü hiçbir sunucuya gönderilmez ve kaydedilmez.
  * Uygulama öncesinde veli onam formu ve gerekli kurumsal/MEB izinleri esas alınır.

---

## 🚀 Kurulum ve Çalıştırma

Proje tamamen **HTML, CSS ve Vanilla JavaScript** ile yazılmıştır; herhangi bir derleme aracı (`npm`, `webpack` vb.) gerektirmez:

1. `Davet Ağacı.html` dosyasını bilgisayarınıza indirin.
2. Dosyayı modern bir web tarayıcısında (Chrome, Edge, Safari, Firefox) doğrudan açın.
3. *(İsteğe bağlı)* Kamera ile el kontrolünü test etmek için sayfayı `localhost` veya `HTTPS` üzerinden açıp kamera izni verin.