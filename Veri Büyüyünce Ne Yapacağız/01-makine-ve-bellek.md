# Modül 1: Verinin makinede nasıl durduğu

## Modüle giriş

Kod yazarken adlarla çalışırsınız. `total` adında bir değişkeniniz ya da `customers` adında bir listeniz olabilir. Ama bilgisayar bu adları hiç görmez. Elinde olan tek şey bitlerdir: kapalı ya da açık olan minik anahtarlar, bellekte numaralı yerlerde dururlar.

Diyelim `total` 42 sayısını tutuyor. Makinenin içinde bu, sekiz anahtardan üçünün açık olması demek. Aynı sekiz anahtar bir harf ya da bir rengin bir parçası da olabilir. Her şey kodunuzun onları nasıl okuduğuna bağlı.

Bu modül tam buradan başlıyor. Tam sayıların, negatif sayıların, metnin ve kesirlerin nasıl bite dönüştüğünü gösteriyor. Sonra program çalışırken bu bitlerin nerede durduğuna bakıyor, işlemcinin yanındaki hızlı bellekten diske kadar. Verinizi bir yerden bir yere geçirdiğinizde ne olduğuyla bitiyor. Bu, "nerede durur?" sorusu ve kurs boyunca tekrar tekrar karşımıza çıkacak.

## Bilgisayar nasıl sayar?

Nasıl saydığınızı düşünün. 0'dan 9'a kadar 10 rakamınız var. 9'a gelip rakamlar bitince yeni bir sütun açarsınız ve 9, 10 olur. Yazdığınız her sayı bu şekilde kurulur. Bu yüzden buna 10 tabanı denir. Taban, kullanabileceğiniz rakam sayısıdır.

Bilgisayarın yalnız iki rakamı vardır: 0 ve 1. Bu yüzden 2 tabanında sayar. Buna ikili sistem (binary) de denir.

Neden yalnız iki? Donanımın içinde her veri parçası iki durumu olan bir şeyde tutulur. Bir bellek yongasında bu, ya olan ya da olmayan minik bir yük olabilir. Bir telde, yüksek ya da düşük bir gerilim olabilir. İki durumu, çok küçük parçalarda bile ayırt etmek kolaydır. Bu yüzden bilgisayar her birini 0 ya da 1 olarak okur.

Bu sıfır ya da birlerden her birine **bit** denir. Bit, bilgisayarın saklayabileceği en küçük bilgi parçasıdır. Onu bir ışık anahtarı gibi düşünün. Ya kapalıdır ya açık, ikisi birden olamaz. Tek başına bir bit yalnız evet ya da hayır sorusunu cevaplayabilir. Güç, bitleri gruplar halinde bir araya getirmekten gelir.

Anahtar ekledikçe ne olduğuna bakın. Bir anahtar iki desen verir: kapalı ya da açık. İki anahtar dört desen verir: ikisi de kapalı, ikisi de açık, yalnız birincisi açık ya da yalnız ikincisi açık. Şimdi üç anahtar deneyin. Kaç desen yapabilirsiniz? Cevaplamadan önce bir saniye sayın.

Cevap sekiz. Her anahtar eklediğinizde desen sayısı ikiye katlanır. Çünkü yeni anahtar, zaten var olan her desen için kapalı ya da açık olabilir. İkiye katlamaya devam edin: dört anahtar 16 desen, sekiz anahtar 256 desen verir. Bu, 2'nin kendisiyle 8 kez çarpımıdır, 2 üzeri 8 diye yazılır.

Sekiz bitlik bir grubun kendi adı vardır: **byte**. Bellek, dosya boyutları ve depolama alanı hep byte ile ölçülür.

Peki bir byte bir sayıyı nasıl gösterir? 10 tabanında zaten kullandığınız hileyle. 235 sayısını alın. 2, iki yüzlük demek. 3, üç onluk ve 5, beş birlik. Her rakamın değeri durduğu yere bağlıdır. Her basamak, sağındakinin 10 katı değerindedir.

İkili sistem aynı fikri ikilerle kullanır. Her basamak, sağındakinin iki katı değerindedir. En sağdaki basamak 1 değerindedir. Sonraki 2, sonra 4, sonra 8. Değerler ikiye katlanarak gider ve bir byte'ın en solundaki basamak 128 değerindedir.

Artık bir ikili sayıyı okuyabilirsiniz. Yalnız 1 olan basamakların değerlerini toplayın. 101'i alın. Sağdaki 1, bir değerinde. Ortadaki basamak iki değerinde ama 0 tutuyor, bir şey eklemiyor. Soldaki 1, dört değerinde. 4 + 1 = 5. Yani ikili sistemde 101, beş sayısıdır. Tam bir byte içinde 5'in önüne beş tane fazladan sıfır gelir. Bu sıfırlar bir şey eklemez, değer yine beştir.

Akılda kalsın: Bilgisayar 2 tabanında, kapalı ya da açık olan bitlerle sayar. Sekiz bit bir byte eder. Bir ikili sayı, açık olan basamakların toplamından ibarettir.

## Bir sayı ikiliye nasıl çevrilir?

Bilgisayar her sayıyı sıfır ve bir olarak saklar. Peki 89 gibi bir sayıyı ikiliye nasıl çevirirsiniz? Tek bir yönteme ihtiyacınız var ve bunu kağıt kalemle yapabilirsiniz.

Yan yana sekiz boş kutu düşünün. Bu bir byte. Her kutu 0 ya da 1 tutacak. Şimdi her kutuya bir etiket verin. En sağdaki kutunun üstüne 1 yazın. Sonra sola gidip etiketi her seferinde ikiye katlayın. Etiketler 1'den 2'ye, sonra 4'e, sonra 8'e büyür. Devam edin, en soldaki kutu 128 değerinde olur. Bu etiketler ikili sistemin basamak değerleridir.

Çevrilecek bir sayı: Bir internet mağazası bugün bir kitaptan 89 adet sattı diyelim. Bu sayıyı bir byte olarak görmek istiyorsunuz.

En soldaki kutudan başlayın. 128, 89'a sığar mı? Hayır, çok büyük. O kutu 0 alır. 64'e geçin. 89'a sığar mı? Evet. O kutuya 1 koyun ve çıkarın. 89 − 64 = 25 kalır. Sırada 32 var. 25'e sığmaz, o kutu 0 alır. 16 sığar, oraya 1 koyun. 25 − 16 = 9 kalır. 8, 9'a sığar. O kutuya 1 koyun, 1 kalır. 4, 1'e sığmaz, o kutu 0. 2 de sığmaz, bir 0 daha. Son kutu 1 değerinde ve kalan tam olarak bu. Oraya 1 koyun. Hiçbir şey kalmadı, bitti.

Şimdi kutuları soldan sağa okuyun: 0 1 0 1 1 0 0 1. Bu, 89'un ikili hali.

Cevabınızı her zaman kontrol edin. 1 tutan kutuların etiketlerini toplayın. 64 + 16 = 80. 8 ekleyin, 88. Bir tane daha ekleyin, 89'a döndünüz. Cevap doğru.

Yani yöntemin tamamı iki adım. Soldan sağa çalışın. Etiket kalana sığıyorsa 1 yazıp çıkarın. Sığmıyorsa 0 yazıp geçin.

Şimdi kendiniz deneyin. Videoyu durdurun ve 150'yi 8 bite çevirin.

Şöyle gidiyor: 128 sığar, 22 kalır. 64 sığmaz, 32 de sığmaz. 16 sığar, 6 kalır. 8 sığmaz. 4 sığar, 2 kalır. 2 sığar ve hiçbir şey kalmaz. Son kutu 0 alır. Yani 150, 1 0 0 1 0 1 1 0'dır.

Aynı kutular bir byte'ın sınırlarını da gösterir. Bütün kutuları kapatın, değer 0 olur. Yazabileceğiniz en küçük sayı bu. Şimdi bütün kutuları açıp sekiz etiketin hepsini toplayın. 255 çıkar. En büyüğü bu.

Bu tavan değeri bulmanın daha hızlı bir yolu var. 8 bit 256 desen verir. Sayma sıfırdan başlar. Yani en büyük değer bir eksiği, 255'tir.

Peki mağaza 300 kitap satarsa ne olur? 300 sekiz kutuya sığmaz. Bütün kutular açık olsa bile ancak 255'e ulaşırsınız. 256 değerinde bir dokuzuncu kutuya ihtiyacınız olurdu. Bu yüzden daha büyük sayılara 2, 4 ya da 8 byte verilir.

Bir şartı var. Bu aralık, ancak her desen sıfır ya da pozitif bir sayı anlamına geliyorsa geçerlidir. Buna işaretsiz (unsigned) sayı denir.

## Negatif sayılar nasıl saklanır?

Bir byte'ı sekiz kutu olarak çizin ve hepsini kapatın. Bu sıfır. Hepsini açın, 255 olur. Arada kalan her sayı sıfır ya da pozitiftir. Böyle bir sayıya, artı ya da eksi işaretine hiç ihtiyacı olmadığı için işaretsiz denir.

Ama pek çok sayının işarete ihtiyacı vardır. Bir banka bakiyesi sıfırın altına düşebilir. Sıcaklık da. Satılandan fazla ürün iade edildiğinde bir mağazanın stok sayısı da. Elinizde yalnız sıfır ve birler varken eksi işaretini nasıl saklarsınız?

Önce en akla yatkın fikri deneyelim. En soldaki kutuya yeni bir görev verin. 128 değerinde olmak yerine artık tek bir soruyu cevaplasın: Bu sayı negatif mi? 0 hayır, 1 evet demek. Bu kutuya işaret biti (sign bit) denir. Diğer yedi kutu yine sayının büyüklüğünü tutar. Etiketleri 1'den 64'e kadar gider. Yedisini de açarsanız toplam 127 eder. İşaret biti kapalıyken bu +127. İşaret bitini açın, −127 olur.

Bu işe yarıyor gibi ama içinde saklı bir sorun var. Yedi sayı kutusunun hepsini kapatın. İşaret biti kapalıyken sıfır elde edersiniz. Şimdi işaret bitini açın. −0 elde edersiniz. Ama 0 sadece sıfırdır. Onun için iki desen olması birini boşa harcar ve sayıları toplamayı karmaşıklaştırır.

Bu ilk fikre işaret ve büyüklük (sign and magnitude) denir. Kafada canlandırması kolaydır ama bilgisayarlar tam sayılar için bunu kullanmaz. Onun yerine kullandıkları şeye **ikiye tümleyen** (two's complement) denir.

Değişiklik küçük. En soldaki kutu 128 değerini korur ama artık negatif sayılır. −128 değerindedir. Diğer bütün kutular normal değerlerini korur. Şimdi byte'ı tam eskisi gibi okursunuz: açık kutuları toplayın.

0 1 0 0 0 0 0 1'i deneyin. Sol kutu kapalı. Yalnız 64 ve 1 açık. Bu 65, pozitif bir sayı.

Şimdi 1 0 0 0 0 0 0 0'ı deneyin. Yalnız sol kutu açık. Değer −128.

Kendiniz deneyebileceğiniz bir tane: Arka arkaya 8 tane 1 hangi sayıdır? Durun ve toplayın. −128 ile başlayın. Diğer yedi kutu 127 eder. −128 + 127 = −1. Yani tamamı 1 olan bir byte −1 demektir.

Ve −0 ortadan kalktı. Artık sıfırı yazmanın tek yolu var, bütün kutular kapalı. Bu bir deseni serbest bırakır. Böylece aralık −128'den +127'ye kadar gider. Ayrıca aynı toplama devresi hem işaretli hem işaretsiz sayılar için çalışır, bu da toplama ve çıkarmayı yapmayı çok kolaylaştırır. Bu yüzden bugün neredeyse her bilgisayar işaretli tam sayıları bu şekilde saklar.

Neyin değişmediğine dikkat edin. İki tür de aynı 8 biti ve aynı 256 deseni kullanır. İşaretsiz tür hepsini sıfır ve üstüne harcar. İşaretli tür yarısını negatif sayılara harcar. Bitlerin kendileri hangi türden olduklarını bilmez. Onları nasıl okuyacağına kodunuzdaki tür karar verir.

Yani ayrı bir işaret biti akla gelen ilk tasarımdır ama iki sıfır verir. İkiye tümleyen, sol kutuyu sizin aleyhinize saydırır ve bu tek değişiklik sorunu çözer.

## Her sayının bir tavanı vardır: taşma

Kodunuzdaki her tam sayı sabit sayıda bitte saklanır. Bu, her sayı türünün bir tavanı olduğu anlamına gelir. Onu nasıl bulacağınızı ve bir değer onu geçince ne olduğunu görelim.

Yalnız sıfır ve pozitif sayı tutan bir byte ile başlayın. Bütün kutular açıkken 255'e ulaşır. Sekiz bitin tavanı bu.

Bir mağazanın her müşterinin sadakat damgalarını böyle bir byte'ta saydığını düşünün. Her alışveriş bir damga ekler. 255 alışverişten sonra bütün kutular zaten açık. Gidecek yer kalmadı.

Sekiz kutu daha ekleyin. Artık 16 bitiniz, yani iki byte'ınız var. Etiketler sola gittikçe ikiye katlanmaya devam eder. 16 kutunun hepsini açın, 65.535'e kadar sayabilirsiniz. Sekiz fazladan kutu tavanı 255'ten 65.000'in üstüne çıkardı.

Ve büyümeye devam eder. 32 bit yaklaşık 4 milyara ulaşır. Desenlerin yarısı negatif sayılara gidince bu, iki yönde de yaklaşık 2 milyar olur. 64 bit bunun çok ötesine gider. Ama her boyutun yine de bir tavanı var.

Gerçek kodda tavana çarpmak şöyle görünür. Bir web sitesi sayfa görüntülemelerini 32 bitlik işaretli bir sayıda sayıyor. Aylarca sayı normal şekilde artıyor. Sonra bir sabah gösterge paneli büyük bir negatif sayı gösteriyor.

Nedenini görmek için sorunu tek bir işaretli byte'a küçültün. İkiye tümleyende en soldaki kutu −128 değerinde. Yani en büyük değer, bir sıfır ve ardından yedi 1'dir. Bu +127. Şimdi bir ekleyin. Normal toplamadaki elde gibi çalışır. Bütün 1'ler sıfıra döner ve elde en soldaki kutuya düşer. Elinizde bir 1 ve yedi sıfır kalır, o kutu da −128 değerinde. Yani 127 + 1, −128'e dönüştü.

Tavanı geçmeye **taşma** (overflow) denir. Değer böyle öbür uca atlayınca buna başa sarma (wrapping around) denir. Sayfa görüntüleme sayacı da tam olarak bunu yaptı, sadece 32 bitle. Yaklaşık 2 milyarı geçti ve yaklaşık −2 milyara sardı.

Kodunuzun başa sarıp sarmadığı dile bağlıdır. Java başa sarar ve hiç uyarı vermez. Swift ise programı bir hatayla durdurur. Python'un tam sayıları büyümeye devam eder. Diğer diller başka seçimler yapar. Yani bir sayaç tuhaf bir şey yaparsa, dilinizin ne yaptığını öğrenin.

Çözüm türü seçmekle başlar. Bu değerin bugün ne kadar büyük olduğunu değil, ne kadar büyüyebileceğini sorun. Sayaçlar, dosya boyutları ve kuruş cinsinden para için bu genellikle 64 bit demektir. Bir değer yine de tavanını geçebiliyorsa ne olacağına karar verin. Güncellemeyi reddedebilir, bir hata fırlatabilir ya da daha büyük bir türe geçebilirsiniz.

Yani daha fazla bit tavanı yükseltir ama hiçbir zaman ortadan kaldırmaz.

## On altılık sistem (hex)

Hata ayıklayıcı (debugger), programınızı duraklatıp içine, değişkenlerine ve belleğine bakmanızı sağlayan bir araçtır. Ham belleğe bu şekilde bakınca nadiren uzun sıfır ve bir dizileri görürsünüz. 0x53 gibi şeyler görürsünüz.

Ama bilgisayar her şeyi ikili olarak saklıyor. O zaman geliştiriciler neden başka bir şekilde yazıyor? Cevap, on altılık (hexadecimal) ya da kısaca hex denen bir sayı sistemi. Aynı bitleri yazmanın daha kısa bir yolu.

Nasıl çalıştığını görmek için tek bir byte'ı, 0x53'ü baştan sona izleyin.

Hex, 16 tabanıdır, yani 16 rakamı vardır. İlk 10'u bildiğimiz 0'dan 9'a. Ondan sonra harf ödünç alır. A, 10 demek, B, 11. Harfler F'ye kadar gider ve F, 15 demektir.

Neden 16? Çünkü ikili sisteme tam oturur. Etiketleri 8, 4, 2 ve 1 olan dört kutu çizin. Hepsini kapatın, sıfır elde edersiniz. Hepsini açın, 15 elde edersiniz. Yani dört bit tam 16 desen verir ve bir hex rakamının tam 16 değeri vardır. Dört bitlik her grup bir hex rakamı olur. Bir byte 8 bit olduğu için her byte tam iki hex rakamıdır.

Şimdi byte'ı ikili olarak alın: 0 1 0 1 0 0 1 1. Ortadan ikiye bölün. Sol yarı 0 1 0 1. Etiketlere göre bu 4 + 1, yani sol yarı 5. Sağ yarı 0 0 1 1. Bu 2 + 1, yani 3. Bir araya koyun, byte hex'te 53'tür. Programcılar bunu 0x53 diye yazar. Öndeki 0x, sayının hex yazıldığını söyler.

Hata ayıklayıcıların hex kullanmasının nedeni bu. Sekiz sıfır ve bir, iki kısa karaktere dönüşür ve yine de içindeki her biti okuyabilirsiniz.

Şimdi kendiniz deneyin. 0xFF ikili sistemde nedir? Durun ve çözün. Her F, 15'tir ve 15, dört kutunun hepsinin açık olması demek. Yani FF, arka arkaya sekiz 1'dir. Bütün kutuları açık tam bir byte.

Hex'in bir sınırı var ama. Hangi bitlerin orada olduğunu söyler, ne anlama geldiklerini söylemez. 0x53'e geri dönün. İşaretsiz sayı olarak okuyun, 83'tür. Metin olarak okuyun, büyük S harfidir. Çünkü İngilizce harfler için standart kod olan ASCII, S'ye 83 sayısını verir. UTF-8 de aynı sayıyı kullanır. Byte'ın içinde minik bir S harfi yok. Sekiz anahtar var ve onları nasıl okuyacağına program karar veriyor.

0xFF de aynı şekilde çalışır. İşaretsiz byte olarak okuyun, 255'tir. Sol kutunun −128 sayıldığı işaretli byte olarak okuyun, −1'dir. Anahtarlar aynı, cevaplar farklı.

Bunu bir kez görünce hex'i pek çok yerde fark edersiniz, hep aynı nedenle. Byte'ları göstermenin kısa bir yolu. Bir web rengi iyi bir örnek. Arka arkaya üç byte'tır, biri kırmızı, biri yeşil, biri mavi için. FF0000 renginde kırmızı sonuna kadar açık, diğer ikisi kapalıdır. Bu saf kırmızı.

Yani hex, her rakamına dört bit düşen, kısa yazılmış ikili sistemdir. O bitlerin ne anlama geldiği onları okuyan türe bağlıdır.

## Metin nasıl saklanır?

Bir bilgisayar yalnız sayı saklayabilir. Peki bir adı, bir e-postayı ya da bir mesajı nasıl saklar? Her harfi bir sayıya çevirir. Bu da ancak herkes hangi sayının hangi harf olduğunda anlaşırsa işe yarar.

Anlaşmadıklarında ne olduğuna bakalım. José adında bir müşteri kaydoluyor ve uygulamanız adını gayet düzgün gösteriyor. Sonra biri müşteri listesini bir hesap tablosuna aktarıyor. Orada adı, é yerine J O S ve ardından iki tuhaf simge olarak çıkıyor. Adının byte'ları dosyada hâlâ duruyor. İki program aynı byte'ları okudu ve hangi harfler oldukları konusunda anlaşamadı.

Anlaşmayı baştan kuralım. İki sütunlu bir tablo düşünün. Solda bir sayı, sağda bir harf. 65 büyük A demek. 66 büyük B. Her harf, rakam ve noktalama işaretinin kendi satırı var. Böyle bir tabloya kodlama (encoding) denir.

Hâlâ kullanılan en eskisinin adı ASCII. İngilizce harfleri, rakamları ve noktalamayı kapsar. Her satır tek bir byte'a sığar. Eski bilgisayarlarda İngilizce metin için bu yetiyordu.

Dünyanın geri kalanı için yetmiyordu. Aksanlı harflerin de sayıya ihtiyacı vardı. Başka alfabelerin, Çince karakterlerin ve sonra emojilerin de. Tek bir byte'ın yalnız 256 deseni var, yeterli satıra hiç yakın değil.

Bu yüzden insanlar çok daha büyük bir tablo kurdu: Unicode. Neredeyse her yazı sistemindeki her karaktere ve simgeye bir sayı verir. Bir milyonun üstünde karaktere yer vardır. Bu tablodaki her sayıya kod noktası (code point) denir.

Unicode her karakterin hangi sayıyı alacağına karar verir ama o sayının byte olarak nasıl saklanacağına karar vermez. O ayrı bir adımdır ve bugün bunu yapmanın en yaygın yolu UTF-8'dir.

UTF-8'in akıllıca bir kuralı var. Eski İngilizce karakterler yine birer byte alır, böylece eski dosyalar çalışmaya devam eder. Diğer karakterler iki, üç ya da dört byte alır.

Peki José kaç byte tutar? Cevaplamadan önce sayın. Beş. J, O ve S birer byte alır. é iki byte alır. Yani UTF-8'de bir metnin byte cinsinden uzunluğu ile karakter cinsinden uzunluğu farklı şeylerdir. Emojilerin çoğu dörder byte alır, 10 tanesi yaklaşık 40 byte eder. Bazı emojiler daha da fazlasını alır. Bazı diller de metin uzunluğunu kendi yöntemleriyle sayar, bu yüzden tek bir emoji 2 uzunluğunda görünebilir.

Artık klasik hatalar anlam kazanıyor. Bir program her karakterin bir byte olduğunu varsaysın. Üç byte'lık bir karakterle karşılaşır ve üç tane anlamsız simge ya da bir soru işareti gösterir. Ya da bir UTF-8 dosyası daha eski bir Batı kodlamasıyla açılır. Her aksanlı harf bir çift tuhaf simgeye dönüşür. Hesap tablosundaki José bu. Ya da bir metin bir karakterin ortasından kesilir ve sonunda bozuk bir simge çıkar.

Bunların hepsi aynı hata. Bir kod, kodlamayı bilmeden byte'ları karakter olarak okudu.

Metniniz programınıza girerken de çıkarken de kodlamasını bilin. Bugün UTF-8 yaygın olarak kullanılıyor ve güvenli bir seçim. Ve karakterleri kastederken metni byte üzerinden ölçen ya da kesen her koda şüpheyle bakın. Düz İngilizcenin dışından bir karakter, mesela bir kıvrık tırnak bile, geldiği anda bozulabilir.

Metin bozuk görünüyorsa iki soru sorun: Bu byte'ları hangi kodlama yazdı? Ve hangi kodlama okuyor? Böyle hatalarda neden genellikle ikisinin uyuşmamasıdır.

## 0,1 + 0,2 neden 0,3 etmez?

Pek çok programlama dilinde şunu deneyin. 0,1 ile 0,2'yi toplayın. Sonra cevabın 0,3'e eşit olup olmadığını kontrol edin. 0,3, ardından uzun bir sıfır dizisi ve en sonda bir 4 elde edebilirsiniz.

O fazla rakam tuhaf görünüyor ama bir hata değil. Bilgisayarların kesirleri nasıl sakladığından geliyor.

Tanıdık bir şeyle başlayın. 1'i 3'e bölün. 0,333 elde edersiniz ve üçler hiç bitmez. Bizim onluk sistemimiz 1/3'ü tam olarak yazamaz.

Bilgisayarlar aynı sorunla, sadece farklı kesirlerle karşılaşır. İkili kesirler yarımları, çeyrekleri, sekizde birleri ve daha küçük parçaları kullanır. Her parça bir öncekinin yarısıdır. Bazı kesirler tam oturur. Örneğin 3/4, 1/2 + 1/4'tür.

Ama 1/10 farklı. 1/10'u yarımlardan, çeyreklerden, sekizde birlerden ve daha küçük parçalardan kurmayı deneyin. 1/10'a gittikçe yaklaşabilirsiniz. Ama hangi parçaları birleştirirseniz birleştirin, sabit sayıda parça hiçbir zaman tam 1/10 yapamaz. İkili rakamlar bitmeden devam eder. Yani 1/10 ikili sistemde hiç bitmez, tıpkı 1/3'ün onlukta bitmemesi gibi.

Ama bilgisayarlar sonsuz bir rakam dizisi saklayamaz. Sınırlı sayıda rakam tutup saklayabilecekleri en yakın değeri saklarlar. Bu değer kastettiğiniz sayıdan biraz farklıdır. Bu değerlerden ikisini topladığınızda küçük farklar sonuçta ortaya çıkabilir. 0,1 ile 0,2'yi toplamanın genellikle tam 0,3 vermemesinin nedeni bu.

Bu şekilde saklanan sayılara **kayan noktalı sayılar** (floating point) ya da kısaca float denir. Yaygın 64 bitlik bir float yaklaşık 15 ila 17 basamak doğruluk tutar. Grafikler ve ölçümler gibi pek çok iş için bu yeterli. Ama üç durum ekstra dikkat ister.

Birincisi para. Binlerce siparişte fiyatları topladığınızı düşünün. Pek çok hesaplama boyunca o minik hatalar son tutarı değiştirebilir. Kuruşu kuruşuna doğru olması gereken para için tam sayı olarak kuruş saklayın ya da ondalık (decimal) bir tür kullanın. Bu, 0,1 gibi ondalık değerleri tam olarak saklamak için yapılmış bir sayı türüdür.

İkincisi sayıları karşılaştırmak. Aynı sonucu vermesi gereken iki hesaplama çok az farklı çıkabilir. Bu yüzden tam olarak eşit olup olmadıklarını kontrol etmek yerine yeterince yakın olup olmadıklarını kontrol edin. Bu küçük izin verilen farka tolerans denir.

Üçüncüsü döngüler. Toplam tam 1,0'a eşit olana kadar tekrar tekrar 0,1 eklediğinizi düşünün. Minik hatalar yüzünden toplam hiçbir zaman tam 1,0 olmayabilir. Döngü de sonsuza kadar dönebilir. Daha güvenli yol, 1'den 10'a saymak ve ondalık değeri sayaçtan hesaplamaktır.

Ana fikri hatırlayın. İkili sistem bazı kesirleri tam olarak saklayabilir ama pek çok onluk kesir sonsuz ikili rakam ister. Float'lar her zaman tam değeri değil, yakın bir tahmini saklar. Ve bir hesaplamanın tam olması gerektiğinde o iş için yapılmış bir sayı türü seçin.

## Bellek ve disk: unutan ve hatırlayan

Programınızın kullandığı her değer, program çalışırken bir yerde durmak zorundadır. Bir bilgisayarın onu koyabileceği birden fazla yeri vardır. Basit tutmak için iki büyüğüyle başlayalım: unutan bellek ve tutan bellek.

Bir beyaz tahta ve bir defter düşünün. Beyaz tahtaya hızlı yazılır ve hızlı okunur ama gün sonunda silinir. Deftere yazmak daha yavaştır ama oraya yazdığınız yarın da oradadır.

Bilgisayarın beyaz tahtası RAM'dir. Rastgele erişimli bellek (random access memory) demek. Çalışan bir program verisini orada tutar. Hızlıdır ve içindeki her byte'a adresiyle doğrudan ulaşılabilir. Ama RAM'in bitlerini tutabilmesi için elektriğe ihtiyacı vardır. Makineyi kapatın, RAM'deki her şey gider. Programınız çöker ya da yeniden başlarsa RAM'de tuttuğu her şey de gider. Bu yüzden RAM'e uçucu (volatile) bellek denir. Uçucu, kalıcı olmayan demek.

Bilgisayarın defteri disktir. Bugün genellikle bir katı hal sürücüsü, SSD'dir. Eski makineler dönen bir sabit disk kullanır. Her iki durumda da disk elektrik kapalıyken verisini korur. Bu yüzden ona uçucu olmayan (nonvolatile) ya da kalıcı depolama denir. Dosyalarınızın yaşadığı, bir veritabanının tablolarını tuttuğu yer orasıdır.

O zaman neden her şeyi diskte tutmuyoruz? Çünkü disk çok daha yavaş. Hızlı modern bir sürücü bile RAM'den yüzlerce ila binlerce kat yavaş olabilir. Peki neden her şeyi RAM'de tutmuyoruz? Çünkü RAM byte başına daha pahalı ve unutuyor. Bu yüzden bilgisayarlar ikisini de kullanır. RAM şu anda olan iş için. Disk, hayatta kalması gereken her şey için.

Bunun çalışan bir program için anlamı şu. Bir dosya açtığınızda program onu diskten RAM'e kopyalar. Bütün düzenleme hızlı olduğu RAM'de olur. Kaydet'e bastığınızda program byte'ları diske geri yazar. Kaydetmeden önce elektrik giderse değişiklikleriniz gider. Onlar hep yalnız beyaz tahtada yaşadı.

Aynı kural bir internet mağazasında da çıkar. Mağaza her alışveriş sepetini yalnız sunucusunun belleğinde tutsun. Sonra sunucu yeniden başlıyor ve bütün sepetler birden boşalıyor. Ama siparişler diskteki bir veritabanına yazılmıştı, onlar hayatta kalıyor.

Yani bir şeyi saklamadan önce basit bir soru sorun: Bunun bir yeniden başlatmadan sağ çıkması gerekiyor mu? Gerekiyorsa kalıcı depolamaya ulaşmak zorunda.

Peki işlemci veriyi RAM'den yeterince hızlı nasıl alıyor? Cevap, RAM'in işlemciye en yakın bellek olmaması. İşlemci (CPU), üzerinde çalıştığı değerleri kayıtçı (register) denen bir avuç minik yuvada tutar. Bir kayıtçıyı okumak olabilecek en hızlı şeydir. İşlemcinin hemen yanında önbellek (cache) denen küçük, çok hızlı bir bellek vardır. İşlemcinin yakında yine ihtiyaç duyacağı RAM verilerinin kopyalarını tutar.

Bütün bunu katmanlar olarak düşünün. İşlemciye en yakın kayıtçılar, sonra önbellek, sonra RAM. RAM'in altında disk, daha da uzakta ağ üzerinden erişilen her şey. İşlemciden uzaklaştıkça her katman daha büyük ve byte başına daha ucuz olur. Ama her biri daha da yavaşlar. RAM'e bir gidiş, en hızlı önbelleği okumaktan yaklaşık 100 kat uzun sürebilir. Bu sayıları kabaca alın, makineden makineye değişir, ama sıralama neredeyse her yerde geçerlidir.

Yani herhangi bir veri parçası hakkında sorulacak iki soru var. İşlemciye ne kadar yakın? Ve elektrik giderse hâlâ orada olacak mı?

## Önbellek: yakın olan hızlıdır

Çoğu kod belleğe, her okumanın aynı maliyette olduğu tek büyük bir alan gibi davranır. Donanım öyle çalışmaz ve bazen bu ortaya çıkar.

Büyük bir sayı ızgaranız olsun, 1.000 satır ve 1.000 sütunlu bir hesap tablosu gibi. İçindeki her sayıyı toplayan iki döngü yazıyorsunuz. İlk döngü ızgarayı satır satır dolaşıyor. İkincisi sütun sütun. İki döngü de tam olarak aynı miktarda toplama yapıyor. Ama büyük bir ızgarada ikincisi çoğu zaman birkaç kat daha yavaş. Kesin fark dile, donanıma ve ızgaranın boyutuna bağlı.

Açıklamadan önce bir tahminde bulunun. Okuma sırası neden önemli olsun ki?

Cevap, işlemcinin hemen yanında duran küçük ve çok hızlı bir bellek: önbellek.

Nasıl çalıştığını görmek için verinin ne kadar uzakta olduğuyla başlayın. İşlemci, toplama ve karşılaştırma gibi asıl işi yapar. Programınızın verisi ana bellek olan RAM'de yaşar. RAM günlük ölçülere göre hızlıdır ama işlemcinin yanında uzun bir bekleyiştir. En hızlı önbellek, RAM'in harcadığı zamanın küçük bir kısmında cevap verebilir. İşlemci her bir sayı için RAM'e gitmek zorunda kalsa zamanının çoğunu beklemekle geçirirdi.

Bu yüzden önbellek, işlemcinin yakında ihtiyaç duyması muhtemel verilerin kopyalarını tutar. Genellikle birkaç seviyesi vardır. Her seviye bir öncekinden daha büyük ve biraz daha yavaştır. En büyüğü birkaç megabayttan birkaç on megabayta kadar tutar.

İşlemcinin bir değere ihtiyacı olduğunda önce önbelleğe bakar. Değer oradaysa buna önbellek isabeti (cache hit) denir ve çok hızlıdır. Orada değilse önbellek ıskası (cache miss) olur. İşlemci, değer bir alt seviyeden ya da ta RAM'den gelene kadar beklemek zorundadır.

Önbelleği çalıştıran hile şu. İşlemci RAM'den bir değer getirdiğinde yalnız o değeri getirmez. Yanındaki komşu byte'lardan oluşan küçük bir bloğu da beraberinde getirir. Bu bloğa önbellek satırı (cache line) denir ve çoğu zaman 64 byte'tır. Donanım, sıradaki ihtiyacınızın komşular olacağına bahse giriyor.

Önbelleğe ne gireceğini kodunuz seçmez. Onu donanım kendi başına halleder. Ama kodunuz şeyleri hangi sırayla okuyacağını seçer ve donanımın bahsinin tutup tutmayacağına o sıra karar verir.

Şimdi ızgaraya geri dönün. Pek çok dilde bir ızgara bellekte satır satır saklanır. Her satır bir öncekinin hemen arkasından gelir. Satır satır dolaşırsanız her sayı bir öncekinin hemen yanında durur. İlk okuma bütün bir önbellek satırını getirir. Sonraki birkaç okuma zaten önbellektedir. Okuma ardına okuma isabet olur ve döngü uçar.

Şimdi sütun sütun dolaşın. Her adım bir sonraki satıra aşağı atlar. Bu, bellekte bir satır dolusu sayı daha ileri demek. Son okumayla gelen komşular işe yaramaz, çünkü uzun süre onlara ihtiyacınız olmayacak. Neredeyse her okuma ıska olur ve döngü zamanını RAM'i bekleyerek geçirir.

Düz bir dizi öğelerini yan yana tutar, bu yüzden onu dolaşmak önbellek dostudur. Her öğenin bir başka öğeyi gösterdiği bir yapı bellekte oradan oraya atlayabilir ve bu çoğu zaman daha fazla ıska demektir. Basit dizinin kağıt üzerinde daha iyi görünen yapıları bu kadar sık yenmesinin bir nedeni bu.

Neredeyse her yerde işe yarayan bir pratik kural: Yakın hızlıdır, dağınık yavaştır. Veriyi nasıl saklayacağınızı ya da üzerinde nasıl döneceğinizi seçerken belleği yerleştiği sırayla okuyun. Önbellek sizi ödüllendirir.

## Yığıt (stack): fonksiyon çağrılarının yeri

Makinede gigabaytlarca boş bellek varken bir servis nasıl "stack overflow" ile çökebilir?

Çalışan bir program verisini RAM'de tutar ama hepsini tek bir büyük yığına atmaz. İki ana bölge kullanır ve her birinin kendi kuralları vardır. Bunlara yığıt (stack) ve öbek (heap) denir. Stack, fonksiyon çağrılarını takip edendir.

Bir kafeteryadaki tepsi yığını düşünün. Yalnız en üste tepsi koyabilir ve yalnız en üstten tepsi alabilirsiniz. Stack tam böyle çalışır. Programınız bir fonksiyonu her çağırdığında yığının üstüne yeni bir tepsi koyar. Bu tepsiye yığıt çerçevesi (stack frame) denir. O tek fonksiyon çağrısının ihtiyacı olanı tutar. Fonksiyon bittiğinde nereye dönüleceğini kaydeder ve fonksiyonun parametrelerini ve yerel değişkenlerini tutabilir. Fonksiyon bitince çerçevesi üstten kalkar. O alan hemen yeniden kullanılmaya hazırdır.

Adım adım çizelim. Uygulamanız bir müşterinin profilini gösteriyor diyelim. Program `main` adlı bir fonksiyonda başlıyor. Yığında tek çerçeve var: `main`. `main`, müşterinin bilgilerini getirmek için `loadCustomer` adlı bir fonksiyonu çağırıyor. Artık iki çerçeve var, `loadCustomer`, `main`'in üstünde. Sonra `loadCustomer`, e-posta adresini temizlemek için `parseEmail`'i çağırıyor. Artık üç çerçeve var. `main` en altta, `parseEmail` en üstte.

Şimdi geri saralım. `parseEmail` bitince çerçevesi kalkar. Sonra `loadCustomer` biter ve onun çerçevesi de kalkar. Yalnız `main`'e döndünüz. En son giren çağrı her zaman ilk çıkandır.

Stack'i bu kadar hızlı yapan bu katı sıradır. Bir çerçeve eklemek sadece bir işaretçiyi yukarı kaydırmaktır. Kaldırmak, işaretçiyi geri aşağı kaydırmaktır. Boş yer aramak yoktur.

Bu, resmin kendisi tabii. Pratikte derleyici, yani kodunuzu makinenin çalıştırdığı talimatlara çeviren araç, daha iyisini yapabilir. Bazı değerleri kayıtçılarda, işlemcinin kendi minik yuvalarında tutabilir ya da tamamen optimize edip ortadan kaldırabilir.

Katı sıranın iki sonucu daha var. Birincisi, bir fonksiyonun yerel değişkenleri fonksiyon dönünce yok olur, çünkü çerçevesi gitmiştir. İkincisi, stack'in boyutu sınırlıdır. Çoğu zaman yalnız birkaç megabayttır. RAM'in geri kalanının yanında minicik.

Peki çağrılar birikmeye devam ederse ne olur? Bir fonksiyon kendini çağırsın. Buna özyineleme (recursion) denir. Hiç durduğu bir noktaya ulaşmazsa her çağrı üste bir çerçeve daha ekler. Yığın yer kalmayana kadar büyür. Program bir **stack overflow** ile çöker. Gigabaytlarca boş bellekle çökme budur. Yer kalmayan makine değil, stack'ti. Kontrolden çıkan özyinelemenin genellikle sadece takılıp kalmak yerine hızla çökmesinin nedeni de bu.

Bazı diller size bu konuda diğerlerinden daha fazla söz hakkı verir. C ve C++'ta bir değişkeni nasıl tanımladığınız, onun stack'e gidip gitmeyeceğine karar vermeye yardım eder. Java, Python ya da Go'da kodunuzu çalıştıran motor, yani çalışma zamanı (runtime), seçimlerin çoğunu sizin yerinize yapar. Her iki durumda da henüz dönmemiş uzun bir çağrı zinciri yine stack alanı tüketir.

Yani bir çökme "Stack Overflow" diyorsa bellek sızıntısı aramayın. Fazla derine giden çağrılara bakın. Çoğu zaman hiç durmayan bir özyinelemedir.

## Öbek (heap): uzun yaşayan verinin yeri

Çökmeyle ilgisi olmayan bir hata. Bir servis düzgün çalışıyor ama belleği her saat biraz artıyor. Biri yeniden başlatmak zorunda kalana kadar hiçbir şey yanlış görünmüyor.

O hatayı anlamak için programın belleğinin ikinci büyük bölgesine ihtiyacınız var. Adı öbek (heap). Stack fonksiyon çağrılarını halleder. Her çağrı bir çerçeve alır ve fonksiyon döndüğü anda çerçeve kaybolur. Ama pek çok verinin tek bir çağrıdan daha uzun yaşaması gerekir. Müşteri ürün ekledikçe büyüyen bir listeyi düşünün ya da bir fonksiyonun yükleyip sonra onu çağırana geri verdiği bir kaydı. Bu tür veri heap'te yaşar.

Stack düzgün bir tepsi yığınıysa heap daha çok büyük bir depo odası gibidir. Programınız doğru boyutta bir alan ister. O alanın adresini geri alır, bir dolap numarası gibi. Sonra o adresteki veriyle çalışır. Veri, bir şey onu serbest bırakana kadar orada kalır.

Onu kimin serbest bırakacağı dile bağlıdır. Bazı dillerde programın belleği kendisinin serbest bırakması gerekir. Diğerlerinde bunu otomatik bir yardımcı yapar. Bu yardımcıya çöp toplayıcı (garbage collector) denir. Arada bir, programın artık ulaşamadığı veriyi bulur ve o alanı serbest bırakır.

Şimdi stack ve heap'in birlikte çalışmasını görelim. Bir müşteri kaydı yükleyen `loadCustomer` adlı bir fonksiyon alın. Kayıt, müşterinin adı, e-postası ve sipariş listesiyle heap'e gider. Fonksiyonun yerel değişkeni kaydın tamamını tutmaz. Yalnız kaydın heap'teki adresini tutar.

Şimdi `loadCustomer` bitince ne olduğunu izleyin. Önce kaydın adresini onu çağıran fonksiyon olan `main`'e geri verir. Sonra çerçevesi stack'ten kalkar. `main` o adresi kendi değişkeninde tutar. Çerçeve geldi ve gitti ama heap'teki kayda dokunulmadı. `main` hâlâ nerede olduğunu bildiği için hayatta kalıyor.

Dürüst bir uyarı. Bu basitleştirilmiş bir resim. Farklı diller şeylerin nereye gideceğine farklı şekillerde karar verir. Ama fikir geçerli. Kısa ömürlü çağrı verisi stack gibi davranır. Büyüyen ya da fonksiyonundan uzun yaşayan veri heap gibi davranır.

Heap esnektir ama bedelleri vardır. Heap'ten alan istemek, bir stack çerçevesi eklemekten daha fazla iş ister. Programın boş bir alan bulması ve onu takip etmesi gerekir. Heap verisi bellekte dağınık da kalabilir ve dağınık veriyi okumak daha yavaştır, çünkü işlemcinin küçük hızlı belleği hepsini yakında tutamaz.

En büyük bedel sızıntıdır. Programınız artık ihtiyacı olmayan belleği tutmaya devam edince **bellek sızıntısı** (memory leak) olur. Belleği kendiniz serbest bıraktığınız dillerde bunu basitçe unutabilirsiniz. Çöp toplayıcılı dillerde toplayıcı, programınızın artık ulaşamadığını serbest bırakır. Artık neye ihtiyacınız olmadığını bilemez. Bunlar aynı şey değil. Yani bir şey hâlâ eski veriyi gösteriyorsa o veri sonsuza kadar kalır. Çöp toplayıcı olsa bile sızıntı böyle olur.

Yalnız büyüyen ve hiçbir şeyi silmeyen bir önbellek düşünün. Ya da hiç kaldırmadığınız bir olay dinleyicisi (event listener), yani bir tıklama gibi bir şey her olduğunda çalışmasını istediğiniz bir fonksiyon. Her biri eski veriyi ulaşılabilir tutar. Bellek sorun olana kadar azar azar tırmanır. Her saat belleği artan servis bu.

Yani bellek tırmanmaya devam ediyorsa çökme aramayın. Heap'e eklemeye devam edip hiçbir şeyi kaldırmayan bir şey arayın. O eski veriyi hâlâ neyin gösterdiğini ve neden gösterdiğini sorun.

## Değişkenler, nesneler ve referanslar

Bu hatayı neredeyse herkes yazmıştır. Bir fonksiyona bir liste geçiyorsunuz. Fonksiyonun içinde listeye bir öğe ekliyorsunuz. Sonra kendi kodunuza döndüğünüzde sizin listenizde de o fazla öğe var. Sonra tersi oluyor. Bir sayı geçiyorsunuz, fonksiyonun içinde değiştiriyorsunuz ve sizin sayınız aynen kalıyor. Bir liste neden bir şekilde, sayı neden başka şekilde davranıyor?

Görmek için bir sayfaya üç şey çizin. Solda bir isim etiketi çizin. Bu değişken, kodunuzda kullandığınız ad. Sağda içinde veri olan bir kutu çizin, bir liste ya da bir müşteri kaydı gibi. Bu nesne (object), bellekteki asıl veri. Şimdi isim etiketinden kutuya bir ok çizin. O ok referanstır. Bazı diller ona işaretçi (pointer) der.

Bu çizimden iki kural çıkar. Birincisi, iki isim etiketi aynı nesneyi gösterebilir. O nesne değiştirilebiliyorsa birinden yapılan değişiklik diğerinden de görünür. Hâlâ tek bir nesne var.

İkincisi, bir değişkene atama yapmak ya da onu bir fonksiyona geçmek genellikle yeni bir nesne yapmaz. Çoğu zaman yeni ad ya da fonksiyonun parametresi, aynı nesneye giden bir ok daha alır.

Liste hatasını bu açıklıyor. Fonksiyonun parametresi ve sizin değişkeniniz, tek bir listeye ok çizen iki isim etiketi. Fonksiyon bir öğe eklediğinde siz de görüyorsunuz. Bir müşteri kaydı için de aynısı geçerli. Bir fonksiyonun sadece müşteriyi ekranda göstermek için biçimlendirmesi gerekiyor ama sessizce e-posta alanını değiştiriyor. O fonksiyon sizin müşterinizi değiştirmiş oldu.

Burada bir ayrıntı önemli. Fonksiyon sizin değişkeninizin kendisini değil, okun kendi kopyasını alır. Yani parametresini yepyeni bir listeye yönlendirirse sizin değişkeniniz yerinden oynamaz.

Sayı durumu farklı. Bir fonksiyon bir sayıyı değiştirdiğinde sonunda kendi sayısına sahip olur. Bazı dillerde baştan bir kopya almıştır. Python gibi diğerlerinde bir eklemek, yerel adı yepyeni bir sayıya yönlendirir. Her iki durumda da sizin değişkeniniz eskisini korur. Popüler dillerin çoğunda metinler (string) de aynı şekilde çalışır. Bir metni değiştirmek size yenisini verir ve asıl metin olduğu gibi kalır.

Kısa bir test yapalım. İçinde üç müşteri olan `A` adında bir liste yapın. `B`'yi `A`'ya eşitleyin. `B` üzerinden dördüncü bir müşteri ekleyin. Bir saniye düşünün ve `A`'nın ne gösterdiğini tahmin edin.

Dört müşteri gösterir. `B`, aynı listeye ok çizen ikinci bir isim etiketinden ibaret.

Şimdi `B`'ye yepyeni boş bir liste verin ve ona bir müşteri ekleyin. `A` şimdi ne gösterir? Hâlâ dört. `B`'ye atama yapmak eski listeye dokunmadı. Sadece `B`'nin okunu yeni bir listeye çevirdi. `A`'nın oku hiç oynamadı. İlk adım nesneyi değiştirdi. İkinci adım yalnız bir adı taşıdı.

Akılda tutmanın gündelik bir yolu: Bir belge bağlantısı paylaşmak ile bir ek göndermek arasındaki farkı düşünün. Birine bağlantı gönderin, onların düzenlemeleri sizin belgenizde görünür. Ek gönderin, kendi kopyalarını düzenlerler. Referans bağlantıdır. Kopya ektir.

Yani bir nesneyi bir fonksiyona vermeden önce o fonksiyonun onu değiştirmesine izin olup olmadığına karar verin. İzin yoksa ona bir kopya verin ya da değiştiremeyeceği bir şey verin.

## Sığ kopya ve derin kopya

Bir fonksiyonun verinizi değiştirmemesi gerektiğinde olağan çözüm ona bir kopya vermektir. Ama bir kopya da sizi şaşırtabilir.

Şöyle. Bir taslak üzerinde indirim denemek için bir müşteri kaydını kopyalıyorsunuz. Taslakta sipariş listesindeki fiyatları düşürüyorsunuz. Sonra gerçek müşteriye bakıyorsunuz, fiyatlar orada da değişmiş. Kayıt kopyalandı ama içindeki sipariş listesi kopyalanmadı.

Nedenini görmek için çizin. Değişken bir isim etiketi, referans da o addan bir nesneye giden bir ok. Bir değişkeni kopyalamak genellikle nesneyi değil oku kopyalar. Yani iki ad aynı veriyi gösterebilir.

Şimdi müşteri kaydını bir kutu olarak çizin. İçinde bir ad, bir e-posta ve ikinci bir kutuyu, sipariş listesini gösteren bir ok var.

Kaydın **sığ kopyasını** (shallow copy) alın. Kendi adı ve e-postası olan yepyeni bir dış kutu elde edersiniz ama içindeki ok olduğu gibi kopyalanır, yani hâlâ aynı sipariş listesini gösterir. Kopya üzerinden bir fiyatı değiştirin, asıl kayıt da görür. İndirim hatası bu.

Şimdi **derin kopya** (deep copy) alın. Bu sefer her kutu en alta kadar kopyalanır. Yeni kayıt, kendi fiyatlarıyla kendi yeni sipariş listesini alır. Taslağı değiştirin, asıl kayıt yerinden oynamaz.

"Kopyaladım ve yine değişti" gibi duyulan pek çok hata buna dayanır. Biri derin kopya gerekirken sığ kopya almıştır.

Sizin yerinize neyin kopyalandığı da dile bağlıdır. Ama altta yatan kural hep aynı. Ya veri kopyalanır ya da yalnız ok. Java ve Python'da bir nesneyi geçmek yalnız oku kopyalar. Yani bir fonksiyon yine de nesnenizi değiştirebilir.

Bazı dillerde ayrıca C, Swift ve Go'daki struct'lar gibi değer türleri vardır. Struct küçük bir alan demetidir ve birini atamak alanların kendisini kopyalar. Go'da ise bir slice'ı (Go'nun büyüyebilen listesi) ya da bir map'i (arama tablosu) kopyalamak, altta yatan veriyi yine paylaşır. Yani hangi türe sahip olduğunuzdan emin değilseniz bir kopyaya güvenmeden önce dilinizin kurallarına bakın.

Python yazıyorsanız bir tuzağa daha dikkat edin. Bir fonksiyonun varsayılan değeri boş bir liste olan bir parametresi olsun. Python bu listeyi fonksiyon tanımlandığında yalnız bir kez kurar. Kendi listesini geçmeyen her çağrı o aynı listeyi paylaşır. Böylece bir çağrıda eklenen öğeler bir sonrakinde görünür.

Yani bir kopya ancak sizin yaptığınız kadar derindir. Bir fonksiyonun değiştirebileceği kendi kopyasına ihtiyacı varsa o kopyayı kendiniz alın ve içindeki şeylerin de kopyalanması gerekip gerekmediğini kontrol edin.

## Modül özeti

Kodunuzun makine tarafı bu kadar. Her şey bit olarak başlar. Her biri bir öncekinin iki katı değerinde olan bir anahtar dizisi, bir tavana kadar her tam sayıyı tutabilir. Soldaki anahtarı negatif saydırın, aynı dizi negatif sayıları da tutar. Bir sayıyı o tavanın ötesine itin, başa sarar ya da bir hatayla durur. Aynı byte bir sayı, bir harf ya da bir renk olabilir ve onu nasıl okuyacağına programınız karar verir. Metin ancak iki taraf kodlamada anlaşırsa çalışır. Ve çoğu onluk kesir tam saklanamaz. Bu yüzden kuruşu kuruşuna doğru olması gereken para, tam sayı kuruş ya da ondalık türde tutulur.

O bitlerin nerede durduğu da aynı derecede önemli. RAM hızlıdır ama elektrik gidince her şeyi unutur. Hayatta kalması gereken her şey diske gider. Birbirine yakın duran veri, önbellek sayesinde hızlıdır. Fonksiyon çağrıları stack'i kullanır. Uzun yaşaması gereken veri genellikle heap'te durur. Ve bir değişken çoğu zaman bir referans tutar, yani iki ad tek bir nesneyi paylaşabilir.

Yani bir program sizi şaşırttığında iki şey sorun: Bu değer gerçekte neyi temsil ediyor? Ve nerede duruyor?

Sırada diğer soru var: Nasıl büyür?
