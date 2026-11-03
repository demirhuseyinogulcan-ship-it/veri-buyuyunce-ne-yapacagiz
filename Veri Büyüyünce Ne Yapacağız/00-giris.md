# Bilgisayar bilimi temelleri (Türkçe çeviri)

- 00-giris.md: Kursun tanıtımı
- 01-makine-ve-bellek.md: Modül 1, verinin makinede nasıl durduğu
- 02-is-nasil-buyur.md: Modül 2, Büyük O ve büyüme şekilleri
- 03-sirali-veri-yapilari.md: Modül 3, dizi, bağlı liste, yığıt, kuyruk
- 04-hizli-bulan-yapilar.md: Modül 4, hash tablosu, ağaç, öncelik yığını, çizge
- 05-calisma-zamani-ve-vaka.md: Modül 5, derleme, çöp toplama, iş parçacıkları, async ve gerçek bir vaka

---

## Kursun tanıtımı

Yüz kayıtla gayet iyi çalışan, ama gerçek veri gelince çok yavaşlayan bir kod yazdınız mı hiç? Ya da bir iş görüşmesinde "Büyük O" (Big O) lafını duyup terimi bildiğiniz halde kodunuz için ne anlama geldiğini bilmediğiniz oldu mu? Size tanıdık geliyorsa, bu çok yaygın bir boşluk. Çoğu yazılım kursu size kod yazmayı öğretir. Çok azı, veri büyüyünce o kodun neye mal olduğunu gösterir.

Bu kurs zaten kod yazan ama arkasındaki bilgisayar bilimini hiç anlatılmamış kişiler için. Bazıları eğitim videolarından ya da işte öğrendi. Bazıları bir yoğun eğitim kampından (bootcamp) geçti ya da artık çoğunlukla yapay zeka araçlarıyla geliştiriyor. Bunların hepsi başlamak için iyi yollar. Sadece bir şeyi atlama eğilimindeler. O da kodun neden öyle çalıştığı ve bazen neden çalışmadığı.

Küçük bir örnek. Uygulamanız bir müşteri listesinde tekrar eden kayıtları kontrol ediyor diyelim. Her müşteriyi alıp listedeki diğer her müşteriyle karşılaştırıyor. 100 müşteride bu 100 × 100 eder. Yaklaşık 10.000 karşılaştırma, göz açıp kapayıncaya kadar biter. Şimdi liste 100.000 müşteriye çıksın. 100.000 × 100.000, yani 10 milyar karşılaştırma. Kod hiç değişmedi. Değişen sadece veri. Ve aynı kontrol artık dakikalar sürebilir.

Bir örnek daha. Bir müşteri kaydını, kopyası üzerinde indirim denemek için kopyalıyorsunuz. Kopyanın sipariş listesindeki fiyatları düşürüyorsunuz. Sonra asıl müşteriye bakıyorsunuz, onun fiyatları da düşmüş. Kodda yanlış görünen hiçbir şey yok. Bir kopya aldınız ve kopyayı değiştirdiniz. Ama yalnız kayıt kopyalandı, içindeki sipariş listesi kopyalanmadı. İki kayıt da bellekteki aynı listeyi gösteriyor. Bu yüzden birinden yapılan değişiklik diğerinde de görünüyor.

Bu iki hata, kursun sürekli geri döndüğü iki soruya götürüyor. Birincisi: **Nasıl büyür?** Bu, tekrar kontrolü örneği. Veri çok büyüyünce kodunuzun zamanı ve belleği ne olur? İkincisi: **Nerede durur?** Bu da paylaşılan sipariş listesi örneği. Program çalışırken veriniz nerede duruyor? Yalnız sizin mi, yoksa başka bir şeyle paylaşılıyor mu? Bu iki soruyu sormaya başladığınızda yavaş kod ve tuhaf hatalar tahmin oyunu olmaktan çıkar.

Kurs makinenin kendisiyle başlıyor. Bir sırayı kutular çizip doldurarak bir sayıyı sıfır ve birlere çevireceksiniz. Bir sayacın neden birden eksiye düşebildiğini ve 0,1 + 0,2'nin neden tam 0,3 etmediğini göreceksiniz. Bazı belleklerin elektrik kesildiği anda neden her şeyi unuttuğunu da göreceksiniz.

Sonra kurs, veri büyüdükçe işin nasıl büyüdüğüne bakıyor. Büyük O'yu bir tabloyu ezberleyerek değil, gerçek döngüleri okuyarak öğreneceksiniz. Ardından her gün kullandığınız veri yapıları geliyor: diziler (array), hash tabloları (hashmap), ağaçlar (tree) ve çizgeler (graph). Her biri için soru basit: Neye mal olur ve ne zaman seçilmeli?

Son bölüm, kodunuz çalışırken neler olduğuna bakıyor. Çöp toplama (garbage collection), iş parçacıkları (thread), yarış durumları (race condition) ve asenkron kod. Sonra hepsi gerçek hayattan bir vakada birleşiyor: yavaş bir API uç noktası. Uygulamanızın çağırdığı bir web servisindeki tek bir adres, baştan sona inceleniyor.

Belirli bir programlama dili bilmenize ya da ileri matematiğe gerek yok. Biraz kod yazdıysanız hazırsınız. Her ders ekranda şemalarla gerçek bir örnek üzerine kurulu. Arada bir, ders sizden bir şeyi önce kendiniz çözmenizi istiyor. Cevabı duymadan önce videoyu durdurup deneyin. Dersler kısa ve her biri öncekilerin üzerine kuruluyor, mümkünse sırayla izleyin. Her modülde ayrıca kendinizi sınamak için bir test, notlar ve mülakat soruları var.

Yapay zeka saniyeler içinde kod yazabildiği için bu artık daha da önemli. Birinin o kodun doğru olup olmadığına ve dayanıp dayanmayacağına yine de karar vermesi gerekiyor. O kişi sizsiniz. Bu fikirler kafanızda netleşince başkasına anlatmak da kolaylaşıyor, mesela yeni bir takım arkadaşına ya da bir mülakatçıya.

Makinenin verinizi nasıl sakladığıyla başlıyor ve gerçek bir hata ayıklama vakasıyla bitiyor. Kodunuzun altında neler olduğunu anlamak istiyorsanız başlayalım.
