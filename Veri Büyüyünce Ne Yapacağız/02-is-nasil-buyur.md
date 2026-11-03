# Modül 2: İş nasıl büyür?

## Modüle giriş

Çoğu kod, onunla test ettiğiniz veride gayet iyi çalışır. Belki birkaç yüz satır, belki bir avuç kullanıcı. Sorun gerçek veri geldiğinde ve bin kat büyük olduğunda başlar.

Diyelim bir rapor bugün 1 saniye sürüyor. Gelecek yıl veri 10 kat büyük. Rapor 10 saniye mi sürer, 100 mü? İkisi de mümkün. İşin şekline bağlı ve bu şekli çoğu zaman sadece kodu okuyarak görebilirsiniz.

Bu modül, onu görmeyi soru soru öğretiyor. İki kod parçasını adil şekilde nasıl karşılaştırırsınız? İşlerinin nasıl büyüdüğünü nasıl yazarsınız? Hangi büyüme şekilleri ucuz, hangileri dert? Onları gerçek kodda nasıl fark edersiniz? Peki ya bellek? Sonra büyüme çok dik olduğunda çözümler geliyor: cevapları hatırlamak, sıralamak ve aramak. En sonda da hiç hızlı cevabı olmayan problemler var.

Bu, "nasıl büyür?" sorusu. Elinizin altında tutun, kursun geri kalanı ona dayanıyor. O zaman bir kronometrenin neden yetmediğiyle başlayalım.

## Kronometre neden yetmez?

Beş sayı: 6, 3, 8, 2 ve 5. Hangisi en büyük? Bir saniye düşünün. 8.

Şimdi onu nasıl bulduğunuzu düşünün. İlk sayıya baktınız ve onu şimdiye kadarki en büyük olarak aklınızda tuttunuz. Sonra her sonraki sayıyı onunla karşılaştırdınız. 8'e gelince onu değiştirdiniz. Sayılar bitince cevabınız 8'di.

Bu bir **algoritma**. Algoritma, bir girdiyi cevaba dönüştüren açık adımlar dizisidir. Kodunuz bunlarla dolu. Ve çoğu iş birden fazla algoritmayla yapılabilir.

Peki hangisinin daha iyi olduğunu nasıl anlarsınız? Daha iyi, okunması daha kolay ya da belleğe daha hafif demek olabilir. Burada soru hız.

Akla gelen fikir onları zamanlamak ama bir kronometre sandığınızdan az şey söyler. Aynı kod yeni bir dizüstü bilgisayarda, bir web sitesini çalıştıran eski bir sunucudan daha hızlı çalışır. Tek bir makinede bile iki çalıştırma nadiren aynı süreyi verir. Ve bin kayıt için ölçülen süre bir milyon kayıt hakkında hiçbir şey söylemez.

Bu yüzden kodu zamanlamak yerine adımlarını sayın. Adım sayısı her makinede aynıdır. Sonra veri büyüdükçe bu sayının nasıl değiştiğini sorun. Buna büyümenin şekli denir.

Gerçek bir örnek. Bir internet mağazası her gece bir rapor hazırlıyor. Her sipariş için onu veren müşteriyi buluyor. Basit algoritma, her bir sipariş için bütün müşteri listesini baştan tarıyor.

Küçükten başlayın. 100 sipariş ve 100 müşteriyle bu yaklaşık 10.000 kontrol. Göz açıp kapayıncaya kadar biter.

Şimdi mağaza 10 kat büyüsün, 1.000 sipariş ve 1.000 müşteri. Devam etmeden tahmin edin: Rapor ne kadar fazla iş yapar? Her sipariş artık 1.000 müşteri tarıyor. Yani yaklaşık bir milyon kontrol. Veri 10 kat büyüdü, iş 100 kat büyüdü. Bir 10 kat daha büyütün, yaklaşık 100 milyon kontrol olur. Artık rapor yavaş ve daha hızlı bir sunucu size yalnız biraz zaman kazandırır.

Şimdi farklı bir algoritma deneyin. Önce müşterileri bir arama tablosuna koyun, her müşteri sözlükteki kelimeler gibi müşteri numarasının altına dosyalansın. Bir müşteriyi bulmak artık yaklaşık bir adım sürer. Yani 100 sipariş yaklaşık 100 adım, 10.000 sipariş yaklaşık 10.000 adım tutar. Veri 10 kat büyüyünce iş de 10 kat büyür.

İki algoritma da aynı raporu hazırlıyor. Küçük veride kronometre onları ayırt edemez. Ama ilkinin işi verinin karesiyle büyür, ikincisininki veriyle aynı adımda.

Mühendislerin bu şekilleri yazmanın kısa bir yolu var. Adı **Büyük O** (Big O). Yani bir mülakatçı "zaman karmaşıklığı nedir?" diye sorduğunda kastettiği şu: Çalışma süresi nasıl büyüyor?

Bir dahaki sefere biri size milisaniye cinsinden bir sayı verdiğinde daha iyi soruyu sorun: Veri 10 kat büyüyünce ne olur?

## Büyük O nasıl okunur?

Adım saymak zamanlamaktan iyidir. Ama her seferinde "iş verinin karesiyle büyür" demek çabuk yorar. Bu yüzden mühendisler onun yerine kısa bir etiket kullanır: Büyük O. Matematik gibi görünür ama aslında bir desenin adından ibarettir.

Nasıl okunacağı şöyle. Bir milyon isimlik bir liste düşünün, dev bir rehber gibi. Bir kişiyi bulmak istiyorsunuz.

En yavaş yolla başlayın. İlk isme bakın, sonra sonrakine, kişiyi bulana kadar tek tek. Şanssızsanız milyonun hepsine bakarsınız. Şimdi iki kat uzun bir liste alın. İki kat fazla isme bakmanız gerekir. İş, listenin uzunluğuyla aynı adımda büyür.

Mühendisler bunu şöyle yazar. Girdinin ne kadar büyük olduğu için, yani koda verdiğiniz veri için N harfini kullanırlar. Burada N listedeki isim sayısı. Sonra deseni O(N) diye yazarlar. O, "order" (mertebe) demek, yani iş N mertebesinde. Gündelik adı **doğrusal** (linear), çünkü bir grafiğe çizseniz düz bir çizgi olurdu.

Diğer desenler de aynı tür etiket kullanır. O(1), liste ne kadar uzarsa uzasın işin aynı kaldığı anlamına gelir. O(N²), işin N × N ile büyüdüğü anlamına gelir, her ismi diğer her isimle karşılaştırmak gibi. Yalnız 10 isim bile yaklaşık 100 karşılaştırma demek.

Büyük O yalnız N gerçekten büyüyünce ne olduğuyla ilgilenir, bu yüzden küçük şeyleri görmezden gelir. Bir örnek. Aradığınız kişinin listenin ilk yarısında bir yerde olduğunu biliyorsunuz, yalnız ilk yarıya bakıyorsunuz. Bu iki kat hızlı, harika. Ama listeyi ikiye katlayın, iş yine ikiye katlanır. N'nin yarısı yine düz bir çizgi, sadece daha yatık. Yani yine O(N).

Aynısı sabit ek iş için de geçerli, 50 adımlık bir hazırlık gibi. Bir milyon ismin yanında 50 adım hiçtir. Her adımı hızlandırmak sizi hızlandırır. Ama şekli yalnız farklı bir fikir değiştirir.

Şimdi bir işin her ismi diğer her isimle karşılaştırdığını ve sonra listeyi bir kez daha dolaştığını düşünün. Bu N² adım artı N adım daha. Bir milyon isimle N², bir milyon × bir milyon. Bu bir trilyon. Ek dolaşma yalnız 1 milyon ekler. Bir trilyonun yanında bir milyon, bir milyon sayfalık bir yığına eklenmiş bir sayfa gibidir. Hiç fark etmezsiniz. Yani bu O(N²). En hızlı büyüyen kısmı tutun, gerisini atın.

Kendiniz bir tane deneyin. Bir iş, bilgilerini düzenlemek için her müşteriyi bir kez dolaşıyor. Sonra e-postalarını kontrol etmek için hepsini bir kez daha dolaşıyor. Son olarak listedeki en ilk müşteriyi alıyor. Büyük O nedir? Durun ve çözün.

İki tam dolaşma 2N eder. İlk müşteriyi almak bir adım, yani 2N + 1. Küçük şeyleri atın, cevap O(N). İki dolaşma iki kat uzun sürer ama şekil yine düz bir çizgidir.

Bir şey daha. Aynı arama şanslı ya da şanssız olabilir. Aradığınız isim listede ilk olabilir. Bu en iyi durum. Ya da son olabilir. Bu en kötü durum. İsmi bulur bulmaz durmak iyi bir alışkanlıktır ama isim sondaysa işe yaramaz. Bu yüzden insanlar hangi durum olduğunu söylemeden bir Büyük O verdiklerinde genellikle en kötüsünü kastederler. Emin değilseniz sorun.

Yani kodunuzu inceleyen biri O(N²) yazdığında size not vermiyor. N büyüyünce ne olacağını söylüyor.

## Ucuz büyüme şekilleri

Artık O(N) gibi bir etiketi okuyabiliyorsunuz. Sonraki soru, hangi etiketlerin iyi haber olduğu. Üçü öyle. Veri büyüyünce bu işler neredeyse fark etmez. Bunlar ucuz büyüme şekilleri.

Onları görmek için bir müşteri listesi alın ve bin müşteriden bir milyona büyümesine izin verin. Sonra üç gündelik işin ne kadar iş tuttuğunu izleyin.

İlk iş, 500. sıradaki müşteriyi almak. Numaralı dolaplardan oluşan bir sıra düşünün. Dolap numarasını biliyorsanız doğrudan ona gidersiniz. Önündeki her dolabı açmazsınız. Bir liste de aynı şekilde çalışır. Bilgisayar doğrudan 500. sıraya atlar. Bu, bin müşteri için bir adım, bir milyon için de yine bir adım. Bu şekil O(1) diye yazılır ve adı **sabittir** (constant). Listenin boyutu hiç önemli değil.

İkinci iş, ada göre sıralı bir listede bir müşteriyi adıyla bulmak. Sözlükte bir kelime aradığınızı düşünün. Ortaya yakın bir yerden açarsınız. Kelimeniz daha sonra geliyorsa ilk yarıyı görmezden gelirsiniz. Sonra kalan kısmın ortasını açar ve tekrarlarsınız. Her bakış kalanın yarısını atar. Buna **ikili arama** (binary search) denir.

Bin müşteri için kaç bakış sürer? Durun ve tahmin edin. Yaklaşık 10. 1.000 yarıya iner, 500, sonra 250 ve böylece yaklaşık 10 adımda bire kadar. Bir milyon müşteri için yalnız yaklaşık 20. Bu şekil O(log N) diye yazılır ve adı **logaritmiktir**. Liste bin kat büyüdü ama iş yalnız ikiye katlandı.

Bu fark ham hızı yener. Diyelim isimleri tek tek kontrol eden 100 kat hızlı bir bilgisayarınız var, bir de eski bir dizüstünde yarıya bölmeyi kullanıyorsunuz. 100 isimle hızlı bilgisayar rahatça kazanır. Ama bir milyon isimle hızlı bilgisayar yine de dizüstünün 10.000 adımı kadar süre harcar, eski dizüstü ise yalnız 20 adıma ihtiyaç duyar. Belirli bir boyutun ötesinde daha iyi şekil her zaman kazanır.

Üçüncü iş, en çok harcayan müşteriyi bulmak. Bu sefer kısayol yok. Bir yığın sınav kağıdında en yüksek notu bulan bir öğretmen düşünün. Her kağıda bir kez bakıp şimdiye kadarki en iyiyi hatırlamak zorundadır. Bin müşteri için bin adım. Bir milyon için bir milyon. Bu şekil O(N) diye yazılır ve adı doğrusaldır. Liste ikiye katlanınca iş ikiye katlanır.

Üçü de pratikte ucuz. Doğrusal bile çoğu iş için iyidir, çünkü genellikle veriye en az bir kez bakmak zorundasınız zaten. Ama sıralı bir liste kullanabiliyor ya da doğrudan bir sıraya atlayabiliyorsanız o işin neredeyse tamamını atlayabilirsiniz.

Akılda kalsın: Sabit büyümez. Logaritmik neredeyse büyümez, çünkü her adımda işi yarıya indirir. Doğrusal veriyle aynı adımda büyür. Bu üç şekil veri ne kadar büyürse büyüsün ucuz kalır.

## Pahalı büyüme şekilleri

Diğer üçü öyle değil. Bunlar pahalı büyüme şekilleri. Onlarla kod testte hızlı, gerçek veride çok yavaş olabilir. Aynı büyüyen müşteri listesini alıp üzerinde üç zor iş çalıştırın.

İlk iş, listeyi harcamaya göre sıralamak. Uzun bir sınav kağıdı yığınını nota göre sıraladığınızı düşünün. İyi bir yöntem yığını daha küçük destelere böler, onları sıralar ve sonra tekrar birleştirir. Bin müşteri için bu yaklaşık 10.000 adım tutar. Bir milyon için yaklaşık 20 milyon. Bu şekil O(N log N) diye yazılır. Doğrusaldan biraz kötü ama çok değil. İyi sıralamanın şekli bu.

İkinci iş, aynı adresi paylaşan müşterileri bulmak. Basit yol her müşteriyi diğer her müşteriyle karşılaştırır. Herkesin diğer herkesle tokalaştığı bir oda düşünün. Dört kişi altı tokalaşma yapar. 10 kişi 45. 100 kişi yaklaşık 5.000. Grup ikiye katlanınca tokalaşma yaklaşık dört katına çıkar.

Şimdi bunu müşteri listesine uygulayın. Durun ve tahmin edin, bir milyon müşteri için ne olur? Bin müşteri yaklaşık yarım milyon karşılaştırma demek, sorun değil. Bir milyon müşteri yaklaşık yarım trilyon demek, bu çok fazla yavaş. Bu şekil O(N²) diye yazılır ve adı **ikinci dereceden** (quadratic). Kodda genellikle iç içe bir döngü olarak görünür.

Üçüncü iş en kötüsü. Pazarlama, indirim tekliflerinin mümkün olan her kombinasyonunu denemek istiyor. Burada N müşteri değil, teklif sayısı. Bir sıra ışık anahtarı düşünün. Her yeni anahtar onları ayarlayabileceğiniz yol sayısını ikiye katlar. Bir anahtar iki ayar verir. 10 anahtar yaklaşık bin. 20 anahtar yaklaşık bir milyon. 30 anahtar yaklaşık bir milyar. Bu şekil O(2ᴺ) diye yazılır ve adı **üsteldir** (exponential). Sadece birkaç düzine öğeden sonra çok yavaşlar.

Akılda kalsın: N log N iyi sıralamanın bedelidir ve sorun değil. İkinci dereceden küçük listeler için iyi, büyükler için çok yavaştır. Üstel ise ancak N çok küçük kaldığında kullanılabilir.

## Şekli kodda fark etmek

Altı şekli adıyla biliyorsunuz artık. İşe yarayan beceri, onları kendi kodunuzda çalıştırmadan fark etmek.

Bir partide misafirleri kontrol etmeyi düşünün. Her misafir davetiyesini kapıda bir kez gösterirse iş misafir sayısıyla büyür. Ama her misafirin diğer her misafirle tanıştırılması gerekiyorsa iş çok daha hızlı büyür. Kodda da aynı iki desen var. İpuçları döngülerde.

Tek satırlarla başlayın. İki sayıyı toplamak, iki değeri karşılaştırmak, bir değeri saklamak ya da bilinen bir sıradaki öğeyi okumak birer adım sürer. Sayılar ne olursa olsun maliyetleri aynıdır.

Şimdi döngüler için en basit kural. Veri üzerinde tek bir döngü doğrusal demektir. Bir listede n öğe varsa ve döngü her birini bir kez ziyaret ediyorsa bu n adımdır.

Şimdi o döngünün içine bir döngü daha koyun, ikisi de aynı veri üzerinde. Her öğe için iç döngü her öğeyi tekrar ziyaret eder. Bu n × n, yani ikinci dereceden.

Şimdi iki döngüyü iç içe değil, yan yana koyun, biri ötekinin ardından. İlk döngü n adım sürer. Sonra ikinci döngü n adım daha. Bu 2n, yine doğrusal.

Hatırlanacak kural şu: **Art arda gelen döngüler toplanır. İç içe döngüler çarpılır.**

Bir şartı var. Bu kural iki döngünün de aynı veri üzerinde döndüğünü varsayar. Diyelim bir döngü müşteriler, diğeri siparişler üzerinde dönüyor. Bunlar farklı boyutlar, o yüzden onlara farklı harfler verin, a ve b. Yan yana a + b eder. İç içe a × b. Ancak iki liste birlikte büyüyorsa n² deyin.

Bu kurallar çoğu zaman işe yarar ama kod sizi kandırabilir. Bu yüzden döngüleri değil, işin gerçekte kaç kez yapıldığını sayın.

Doğrusal görünen ama öyle olmayan bir döngü. Sayaç 1'den başlıyor ve n'yi geçene kadar her seferinde ikiye katlanıyor. Yani 1, 2, 4, 8 diye gidiyor. Bir milyon için yaklaşık 20 turdan sonra durur. Bu n değil, log n.

Bir de doğrusal görünen ama sabit olan. Veri ne kadar büyürse büyüsün her zaman tam 10 kez dönen bir döngü, sabit bir iş parçasıdır. Yani O(1).

Şimdi gözünüzü test edin. Üç kısa durum var, cevaplamadan önce her birini tahmin edin.

Birincisi. Bir fonksiyon her müşteriyi dolaşıyor. Her biri için e-postasının engellenmiş e-postalar listesinde olup olmadığını kontrol ediyor. Şekil ne? Görebildiğiniz tek bir döngü var. Ama bir e-postanın bir listede olup olmadığını kontrol etmek gizli bir döngüdür. O tek satırın arkasında bilgisayar her seferinde bütün engel listesini dolaşır. Yani iş, n × o listenin boyutu. İki liste birlikte büyüyorsa bu ikinci dereceden. Şimdi engellenmiş e-postaları hızlı arama için yapılmış bir koleksiyon olan bir hash kümesinde (hash set) saklayın. Her kontrol yaklaşık bir adım sürer ve işin tamamı doğrusal olur.

İkincisi. Bir fonksiyon adreslerini kontrol etmek için her müşteriyi dolaşıyor. Sonra ayrı bir döngüde toplamını kontrol etmek için her siparişi dolaşıyor. Şekil ne? İki döngü yan yana duruyor, yani işleri toplanıyor. Ama farklı listeler üzerinde dönüyorlar. Yani a + b, a müşteri sayısı ve b sipariş sayısı.

Üçüncüsü. Her müşteri için iç döngü onu yalnız listede kendinden sonra gelen müşterilerle karşılaştırıyor. Yani iç döngü her seferinde kısalıyor. Bu ikinci dereceden daha ucuz mu? Yaklaşık yarı iş. Ama daha önce gördüğünüz gibi işi yarıya indirmek şekli değiştirmez. Yine n².

Yani bir şey yavaşsa önce şeklini bulun. İkinci derecedense iç içe döngüyü arayın. Sonra iç döngünün yerine bir hash kümesi ya da arama tablosu geçip geçemeyeceğini sorun. O tek değişiklik çoğu zaman n²'yi n'ye çevirir.

## Bellek de büyür

Şimdiye kadar her şekil zamanı ölçtü. Ama kod bellek de kullanır ve bellek önce tükenebilir.

Şöyle görünür. Bir mağaza, dükkan kapalıyken her gece küçük bir program çalıştırıyor. Günün siparişlerini listeleyen bir dosyayı açıp üzerinde çalışıyor. Küçük bir test dosyasında gayet iyi çalışıyor. Sonra gerçek dosya 100 kat büyük geliyor. Program yavaşlamıyor. Bellek yetersiz hatasıyla çöküyor. Makinenin sahip olduğundan daha fazla belleğe ihtiyacı vardı.

Yani zamanın nasıl büyüdüğünün yanında belleğin nasıl büyüdüğünü de sorabilirsiniz. Buna **alan karmaşıklığı** (space complexity) denir ve aynı Büyük O şekillerini kullanır. Genellikle yalnız bir işin, kendisine verilen verinin üstüne ihtiyaç duyduğu ek belleği sayarsınız. O ek bellek yeni değerlerden, yeni listelerden ve hatta fonksiyon çağrılarından gelir, çünkü hâlâ bekleyen her çağrı kendi ayrıntılarını bellekte tutar.

Bir masada bir deste sipariş formuyla çalıştığınızı düşünün. Bazı işler sadece bir yapışkan not ister. Bazıları her formun fotokopisini ister. Bazıları da duvar büyüklüğünde bir tablo.

Yapışkan not işi, her siparişin toplamını toplamak. Tek bir yürüyen toplam tutarsınız, o kadar. 10 sipariş de olsa 10 milyon da olsa tek bir sayı. Bu O(1) alan.

Fotokopi işi, her siparişin temizlenmiş bir kopyasını çıkarmak. Kopya asıl kadar uzundur. Siparişleri ikiye katlayın, belleği ikiye katlarsınız. Bu O(N) alan.

Duvar tablosu işi, her siparişi diğer her siparişle karşılaştırıp her sonucu yazmak. Her çift için bir kutu. Bir milyon sipariş bir trilyon kutu demek. Bu O(N²) alan ve hiçbir normal makineye sığmaz.

Çoğu zaman belleği zamanla takas edebilirsiniz. Diyelim herhangi bir sipariş numarasının iki kez geçip geçmediğini bilmek istiyorsunuz. Bir yol her siparişi diğer her siparişle karşılaştırmak. Yavaş ama ek bellek gerektirmiyor. Diğer yol desteyi bir kez dolaşmak ve zaten gördüğünüz sipariş numaralarından bir hash kümesi tutmak. Hızlı ama küme veriyle birlikte büyüyor. Belleği harcayıp zaman satın aldınız. Doğru seçim hangisinden daha kıt olduğunuza bağlı.

Bir tane deneyin. Bir fonksiyonun en eski 10 siparişe ihtiyacı var. Bütün destenin fotokopisini çekiyor, kopyayı tarihe göre sıralıyor ve ilk 10'u alıyor. Bu ne kadar ek bellek kullanır? Durun ve düşünün. Kopya deste kadar büyük. Yani O(N) ek alan.

Daha iyi bir yol: Desteyi bir kez dolaşın ve şimdiye kadar görülen en eski siparişler için yalnız 10 yuva tutun. Bin sipariş de olsa bir milyon da olsa bellek 10 yuva. Bu O(1) ek alan. Dev dosyada çökmekle bitirmek arasındaki fark bu.

Akılda kalsın: Belleğin de bir şekli vardır. Her şeyi kopyalamadan önce bir yapışkan notun yetip yetmeyeceğini sorun.

## Metni döngüde birleştirme tuzağı

Zaman da bellek de basit görünen tek bir kod satırında ters gidebilir. Neredeyse herkesi yakalayan bir tuzak var ve çok sıradan kodda saklanıyor.

Diyelim bir raporu satır satır oluşturuyorsunuz. Boş bir metinle başlıyorsunuz, sonra bir satır ekliyorsunuz, sonra bir tane daha, bin kez. Basit, doğrusal bir iş gibi görünüyor. Çoğu zaman öyle değil.

Nedenini görmek için tuhaf bir kuralla elle mektup yazdığınızı düşünün. Bir sayfaya ekleme yapmanıza izin yok. Bir satır eklemek için şimdiye kadar yazdığınız her şeyi temiz bir kağıda kopyalamak zorundasınız. Sonra yeni satırı en alta yazıyorsunuz.

İlk satır kolay. Hiçbir şey kopyalamıyor, bir satır yazıyorsunuz. İkinci satır için bir satır kopyalayıp bir satır yazıyorsunuz. Üçüncüsü için iki kopyalayıp bir yazıyorsunuz. Son satıra geldiğinizde sadece bir satır daha eklemek için neredeyse bin satır kopyalıyorsunuz.

Toplam ne kadar kopyalama bu? Durun ve tahmin edin. Hepsini topladığınızda bin satırlık bir mektup üretmek için yaklaşık yarım milyon satır kopyalanmış oluyor. Basit gibi hissettiren bir iş için ikinci dereceden bir iş. Raporu ikiye katlayın, kopyalama yaklaşık dört katına çıkar.

O tuhaf kural, pek çok popüler dilde metinlerin nasıl çalıştığıdır. Bir metin oluşturulduktan sonra değiştirilemez. Buna değişmez (immutable) denir. Yani bir metne ekleme yapmak aslında yepyeni, daha uzun bir metin yapıp eskisini onun içine kopyalamak demektir. Bazı diller bunu sessizce hızlandırır ama buna güvenemezsiniz.

Çözüm alışkanlığı değiştirmek. Önce her satırı kendi kartına yazın. Bitince bütün kartları sonda tek bir metinde birleştirin. Her satır yaklaşık bir kez kopyalanır, yani iş doğrusaldır. Kodda bu, parçaları bir listede toplayıp sonda birleştirmek demek. Python'da bunun için `join` var. Java ve C#'ta aynı işi yapan `StringBuilder` adlı bir araç var.

Akılda kalsın: Bir döngünün içinde bir şeyi biriktirerek oluşturduğunuzda her adımın gerçekte neyi kopyaladığını sorun. Ucuz görünen bir satır, kendisinden önce gelen her şeyi sessizce kopyalıyor olabilir.

## Özyineleme (recursion)

Şimdiye kadarki her örnek bir liste üzerinde dönen bir döngüydü. Ama bazı problemler, parçaların içinde parçalar olan bir ağaç şeklindedir. Döngüler onları zor bulur. Özyineleme kolaylaştırır.

Diyelim bir klasördeki, alt klasörlerindeki dosyalar dahil her dosyayı saymanız gerekiyor. Tek seviye kolay, bir döngü yazarsınız. Ama alt klasörlerin kendi alt klasörleri var ve ne kadar derine gittiklerini bilmiyorsunuz. O zaman kaç döngü yazarsınız?

Daha fazla döngüye ihtiyacınız yok. Bu klasördeki dosyaları sayan, sonra her alt klasör için kendini çağıran tek bir fonksiyon yazarsınız. Kendini çağıran fonksiyona **özyineleme** (recursion) denir.

Her özyinelemeli fonksiyonun iki parçası vardır. Birincisi **temel durum** (base case). Problemin cevabın apaçık olduğu en basit hali. Burada alt klasörü olmayan bir klasör. Fonksiyon sadece kendi dosya sayısını döndürür. İkinci parça özyinelemeli durum. Fonksiyon biraz iş yapar, sonra daha küçük bir şey üzerinde kendini çağırır. Burada kendi dosyalarını sayar, sonra her alt klasörün geri gönderdiğini ekler.

Bu, daha önceki tepsi yığını olan çağrı yığıtı (call stack) üzerinde çalışır. Her çağrı, hangi klasör üzerinde çalıştığı gibi ayrıntıları tutan kendi tepsisini alır. Bir çağrı bitince tepsisi kalkar ve altındaki çağrı kaldığı yerden devam eder.

Küçük bir örneği izleyelim. En üstteki klasörde iki dosya ve iki alt klasör var, A ve B. A klasöründe üç dosya var, başka bir şey yok. B klasöründe bir dosya ve bir alt klasör var, C. C'de de dört dosya var.

En üst klasörün çağrısı iki dosyasını sayar. Sonra A için kendini çağırır ve bekler. A'nın alt klasörü yok, yani bu bir temel durum. Üç geri gönderir. Üst çağrı devam eder ve B için kendini çağırır. B bir dosyasını sayar, sonra C için kendini çağırır ve o da bekler. Bu anda yığında üç çağrı var. En üst klasör en altta, B ortada, C en üstte. C bir temel durum, dört geri gönderir. B kendi birini ekler ve beş gönderir. Üst çağrı kendi ikisini, A'dan üçü ve B'den beşi ekler. Cevap 10 dosya.

Yığının hiçbir zaman üç tepsiden yüksek olmadığına dikkat edin. Her klasör seviyesi için bir tane. Yani özyinelemenin kullandığı bellek, ne kadar derine gittiğiyle büyür.

Bir tehlike var. Bir şey çok derin iç içeyse ya da temel durum eksikse yığın yer kalmayana kadar büyür. Sonra program bir stack overflow ile çöker. Yapılacak iş listesini kendiniz tutarak özyinelemeyi her zaman bir döngüye çevirebilirsiniz. Bu, yığının boyut sınırından kaçınır. Ama klasörler, menüler ya da cevaplı yorumlar gibi iç içe şeyler için özyinelemeli sürüm genellikle daha kısadır ve doğru yazması daha kolaydır.

Yani bir şeyin ne kadar derine gittiğini bilmiyorsanız seviyeleri saymayın. Her çağrının bir seviyeyi halletmesine izin verin.

## Cevapları hatırlamak: memoizasyon

Özyineleme bazı problemleri yazmayı kolaylaştırır. Ama bariz şekilde yazıldığında aynı işi tekrar tekrar da yapabilir. Bunu gösteren bir bulmaca var.

Altı basamaklı bir merdiven düşünün. Tek tek ya da ikişer ikişer çıkabilir, istediğiniz gibi karıştırabilirsiniz. Tepeye ulaşmanın kaç farklı yolu var?

Küçükten başlayıp sayın. Bir basamakta bir yol var. İki basamakta iki yol. İki tek adım ya da bir çift adım atabilirsiniz. Şimdi üç basamağı deneyin. Durun ve sayın. Üç yol var. Bir-bir-bir, bir sonra iki, ya da iki sonra bir.

Gerisini kolaylaştıran fikir şu. Son hamlenizi düşünün. Üçüncü basamakta durmak için ya ikinci basamaktan tek adımla ya da birinci basamaktan çift adımla geldiniz. Yani üçe ulaşma yolları, ikiye ulaşma yolları artı bire ulaşma yolları. 2 + 1 = 3 ve tutuyor.

Bu kural her basamak için geçerli. Altıya ulaşma yolları, beşe ulaşma yolları artı dörde ulaşma yolları. Bunu kendini çağıran bir fonksiyon olarak yazmak kolay. Beşi ve dördü sorar ve toplar.

Bu fonksiyon doğru ama israfçı. Altıyı hesaplamak için beşi ve dördü sorar. Ama beşi hesaplamak için dördü yine sorar. Yani dört iki kez hesaplanır. Üç, üç kez hesaplanır. İki, beş kez. Sadece altı basamak için fonksiyon altı farklı soruyu cevaplamak için 15 çağrı yapar.

Merdiven büyüdükçe çok daha kötüleşir. Her çağrı iki çağrıya daha bölünür ve çoğu zaten yapılmış işi tekrarlar. 50 basamak için on milyarlarca çağrı yapardı. Bu üstel büyüme.

Çözüm neredeyse fazla basit. Cevapları yazın. Her basamak sayısı için bir girişi olan küçük bir defter tutun. Kodda bu bir hash tablosu, bir sözlük gibi. Bir şeyi hesaplamadan önce fonksiyon deftere bakar. Cevap oradaysa onu kullanır. Değilse bir kez hesaplar, yazar ve devam eder.

Artık her soru yalnız bir kez cevaplanıyor. Altı basamak altı gerçek hesaplama tutar. 50 basamak 50. Üstel büyüme doğrusala döndü. Bedeli, defter için biraz bellek, basamak başına bir giriş.

Cevapları böyle yazmaya **memoizasyon** (memoization) denir. **Dinamik programlama** (dynamic programming) denen bir tekniğin bir biçimidir. Basit bir fikrin süslü adı: Büyük bir cevabı küçük cevaplardan kurun ve aynı şeyi hiç iki kez hesaplamayın.

Peki ne zaman kullanacağınızı nasıl bilirsiniz? İki işarete bakın. Birincisi, cevap aynı problemin daha küçük hallerinin cevaplarından kurulabiliyor. İkincisi, o küçük haller tekrar tekrar karşınıza çıkıyor. Bu ikinci işarete örtüşen alt problemler (overlapping subproblems) denir.

Özyinelemeli bir fonksiyon aynı soruları sormaya devam ediyorsa cevapları yazın.

## Aşağıdan yukarı dinamik programlama

Cevapları yazmak, özyinelemenin kendini tekrar etmesini durdurmanın bir yolu. İkinci bir yol var ve kendini çağıran bir fonksiyona hiç ihtiyaç duymuyor.

Aynı altı basamaklı merdiven bulmacasını alın. Kuralı zaten biliyorsunuz. Herhangi bir basamağa ulaşma yolları, altındaki iki basamağa ulaşma yollarının toplamı. Bu sefer en alttan başlayıp yukarı çıkın.

Her basamak için bir tane olmak üzere yan yana altı kutu çizin ve onları soldan doldurun. Birinci basamağın bir yolu var. İkinci basamağın iki. Üçüncü basamak 2 + 1, yani üç. Şimdi dördüncü basamağı kendiniz deneyin. Durun ve önündeki iki kutuyu toplayın. 3 + 2, yani beş. Beşinci basamak 5 + 3, yani sekiz. Altıncı basamak 8 + 5, yani 13. Altı basamağı çıkmanın 13 yolu var.

Ne olmadığına dikkat edin. Hiçbir şey iki kez hesaplanmadı ve hiçbir fonksiyon kendini çağırmadı. Yani taşabilecek bir bekleyen çağrı yığını yok. En küçük durumdan başlayarak bir tabloyu böyle doldurmaya **aşağıdan yukarı** (bottom-up) dinamik programlama denir.

Şimdi kutulara tekrar bakın. Her biri yalnız hemen önündeki iki kutuya ihtiyaç duydu. Yani bütün sıraya ihtiyacınız yok. Yalnız iki sayı tutup ilerledikçe onları kaydırabilirsiniz. Zaman yine basamak sayısıyla aynı adımda büyür ama bellek, merdiven ne kadar yükselirse yükselsin aynı kalır.

Bu desen gerçek uygulamalarda da çıkar. Diyelim bir müşteri arama kutusuna "Jhn" yazdı. "John" önermek için uygulama, bir kelimeyi diğerine çeviren en az tek harflik değişiklik sayısını hesaplar. Burada bu bir değişiklik, eksik O'yu eklemek. O sayıya düzenleme mesafesi (edit distance) denir. Tıpkı merdiven gibi bir tablo doldurarak hesaplanır. Her kutu, iki kelimenin biraz daha kısa parçaları için soruyu cevaplar. Yazım denetleyicileri ve "Bunu mu demek istediniz?" önerileri de aynı şekilde çalışır.

Bir şartı var. Cevapları saklamak, yalnız aynı küçük sorular birden fazla kez çıkıyorsa işe yarar. Bir listeyi ikiye bölüp her yarıyı sıralayan ve sonra birleştiren birleştirmeli sıralamayı (merge sort) alın. Aynı parçayı hiçbir zaman iki kez sıralamaz. Yani kaydedilmiş cevaplar tablosu sadece bellek israfı olur.

Basit bir test: Yavaş bir özyinelemeli fonksiyon aynı soruları sormaya devam ediyorsa bunun yerine bir tabloyu aşağıdan yukarı doldurun.

## Sıralama

Cevapları hatırlamak işi azaltmanın bir yolu. Veriyi sıraya koymak başka bir yol ve pek çok gündelik özelliğin zaten buna ihtiyacı var: lider tabloları, raporlar, fiyata göre sırala düğmeleri.

Basit yoldan yapıldığında neyin ters gittiğine bakalım. Mağaza ekibi için grafikler ve sayılardan oluşan bir gösterge panelinde "harcamaya göre sırala" düğmesi var. Birkaç yüz müşteriyle yapılan testte anında çalışıyor. Bir milyon gerçek müşteriyle sayfa donuyor. Biri sıralamayı elle, basit yoldan yazmış.

Önce basit yola bakalım. Listeyi dolaşın ve her komşu çiftini karşılaştırın. Yanlış sıradaysalar yerlerini değiştirin. Bir tam dolaşmadan sonra en büyük sayı sona taşınmış olur. Sonra bir dolaşma daha yapın, sonra bir tane daha, değiştirilecek bir şey kalmayana kadar. Buna **kabarcık sıralaması** (bubble sort) denir, çünkü büyük sayılar kabarcık gibi sona doğru yükselir.

Bu ne kadar iş? Her dolaşma yaklaşık n çifti karşılaştırır ve yaklaşık n dolaşmaya ihtiyacınız olabilir. Bu n × n, yani ikinci dereceden. Bin müşteri yaklaşık bir milyon adım tutar. Devam etmeden tahmin edin, bir milyon müşteri ne tutar? Yaklaşık bir trilyon adım. Donan düğme bu.

Hızlı yol farklı bir fikir kullanır. Listeyi ikiye bölün. Her yarıyı sıralayın. Sonra iki sıralı yarıyı birleştirin. Peki her yarıyı nasıl sıralarsınız? Aynı yolla, tekrar bölerek. Buna **birleştirmeli sıralama** (merge sort) denir.

Sekiz sayı üzerinde izleyelim: 5, 2, 7, 1, 6, 3, 8 ve 4. Her parça tek bir sayı olana kadar bölmeye devam edin. Tek bir sayı zaten sıralıdır. Şimdi komşuları sıralı çiftler halinde birleştirin. 2 ve 5, sonra 1 ve 7, sonra 3 ve 6, sonra 4 ve 8 elde edersiniz. Sonra çiftleri dörtlüler halinde birleştirin. Solda 1 2 5 7, sağda 3 4 6 8 elde edersiniz. Son olarak bu ikisini birleştirin. Her seferinde öndeki sayıları karşılaştırın ve küçüğünü alın. Bu 1, sonra 2, sonra 3 ve böyle 8'e kadar verir.

Şimdi birleştirme turlarını sayın. Önce çiftler, sonra dörtlüler, sonra sekizin tamamı. Bu üç tur. Üç, sekizi bire ulaşana kadar kaç kez yarıya bölebileceğinizdir. log n'nin anlamı bu. Ve her turda her sayıya bir kez bakılır. Yani her tur n adım. log n tur çarpı n adım, n log n eder. Bir milyon müşteri için bu bir trilyon yerine yaklaşık 20 milyon adım. Düğme anında çalışmaya devam eder.

Bir başka hızlı yol daha var: **hızlı sıralama** (quicksort). Pivot denen bir öğe seçer. Ondan küçük her şeyi bir tarafa, büyük her şeyi diğer tarafa taşır. Sonra her tarafa aynısını yapar. Genellikle o da çok hızlıdır, ama şanssız bir pivot seçimi onu yavaşlatabilir.

Bunların hiçbirini nadiren kendiniz yazarsınız. Dilinizle gelen sıralama zaten bunlar gibi hızlı bir yöntem kullanır, onu kullanın. Ve öğeleri karşılaştırarak çalışan sıralamalar için n log n kanıtlanmış bir sınırdır. Hiçbir akıllı hile onu yenemez.

Son bir uyarı. Bir döngünün içinde sıralarsanız her turda tam bir sıralamanın bedelini ödersiniz. Yani döngüden önce bir kez sıralayın.

## İkili arama

Sıralamanın büyük bir getirisi var. Veri bir kez sıraya girince içinde bir şey bulmak çok daha hızlı olur.

Diyelim bir web sitesi, biri her sayfa yüklediğinde bir müşteri numarasının var olup olmadığını kontrol etmek zorunda. Bir milyon numaralık bir listeyi kontrol ediyor. Tek tek kontrol etmek her seferinde bir milyona kadar kontrol demek. Ama liste sıralı ve bu her şeyi değiştiriyor. Sözlükte gördüğünüz yarıya bölme hilesi olan ikili aramayı kullanabilirsiniz.

Ayrıntılı olarak nasıl çalıştığı şöyle. Ortadaki öğeye bakın. Aradığınız oysa bitti. Aradığınızdan küçükse cevap yalnız sağ yarıda olabilir. Yani sol yarıyı atın. Büyükse sağ yarıyı atın. Sonra tuttuğunuz yarıda tekrarlayın. Her adım listeyi yarıya böler. Yani bir milyon öğe yaklaşık 20 adım sürer. Bu O(log N).

İzleyelim. Üçer üçer artan 16 sıralı sayı alın: 3, 6, 9 ve böyle 48'e kadar. Sıraları 0'dan 15'e numaralayın. 39'u arıyorsunuz. İzlemeden önce tahmin edin, kaç adım sürecek?

Düşük ve yüksek adında iki işaretçi tutun. Düşük 0. sırada, yüksek 15'te başlar. Orta 7. sıra. Orada 24 var, çok küçük. Düşüğü 8'e çıkarın. Şimdi orta 11. sıra. Orada 36 var. Hâlâ küçük. Düşüğü 12'ye çıkarın. Orta şimdi 13. sıra. Orada 42 var, çok büyük. Yükseği 12'ye indirin. Artık düşük ve yüksek ikisi de 12. 12. sırada 39 var. Dört adımda buldunuz.

Atlayamayacağınız bir kural var. Liste sıralı olmak zorunda. Sıralı olmayan bir listede ikili arama sadece yavaşlamaz, yanlış cevaplar verir. Bir yarıyı atmak ancak cevap o yarıda olamıyorsa güvenlidir.

Bu bir takasa götürür. Bir listeyi sıralamak baştan biraz iş ister. Bundan sonra sıralı olmayan bir listede aramak her seferinde n adım sürer. Ama sıralı bir listede aramak yalnız log n sürer. Yani yalnız bir kez arayacaksanız sıralamayla uğraşmayın. Pek çok kez arayacaksanız bir kez sıralayın. Ondan sonraki her arama hızlı olur. Bu yüzden pek çok gerçek veri sıralı ya da indekslenmiş tutulur, yani hızlı aramalar için önceden düzenlenmiştir.

İkili aramayı anlatmak kolaydır ama kodda yanlış yazmasıyla ünlüdür. Klasik hata işaretçileri taşırken yapılır. Düşük, ortanın hemen ötesine, yüksek ortanın hemen önüne taşınmalıdır. Düşük yalnız ortanın kendisine taşınırsa arama takılıp sonsuza kadar dönebilir. Böyle küçük kaymalara birer kayma hataları (off-by-one error) denir.

Yani dilinizde hazır bir ikili arama ya da sıralı arama varsa onu kullanın. Fikir basit. Hatalar ayrıntılarda saklanır.

## Açgözlü algoritmalar ve zor problemler

Şimdiye kadarki her yavaş problemin bulunmayı bekleyen daha hızlı bir yöntemi vardı. Bazı problemlerin yoktur. Bir hafta boyunca aramadan önce onları tanımak işe yarar.

Tekrar tekrar gelen bir özellik isteği. Bir web uygulaması yerel müşterilere satış yapıyor. Her sabah bir şoför 20 siparişle depodan çıkıyor. Ürün yöneticisi, yani uygulamanın ne yapacağına karar veren kişi, uygulamanın iyi bir rota değil, en iyi rotayı planlamasını istiyor. Bir hafta sonu işi gibi geliyor ama bilgisayar biliminin en ünlü zor problemlerinden biri. Bu tek şoförü izleyip nedenini görelim.

Akla gelen fikirle başlayın. Durakların her sırasını deneyin ve en kısasını tutun. Buna **kaba kuvvet** (brute force) denir ve doğru olacağı garantidir.

Peki kaç sıra var? 20 duraktan herhangi biri ilk gidebilir. Kalan 19'dan herhangi biri ikinci, sonra 18 ve böyle bire kadar. Bu 20 faktöriyel, yaklaşık 2,4 kentilyon sıra. Saniyede bir milyar sıra kontrol etseniz kabaca 77 yılda bitirirsiniz. Bu bir faktöriyel şekil. Üstelden bile hızlı büyür. 21. bir durak ekleyin, iş 21 ile çarpılır. Hiçbir donanım bu şekli düzeltemez. Bin sunucu yalnız birkaç durak daha kazandırır.

Bu yüzden insanlar bir kısayola uzanır. Bariz olanı, her zaman henüz ziyaret etmediğiniz en yakın durağa gitmek. Buna **açgözlü algoritma** (greedy algorithm) denir. Her adımda o an en iyi görüneni alırsınız ve hiç geri dönüp düşünmezsiniz. Hızlıdır ve gerçek haritalarda genellikle fena değildir, ama her zaman doğru değildir.

Ters gittiği minik bir örnek. Depo düz bir yolun üzerinde. Şoförün sonunda geri dönmesi gerekmiyor. A durağı 1 mil doğuda. B durağı 2 mil batıda, C durağı 5 mil doğuda.

Açgözlü şoför en yakın olduğu için önce A'ya gider. Bu 1 mil. A'dan B 3 mil, C 4 mil uzakta. B daha yakın, şoför batıya B'ye gider. Bu 3 mil daha. Sonra B'den ta C'ye 7 mil. 1 + 3 + 7 = 11 mil.

Daha iyi planı göstermeden önce tekrar bakın. Daha kısa bir rota bulabilir misiniz? Bir saniye.

Şimdi daha iyi planı deneyin. Önce batıya B'ye gidin. Bu 2 mil. Sonra doğuya sürün. A'ya 3 mil, oradan C'ye 4 mil daha. 2 + 3 + 4 = 9 mil. En yakın durağı önce almak fazladan iki mile mal oldu.

Bu, açgözlüye hiç güvenmemeniz gerektiği anlamına gelmiyor. Bazen yapabileceğinizin en iyisi olduğuna dair bir kanıt vardır. Harita uygulamaları iki yer arasındaki en kısa yolu bulmak için açgözlü bir yöntem kullanır. Adı Dijkstra algoritması ve her zaman en iyi yolu bulduğuna dair bir kanıt var.

Para üstü vermek de açgözlüdür. Sığan en büyük madeni parayı verip tekrarlarsınız. ABD paraları gibi yaygın para sistemlerinde bu her zaman en az parayı kullanır. Ama bu o paraların bir özelliği, her para sisteminin değil. Diyelim 1, 3 ve 4 değerinde paralar icat ettiniz. Sonra 6 istiyorsunuz. Açgözlü yöntem bir 4, sonra bir 1, sonra bir 1 daha verir. Toplam üç para. 3 + 3 yalnız iki para ister.

Yani soru hiçbir zaman "açgözlüye izin var mı?" değil. "Burada açgözlünün doğru olduğuna dair bir kanıt var mı?"

## Kolay görünen zor problemler

O teslimat rotası koca bir zor problemler ailesine ait. Neden zor olduğunu görmenin en iyi yolu, onu neredeyse aynı gibi duyulan ama öyle olmayan bir problemle karşılaştırmak.

Ortam yine aynı. Bir şoför her sabah 20 siparişle depodan çıkıyor. Ürün yöneticisi iki özellik istiyor. Birincisi depodan tek bir müşteriye en kısa yolu bulmak. İkincisi 20 durağın hepsini ziyaret etmek için en iyi sırayı bulmak. Devam etmeden bir tahminde bulunun: Hangisi zor?

Birinciyle başlayın. Sokakları köşelerden oluşan bir ızgara olarak düşünün. Depodan dışarıya doğru çalışın. Her köşe için şimdiye kadar bulduğunuz en kısa mesafeyi tutun. Her seferinde henüz kesinleştirmediğiniz en yakın köşeyi seçip kesinleştirin. Bir köşe kesinleşince mesafesi bir daha değişmez. Yani her köşe yaklaşık bir kez ele alınır. Bütün bir şehir için bile bir harita uygulaması bunu bir saniyenin altında yapar.

Şimdi ikinciyi küçük ölçekte deneyin. Üç durakla, A, B ve C, onları kaç sırada ziyaret edebilirsiniz? Durun ve sayın. Altı. A ile başlayıp A B C ya da A C B gidebilirsiniz. B ile başlayıp B A C ya da B C A gidebilirsiniz. Ya da C ile başlayıp C A B ya da C B A gidebilirsiniz.

Dördüncü bir durak ekleyin, 24 sıra olur. Beş durakla 120. Her yeni durak sayıyı çarpar. 20 durakla daha önceki 2,4 kentilyon sıraya ulaşırsınız.

Kolay bir kısayolun olmamasının nedeni şu. Haritada bir köşeyi kesinleştirip devam edebiliyordunuz. Duraklarla yapamazsınız. Bir durağı nereye koyduğunuz ondan sonraki her seçimin maliyetini değiştirir. Başta iyi görünen bir seçim rotayı sonradan mahvedebilir. Yani birinci problem kolay, ikincisi zor.

En iyi teslimat rotası gibi problemlere **NP-zor** (NP-hard) denir. Koca bir zor problemler ailesine aittirler. Hiçbiri için her zaman en iyi cevabı bulan hızlı bir yol bilinmiyor. Ve birbirlerine bağlılar, yani birini çözen hızlı bir yol hepsini çözerdi. Pek çok zamanlama problemi bu ailede ve öğeleri kutulara yerleştirmek de.

Peki ürün yöneticisine ne söylersiniz? Önce şekli tanıyın. Bir problem çok sayıda seçenek arasından en iyi sırayı, en iyi grubu ya da en iyi düzenlemeyi istiyorsa bu ailenin üyesi olduğundan şüphelenin. Mükemmel hızlı bir yöntem aramayı bırakın ve bunun yerine üç hamleden birini seçin.

Girdi küçükse, diyelim 10 durak, her sırayı deneyin. Daha büyükse bir sezgisel yöntem (heuristic), yani en iyisi olduğunu kanıtlamadan güçlü bir cevap bulan bir yöntem kullanın ve ona bir süre sınırı verin. Ya da problemi bir çözücü kütüphaneye (solver library), bu problemler için yapılmış hazır bir araca verin. Akıllı aramayı sezgisel yöntemlerle birleştirir. Gerçek rota planlama yazılımları bunu yapar. Mükemmel değil, çok iyi bir rota vaat eder.

Yani bir problem devasa sayıda seçenek arasından en iyisini istiyorsa mükemmeli kovalamayın. Bir süre sınırı içinde yeterince iyisini isteyin.

## Modül özeti

İşin nasıl büyüdüğü bu kadar. Bir kronometre size tek bir çalıştırma hakkında bilgi verir. Şekil ise veri büyüyünce ne olacağını söyler. Büyük O o şeklin kısa adıdır: sabit ve logaritmikten doğrusal ve n log n'ye, oradan ikinci dereceden ve üstele kadar. Şekli döngüleri okuyarak fark edebilirsiniz. Art arda gelen döngüler toplanır, iç içe döngüler çarpılır. Bellek de büyür, o yüzden bir döngünün içinde biriken kopyalara dikkat edin.

Özyineleme iç içe problemleri, her çağrının yığında sırasını beklemesiyle halleder. Aynı soruları sormaya devam ettiğinde cevapları yazın ya da bir tabloyu aşağıdan yukarı doldurun. Hızlı sıralama n log n sürer ve veri bir kez sıralanınca ikili arama her şeyi bir avuç adımda bulur. En iyi teslimat rotası gibi bazı problemlerin bilinen hızlı bir cevabı yoktur. Onlar için bir süre sınırı içinde çok iyi bir cevap verin.

Yani veri büyüdükçe kod yavaşlıyorsa önce bir şeyi sorun: Şekli ne?

Sırada veri yapıları var. Kodunuzun bu işin ne kadarını yapmak zorunda olduğuna onlar karar verir.
