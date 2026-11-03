# Modül 5: Kodunuz çalışırken neler olur?

## Modüle giriş

Şimdiye kadar öğrendiğiniz her şey gerçek bir makinede çalışır. Bir çalışma zamanının (runtime), yani kodunuzu çalıştıran yazılımın içinde çalışır ve en zor hataların bazıları orada başlar.

Bir servis ilk birkaç dakikasında yavaş olabilir. Bellek, biri sunucuyu yeniden başlatana kadar günlerce tırmanabilir. Ya da bir sayaç eksik çıkabilir, ama yalnız pek çok kişi uygulamayı aynı anda kullandığında.

Bu modül kodunuz çalışırken ne olduğuna bakıyor. Kodunuzun işlemcinin çalıştırabileceği talimatlara nasıl dönüştüğüyle başlıyor. Sonra belleğin nasıl temizlendiğini ve neden yine de sızdığını anlatıyor. Ardından birkaç şey aynı anda çalışınca neyin ters gittiği geliyor. Ve en sonda kurstaki her şey, hem yavaş hem de yanlış cevaplar veren tek bir uç noktada birleşiyor.

O zaman kodunuzun işlemciye nasıl ulaştığıyla başlayalım.

## Derleme, yorumlama ve JIT

Bir Java servisinin yeni bir sürümünü canlıya aldınız diyelim. İlk istekler yavaş. Birkaç dakika sonra aynı istekler hızlı. Tek bir satır kod değiştirmediniz. O zaman ne değişti?

Çoğu kişi önce veritabanından ya da ağdan şüphelenir. Burada neden çalışma zamanının kendisi. Cevap, kodunuzun işlemcinin çalıştırabileceği bir şeye nasıl dönüştürüldüğünde.

İşlemci kodunuzu anlamaz. Makine talimatlarını anlar. Bunlar "bunları topla", "şunları karşılaştır" ve "oraya atla" gibi küçük adımlar için sayı kodlarıdır. Bir şeyin yazdığınız metni bu talimatlara çevirmesi gerekir. Bunu yapmanın üç temel yolu var.

**Birinci yol derlemektir** (compile). Derleyici (compiler) denen bir program önce bütün kodunuzu okur. Kontrol eder, sonra önceden makine talimatlarına çevirir. Sonuç, çalışırken hiç çeviri yapmadan doğrudan işlemcide çalışır. Derlenmiş programların hızlı başlayıp hızlı çalışma eğiliminde olmasının nedeni bu.

Bedeli, kodu her değiştirdiğinizde bir derleme adımı. Ayrıca her işlemci türü için ayrı bir derlemeye ihtiyacınız var. Ama pek çok hata, program hiç çalışmadan derleme sırasında yakalanır. C, C++, Rust ve Go bu şekilde çalışır.

**İkinci yol yorumlamaktır** (interpret). Yorumlayıcı (interpreter) denen bir program kodunuzu okur ve her adıma ulaştıkça onu uygular. Derleme adımı yok. Yazarsınız ve çalıştırırsınız. Aynı kod, yorumlayıcının çalıştığı her yerde çalışır. Bedeli, çevirinin program çalışırken, her seferinde olması. Bu yüzden saf bir yorumlayıcı genellikle daha yavaştır.

Kısa bir soru. Bu ikisinden hangisi size hataları program daha başlamadan verir? Bir saniye düşünün. Derleyici, çünkü bir şey çalışmadan önce her şeyi okur. Bir yorumlayıcı pek çok hatayı ancak o satıra ulaştığında bulur.

**Üçüncü yol ikisini karıştırır.** Anında derleyici (just-in-time compiler), kısaca **JIT** kullanır. Program daha yavaş bir modda başlar, çoğu zaman yorumlayarak. Bu arada JIT, hangi kısımların en çok çalıştığını izler. Bir kısım pek çok kez çalışınca JIT onu makine talimatlarına derler. O andan itibaren o kısım hızlı çalışır. Yani yoğun bir döngüden ilk geçiş yavaş, sonraki geçişler hızlıdır.

Bir derleyicinin hızının büyük kısmını elde edersiniz ve derleme adımını atlarsınız. Bedeli bir ısınma süresi ve JIT'in kendisi için ek bellek. Java ve JavaScript'in arkasındaki çalışma zamanları bu şekilde çalışır. Java servisindeki yavaş başlangıç bu. Kodun yoğun kısımları henüz derlenmemişti. Isınmanın ne kadar süreceği çalışma zamanına ve işe bağlıdır.

Gerçek diller bu fikirleri çoğu zaman karıştırır. Pek çoğu kodunuzu önce bayt kodu (bytecode) denen ara bir biçime çevirir. Sonra o bayt kodu yorumlanır ya da bir JIT tarafından derlenir. Java'nın bayt kodu üreten bir derleme adımı var. Python ise kodunuzu her çalıştırdığınızda sessizce bayt kodu üretir. Yani üç yolu her dilin oturduğu kutular olarak değil, bir düşünme biçimi olarak alın.

Pratik tarafı şu. Taze bir sürüm yavaş geliyorsa ayar yapmaya başlamadan önce JIT'e bir dakika verin. Ve hızlı başlangıç gerçekten önemliyse dil seçiminizi buna göre yapın. Çalışıp kapanan küçük bir araç iyi bir örnek.

Akılda kalsın: Bir derleyici program çalışmadan önce bir kez çevirir. Bir yorumlayıcı ilerledikçe çevirir. Bir JIT en çok çalışan kısımları çevirir.

## Çöp toplama nasıl çalışır?

Çalışma zamanı kodunuzu çalıştırmaktan fazlasını yapar. Çoğu dilde heap'i de sizin için temizler.

Kursun başındaki heap'i hatırlıyor musunuz? Onu oluşturan fonksiyondan uzun yaşayan verinin depo odası. Nasıl temizlendiğinin kısa halini de gördünüz. Çöp toplayıcı, programınızın artık ulaşamadığını serbest bırakır. Bu ders, onun bunun ne olduğuna nasıl karar verdiğini gösteriyor.

Önce, neden bir toplayıcı var? C gibi bazı dillerde odayı kendiniz temizlersiniz. Bellek istersiniz ve işiniz bitince geri vermek zorundasınız. Unutursanız bir bellek sızıntısı olur. İki kez geri verirseniz ya da geri verdikten sonra kullanmaya devam ederseniz program kendi verisine zarar verebilir. Bunlar yazılımdaki en zor hatalardan bazılarıdır. Bu yüzden çoğu modern dil temizliği sizin için yapar. İki ana çalışma şekli var.

**Birinci yol referansları takip etmek.** Toplayıcı, programın doğrudan ulaşabildiği her şeyden başlar. Bu, stack'teki değişkenler ve bütün program boyunca yaşayan küresel (global) değerler demek. Bu başlangıç noktalarına kökler (roots) denir. Toplayıcı köklerden her referansı takip eder ve ulaştığı her nesneyi işaretler. Sonra o nesnelerden çıkan referansları takip eder ve devam eder. Bitirdiğinde, heap'te işaretlenmemiş her şey bir daha asla kullanılamaz. Bu yüzden belleği serbest bırakılır. Buna **izleyen çöp toplama** (tracing garbage collection) denir. Java, C#, Go ve JavaScript bunun türevlerini kullanır.

**İkinci yol saymak.** Her nesne kendisini kaç referansın gösterdiğinin sayısını tutar. Her yeni referans bir ekler. Ortadan kalkan her referans bir çıkarır. Sayı sıfıra ulaşınca nesne hemen serbest bırakılır. Buna **referans sayma** (reference counting) denir.

Şimdi bir tane deneyin. İki nesne birbirini gösteriyor ve başka hiçbir şey ikisinden birini göstermiyor. Referans saymayla serbest bırakılırlar mı? Durun ve düşünün.

Bırakılmazlar. Her birinin diğerinden gelen hâlâ bir sayısı var. Yani hiçbir sayı sıfıra ulaşmaz ve ikisi de sonsuza kadar bellekte kalır. Referans saymanın zayıf noktası bu. Bir nesne döngüsü kendini hayatta tutar.

İzleyen bir toplayıcının bu sorunu yok. Yalnız köklerin bir nesneye ulaşıp ulaşamadığını sorar. Hiçbir şey çifte ulaşmıyor, yani ikisi de serbest bırakılır.

Gerçek diller döngüleri farklı şekillerde halleder. Python referansları sayar ve ayrıca döngüleri bulup serbest bırakan ayrı bir kontrol çalıştırır. Swift referansları sayar ve izleyen bir toplayıcısı yoktur. Yani Swift'te bir döngüyü kendiniz kırarsınız. Bir tarafını, sayıya eklenmeyen zayıf referans (weak reference) olarak işaretlersiniz.

Kısacası, izleyen bir toplayıcı referansları köklerden takip eder ve hiç ulaşamadığını serbest bırakır. Referans sayma, onu hiçbir şey göstermediğinde bir nesneyi serbest bırakır. Ama bir nesne döngüsü kendini hayatta tutabilir.

## Toplayıcı varken sızıntı

Kursun başında, çöp toplayıcısı olmasına rağmen belleği her saat tırmanan bir servisle tanıştınız. Kısa nedenini zaten biliyorsunuz. Bir şey hâlâ eski veriyi gösteriyor, bu yüzden toplayıcı onu tutmak zorunda. Artık bunun olmasını minik bir heap üzerinde adım adım izleyebilirsiniz.

Bir fonksiyon bir ödemeyi işliyor. Stack'inde bir siparişi gösteren bir değişken var. Sipariş müşterisini gösteriyor. Kenarda, eski bir istekten kalmış bir oturum (session) ve bir alışveriş sepeti var. Birbirlerini gösteriyorlar ve başka hiçbir şey onları göstermiyor.

Program ayrıca bir önbellek tutuyor. Bu, küresel bir değişkende tutulan bir map, yani program yaşadığı sürece yaşıyor. O map hâlâ dünden kalma eski bir oturumu tutuyor. Hiçbir kod onu bir daha kullanmayacak.

Şimdi toplayıcı çalışıyor. Bu nesnelerden hangileri hayatta kalır? Durun ve çözün.

Köklerden başlayın. Stack değişkeninden toplayıcı siparişe ulaşıp onu işaretler. Sipariş müşteriye götürür, o da işaretlenir. Küresel değişkenden önbelleğe ve sonra içindeki eski oturuma ulaşır. İkisi de işaretlenir. Oturum ve sepet çiftine hiçbir şey ulaşmıyor. Yani birbirlerini göstermelerine rağmen ikisi de serbest bırakılır.

Ve önbellekteki eski oturum hayatta kalır. Program onunla işini bitirdi ama önbellek hâlâ ona bir referans tutuyor. Yani toplayıcı onu tutmak zorunda. İşte sızıntı bu.

Birbirini gösteren çiftin serbest bırakıldığına dikkat edin. Ve zararsız görünen sızıntı oldu. Bu, kursun başındaki büyüyen önbellek, şimdi toplayıcının tarafından görülüyor. Toplayıcı işini yaptı. Sorun referanstaydı.

Peki gerçek bir programda referansı nasıl bulursunuz? Bir **heap anlık görüntüsü** (heap snapshot) alın. Bellekteki her nesnenin ve her birini neyin tuttuğunun bir resmidir. Bir saat arayla iki tane alın ve karşılaştırın. Büyümeye devam eden nesne türü ve onu tutan koleksiyon doğrudan sızıntıyı gösterir.

Bir toplamadan sonraki bellek grafiğiyle ilgili bir uyarı. Pek çok çalışma zamanı serbest bırakılan belleği yeniden kullanmak için hazır tutar. Onu işletim sistemine geri vermez. Yani yoğun bir saatten sonra yüksek kalan bellek sızıntı kanıtı değildir. Saatten saate istikrarlı büyüme uyarı işaretidir.

Toplayıcının bir bedeli daha var. Bazı toplayıcılar çalışırken programı bir an duraklatır. Çoğu uygulama için kimse fark etmez ama bir oyun ya da canlı fiyat gösteren bir ekran o duraklamaları hissedebilir. Onları kısa tutmak için pek çok toplayıcı heap'i yaşa göre böler. Çoğu nesne kısa süre kullanılıp atılır. Bu yüzden toplayıcı genç nesneleri sık kontrol eder, bu ucuzdur. Eski, uzun yaşayanları çok daha seyrek kontrol eder.

Çoğu çalışma zamanı ayrıca toplayıcının ne zaman ve ne kadar süre çalıştığını size söyleyebilir. Yani bir program yoğun kullanımda arada bir takılıyorsa önce bunu kontrol edin.

Yani bellek yanlış göründüğünde üç şeyi kontrol edin. İstikrarlı mı büyüyor, yoksa sadece yüksek mi kalıyor? Heap anlık görüntüsü neyin tuttuğunu söylüyor? Ve toplayıcı programı çok sık mı duraklatıyor?

## Süreçler ve iş parçacıkları

Bellek, çalışma zamanının sizin için hallettiği bir şey. Aynı anda birkaç şey yapmak ise çoğunlukla size kalıyor.

Bir internet mağazasının arkasındaki sunucuyu düşünün. Pek çok müşteri onu aynı anda kullanıyor. Biri geziniyor, bir diğeri ödeme yapıyor. Sunucu onlara tek tek hizmet etseydi herkes en yavaş müşterinin arkasında beklerdi. Yani tek bir programın aynı anda birkaç şey yapması gerekiyor. Bilgisayarınız da açık olan bütün programlarla aynı şeyi yapıyor.

İkisi de işletim sisteminden gelen iki fikre dayanır. Adları süreçler (process) ve iş parçacıkları (thread).

**Süreç**, kendi belleği olan çalışan bir programdır. İşletim sistemi her sürecin belleğini ayrı tutar. Yani bir süreç normalde başka birinin belleğini okuyamaz ya da ona zarar veremez. Bir süreç çökerse diğerleri genellikle çalışmaya devam eder. Süreçler bellek paylaşabilir ama yalnız bunu bilerek kurarlarsa. Aksi halde veriyi dosyalar ya da mesajlar üzerinden geçirirler.

**İş parçacığı** (thread), bir sürecin içindeki tek bir iş hattıdır. Bir sürecin pek çok iş parçacığı olabilir. Hepsi aynı belleği paylaşır ama her birinin kendi stack'i vardır.

Bir süreci bir ev, iş parçacıklarını da içinde yaşayan insanlar gibi düşünün. Her evin kendi duvarları var. Bir evin içinde herkes aynı mutfağı kullanır.

İş parçacıklarını başlatmak süreçlerden daha ucuzdur ve veriyi sadece aynı değişkenleri okuyarak paylaşırlar. Bu onları aynı anda pek çok müşterisi olan bir sunucu için kullanışlı yapar. Bir uygulamanın bir şey indirirken yanıt vermeye devam etmesi de böyle olur.

Şimdi bir soru. Aynı süreçteki iki iş parçacığı tek bir alışveriş sepeti nesnesini kullanıyor. Biri bir ürün eklerken diğeri toplamı okuyor. Bu ters gidebilir mi? Bir saniye düşünün.

Gidebilir. Kursun başındaki tek bir nesneyi gösteren iki değişkeni hatırlıyor musunuz? Birinden yapılan değişiklik diğerinden görünüyordu. İş parçacıkları da aynı, sadece şimdi ikisi de nesneyi tam aynı anda kullanıyor. Yani bellek paylaşmak iş parçacıklarını kullanışlı yapan şey. Onları tehlikeli yapan da bu.

Diller iş parçacıklarını farklı şekillerde ele alır. Java, C#, Go ve C++ iş parçacıklarının farklı çekirdeklerde gerçekten aynı anda çalışmasına izin verir. Çekirdek (core), işlemcinin içindeki işlem birimlerinden biridir. Bugün çoğu işlemcide birkaç tane vardır.

Standart Python farklı çalışır. GIL kısaltmasıyla bilinen küresel yorumlayıcı kilidi (global interpreter lock) denen bir kilidi var. Aynı anda yalnız bir iş parçacığının Python kodu çalıştırmasına izin verir. Yani iş parçacıkları Python'a ağı beklemek gibi beklemelerde yardım eder ama birkaç çekirdekte ağır hesaplamada yardım etmez. Python'un yeni sürümleri GIL'siz isteğe bağlı bir derleme sunuyor.

JavaScript kodunuzu tek bir iş parçacığında çalıştırır. Ek işçiler (worker) başka JavaScript'i paralel çalıştırabilir ama her birinin kendi belleği vardır ve mesaj geçirerek konuşurlar.

Bir sürecin kendi belleği vardır. İş parçacıkları bir sürecin belleğini paylaşır. O paylaşım iş parçacıklarını hızlı yapar ve tehlike tam orada başlar.

## Yarış durumu (race condition)

O tehlikenin gerçek kodda nasıl göründüğüne bakalım. Mağazanın sunucusu bugünkü siparişlerin tek bir paylaşılan sayacını tutuyor. İki sipariş tam aynı anda geliyor, her biri kendi iş parçacığında. Her iş parçacığı sayaca bir ekliyor.

Bir eklemek tek bir adım gibi görünür. Aslında üç adımdır: Sayacı oku. Bir ekle. Geri yaz.

Şimdi iki iş parçacığının bunu yapmasını izleyin. Sayaç 100'den başlıyor. Devam etmeden, nerede bitmeli? Ve başka bir yerde bitebilir mi? Bir saniye düşünün.

102'de bitmeli. Ama şöyle gidebileceği bir yol var. A iş parçacığı 100 okur. A yazmadan önce B iş parçacığı da 100 okur. A bir ekleyip 101 yazar. B bir ekleyip o da 101 yazar. İki sipariş oldu. Sayaç yalnız bir arttı. Bir sipariş sayımdan kayboldu.

Buna **kayıp güncelleme** (lost update) denir. Hataya **yarış durumu** (race condition) denir. Sonuç, sanki birbirleriyle yarışıyormuş gibi iş parçacıklarının tam zamanlamasına bağlıdır ve o zamanlama çalıştırmadan çalıştırmaya değişir.

Bir yarışın olması için üç şeyin doğru olması gerekir. İki ya da daha fazla iş parçacığı aynı değiştirilebilir veriyi aynı anda kullanıyor. En az biri ona yazıyor. Ve hiçbir şey onları sırayla çalışmaya zorlamıyor. Bu üçünden birini kaldırın, yarış olamaz.

Testlerin geçmesinin nedeni de bu. Testler her seferinde tek bir istek gönderiyordu, yani çarpışacak ikinci bir iş parçacığı hiç yoktu.

Ne kadar tuhaflaştığına bakın. Diyelim iki iş parçacığının her biri sayaca bir milyon kez bir ekliyor. 2 milyon beklersiniz. Çalıştırın, 1.400.000 alabilirsiniz. Tekrar çalıştırın, farklı bir sayı alırsınız. Her çalıştırmada iş parçacıkları farklı bir sırayla nöbetleşir, bu yüzden farklı sayıda güncelleme kaybolur.

Şimdi neler olduğunu görmek için döngünün içine bir print satırı ekleyin. Çoğu zaman hata kaybolur. Yazdırmak yavaştır, bu yüzden zamanlamayı değiştirir. İş parçacıkları o kadar sık çarpışmayı bırakır. Bir hata ayıklayıcı da iş parçacıklarını duraklattığı için aynı şeyi yapar. Yani bakmak, hatanın dayandığı zamanlamayı değiştirir.

Kayıp güncelleme en kötü durum bile değil. C ve C++'ta sıradan veri üzerinde böyle bir yarışa **tanımsız davranış** (undefined behavior) denir. Bu, dilin ne olacağı hakkında hiçbir söz vermediği anlamına gelir. Program bir güncellemeyi kaybedebilir ya da çok daha tuhaf bir şey yapabilir.

Yarışları yakalamak zordur ve bu beceriyle ilgili değil. Tek iş parçacıklı kod şeyleri tek bir sırayla yapar, yani onu yukarıdan aşağı okuyabilirsiniz. Çok iş parçacıklı kodun, iş parçacıklarının nöbetleşebileceği her yol için olası bir sırası vardır. Olası sıraların sayısı çok hızlı büyür. Yani tek başına test kodun güvenli olduğunu kanıtlayamaz. Başarısız olan sıra milyonda bir çalıştırmada ortaya çıkabilir.

İş parçacıkları değiştirilebilir veriyi paylaşıp biri yazdığında sonuca zamanlama karar verir. Hiçbir şey onları sırayla çalışmaya zorlamıyorsa güncellemeler eninde sonunda kaybolur.

## Kilitler ve kilitlenme (deadlock)

Peki iş parçacıklarını nasıl sırayla çalıştırırsınız? Kayıp güncellemenin her çözümü tek bir fikre iner. O oku, ekle, yaz işleminin içinde aynı anda yalnız bir iş parçacığı olabilir.

En yaygın çözüm bir **kilit** (lock). Kilidi aynı anda yalnız bir iş parçacığı tutabilir. Yani sayaca aynı anda yalnız bir iş parçacığı dokunabilir.

Aynı iki sipariş, bu sefer kilitle. A iş parçacığı kilidi alır. 100 okur, bir ekler ve 101 yazar. Sonra kilidi bırakır. Bu arada B iş parçacığı bekliyordu. Şimdi kilidi alır, 101 okur ve 102 yazar. Hiçbir şey kaybolmaz.

Bir kilit ancak herkes aynısını kullanırsa işe yarar. Sayaca dokunan her kod parçasının aynı kilidi alması gerekir.

Basit bir sayaç için çoğu dil ayrıca bir **atomik artırma** (atomic increment) sunar. Atomik, bölünemez demek. Okumayı, eklemeyi ve yazmayı, başka hiçbir iş parçacığının araya giremeyeceği tek bir adım olarak yapar. Pek çok iş parçacığı onu aynı anda kullandığında doğru kalan koda iş parçacığı güvenli (thread-safe) denir.

Ama kilitler kendi hatalarını getirir. Mağaza ayrıca müşteri hesapları arasında para taşıyor. Parayı güvenle taşımak için kod önce paranın çıktığı hesabı kilitliyor. Sonra paranın gittiği hesabı kilitliyor.

Şimdi tam aynı anda iki istek geliyor. Biri 1 numaralı hesaptan 2 numaralı hesaba para taşıyor. Diğeri 2 numaralı hesaptan 1 numaralı hesaba para taşıyor. Ne olur? Durun ve düşünün.

İlk iş parçacığı 1 numaralı hesabı kilitler. İkinci iş parçacığı 2 numaralı hesabı kilitler. Şimdi ilk iş parçacığının 2 numaralı hesaba ihtiyacı var, bekler. İkinci iş parçacığının 1 numaralı hesaba ihtiyacı var, o da bekler. Her biri diğerini bekler. Hiçbiri bırakmaz. İkisi de sonsuza kadar bekler.

Buna **kilitlenme** (deadlock) denir. Program sadece durur. Hata yok, çökme yok. Sadece hiç bitmez.

Çözüm basit bir kural. Kilitleri her zaman aynı sabit sırayla alın. Burada bu, önce düşük numaralı hesabı kilitlemek demek. Artık iki transfer de 1 numaralı hesabı kilitleyerek başlar. Hangi iş parçacığı onu önce alırsa bütün transferini yapar. Diğeri sırasını bekler ve sonra aynısını yapar. Her iş parçacığı kilitleri aynı sırayla aldığında kimse bir daire içinde beklemeye düşemez.

Kilitler işe yarar ama her kilit iş parçacıklarının beklediği bir yerdir. Ve her ek kilit, bir kilitlenme için bir şans daha. Bu yüzden pek çok takım hiç kilide ihtiyaç duymayan kod tasarlamaya çalışır.

Nasıl? Kaçınabiliyorsanız değiştirilebilir veriyi iş parçacıkları arasında paylaşmayın. Her iş parçacığına kendi verisini verin. İş parçacıklarının birbirine mesaj geçirmesine izin verin. Ve oluşturulduktan sonra değiştirilemeyen değerler kullanın. O zaman iki iş parçacığının aynı anda değiştireceği hiçbir şey kalmaz. Paylaşmaktan kaçınılamıyorsa dilinizin zaten verdiği iş parçacığı güvenli araçları kullanın. Ve paylaşılan kısmı olabildiğince küçük tutun.

Akılda kalsın: Bir kilit ya da atomik bir adım iş parçacıklarını sırayla çalıştırır. Kilitleri tek bir sabit sırayla almak kilitlenmeleri durdurur. Ve en güvenli paylaşılan veri, paylaşılmayan veridir.

## Bekleyen iş ve olay döngüsü (event loop)

İş parçacıkları aynı anda pek çok müşteriye hizmet etmenin bir yolu. Başka bir yol daha var ve her biri için bir iş parçacığına ihtiyaç duymuyor.

Tuhaf bir manzara. Yoğun bir sunucu çok yavaş cevap veriyor ama işlemcisi çoğunlukla boşta. Çok çalışmıyor. O zaman ne yapıyor?

Öğrenmek için tek bir isteği takip edin. Bir müşteri sipariş veriyor. Kodunuz veritabanından müşterinin kaydını istiyor. Sonra kartından çekim yapmak için bir ödeme servisini çağırıyor. O isteği zamanlayın, zamanının neredeyse tamamını beklemekle geçirdiğini görürsünüz. Ağ ve disk bellekten çok daha yavaştır. Cevaplar yoldayken işlemcinin yapacak bir şeyi yoktur.

Yani iki tür iş var. Bazı işler beklemektir, bir dosya okumak, bir veritabanına sormak ya da başka bir servisi çağırmak gibi. Buna **G/Ç'ye bağlı** (IO-bound) denir. G/Ç girdi ve çıktı demek ve burada temposu onlar belirler. Diğer işler hesaplamaktır, bir fotoğrafı yeniden boyutlandırmak ya da bir şifreyi kontrol etmek gibi. Buna **işlemciye bağlı** (CPU-bound) denir, çünkü temposu işlemci belirler.

Pek çok kullanıcıya hizmet etmenin basit yolu istek başına bir iş parçacığı. Her iş parçacığı isteğini yukarıdan aşağı çalıştırır. Beklerken orada engellenmiş (blocked) halde oturur. Bu işe yarar ama iş parçacıkları bedava değil. Her birinin kendi stack'i vardır, yani belleğe mal olur. Ve işletim sistemi iş parçacıkları arasında geçiş yapmaya zaman harcar. Çoğunlukla boşta olan binlerce iş parçacığı, sadece beklemek için çok şeye mal olur.

Daha iyi bir yol için bir restoranı düşünün. Bir restoranda her masanın kendi garsonu var. Garson siparişi alır, sonra yemek hazır olana kadar mutfağın yanında bekler. Diğer restoranda tek bir garson var. Garson bir siparişi alıp mutfağa verir. Sonra sıradaki masanın siparişini almaya gider. Bir yemek hazır olunca mutfak bir zil çalar ve garson onu getirir. Yavaş kısmı mutfak yapar. Garson hiç boş durmaz.

O ikinci restoran bir **olay döngüsü** (event loop). Yaygın bir tasarımda tek bir iş parçacığı ve çalışmaya hazır görevlerden oluşan bir kuyruk vardır. Döngü bir görevi alır ve görev beklemek zorunda kalana kadar çalıştırır. Sonra görev beklemeyi işletim sistemine devreder ve kenara çekilir. Döngü sıradaki hazır göreve geçer. Daha sonra işletim sistemi cevabın geldiğini söyler. Bekleyen görevin geri kalanı çalışmaya hazır olarak kuyruğa geri girer. O kalan kısma devamı (continuation) denir.

Şimdi zamanlayalım. Bu sayılar ölçüm değil, örnek. Üç istek birlikte geliyor. Her birinin 100 milisaniye süren bir veritabanı çağrısına ihtiyacı var. Onları birbiri ardına yapan tek bir iş parçacığıyla bu 300 milisaniye. Olay döngüsü ne kadar sürer? Durun ve çözün.

Döngü 1. isteği başlatır, sorgusunu gönderir ve devam eder. 2 ve 3 için de aynısını yapar. Bu bir iki milisaniye sürer. Artık üç sorgunun hepsi aynı anda bekliyor ve iş parçacığı boşta. Yaklaşık 100 milisaniye sonra cevaplar gelir ve her istek biter. Yani toplamda yaklaşık 100 milisaniye artı biraz fazlası. Tek bir iş parçacığı üçünün işini yaptı.

Ama neyi kazandırdığına dikkat edin. Bekleme süresini kazandırdı, hesaplama süresini değil. Bir olay döngüsü birinci tür işte yardım eder, ikincisinde değil.

Yani bir sunucu yavaş ama işlemcisi çoğunlukla boştaysa iş beklemektir. Bir olay döngüsünün en çok yardım ettiği yer burası.

## Callback, promise ve async/await

Peki beklerken kenara çekilen kodu nasıl yazarsınız? Diller birbiri ardına üç cevap buldu.

İlk cevap **geri çağırmalardı** (callback). "Cevap gelince bunu çalıştır" diyen bir fonksiyon verirsiniz. Bu tek bir adım için işe yarar ama birkaç tanesini zincirleyin, koda gömülü koda gömülü daha fazla kod elde edersiniz. Okuması zordur.

Sonra **promise'ler** geldi. Bazı diller aynı fikre future der. Promise, henüz gelmemiş bir cevabı temsil eden bir nesnedir. İç içe koymak yerine sonraki adımı ona bağlarsınız.

Sonra **async ve await** geldi. Aynı şeyi yazmanın daha güzel bir yolu. Await şu demek: Cevap hazır değilse bu görevi burada duraklat ve diğer görevlerin çalışmasına izin ver. Yani sipariş kodu yine yukarıdan aşağı okunur. Müşteriyi await eder, sonra ödemeyi await eder. Ama altta hâlâ olay döngüsü var.

Şimdi ilk tuzak. Tek iş parçacıklı ve olay döngülü bir sunucu düşünün. Bir uç nokta 2 saniye süren bir hesaplama yapıyor. O iki saniye boyunca diğer her kullanıcıya ne olur? Durun ve karar verin.

Hepsi bekler. Basit bir sayfa için bile kimse cevap almaz. Nedeni şu. Hesaplama hiçbir zaman bir await'e ulaşmaz. Yani hiç kenara çekilmez. Tek iş parçacığı bütün süre meşguldür. Buna döngüyü engellemek (blocking the loop) denir. Eski usul bekleyen bir çağrı da aynı zararı verir. Eski usul bir dosya okuma bir örnek, çünkü dosya gelene kadar iş parçacığını tutar.

Çözümün iki parçası var. Döngü boş kalsın diye ağır hesaplamayı ayrı bir iş parçacığına ya da sürece taşıyın. Ve bekleyen her şey için async için yapılmış, await edebileceğiniz sürümü kullanın.

İkinci tuzak hızla ilgili. Yavaş bir hesaplama, önüne bir await koyunca da aynı yavaşlıktadır. Async'in her şeyi hızlandırdığını düşünmek kolay. Hızlandırmaz. Async beklemeyi ucuz yapar. Hesaplamayı hızlı yapmaz.

Üçüncü tuzak en şaşırtıcısı. Tek iş parçacığıyla yarışlar ortadan kalkmış gibi görünür, ama diğer görevler her await'te çalışabilir. Bir örnek. Bir görev bugünkü sipariş sayısını okuyup 100 alıyor. Sonra yeni siparişi kaydederken await ediyor. Uyandığında 101 yazıyor. Ama o duraklamadayken ikinci bir görev de 100 okudu. Yani iki görev de 101 yazar ve bir sipariş kaybolur. İş parçacıklarındakiyle aynı kayıp güncelleme. Burada her await, başka bir görevin araya girebileceği bir yerdir.

Yani her await'i her şeyin değişebileceği bir duraklama olarak görün. Paylaşılan veriyi bir await'ten sonra tekrar okuyun ya da okumayla yazmayı arada hiçbir şey olmadan bir arada tutun.

Farklı diller async kodu farklı şekillerde çalıştırır. Node.js, JavaScript'i bir olay döngüsüyle tek bir iş parçacığında çalıştırır, Python'un async kütüphanesi de öyle. C# ve diğer bazı dillerde de async ve await var ama duraklatılmış bir görevi bir havuzdaki farklı bir iş parçacığında devam ettirebilirler.

Yani await'in ne yaptığını ve ne yapmadığını hatırlayın. Tek bir iş parçacığının aynı anda pek çok şeyi beklemesini sağlar. Hesaplamayı hızlandırmaz ve yarışları ortadan kaldırmaz.

Bir sunucu yavaş geliyorsa önce bir soru sorun: Bekliyor mu, hesaplıyor mu?

## Vaka: yavaş ve yanlış bir uç nokta, 1. bölüm

Bu fikirlerin her birini tek tek gördünüz. Hepsi tek bir yerde ortaya çıkınca ne olduğu şöyle. Bu vaka uydurma ama gerçek kodda sürekli çıkan hatalardan kuruldu.

Bir takımın `getCustomerReport` adında bir uç noktası var. Uç nokta (endpoint), bir web servisinde başka kodun çağırabileceği tek bir adrestir. Bu, her müşteriyi siparişleri ve toplamlarıyla listeliyor. Ayrıca e-postası engel listesinde olan her müşteriyi işaretliyor.

Yayına çıkışta bir saniyenin altında cevap veriyordu. Bir yıl sonra bir dakikaya yakın sürüyordu. Bazı toplamlar bir kuruş kayıktı. Ve sunucunun belleği biri yeniden başlatana kadar günlerce büyüyordu. Takımın bunu nasıl çözdüğü, onların yaptığı sırayla şöyle.

İlk hamle kod okumak değildi. Ölçmekti. Raporu 500 müşterili test verisinde çalıştırdılar. Sonra 1.000'i ve sonra 2.000'i denediler. 500 yaklaşık 1 saniye sürdü. 1.000 yaklaşık 4. 2.000 yaklaşık 16.

Bu hangi şekil? Durun ve çözün. Müşterileri ikiye katlamak dört kat yavaşlattı. Bu ikinci dereceden. Yani "nasıl büyür?" sorusunun cevabı netti. Daha hızlı bir sunucu bunu düzeltmezdi. Her çalıştırma biraz hızlanırdı ama şekil ikinci dereceden kalırdı.

Bu yüzden başka bir işin içinde saklanan işi aradılar. Ben söylemeden, önce hangi satıra bakardınız?

Kodun hiçbir yerinde iç içe döngü yoktu. Müşteriler üzerinde tek bir döngü vardı. İçinde basit görünen bir satır vardı. Müşterinin e-postasının engel listesinde olup olmadığını kontrol ediyordu.

Engel listesi düz bir listeydi ve müşterilerle birlikte büyüyordu. Bir listeyi kontrol etmek, bir eşleşme bulana kadar her öğeyle karşılaştırmak demek. Yani o tek satır gizli bir döngüydü. Hash tablosu dersindeki müşteri içe aktarmasındaki hatanın aynısı. Bin engellenmiş e-postaya karşı kontrol edilen bin müşteri, bir milyona kadar karşılaştırma demek.

Çözüm bir hash kümesiydi. Onu döngüden önce listeden bir kez oluşturdular. Ondan sonra her kontrol yaklaşık bir adım sürdü. Yani o kısım ikinci dereceden doğrusala indi.

Süre düştü ama hâlâ çok hızlı büyüyordu. Bu yüzden ikinci soruyu sordular: Veri nerede duruyor?

Her müşterinin siparişleri diskteki veritabanında yaşıyordu. Her müşteri için kod o müşterinin siparişlerini getirmek üzere bir sorgu çalıştırıyordu. Ve siparişler tablosunda müşteri numarası üzerinde bir indeks yoktu. Yani bu sorguların her biri bütün tabloyu okuyordu.

Müşteri numarası üzerine bir indeks eklediler. Genellikle sıralı tutulan bir B ağacı. Artık her sorgu genellikle doğrudan bir müşterinin siparişlerine atlayabiliyordu.

Sonra desenin kendisini düzelttiler. Müşteri başına bir sorguya **N+1 sorgu problemi** denir. Bir sorgu müşterileri getirir, sonra her müşteri için bir sorgu daha. Her sorgu veritabanına gidip gelmenin bedelini öder. Bu yüzden onları grupladılar. Her sorgu artık aynı anda birkaç yüz müşterinin siparişlerini getiriyordu.

İki çözüm de aynı alışkanlıktan geldi. Önce ölç, sonra nasıl büyüdüğünü ve verinin nerede durduğunu sor.

## Vaka, 2. bölüm: kuruş farkı ve bellek

Rapor artık hızlıydı. Ama iki sorun kalmıştı. Bazı toplamlar bir kuruş kayıktı ve bellek büyümeye devam ediyordu. Takım onları birer birer ele aldı.

Önce kuruşlar. Hatayı tekrarlamayı kolaylaştırdılar. Bir günün siparişlerini alıp her müşterinin toplamını tam sayı kuruş cinsinden elle topladılar. Sonra bunu raporun yazdırdığıyla karşılaştırdılar. Bir avuç müşteri tam olarak 1 kuruş kayıktı.

Sonra tutarların nasıl saklandığını buldular. Her biri ondalık noktalı bir dolar tutarıydı ve kayan noktalı sayı olarak saklanıyordu. Devam etmeden, bu neden bir kuruş kayık olsun? Kursun başını düşünün. Bir saniye.

Kayan noktalı sayılar sayıları ikili sistemde saklar. Ve 10 sent gibi çoğu onluk kesir ikili sistemde tam olarak yazılamaz. 0,1 + 0,2'nin tam 0,3 etmemesiyle aynı neden. Yani her tutar çok az yanlıştı. Binlercesini toplayınca o minik hatalar da toplanır. Arada bir yuvarlama bir toplamı yanlış kuruşa itiyordu.

Çözüm parayı tam sayı kuruş olarak saklayıp toplamaktı. Kuruşlar tam sayıdır, yani tam olarak toplanırlar. Rapor onları yalnız ekranda gösterirken dolara geri çeviriyordu.

Sonra bellek geldi. Takım sunucunun belleğinin bir haftalık grafiğine baktı. İstikrarlı biçimde büyüyor ve hiç düzleşmiyordu. Neredeyse kimsenin uygulamayı kullanmadığı gece bile büyümeye devam ediyordu. Bu istikrarlı büyüme bir sızıntının uyarı işaretidir.

Bu yüzden bir heap anlık görüntüsü aldılar. Neredeyse her şeyi tutan tek bir map gösterdi. Bir noktada biri raporu hızlandırmak için bir önbellek eklemişti. Programın en üst seviyesinde saklanan bir map'ti, yani program yaşadığı sürece yaşıyordu. Şimdiye kadar yapılan her rapor tarih ve müşteriye göre dosyalanarak oraya giriyordu ve hiçbir şey çıkmıyordu. Her yeni gün yepyeni bir giriş seti getiriyordu.

İnsanlar çöp toplayıcının temizleyeceğini varsaymıştı. Ama toplayıcı yalnız hiçbir şeyin ulaşamadığı nesneleri serbest bırakır ve önbellek her rapora hâlâ bir referans tutuyordu. Yani hiçbir şey serbest bırakılamıyordu. Çöp toplayıcılı bir programın yine de sızmasının yolu bu.

Çözüm boyut sınırı olan bir önbellekti. Bir hash tablosu ve bir çift bağlı listeden kurulan LRU önbelleğini kullandılar. Dolunca en uzun süredir kimsenin kullanmadığı girişi atıyor. Bellek düzleşti. Emin olmak için grafiği bir hafta izlediler.

Bundan iki ders çıktı. Yanlış kuruşlar kayan noktalı sayıları işaret eder, bu yüzden parayı tam sayı kuruş olarak tutun. Ve yalnız büyüyen bellek, bir şeyin tuttuğunu işaret eder, bu yüzden hiç bırakmayan koleksiyonu arayın.

## Vaka, 3. bölüm: eksik sayaç ve kaçan müşteri

Rapor uç noktasında iki hata kalmıştı. Pek çok kişi uygulamayı kullandığında bir sayaç eksik çıkıyordu ve bazı engellenmiş müşteriler kontrolden geçiyordu. İkisi de bir dizüstü bilgisayarda hiç ortaya çıkmamıştı.

Önce sayaç. Bir gösterge paneli bugün kaç rapor çalıştığını gösteriyordu. Canlı uygulamada düşük okumaya başladı. Takım onu raporlar tablosundaki satır sayısıyla karşılaştırdı. Tablo her zaman daha yüksekti.

Bilerek tekrarlamak için bir yük testi (load test) çalıştırdılar. Bu, gerçek trafik gibi aynı anda pek çok istek gönderen bir test. 200 rapor isteğini birlikte gönderdiler. Sayaç her seferinde eksik geldi, her çalıştırmada farklı bir miktarda.

Her çalıştırmada farklı bir yanlış cevap veren hata ne tür bir hatadır? Durun ve karar verin.

Bu bir yarış durumu. Sunucu istekleri birkaç iş parçacığında işliyordu ve hepsi aynı sayaca bir ekliyordu. Daha önceki kayıp güncelleme. İki iş parçacığı da 40 okuyor, ikisi de 41 yazıyor ve bir rapor sayımdan kayboluyor. Dizüstü testleri aynı anda yalnız bir istek gönderiyordu, yani yarış hiç olmamıştı.

Çözüm atomik bir artırmaydı, böylece üç adım bölünemiyordu. Bir kilit de işe yarardı. Aynı yük testini tekrar çalıştırdılar ve sayaç tabloyla eşleşti.

Son hata en tuhafıydı. Arada bir, engellenmiş bir müşteri işaretlenmeden geçiyordu. Başta kimse bunu bilerek tekrarlayamıyordu. Bu yüzden takım bir log satırı ekledi. Engel kontrolü anında her e-postayı yazdırıyordu. Kötü durumlarda e-posta yıldızlarla doluydu.

Olan şuydu. `formatCustomer` adlı bir yardımcı, müşterinin ekranda gösterilecek bir sürümünü yapıyordu. O sürüm e-postanın çoğunu yıldızların arkasına gizliyor. Ama yardımcı yeni bir nesne yapmadı. Kendisine verilen müşteri nesnesindeki e-postayı değiştirdi. Ve ona bir kopya değil, aynı nesneye bir referans verilmişti. Tek bir nesneyi gösteren iki adı hatırlıyor musunuz? O hata.

Yani kodun diğer her kısmı da yıldızlı e-postayı gördü. Kodun bir yolunda biçimlendirme engel kontrolünden önce çalışıyordu. Yani kontrol yıldızlı e-postayı aradı, eşleşme bulamadı ve müşteriyi geçirdi.

Çözüm, yardımcının yeni bir gösterim nesnesi oluşturması ve aslına dokunmamasıydı. Ayrıca küçük bir test eklediler. Bir müşteriyi biçimlendirmenin aslını değiştirmeden bıraktığını kontrol ediyor.

Şimdi takımın ne yapmadığına bakın. Tahmin etmediler ve bütün uç noktayı baştan yazmadılar. Her seferinde önce ölçtüler. Nasıl büyüdüğünü ve verinin nerede durduğunu sordular. Sonra tek bir şeyi değiştirip tekrar ölçtüler.

Bir hatayı düzeltmek diğerlerini hiç düzeltmedi. Her birinin kendi ölçümüne ya da kendi testine ihtiyacı vardı. Sonunda korkutucu tek bir uç nokta yedi sıradan hataya dönüştü. Ve her birinin artık bildiğiniz bir adı vardı.

## Kursun özeti

Kursun baştan sona kapsadığı her şeye geri bakalım.

Bir bilgisayarın veriyi nasıl sakladığıyla başladı. Her şey byte'lar halinde gruplanmış ve ikili sistemde sayılan bitlerdir. Her sayı türünün öbür uca sarabileceği bir sınırı vardır. Metin, üzerinde anlaşılmış bir anlamı olan sayılardan ibarettir. Kayan noktalı sayılar çoğu onluk kesri tam saklayamaz, bu yüzden para tam sayı kuruşa aittir.

Sonra veri belleğe girdi. Önbellek yakındaki veriyi işlemciye yakın tutar. Stack fonksiyon çağrılarını tutar, heap ise daha uzun yaşaması gereken veriyi. Ve bir değişken ya bir değer ya da başkalarının paylaşabileceği veriye bir referans tutar.

İkinci modül veri büyüdükçe işin nasıl büyüdüğüyle ilgiliydi. Büyük O o büyümeyi sabitten üstele kadar altı yaygın şekille adlandırır. Döngüler şekli ortaya çıkarır ve yalnız zaman değil bellek de büyür. Özyineleme problemlerin içindeki problemleri halleder ve cevapları hatırlamak onu hızlandırır. İyi sıralama n log n sürer ve ikili arama her adımda işi yarıya indirir.

Sonra veri yapıları geldi, bazı işler hızlı olsun diye veriyi düzenlemenin yolları. Diziler öğeleri yan yana tutar. Bağlı listeler onları işaretçilerle zincirler. Yığıtlar önce en yeni öğeye, kuyruklar en eskiye hizmet eder. Ve doğru yapı kodun en çok ne yaptığına bağlıdır.

Dördüncü modül şeyleri hızlı bulan yapıları kapsadı. Hash tabloları anahtara göre yaklaşık bir adımda bulur. Dengeli ağaçlar şeyleri sıralı tutar. Heap'ler en acil öğeyi her zaman bilir. Çizgeler her şeyi her şeye bağlar. Ve gerçek sistemler onları önbelleklerde ve veritabanı indekslerinde birleştirir.

Son modül çalışırken koda baktı. Kod derlenir, yorumlanır ya da çalışırken derlenir. Bir çöp toplayıcı yalnız hiçbir şeyin ulaşamadığını serbest bırakır. İş parçacıkları belleği paylaşır, bu yüzden yarışlar güncellemeleri kaybedebilir ve kilitler iş parçacıklarını sırayla çalıştırır. Async kod beklerken pek çok kişiye hizmet eder.

Peki neden bunların hepsini öğrenmeli? Çünkü her dil, framework ve yapay zeka aracı bu aynı fikirlerin üzerinde durur. Araçlar birkaç yılda bir değişir. Bu fikirler değişmez. Onları bir kez öğrenin, bütün kariyeriniz boyunca işe yarar kalırlar.

Sizin için şimdi değişen şu. Kod okurken veri büyüyünce nasıl davranacağını söyleyebilirsiniz. Bir veri yapısı seçmek açıklayabileceğiniz bir karar olur. Yavaş bir uç nokta bir gizem olmaktan çıkar. Tuhaf hataların adları olur. Ve kod bir takım arkadaşından ya da bir yapay zeka aracından geldiğinde dayanıp dayanmayacağına karar verebilirsiniz.

Sırada ne var? Bu hafta küçük bir adım atın. Hash tablosu gibi bir yapı seçip onu kendi dilinizde bir kez kurun. Maliyetlerin akılda kalmasını sağlayan bu.

Sonra bir yön seçin. Mülakatlar yaklaşıyorsa problemleri konu konu çalışın ve kod yazmadan önce işin nasıl büyüdüğünü söyleyin. İşte daha iyi kod istiyorsanız bir döngünün içindeki her satırın neye mal olduğunu sorun. Ve daha derine gitmek istiyorsanız iyi sonraki konular sistem tasarımı, işletim sistemleri ve veritabanları. Her biri buradakilerin üzerine kurulur.

Bütün bunlar boyunca iki soru geri gelip durdu. **Nasıl büyür? Ve nerede durur?** Kod yavaşsa birincisini sorun. Kod tuhafsa ikincisini.

Sona kadar geldiniz. Tebrikler. Pek çok çalışan geliştirici oturup bunu hiç öğrenmez. Bu fikirlerin modası geçmeyecek. İki soruyu sormaya devam edin, üzerinde çalıştığınız her projede size yardım etmeye devam edecekler. İzlediğiniz için teşekkürler. İyi şanslar.
