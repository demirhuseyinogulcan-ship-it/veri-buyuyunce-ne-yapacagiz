# Modül 3: Öğeleri sırayla tutan veri yapıları

## Modüle giriş

Her program şeylerden oluşan koleksiyonlar tutar. Bir sipariş listesi, gönderilmeyi bekleyen bir e-posta sırası ya da bir geri al (undo) düğmesinin arkasındaki geçmiş gibi.

Bu modül bir veri yapısının ne olduğuyla başlıyor. Sonra en basitlerini, öğeleri bir sıra halinde tutanları anlatıyor. Bunlar diziler (array), bağlı listeler (linked list), yığıtlar (stack) ve kuyruklar (queue), artı diziler üzerinde çalışmak için iki gündelik hile. Her biri için neyin ucuz, neyin pahalı olduğunu ve nedenini göreceksiniz. Bu, iki soruyu bir araya getiriyor: Nasıl büyür? Nerede durur?

O zaman küçük bir dükkanla ve şeyleri tuttuğu farklı yollarla başlayalım.

## Veri yapısı nedir?

Az önce bütün bir modül boyunca işin nasıl büyüdüğünü ölçtünüz. Sizi şaşırtabilecek bir şey var. Çoğu zaman bir işin ne kadar hızlı çalıştığı, kodun kendisinden çok verinin nasıl düzenlendiğine bağlıdır.

Küçük bir dükkan düşünün. Şeyleri birkaç farklı şekilde tutar ve her şekil bir işe uygundur.

Teslim alınmayı bekleyen paketler numaralı yerleri olan bir rafta durur. Bir müşteri "214 numara" dediğinde doğrudan oraya gidersiniz. Önündeki yerlere bakmazsınız.

Alışveriş sepetleri kapının yanında üst üste durur. İade edilen her sepet en üste konur ve her yeni müşteri en üstten bir tane alır. En son giren sepet ilk çıkar.

Müşteriler kasada bir sırada bekler. İlk gelen ilk hizmet alır ve yeni gelenler arkaya katılır.

Ve teslimat şoförü bir not zincirini takip eder. Her not bir adres verir ve sonraki notun nerede bulunacağını söyler. Beşinci durağa ulaşmak için ilk dört notu takip etmeniz gerekir.

Bunların her biri şeyleri düzenlemenin bir yolu. Kodda veriyi düzenlemenin bir yoluna **veri yapısı** (data structure) denir. Basitçe, belli bir şekilde düzenlenmiş bazı değerlerdir. Onları ekleme, çıkarma ve bulma kurallarıyla birlikte gelir.

Hiçbir düzenleme her şeyde hızlı değildir. Deneyin. "214 numaradaki paketi getir" için hangi düzenleme en iyi? Ve "en uzun süre bekleyene hizmet et" için hangisi? Durun ve karar verin.

Numaralı raf ilk işi kazanır. Tek adımda doğrudan oraya gidersiniz. Sıra ikinciyi kazanır, çünkü öndeki kişi her zaman doğru kişidir. Ama sıradan 214. kişiyi isteyin, sırayı tek tek saymanız gerekir. Rafa da ilk kimin geldiğini sorun, hiçbir fikri yoktur.

Bu yüzden tek bir en iyi değil, pek çok veri yapısı vardır. Her biri bazı işleri ucuz, diğerlerini pahalı yapar.

O işlerin çoğu kısa bir listeye iner. Bir öğe eklersiniz, bir öğe çıkarırsınız, bir öğe bulursunuz, belirli bir sıradaki öğeyi alırsınız ve her öğeyi bir kez dolaşırsınız. Her veri yapısının bu işlerin her biri için bir Büyük O maliyeti vardır.

Altta bellekte de bir neden var. Bellek, numaralı yuvalardan oluşan uzun bir sıradır. Bazı yapılar öğelerini o sırada yan yana tutar, numaralı raf gibi. Diğerleri onları dağıtır ve her öğe bir sonrakini gösterir, not zinciri gibi. Bu tek seçim, yan yana mı dağınık mı, bir yapının neyde iyi neyde kötü olduğunun çoğunu açıklar.

İyi haber şu ki bunları nadiren kendiniz kurarsınız. Diliniz çoğunu zaten veriyor: listeler, sözlükler ve kümeler gibi. Beceri onları kurmak değil, hangisini seçeceğinizi bilmek.

Akılda kalsın: Veri yapısı, bazı işler hızlı olsun diye veriyi düzenlemenin bir yoludur. Birini seçmeden önce en sık hangi işleri yapacağınızı sorun.

## Dizi (array)

Dükkandaki numaralı rafın kodda bir adı var: **dizi** (array). Çoğu dilde bir listenin altında yatan budur. Sona öğe eklersiniz, buna ekleme (append) denir, ve öğeleri sıralarına göre okursunuz.

Onunla ilgili bir bulmaca. Bir program bir günlük (log) tutuyor diyelim, yani olan her şeyin yürüyen bir listesi. Ona tek tek bir milyon kayıt ekliyor. Neredeyse her ekleme anında oluyor. Ama arada bir, bir ekleme fark edilir bir duraksama yapıyor. Bu arada 500. kaydı okumak, liste ne kadar büyürse büyüsün her zaman hızlı. İkisi de bir dizinin nasıl kurulduğundan geliyor.

Dizi, aralarında hiçbir şey olmadan art arda eşit yuvalara bölünmüş bir bellek bloğudur. Sayılar gibi basit değerler için her yuva değerin kendisini tutar. Nesneler için bir yuva bir referans tutar, nesnenin asıl yaşadığı yere giden bir ok.

Yuvalar eşit aralıklı olduğu için herhangi bir sırayı bulmak basit bir aritmetiktir. Deneyin. Dizi bellek adresi 1.000'de başlıyor ve her yuva 8 byte alıyor diyelim. Sıfırdan sayarak 3. sıradaki öğe nerede durur? Durun ve hesaplayın. 1.000 + 3 × 8. Bu 1.024.

Bilgisayar aynı toplamayı 500. sıra ya da 5 milyonuncu sıra için yapar. Bu O(1). Ve yuvalar yan yana durduğu için önbellek de yardım eder. Bilgisayar bir yuvayı okuduğunda komşuları da beraberinde getirir. Yani bir diziyi baştan sona dolaşmak çok hızlıdır.

Şimdi zayıflıklar. Bloğun, oluşturulurken seçilen sabit bir boyutu var. Her yeri dolu dükkan rafını düşünün. Bir paket daha eklemek için daha büyük bir rafa ihtiyacınız var. Onu kurarsınız, her paketi oraya taşırsınız, sonra yenisini eklersiniz. Bir dizi için o taşıma her öğenin kopyalanması demek. Bu O(N).

Dizi her seferinde yalnız bir yuva büyüseydi her ekleme tam bir kopya tetiklerdi. N öğe eklemek o zaman ikinci dereceden, O(N²) olurdu.

Bu yüzden büyüyebilen listeler önceden plan yapar. Blok dolunca çok daha büyük bir tane alırlar, çoğu zaman 1,5 ya da 2 katı.

Tahmin edilecek bir tane. Küçük başlayıp her seferinde ikiye katlayarak, bir milyon öğe eklerken kaç kopyalama olur? Bir saniye düşünün. Yalnız yaklaşık 20. Her kopya bir öncekinden büyük ama hepsini toplayınca toplamda yaklaşık 2 milyon taşıma eder. Bu, öğe başına yalnız yaklaşık iki taşıma. Eklemelerin çoğu boş yere düşer ve neredeyse hiçbir şeye mal olmaz.

Mühendisler buna **amorti edilmiş** (amortized) der, yani yayılmış demek, bir şeyin bedelini bir seferde değil küçük aylık taksitlerle ödemek gibi. Yani bir listeye eklemek amorti edilmiş O(1)'dir. O seyrek kopyalar, günlükteki duraksamalardır.

Diğer zayıflık orta kısım. 3. yere bir paket sıkıştırmak için ondan sonraki her paketin bir yer aşağı kayması gerekir. Dizi de aynı şekilde çalışır. Sondan başka herhangi bir yere ekleme ya da oradan silme O(N)'dir.

Neredeyse herkesin yaptığı bir hata daha var. Bir listeyi dolaşırken istemediğiniz öğeleri çıkardığınızı düşünün. Birini çıkardığınızda ondan sonraki her şey bir yuva aşağı kayar, böylece döngü yerine yeni kayan öğeyi atlar. Bazı diller bunun yerine bir hatayla durur. Güvenli çözüm, tutmak istediğiniz öğelerden yeni bir liste oluşturmak ya da bir kopya üzerinde dönmektir.

Yani profil şöyle. Sıraya göre okumak hızlı. Sırayla dolaşmak hızlı. Sona eklemek ortalamada ucuz. Ortaya ekleme ya da ortadan silme yavaş. Bu yüzden diziye dayalı liste neredeyse her dilde varsayılandır. Ne kullanacağınızdan emin değilseniz onunla başlayın.

## Bağlı liste (linked list)

Dizinin en büyük zayıflığı orta kısım. Oraya eklemek, ondan sonraki her şeyi kaydırmak demek. Akla gelen çözüm hiç kaydırmak zorunda olmayan bir yapı. Dükkanın teslimat şoförünün not zinciri bu. Kodda adı **bağlı liste** (linked list).

Her not bir öğe artı bir sonraki notun nerede bulunacağını tutar. Kodda her nota düğüm (node) denir. Yer olan neresiyse orada kendi küçük bellek parçasında yaşar. Düğüm öğeyi ve bir işaretçiyi, yani bir sonraki düğüme bir referansı tutar. Siz ilk düğümü tutarsınız, o da sizi geri kalanına götürür.

Bu, eklemeyi kolaylaştırır. Diyelim ikinci ve üçüncü durağın arasına yeni bir durak istiyorsunuz. Yeni bir not yazıp onu üçüncü durağı gösterecek şekilde ayarlıyorsunuz. Sonra ikinci durağı yeni notu gösterecek şekilde değiştiriyorsunuz. Başka hiçbir şey kıpırdamıyor. Bu O(1). Bir durağı çıkarmak da aynı kolaylıkta. Önündeki durağı onun ötesini gösterecek şekilde değiştirirsiniz.

Ama bir sorun var. İkinci duraktan sonra eklemek için önce ikinci durağa ulaşmanız gerekir. Oraya gitmenin tek yolu zinciri baştan takip etmek. Bir dizinin yaptığı gibi 500. sıraya aritmetikle atlamak yok. 500 işaretçi takip edersiniz. Yani bir sıraya ulaşmak O(N).

Bir bellek bedeli de var. Her düğüm öğesinin yanında en az bir işaretçi taşır. Sayılar gibi küçük öğeler için o işaretçiler öğelerin kendisi kadar bellek tutabilir. Dizi yalnız öğeleri saklar.

Yaygın bir sürpriz. Bir ders kitabı eklemelerde bağlı listelerin kazandığını söylüyor. Kodunuz sürekli bir listenin ortasına ekleme yapıyor, siz de geçiyorsunuz. Sonra bir hız testi yapıyorsunuz ve daha yavaş. Ne oldu? Bir tahmin yürütün.

Ekleme işleminin kendisi gerçekten O(1). Ama ortaya ulaşmak için kodun yüzlerce işaretçi takip etmesi gerekti. Ve düğümler bellekte dağınık olduğu için bu atlamaların çoğu önbelleği ıskaladı. Dizi ise öğelerini, önbelleğin iyi idare ettiği tek bir hızlı, yan yana kopyayla kaydırıyor. Bağlı listenin kaybettiği yer, o noktaya yapılan yürüyüş.

Yani bağlı liste ancak değiştirmek istediğiniz düğümü zaten elinizde tutuyorsanız kazanır. Listenin en önü bir örnek. O zaman hiç yürüyüş yoktur.

Her düğümün iki yönü de gösterdiği bir sürümü de var, ileriye sonrakine ve geriye öncekine. Buna **çift bağlı liste** (doubly linked list) denir. Her düğümde ikinci bir işaretçiye mal olur. Karşılığında onu iki yönde de dolaşabilir ve elinizde yalnız o düğüm varken bir düğümü çıkarabilirsiniz.

Bağlı listeleri bilmeye yine de değer. Bazı hash tabloları ve önbellekler gibi başka yapıların içinde kullanılırlar. Mülakatlar, işaretçileri anlayıp anlamadığınızı test etmek için onları kullanır. Ve başka düğümleri gösteren düğümler, kursun ilerisinde gelen ağaçların ve çizgelerin başlangıç noktasıdır.

Yani teoride eklemelerde bağlı liste kazanır. Gerçek donanımda genellikle dizi kazanır. O yüzden varsayılan olarak dizi kullanın.

## Yığıt (stack)

Diziler ve bağlı listeler herhangi bir öğeye ulaşmanıza izin verir. Bazen bu istediğinizden fazla özgürlüktür ve katı bir kural daha iyi çalışır.

Dükkanın kapısının yanındaki alışveriş sepeti yığınını düşünün. Aldığınız sepet her zaman birinin en son geri koyduğu sepettir. Bu bir **yığıt** (stack). Kural, en son giren ilk çıkar. Mühendisler bunu LIFO diye yazar, last in first out.

Bir yığıtın üç hamlesi var. Üste eklemeye push denir. Üstten almaya pop. Üsttekini almadan ona bakmaya peek. Üçü de O(1), çünkü her şey tek bir yerde olur.

Bu kuralın tam doğru olduğu bir yer. Bir metin düzenleyici için geri almayı nasıl kurarsınız? Kullanıcı bir kelime yazıyor, sonra bir satır siliyor, sonra bir paragraf yapıştırıyor, sonra geri al'a basıyor. Hangi değişiklik önce geri gelmeli? Bir saniye düşünün. Yapıştırma, çünkü en yenisiydi. Tekrar geri al'a basın, silme gelir. Her zaman önce en yeni değişikliğe ihtiyacınız var. Yani her düzenleme bir yığıta push edilir ve geri al üsttekini pop eder. Bir tarayıcının geri düğmesi de aynı şekilde çalışabilir.

Elle çözülmüş bir tane daha. Bir koddaki parantezlerin dengeli olup olmadığını kontrol etmek istiyorsunuz. Her açılan parantezin doğru sırada eşleşen bir kapanan parantezi olmalı. Bir açılan normal parantez, bir açılan köşeli parantez, bir kapanan köşeli parantez ve bir kapanan normal parantez alın: `( [ ] )`.

Açılan normal parantezi push edin. Açılan köşeli parantezi push edin. Şimdi kapanan köşeli parantez geliyor. Üsttekini pop edin. Açılan köşeli parantez, eşleşiyor. Sonra kapanan normal parantez geliyor. Üsttekini pop edin. Açılan normal parantez. Bir eşleşme daha. Sonunda yığıt boş, yani parantezler dengeli.

Şimdi kendiniz deneyin. Bir açılan normal parantez, sonra bir kapanan köşeli parantez: `( ]`. Dengeli mi değil mi? Durun ve çözün. Dengeli değil. Açılan normal parantezi push edersiniz, sonra kapanan köşeli parantez onu pop eder ve yanlış türü bulur. Parantezler ayrıca bir pop yığıtı boş bulursa ya da sonunda bir şey artarsa da başarısız olur.

En önemli yığıtla zaten tanıştınız. Daha önceki tepsi yığını olan çağrı yığıtı. Her fonksiyon çağrısı bir çerçeve push eder ve her dönüş bir tane pop eder. En son çağrının her zaman ilk bitmesinin nedeni bu.

Çağrı yığıtı, dilin sizin için yönettiği bellektir ve küçüktür. Kendi yaptığınız bir yığıt aynı kurala uyar ama genellikle heap'te yaşar, bu yüzden çok daha büyüyebilir. Çok derin bir özyinelemenin kendi yığıtınızla bir döngüye çevrilebilmesinin nedeni bu.

İçeride bir yığıt genellikle sadece bir dizidir. Push sona ekler ve pop sondan çıkarır. İkisi de ucuz ve dizinin hızını da elde edersiniz.

Yani bir problem, eskiler sırasını beklerken önce en yeniye ihtiyaç duyuyorsa bir yığıt kullanın.

## Kuyruk (queue)

Yığıt her zaman önce en yeni öğeye hizmet eder. Pek çok gerçek iş tam tersine ihtiyaç duyar. Kasadaki sırayı düşünün. Yeni müşteriler arkaya katılır ve öndeki kişi sıradaki hizmet alandır. Bu bir **kuyruk** (queue). Kural, ilk giren ilk çıkar, FIFO diye yazılır. Bu bir adalet kuralı. En uzun bekleyen sıradaki olur.

Bir kuyruğun iki hamlesi var. Arkaya eklemeye enqueue denir. Önden almaya dequeue. İkisi de yaklaşık bir adım sürmeli.

Bunun önemli olduğu bir yer. Lansman gününde bin kişi bir dakika içinde kaydoluyor. Her biri kaydolduğu sırayla bir hoş geldin e-postası almalı. Ama kimse e-postası gönderilirken bir yükleme sayfasında beklememeli. Bu yüzden kayıt sayfası e-postayı bir kuyruğa ekler ve hemen biter. İşçi (worker) denen ayrı bir program işleri önden, en eskiden başlayarak tek tek alır. Kuyruk bütün yoğunluğu tutar ve her şeyi sırasıyla korur.

Peki bir kuyruğu nasıl kurarsınız? Akla gelen fikir bir dizi. Arkaya eklemek kolay, sadece bir append. Ama önden aldığınızda ne olur? Durun ve düşünün. Her öğenin bir yuva öne kayması gerekir. Bu her seferinde O(N). Uzun bir kuyruk için yavaş.

Bu yüzden gerçek kuyruklar farklı kurulur. Bir yol, önü çıkarmanın sadece bir işaretçi değişikliği olduğu bir bağlı liste. Diğeri **dairesel dizi** (circular array). Dizinin yuvalarını bir masanın etrafındaki sandalyeler gibi bir halka şeklinde dizilmiş düşünün. Kuyruk iki işaretçi tutar, biri ön, biri arka için. Önden almak sadece ön işaretçiyi bir sandalye ilerletir. Eklemek arka işaretçiyi ilerletir. Bir işaretçi son sandalyeye ulaşınca başa, ilkine sarar. Hiçbir şey kaymaz.

Çoğu dil bu iki yoldan biriyle kurulmuş hazır bir kuyruk verir. Onu kullanın.

Bir uyarı var. İş, işçinin halledebileceğinden hızlı gelmeye devam ederse kuyruk sadece büyümeye devam eder. Bu yüzden gerçek sistemler kuyruğa bir sınır koyar ya da gönderenlerden yavaşlamalarını ister.

Kuyruklar programların nasıl keşif yaptığını da şekillendirir. Diyelim bir arkadaş ağını seviye seviye inceliyorsunuz. Önce bütün arkadaşlarınızı, sonra onların bütün arkadaşlarını ziyaret edersiniz. Bir kuyruk sıradakinin kim olduğunu sırasıyla hatırlar. Buna **genişlik öncelikli arama** (breadth-first search) denir. Bir arkadaştan diğerine adım sayısıyla ölçülen en kısa yolu bulur. Onun yerine bir yığıt kullanın, önce tek bir yola dalarsınız. Seçtiğiniz yapı sırayı belirler.

İlişkili iki yapının adını da duyacaksınız. Deque (deste diye okunur, "deck") çift uçlu kuyruğun kısaltmasıdır. İki uçtan da ekleyip çıkarmanıza izin verir. Öncelikli kuyruk (priority queue) ise en eskiye değil, en önemli öğeye önce hizmet eder.

Yani bir kuyruk önce en eskiye hizmet eder. Adalet ve bir yoğunluğu sırayla işlemek için iyidir. İkisinden yanlışını kullanırsanız hatayı fark etmek kolaydır. En eski değişikliğinizi geri getiren bir geri al düğmesi ya da en yeniden başlayarak giden hoş geldin e-postaları elde edersiniz.

## İki işaretçi ve kayan pencere

Diziler basittir ama pek çok yavaş kod aynı dizi üzerinde iç içe bir döngü çalıştırır. İki gündelik hile, bu ikinci dereceden işlerin pek çoğunu tek bir dolaşmaya çevirir. Adları **iki işaretçi** (two pointers) ve **kayan pencere** (sliding window).

İki işaretçiyle başlayın. Bir alışverişçinin 43 dolarlık bir hediye kartı var ve onu tam olarak kullanan iki ürün istiyor. Fiyatlar en ucuzdan en pahalıya sıralı: 5, 12, 18, 25, 32 ve 45 dolar.

Akla gelen yol her çifti denemek, bu da ikinci dereceden. 10.000 ürünlük bir katalog için bu yaklaşık 50 milyon çift.

Hile şu. Bir parmağınızı en ucuz ürüne, birini en pahalıya koyun. Toplayın. Toplam fazlaysa sağ parmağı bir adım sola, daha ucuz bir ürüne kaydırın. Azsa sol parmağı bir adım sağa, daha pahalı bir ürüne kaydırın. Eşleşince ya da parmaklar buluşunca durun.

İzleyelim. 5 + 45 = 50, fazla. Sağ parmak 32'ye kayar. 5 + 32 = 37, az. Sol parmak 12'ye kayar. 12 + 32 = 44, fazla. Sağ parmak 25'e kayar. 12 + 25 = 37, az. Sol parmak 18'e kayar. 18 + 25 = 43. Eşleşme.

Şimdi kendiniz deneyin. Aynı fiyatlar ama kart 30 dolarlık. Hangi iki ürün? Durun ve çözün. 5 + 45 fazla. 5 + 32 hâlâ fazla. 5 + 25 = 30. İşte çiftiniz.

Neden işe yarıyor? Çünkü liste sıralı. Her adım bir öğeyi aramanın geri kalanı için eler. Parmaklar yalnız birbirine doğru hareket eder. Yani bütün arama tek bir dolaşma. Bu O(N²) yerine O(N).

Şimdi kayan pencere. Mağaza en iyi 3 günlük satış dönemini istiyor. Günlük satışlar 4, 2, 7, 1, 5 ve 3.

Akla gelen yol, 3 günlük her grubu sıfırdan toplamak. Uzun dönemler için bu çok fazla tekrarlı toplama, çünkü komşu gruplar günlerinin çoğunu paylaşıyor.

Hile şu. İlk üç günü bir kez toplayın. 4 + 2 + 7 = 13. Şimdi pencereyi bir gün ileri kaydırın. En yeni gün katılır ve en eski gün düşer. Yani yeni günü ekleyip eskisini çıkarırsınız. Sonraki toplam 13 + 1 − 4. Bu 10.

Tekrar kaydırın. Sonraki toplam ne? Durun ve çözün. 10 + 5 − 2. Bu 13. Bir kaydırma daha 13 + 3 − 7 verir, bu 9. En iyi dönem 13.

Pencere ne kadar geniş olursa olsun her kaydırma yalnız iki adım sürer. Yani bütün iş tek bir dolaşma, O(N).

İki hile de aynı şekilde kazanır. Zaten yaptıkları işi hiç tekrar yapmazlar. Sıralı veride bir çift arıyorsanız iki işaretçi kullanın. Arka arkaya gelen şeylerin dönemlerine bakıyorsanız kayan pencere kullanın. Bunlar günler, ölçümler ya da bir kelimedeki harfler olabilir.

## Doğru yapıyı seçmek

Artık dört yapıyla tanıştınız: diziler, bağlı listeler, yığıtlar ve kuyruklar. Zor olan onları öğrenmek değil, gerçek kodda aralarından seçim yapmak.

Diyelim bir pull request'i inceliyorsunuz. Bu, bir takım arkadaşının ana koda eklenmeden önce kontrol etmeniz için gönderdiği bir kod değişikliği. Düz bir listeyi bağlı listeyle değiştirmişler. Notları "Eklemeler O(1)" diyor. Ama kod çoğunlukla öğeleri sıraya göre okuyor ve sona ekliyor. İyi bir değişiklik mi?

Tek bir alışkanlığa iner. Kodun gerçekte hangi işlemleri yaptığını ve ne sıklıkla yaptığını sorun. Öğeleri sıraya göre mi arıyor, sona mı ekliyor? Ortaya ekleme var mı? En yeni öğeyi önce mi istiyor, en eskiyi mi?

Kod sıraya göre arıyor, sona ekliyor ve sırayla tarıyorsa bir dizi kullanın. Üçünü de ucuza yapar.

Şimdi diyelim kod önde ya da zaten elinde tuttuğu düğümlerin yanında çok ekleme ve çıkarma yapıyor ve sıraya göre nadiren arıyor. O zaman bir bağlı liste uygun olabilir, ama geçmeden önce ölçün.

O pull request'e dönersek, kod sıraya göre okuyor ve sona ekliyor. Bu dizinin işi. Yani takım arkadaşınızdan düz listeyi tutmasını istersiniz.

Burada bir ayrım önemli. Diziler ve bağlı listeler veriyi bellekte yerleştirmenin yollarıdır. Yığıtlar ve kuyruklar hangi öğenin sırada çıkacağına dair kurallardır. Yani bir yığıtı ya da kuyruğu ikisinden biri üzerine kurabilirsiniz. Önce en yeni öğeyi mi istiyorsunuz? Bu bir yığıt. Önce en eskiyi? Bu bir kuyruk. İki uca da ihtiyacınız var mı? Bu bir deque.

Bir tane deneyelim. Bir destek uygulaması son 50 müşteri mesajını ekranda tutuyor. Yeni mesajlar sona geliyor. En eskiler önden düşüyor ve ekran onları sırayla gösteriyor. İşlemler arkaya ekle, önden çıkar ve sırayla dolaş. Bu bir kuyruk. Dairesel dizi, sandalye halkası, üçünü de ucuza yapar.

Bir tane daha. Ben seçmeden önce bir an durup karar verin. Bir arka plan e-posta sistemi işleri geldikleri sırayla gönderiyor. Arada bir bir müşteri iptal ediyor. O zaman sistem çıkarılacak bir iş numarası alıyor.

Ana işlem en eski önce, yani bu bir kuyruk. Yeni işler arkaya katılır ve gönderen önden alır. İptal farklı. Bu bir değere göre arama. Kuyruğu dolaşıp her numarayı kontrol edersiniz. Bu O(N). Bu bir sorun mu? İptaller seyrek. Binlerce gönderime karşı günde belki birkaç tane. Arada bir birkaç bin işi taramak neredeyse hiçbir şeye mal olmaz. Yani kuyruğu tutun ve taramayı kabul edin.

İptaller sıklaşırsa bir hash tablosu ekleyebilirsiniz. Bu, daha önce tanıştığınız arama tablosu ve her işi numarasıyla bulur. Kuyruğu bir çift bağlı liste olarak tutun. O zaman tablo işi bulur ve liste onun düğümünü hızla koparır. Gerçek özellikler çoğu zaman bunu yapar. Tek bir yapı nadiren her işi yapar, bu yüzden çoğu zaman ikisi birlikte çalışır.

Dördünün de neyden yoksun olduğuna dikkat edin. Hiçbiri bir öğeyi değerine göre hızlı bulamaz. "Bu müşteri burada mı?" diye sormak her öğeyi kontrol etmek demek ve bu O(N). Sıralı bir dizi ikili aramayı, yarıya bölme hilesini kullanabilir, ama o zaman her ekleme onu sıralı tutmanın bedelini öder. Hash tablolarının ve ağaçların var olmasının nedeni bu tek boşluk.

## Modül özeti

Bir koleksiyonu tutmanın en basit yolları bu kadar. Veri yapısı, bazı işler hızlı olsun diye veriyi düzenlemenin bir yoludur. Hiçbir düzenleme her şeyde hızlı değildir.

Dizi öğeleri yan yana tutar. Yani herhangi bir sırayı okumak bir adım sürer ve sırayla dolaşmak hızlıdır. Sona eklemek ortalamada ucuzdur ama ortaya eklemek ondan sonraki her şeyi kaydırmak demektir.

Bağlı liste öğeleri işaretçilerle zincirler. Elinizde tuttuğunuz bir düğümün yanında değişiklik yapmak ucuzdur. Ama bir sıraya ulaşmak zinciri takip etmek demektir, bu yüzden genellikle yine dizi kazanır. Ve iki gündelik hile, iki işaretçi ve kayan pencere, pek çok ikinci dereceden dizi işini tek bir dolaşmaya çevirir.

Yığıt önce en yeni öğeye hizmet eder: geri al, parantezler ve çağrı yığıtı için. Kuyruk önce en eskiye hizmet eder: adalet ve bir yoğunluğu sırayla işlemek için. Bir kuyruğu bir sandalye halkası ya da bir bağlı liste üzerine kurun. Asla düz bir dizinin önünden çıkararak kurmayın.

Yani bir yapı seçerken önce bir şeyi sorun: Kod onunla gerçekte ne yapıyor? Ve ne sıklıkla?

Bu dördünden hiçbiri bir öğeyi değerine göre hızlı bulamaz. Sonraki modüldeki yapılar bunu çözüyor.
