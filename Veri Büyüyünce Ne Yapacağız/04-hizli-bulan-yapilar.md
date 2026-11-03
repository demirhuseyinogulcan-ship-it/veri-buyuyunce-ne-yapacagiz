# Modül 4: Şeyleri hızlı bulan yapılar

## Modüle giriş

Son modül bir boşlukla bitti. O yapıların hiçbiri bir öğeyi değerine göre hızlı bulamıyor. Bu boşluk önemli, çünkü bazı sorular neredeyse her uygulamada çıkar. Şu an en acil destek talebi hangisi? Şehrin öbür ucuna en ucuz yol hangisi? 10 milyon satırdan hangisi bu e-postayla eşleşiyor?

İlerideki yapılar böyle soruları hızlı cevaplar. Hash tablolarıyla, ağaçlarla, öncelik yığınlarıyla (heap) ve çizgelerle tanışacak ve bir çizgenin nasıl dolaşılacağını göreceksiniz. Ondan sonra gerçek sistemlerin onları bir önbellekte ve bir veritabanı indeksinde nasıl birleştirdiği geliyor. Ve en sonda hepsinin tek bir gerçek uygulamada nerede ortaya çıktığını göreceksiniz. Kursun iki sorusunun en çok önem taşıdığı yer burası: Nasıl büyür? Nerede durur?

En yaygın soruyla başlayalım.

## Hash tablosu (hashmap)

Koddaki en yaygın soru basitçe şu: Bunu daha önce gördüm mü?

Müşteri içe aktarmalarında sürekli çıkan bir hata. Her müşteriyi kaydetmeden önce kod, o e-postanın şimdiye kadar görülen bütün e-postaların listesinde olup olmadığını kontrol ediyor. Birkaç yüz müşteriyle anında çalışıyor. 200.000 ile içe aktarma donuyor. Kodda hiçbir şey yavaş görünmüyor. Biri sadece küme olması gereken yerde liste kullanmış.

Bir listenin burada neden zarar verdiğini zaten gördünüz. Her kontrol bütün listeyi dolaşıyor. Yani içe aktarma iç içe bir döngü. Son modüldeki her yapının aynı boşluğu var. Bir öğeyi değerine göre bulmak için öğe ardına öğe kontrol ediyor.

Çözüm bir **hash tablosu** (hashmap). Fikri görmek için alfabenin her harfi için bir tane olmak üzere 26 çekmeceli bir dosya dolabı düşünün. Bir müşterinin kartını dosyalamak için adının ilk harfine bakarsınız. Sarah'nın kartı S çekmecesine gider. Sonra Sarah'yı bulmak için ilk harfe tekrar bakar ve doğrudan S çekmecesine gidersiniz. Diğer 25'ini hiç aramazsınız.

Bir hash tablosu, ilk harften daha akıllı bir kuralla aynı şekilde çalışır. Diyelim bir müşterinin e-postasını saklamak istiyorsunuz. Önce bir **hash fonksiyonu** e-postayı bir sayıya çevirir. Hash fonksiyonunun tek yaptığı bu: herhangi bir girdiyi bir sayıya çevirir. Ve aynı girdi her zaman aynı sayıyı verir. Sonra tablo o sayıyı yuva sayısına böler ve kalanı tutar. Kalan, yuvadır, çekmece gibi.

Deneyin. Bir e-posta 27'ye hash'leniyor ve tablonun sekiz yuvası var. Hangi yuvaya gider? Durun ve hesaplayın. 27 / 8, 3 eder ve 3 kalır. Yani 3. yuvaya gider. Sonra o e-postayı kontrol etmek için onu tekrar hash'lersiniz. Yine 27, yine 3. yuva. Tarama olmadan doğrudan oraya gidersiniz.

Bir hash tablosunda eklemenin, bulmanın ve çıkarmanın ortalamada yaklaşık bir adım sürmesinin nedeni bu. Hash tablolarının her yerde olmasının nedeni de bu. Dilinizdeki sözlükler ve arama tabloları genellikle altta birer hash tablosudur.

Donan içe aktarmaya dönelim. Bir listeyi kontrol etmek iç içe bir döngüydü, yani içe aktarma ikinci derecedendi. Bir hash tablosuyla her kontrol yaklaşık bir adım sürer, yani bütün içe aktarma doğrusaldır. 200.000 müşteri 20 milyar değil, yaklaşık 200.000 adım tutar.

Ortağıyla zaten tanıştınız: **hash kümesi** (hash set). Aynı yapı ama değer bağlanmadan yalnız anahtarları saklar. "Bunu daha önce gördüm mü?" sorusunu cevaplamanın en hızlı yolu ve içe aktarmanın sorduğu da tam olarak buydu.

Hash tablosunun size vermediği bir şey var: sıra. Yuvalar kalanlardan gelir, bu yüzden öğeler dağınık kalır. İçe aktarma için bu önemli değildi. Yalnız neyi zaten gördüğünü bilmesi gerekiyordu.

Akılda kalsın: Kod "bunu gördüm mü?" diye sorup duruyorsa liste değil, hash kümesi ya da hash tablosu kullanın.

## Hash tablosunun içi: çakışmalar

Bir hash tablosu doğrudan bir yuvaya atlar, yani bir anahtarı bulmak ortalamada yaklaşık bir adım sürer. Ama o ortalama birkaç şeyin yolunda gitmesine bağlı. İçeride ne olduğuna bakalım.

Bir problemle başlayın. İki farklı anahtar aynı yuvaya düşebilir. Buna **çakışma** (collision) denir ve önlenemez. Olası anahtarlar yuvalardan çok daha fazla, yani bazılarının paylaşması gerekir. Dosya dolabını düşünün. Pek çok isim S ile başlar, yani S çekmecesi pek çok kart tutar. Hash tablosunun da aynı sorunu var, sadece daha seyrek.

İki yaygın çözüm var. Biri **zincirleme** (chaining). Her yuva kısa bir liste tutar ve çakışan anahtarlar sadece o listeye katılır. Diğerine **yoklama** (probing) denir. Yuva doluysa bir sonrakini deneyin ve boş bir tane bulana kadar devam edin. Her iki durumda da çakışmalar seyrek kaldıkça aramalar bir adıma yakın kalır.

Sekiz yuvalı minik bir tablo üzerinde izleyelim. Anna'nın e-postası 3. yuvaya düşer. Ben'inki 6. yuvaya. Cara'nınki de 3. yuvaya düşer, yani Anna'nınkiyle çakışır. Zincirlemeyle Cara'nınki 3. yuvadaki kısa bir listeye Anna'nınkinin arkasına katılır. Dev'inki 1. yuvaya düşer.

Tablo fazla dolunca çakışmalar sıklaşır. Bu yüzden tablo ne kadar dolu olduğunu takip eder. Sekiz yuvada dört öğe, yarı dolu demek. Diyelim bu tablo yarıyı geçince büyüyor. Sonra beşinci bir e-posta, Eve'inki geliyor ve onu yarının ötesine taşıyor.

Devam etmeden tahmin edin, zaten saklanmış e-postalara ne olması gerekir? Herkesin taşınması gerekir. Tablo 16 yuva kurar ve her yuvayı yeniden hesaplar, artık 16'ya bölerek. Hash sayıları değişmez ama kalanlar değişir. Anna'nınki 11. yuvaya taşınabilir, Cara'nınki 3'te kalır. Artık paylaşmıyorlar.

Her şeyi taşımak O(N)'e mal olur ama seyrek olur. Tıpkı büyüyebilen bir liste gibi o bedel yayılır. Yani ekleme yine ortalamada yaklaşık bir adım sürer. Bu taşıma öğelerin sırasını da karıştırır. Yani diliniz bir sıra vaat etmiyorsa hash tablosunun size geri verdiği sıraya asla güvenmeyin.

Bir hash tablosunu sağlıklı tutan iki kural daha var. Birincisi, bir anahtar saklıyken asla değişmemeli. Değişirse hash'i değişir, tablo yanlış yuvaya bakar ve öğe kaybolur. Pek çok dilin anahtar olarak yalnız metinler ve sayılar gibi değişemeyen şeyleri kullanmanıza izin vermesinin nedeni bu.

İkincisi, hash fonksiyonu anahtarları eşit şekilde yaymalı. Pek çok anahtar birkaç yuvaya yığılırsa aramalar O(N)'e gittikçe yaklaşır. Saldırganlar çakışmak üzere tasarlanmış anahtarlar bile gönderebilir. Dilinizdeki yerleşik hash fonksiyonları bunu göz önünde tutarak tasarlanmıştır. Yani kendinizinkini yazmayın.

Yani bir hash tablosu yavaş hissettiriyorsa temelleri kontrol edin. Anahtarlar değişiyor mu? Hash fonksiyonu iyi mi? Ve bir şey çakışmaları zorluyor mu?

## İkili arama ağacı (binary search tree)

Hızlı arama her zaman yetmez. Bazı özelliklerin şeyleri her zaman sıralı tutması da gerekir.

Diyelim bir oyun lider tablosu yapıyorsunuz. Herhangi bir skoru hızlı aramanız ve skorları sıralı göstermeniz gerekiyor. Hash tablosu aramayı halleder. Ama hash tablosu sıra hakkında hiçbir şey bilmez. Ondan en küçük skoru isteyin, her girişi kontrol etmek zorunda kalır. O zaman hızlı aramayı ve sıralı düzeni birlikte ne verir? **İkili arama ağacı** (binary search tree) bunun için.

Bağlı listelerdeki her biri sonrakini gösteren düğümleri hatırlıyor musunuz? Bir düğümün altındaki birden fazla düğümü göstermesine izin verin, bir **ağaç** (tree) elde edersiniz. Bir ağaçta döngü yoktur. Bir düğüm köktür (root). Her düğümün çocukları vardır ve çocuğu olmayan düğüme yaprak (leaf) denir.

Seviyeleri olan her şey bir ağaçtır, klasörlerin içindeki klasörler ya da bir şirkette kimin kime bağlı olduğunu gösteren şema gibi. Altındaki her şeyle birlikte herhangi bir düğüme alt ağaç (subtree) denir. Bir ağacı dolaşmanın doğal yolu özyinelemedir, çünkü her alt ağaç kendi başına bir ağaçtır.

İkili arama ağacında ikili, her düğümün en çok iki çocuğu olması demek, bir sol ve bir sağ. Arama ağacı ise bir sıra kuralı olması demek. Bir düğümün sol alt ağacındaki her şey daha küçük. Sağ alt ağacındaki her şey daha büyük.

Bu kural ikili aramayı, yarıya bölme hilesini, doğrudan yapının içine kurar. Bir değeri bulmak için kökten başlayın. Daha küçükse sola gidin. Daha büyükse sağa. "Daha yüksek" ya da "daha alçak" her cevabın bir tarafı tamamen elediği bir sayı tahmin oyunu gibidir. Her adım bütün bir alt ağacı atar.

Ağaç dengeliyse yüksekliği yalnız log N ile büyür. Her karşılaştırma sizi bir seviye aşağı götürür. Bu da bulmak, eklemek ya da silmek için O(log N) verir. "Dengeli" kelimesini aklınızda tutun. Düz bir ikili arama ağacı bunu vaat etmez.

Tek bir ağacı izleyelim. 50, 30, 70, 20, 40, 60 ve 80'i bu sırayla ekleyin. 50 kök olur. 30 daha küçük, sola gider. 70 daha büyük, sağa gider. 20, 50'den ve 30'dan küçük, yani 30'un soluna oturur. 40, 50'den küçük ama 30'dan büyük, yani 30'un sağına oturur. Aynı şekilde 60, 70'in soluna, 80, 70'in sağına düşer. Bu, üç seviyede yedi değer, kusursuz dengeli.

Şimdi 60'ı arayın. 50'den başlayın. 60 daha büyük, sağa 70'e gidin. Bütün sol taraf gitti. 60, 70'ten küçük, sola gidin. İşte 60. Yedi değerden üç kontrol.

Peki sıralı liste nereden geliyor? Dolaşma (traversal) denen bir yürüyüşten. Bir dolaşma her düğümü bir kez ziyaret eder. **Sıralı dolaşma** (in-order traversal) sol alt ağacı, sonra düğümü, sonra sağ alt ağacı ziyaret eder. İzlemeden önce sıra için tahmininiz ne? Durun ve düşünün. 20, 30, 40, 50, 60, 70, 80'i ziyaret eder. Her seferinde sıralı, çünkü daha küçük olan her şey solda durur.

Ve ağaç her şeyi sıralı tuttuğu için aramalardan fazlasını cevaplayabilir. En küçük değeri, bundan sonraki değeri ya da bir aralıktaki her şeyi bulabilir. Bir arama ağacının gerekçesi bu. Size sırayı ve hızlı aramayı birlikte verir.

## Kendini dengeleyen ağaçlar

Bir ikili arama ağacı dengeliyken hızlıdır. Aşağıya her adım bütün bir tarafı atar. Yani bir değeri bulmak yaklaşık log n adım sürer. Ama ağacın kuralında onu dengeli tutan hiçbir şey yok. Bilerek bozalım.

Boş bir ağaca 10, 20, 30 ve 40'ı bu sırayla ekleyin. 10 kök olur. 20 daha büyük, sağa gider. 30 ikisinden de büyük, 20'nin sağına gider. 40 da 30'un sağına gider. Durun ve canlandırın. Bu ağaç ne şekilde?

Sağa yaslanmış düz bir çizgi. Dört değer dört seviye tutuyor ve 40'ı bulmak her birini kontrol etmek demek. Aslında sadece bir bağlı liste. Yani aramalar O(N). Düz bir ağaca tarihler ya da yalnız artan numaralar gibi sıralı veri verin, elde edeceğiniz bu.

**Kendini dengeleyen ağaçlar** (self-balancing trees) bunu düzeltir. Her ekleme ya da silmede bir tarafın fazla derinleşip derinleşmediğini kontrol ederler. Derinleştiyse döndürme (rotation) denen küçük bir yeniden düzenleme yaparlar.

Aynı örnekte bir döndürme. 30 geldiğinde sağ taraf soldan iki seviye daha derin. Ağaç döner. 20 köke çıkar, 10 soluna, 30 sağına geçer. 40 gelince 30'un sağına gider. Artık dört değer dört yerine üç seviye tutuyor. Sıralı değer eklemeye devam edin, döndürmeler de devam eder. Bir milyon sıralı eklemeden sonra ağaç bir milyon değil, 20 ile 40 seviye arası derindir.

Düz bir ikili arama ağacı dengeyi umar. Kendini dengeleyen bir ağaç onu garanti eder. Döndürmeleri kendiniz yazmazsınız. Java'nın TreeMap'i ve C#'ın SortedDictionary'si kendini dengeleyen ağaçlardır, siz sadece kullanırsınız. Python ve JavaScript'te yerleşik bir tane yok, bir kütüphane kullanırsınız.

Kırmızı-siyah ağaç (red-black tree) ve AVL ağacı adlarını duyabilirsiniz. Bunlar dengelemeyi yapmanın iki popüler yolu. Veritabanları **B ağacı** (B-tree) denen ilişkili bir ağaç kullanır. Her düğüm pek çok değer tutar ve pek çok çocuğu vardır, bu yüzden ağaç çok kısa kalır. Aşağıya her adım diskten bir okuma demek olduğunda bu önemli.

Peki ne zaman hash tablosu yerine ağaç istersiniz? Oyun lider tablosuna dönelim. Bir hash tablosu bir oyuncunun skorunu yaklaşık bir adımda bulur ama sıradan haberi yoktur. Dengeli bir ağaç yaklaşık log n adım sürer ama her şeyi sıralı tutar. Pratikte bir hash tablosu araması genellikle tek bir yuvaya düşer. Bir ağaç araması dağınık bellekte 20 ila 40 bağlantı takip eder. Yani yalnız aramaya ihtiyacınız varsa genellikle hash tablosu kazanır. Sıraya da ihtiyacınız varsa ağaç buna değer.

Yani bir dahaki sefere bir soru sorun: Sıraya mı ihtiyacım var, yoksa sadece aramaya mı?

## Öncelik yığını (heap)

Bazen her şeyin sıralı olmasına ihtiyacınız yoktur. Sadece şu an en acil öğeye ihtiyacınız vardır.

Bir hastane acil servisini düşünün. Hastalar geliş sırasına göre bakılmaz. Ayak bileği burkulmuş biri beklerken kalp krizi geçiren biri doğrudan içeri girer. En acil olan sıradaki olur.

Yazılımın da aynı şeye ihtiyacı var. Pazartesi sabahı bir destek sistemi hafta sonu taleplerle doluyor, çoğu sıradan şifre sıfırlama. Sonra bütün sitenin çöktüğünü söyleyen bir talep geliyor. Sıradaki boştaki temsilci önce onu almalı. Her talebin bir öncelik numarası var ve 1 en acil.

Düz bir kuyruk işe yaramaz, çünkü en uzun bekleyene hizmet eder. Her yeni talepten sonra bütün birikmiş işi sıralamak işe yarar ama birikmiş iş büyüdükçe yavaşlar. İhtiyacınız olan bir **öncelikli kuyruk** (priority queue). O adı kuyruklar dersinde duydunuz. Şeyler hangi sırayla gelirse gelsin her zaman en önemli öğeye hizmet eder.

Onu hızlı yapan yapıya **öncelik yığını** (heap) denir. Heap, tek bir basit kuralı olan bir ağaçtır: Hiçbir ebeveyn çocuklarından büyük değildir. Bir arama ağacının yaptığı gibi her şeyi sıralı tutmaz, ama o tek kural yüzünden en küçük öğe her zaman en tepede durur. Yani en acil olanı bulmak tek adım. Sadece tepeye bakarsınız.

Bir öğe eklemek yeni bir hastanın gelmesi gibidir. Arkadan başlarlar, sonra kendilerinden daha az acil olan herkesin önüne geçerler. Heap'te yeni öğe en alttaki bir sonraki boş yere gider. Sonra ebeveyni daha büyük olduğu sürece ebeveyniyle tekrar tekrar yer değiştirir.

Deneyelim. Heap'in tepesi 2. Altında 5 ve 3 var. 5'in altında 9 ve 7. Şimdi öncelik numarası 1 olan yeni bir talep geliyor. 3'ün altındaki bir sonraki boş yere gider. 1, 3'ten küçük, yer değiştirirler. Şimdi 1, 2'nin altında. 1 nereye varır? Bir saniye düşünün. 1, 2'den küçük, yine yer değiştirirler. En acil öğenin ait olduğu tepeye varır.

Tepedeki öğeyi çıkarmak tam tersi çalışır. Tepeyi çıkarın. Son öğeyi boş yere taşıyın ve batmasına izin verin. Kural yeniden sağlanana kadar küçük çocuğuyla yer değiştirir. Her yer değiştirme bir seviye hareket eder.

Bir heap seviye seviye, boşluk bırakmadan doldurulur. Yani her zaman yaklaşık log n seviye derindir. Bu, ekleme ve çıkarmanın ikisinin de yaklaşık log n adım sürmesi demek. Ve boşluk olmadığı için bir heap hiç işaretçi olmadan düz bir diziye sığar. Bu onu pratikte de hızlı yapar.

Kuralı tersine çevirin, yani hiçbir ebeveyn çocuklarından küçük olmasın ve en büyük öğe tepede dursun. Buna en büyük yığını (max heap) denir. Hangisini kullanacağınız, küçük mü büyük mü sayıların en önemli anlamına geldiğine bağlı.

Heap'ler sıradaki en önemli şeye ihtiyaç duyduğunuz her yerde çıkar. İşleri son tarihe göre planlarlar. Bir milyon öğenin hepsini sıralamadan ilk 10'unu bulurlar. Ve rota bulmada sıradaki en yakın keşfedilmemiş noktayı seçerler.

Bilinmesi gereken bir şey daha. Bir heap eşit öncelikler arasındaki geliş sırasını hatırlamaz. Yani öncelik numarası 2 olan iki talep herhangi bir sırayla çıkabilir. Aynı öncelik içinde ilk gelen ilk hizmet önemliyse geliş zamanını eşitlik bozucu olarak ekleyin.

Yani bir hash tablosu anahtara göre bulur ve bir arama ağacı anahtara göre sıralı bulur. Bir heap en uçtaki öğeyi bulur.

## Çizgeler (graph)

Listeler ve ağaçlar çok şeyi halleder ama arkadaşlıkları modelleyemez. Bir liste insanları, bir ağaç bir hiyerarşiyi tutabilir. Ama arkadaşlıkların tepesi ve tek bir ebeveyni yoktur ve geri döngü yaparlar. Arkadaşınızın arkadaşı zaten arkadaşınız olabilir.

Bir sosyal uygulamadaki "Tanıyor olabileceğin kişiler" özelliğini alın. Arkadaşlarınıza ve onların arkadaşlarına ihtiyaç duyar. Bu, her şeyin her şeye bağlanabildiği bir yapı ister.

Bir ağaçla başlayın ve kurallarını kaldırın. Bir düğümün istediği sayıda başka düğümü göstermesine izin verin, döngülere izin verin ve kökü bırakın. Bu bir **çizge** (graph). Var olan en genel yapılardan biri, çünkü gerçek dünyanın çok büyük kısmı birbirine bağlı şeylerden oluşur.

Kelime dağarcığı küçük. Şeylere köşe (vertex) ya da düğüm denir. Bağlantılara kenar (edge) denir. "Bu, şuna bağlı" diyebildiğiniz her yerde bir çizgeniz var. Yol haritaları, sosyal ağlar ve internet hep böyle çalışır.

Kenarların iki özelliği var. Yönlü ya da yönsüz olabilirler. Arkadaşlık iki yönlüdür. Bir takip ilişkisi ya da tek yönlü bir sokak tek yönlüdür. Ve kenarlar ağırlıklı ya da ağırlıksız olabilir. Bir yolun uzunluğu, bir uçuşun fiyatı vardır. Bir arkadaşlığın genellikle bir sayısı yoktur. Çizgelerin rota problemlerini modellemesini sağlayan, yönlü ve ağırlıklı kenarlardır.

Bir çizgeyi saklamanın iki yaygın yolu var. **Komşuluk listesi** (adjacency list) her köşeye bağlandığı köşelerin bir listesini verir. Köşelerin çoğu yalnız birkaç başkasına bağlandığında kompakttır. Gerçek çizgelerin çoğu böyledir. Ve bir köşenin komşularını dolaşmak kısa bir taramadır.

**Komşuluk matrisi** (adjacency matrix) her köşe için bir satırı ve bir sütunu olan bir ızgaradır. Bir kenarın olduğu her yerde bir işaret vardır. İki köşe arasında kenar olup olmadığını kontrol etmek O(1)'dir. Ama ızgaranın n² hücresi vardır. Bir milyon köşe için bu bir trilyon hücre. Yani yalnız küçük çizgelere ya da noktaların çoğunun diğerlerinin çoğuna bağlı olduğu çizgelere uygundur.

Sonraki birkaç derste kullanacağınız küçük bir çizge. A'dan F'ye altı kavşak, yedi iki yönlü yolla birbirine bağlı düşünün. A, B ve C'ye bağlı. B, D'ye, C de E'ye bağlı. D ve E birbirine bağlı ve ikisi de F'ye bağlı.

Komşuluk listesi olarak A'nın girişi "B C" okunur. D'nin girişi "B E F". F'nin girişi "D E". Her yol iki kez görünür, her ucundaki listede bir kez.

Şimdi kendiniz bir tane deneyin. Bu harita için bir matris her kavşak için bir satır ve bir sütun ister. Bu kaç hücre? Ve kaçı bir işaret tutar? Durun ve çözün. 6 × 6, yani 36 hücre. Yedi yolun her biri iki hücreyi işaretler, her yön için bir tane. Yani yalnız 14'ü işaretli ve ızgaranın çoğu boş alan. Komşuluk listelerinin genellikle kazanmasının nedeni bu.

Çizge soruları pek çok biçimde gelir. Buradan oraya bir yol var mı? En kısası hangisi? Bu noktadan neye ulaşılabilir? Döngüler var mı? Sizden iki bağlantı uzaktaki kimler? Bu bizim "tanıyor olabileceğin kişiler".

Bunların her biri çizge üzerinde bir yürüyüştür. Yürüyüş köşeleri ziyaret eder ve kenarları takip eder. Nerede bulunduğunu hatırlaması gerekir. Bir ağaçtan farklı olarak bir çizgede döngüler vardır ve sonsuza kadar dönüp durabilirsiniz. Çizge dolaşması, o hafıza ve ziyaretlerin sırası ile ilgilidir. Yığıtın ve kuyruğun geri döndüğü yer burası.

Bir liste sırada ne olduğunu bilir. Bir ağaç üstte ne olduğunu bilir. Bir çizge neyin bağlı olduğunu bilir.

## Genişlik öncelikli arama (BFS)

O çizge sorularından herhangi birini cevaplamak için çizgeyi dolaşmanız gerekir. Bunu yapmanın iki temel yolu var ve tek bir veri yapısıyla ayrılıyorlar. Bu, son modüldeki ilk giren ilk çıkar sırası olan kuyruğu kullanıyor.

Önceki altı kavşağı alın. Hatırlatma olarak, A, B ve C'ye bağlı. B, D'ye, C de E'ye bağlı. D ve E birbirine bağlı ve ikisi de F'ye bağlı. Bir noktanın birkaç komşusu olduğunda onları alfabetik sırayla alın. Soru şu: En az yolla A'dan F'ye nasıl gidersiniz?

Bunun gibi her yürüyüş aynı adımları izler. Sırada ziyaret edilecek köşelerden oluşan bir koleksiyon tutarlar. Ayrıca zaten görülmüş köşelerden oluşan bir hash kümesi tutarlar. Dönüp durmayı durduran o görülenler kümesi. Hiçbir köşeyi iki kez işlemezsiniz ve hiçbir zaman sonsuza kadar dönmezsiniz.

Sonra koleksiyondan bir köşe alıp komşularına bakın. Görülmemiş olanları görüldü olarak işaretleyip koleksiyona ekleyin. Koleksiyon boşalana kadar tekrarlayın.

Ulaşılabilen her köşe bir kez işlenir. Her iki yönlü yol iki kavşağının listesinde de görünür, yani en çok iki kez bakılır. Bu yine O(köşeler + kenarlar). İş, çizgenin boyutuyla büyür.

Bunu bir kuyruk yapın, **genişlik öncelikli arama** (breadth-first search) ya da BFS elde edersiniz. Kuyruk ilk giren ilk çıkar mantığıyla çalışır. Başlangıcı ziyaret edersiniz, sonra onun bütün komşularını, sonra onların bütün komşularını. Yürüyüş halkalar halinde dışarı doğru genişler.

BFS'i A'dan başlatın. Onu görüldü işaretleyin, kuyruk yalnız A'yı tutar. A'yı çıkarın. Komşuları B ve C yeni, yani kuyruk "B C" olur. B'yi alın. Kuyruğa şimdi ne girer? Bir saniye düşünün. B'nin tek yeni komşusu D. Yani kuyruk "C D". C'yi alın. Bu E'yi ekler, yani kuyruk "D E". D'yi alın. Komşusu E zaten görüldü ama F yeni. Artık kuyruk "E F". E'yi alın. Çevresindeki her şey zaten görüldü, yani kuyruk sadece F. F'yi alın, kuyruk boşalır. Ziyaret sırası A B C D E F.

Şimdi halkalara bakın. A sıfır uzaklıkta. B ve C 1 uzaklıkta. D ve E 2 uzaklıkta ve F 3 uzaklıkta. F, D'den keşfedildi ve D, B'den keşfedildi. Yani en az adımlı yol A B D F. Bu üç yol.

BFS her köşeye mümkün olan en az kenarla ulaşır. Yani her kenar aynı sayıldığında en kısa yolları bulur. Bu da BFS'i şu tür sorular için doğru yürüyüş yapar: Bu iki kişiyi kaç tanışma ayırıyor? Bu bulmacayı çözmek için en az hamle kaç? Bu düğümün iki adım içinde neler var?

Ama gerçek yollar eşit değil. Bazıları daha uzun, bazıları daha yavaş, bazılarını kullanmak para tutar. Böyle bir haritada en az adım her zaman en ucuz rota değildir.

## Derinlik öncelikli arama (DFS)

Genişlik öncelikli arama bir kuyruk kullandı, bu yüzden yürüyüş halkalar halinde yayıldı. Şimdi tek bir değişiklik yapın. Kuyruğu bir yığıtla değiştirin, **derinlik öncelikli arama** (depth-first search) ya da DFS elde edersiniz.

Aynı altı kavşağı ve aynı alfabetik kuralı kullanın. Başlangıcı ziyaret edersiniz, sonra bir komşuyu, sonra o komşunun bir komşusunu. Olabildiğince derine gidersiniz, sonra başka bir dalı denemek için geri dönersiniz.

DFS'i özyinelemeli yazarsanız çağrı yığıtı sizin yığıtınızdır. DFS'in çoğu zaman sadece özyinelemeli bir fonksiyon olmasının nedeni bu. Birazdan izleyeceğim yürüyüş özyinelemeli sürümü takip ediyor. Elle yazılmış bir yığıt da aynı şekilde derine gider ama dalları farklı bir sırayla seçebilir.

DFS'i bu altı kavşakta A'dan başlatın. F'ye en kısa yoldan ulaşacak mı? İzlemeden önce bir tahminde bulunun.

A'dan B'ye gidin. B'nin ilk görülmemiş komşusu D, oraya gidin. D'nin ilk görülmemiş komşusu E, oraya gidin. E'nin ilk görülmemiş komşusu C. C'nin iki komşusu da zaten görülmüş, yani E'ye geri dönün. E'de F hâlâ görülmemiş, F'ye gidin. Ziyaret sırası A, B, D, E, C, F.

Şimdi DFS'in F'ye nasıl ulaştığına bakın. A B D E F yolundan gitti. Bu dört yol. Ama bir BFS yalnız üç yol kullanırdı.

"F'ye hiç ulaşabilir miyim?" sorusu için bu tamamen uygun. "F'ye en kısa yol hangisi?" için yanlış araç. DFS en kısa dalı değil, bulduğu ilk dalı alır.

Farkı hatırlamanın basit yolu şu: Kuyruk yayılır. Yığıt derine iner.

Peki DFS ne işe yarar? Bir yolun hiç var olup olmadığını söyler. Döngüleri tespit eder ve bir labirentin ulaşılabilir her dalını keşfeder. Bir oyunda her hamleyi aramak, kazanamayacak hamleleri atlamak için ek hilelerle aynı fikri kullanır.

DFS ayrıca birbirine bağımlı görevleri, bir görevin ihtiyaç duyduğu her şey önce bitecek şekilde sıralar. Buna **topolojik sıra** (topological order) denir. Yalnız bağımlılıklarda döngü yoksa çalışır. A, B'ye ve B, A'ya ihtiyaç duyuyorsa hiçbir sıra işe yaramaz. Bu yüzden yazılım projelerini derleyen araçlar o döngüyü fark edip bildirmek için aynı yürüyüşü kullanır.

Bir soru daha var. Her yürüyüş bellekte neye mal olur? BFS çok kullanabilir. Kuyruğu geniş bir çizgenin bütün bir halkasını aynı anda tutabilir. DFS ise derinlikle büyüyen bir yığıt tutar. Özyinelemeli yazılmışsa bu çağrı yığıtı derinliğidir ve yeterince derin bir çizge onu taşırır. Derin çizgeler için açık bir yığıt kullanın.

Özet şu. Kuyruk size adım sayısına göre en kısa yolları verir. Yığıt size ulaşılabilirliği, döngü tespitini ve bağımlılık sırasını verir. Aynı adımlar, bir yapı değiştirilmiş.

## Dijkstra algoritması: en ucuz yol

Harita uygulamanız bir şehirde en hızlı rotayı bir saniyenin altında bulur. Şehirde 100.000 kavşak olabilir. Uygulama her rotayı denemez, çünkü herhangi bir bilgisayarın kontrol edebileceğinden çok daha fazla rota var. Onun yerine üç şeyi birleştirir: ağırlıklı bir çizge, bir öncelikli kuyruk ve dışarı doğru genişleyen bir yürüyüş.

Yöntemin adı **Dijkstra algoritması**, onu icat eden bilgisayar bilimcisinin adından. Baştan bir varsayım yapar: Hiçbir yolun negatif bir maliyeti olamaz.

Başlangıçtasınız ve orada olmak sıfıra mal oluyor. Başlangıçtan çıkan her yola bakın ve her komşuya ulaşmanın maliyetini not edin. Sonra şimdiye kadar bilinen maliyeti en küçük olan ziyaret edilmemiş kavşağı seçin ve oradan genişleyin. Her komşusu için bu kavşaktan geçmenin daha önce bulunan her şeyden ucuz olup olmadığını kontrol edin. Öyleyse o komşunun en iyi maliyetini güncelleyin. Kavşağı tamamlandı işaretleyin. Sonra tekrarlayın. Her zaman bilinen en ucuz ziyaret edilmemiş noktayı genişletin.

Üç kavşakla somutlaştırın. A'dan başlıyorsunuz. A'dan B'ye bir yol 5 tutuyor. A'dan C'ye bir yol 2 tutuyor. Ve C'den B'ye bir yol 1 tutuyor. A'dan, B'yi 5'te ve C'yi 2'de not edersiniz.

Devam etmeden tahmin edin, B'ye doğrudan yol oraya en iyi yol mu? En ucuz ziyaret edilmemiş nokta C, yani önce onu genişletirsiniz. C'den geçerek B 3 tutar. Bu 5'ten iyi. Yani B'nin en iyi maliyeti 3 olur. Şimdi en ucuz ziyaret edilmemiş nokta B. Onu genişletirsiniz ve bitti. Rota A'dan C'ye, oradan B'ye. Doğrudan yol daha kısa görünüyordu ama kural her yolu denemeden daha iyi rotayı buldu.

Neden ona güvenebilirsiniz? En ucuz ziyaret edilmemiş noktayı düşünün. Ona ulaşmanın başka herhangi bir yolu önce başka bir ziyaret edilmemiş noktadan geçmek zorunda olurdu. Ama diğer her ziyaret edilmemiş nokta zaten en az onun kadar tutuyor. Ve hiçbir yol sıfırdan az tutmuyor. Yani hiçbir dolambaç daha ucuz olamaz. Bir nokta en ucuz olunca maliyeti kesindir. Ve hedef en ucuz nokta olunca maliyeti cevaptır. Yani durabilirsiniz.

Her seferinde en ucuz ziyaret edilmemiş noktayı seçmek tam olarak bir heap'in işidir. Onu yaklaşık log n adımda verir. Heap olmadan her tur her noktayı tarardı ve arama ikinci dereceden olurdu. Bir heap ile bütün bir şehir bile bir saniyenin çok altında sürer.

## Dijkstra adım adım

Maliyetler yoldan yola değiştiğinde adım saymak yetmez. İşe yarayan kural bilinen en ucuz noktayı genişletmektir. İşte o kural daha büyük bir haritada tur tur, böylece öncelikli kuyruğun işini yapmasını izleyebilirsiniz.

Aynı altı kavşağı alın ve şimdi her yola bir maliyet verin. A'dan B'ye 4, A'dan C'ye 1. B'den D'ye 2, C'den E'ye 2 ve D'den E'ye 2. D'den F'ye 1, E'den F'ye 5. A'dan F'ye en ucuz rotayı istiyoruz.

**Tur 1.** A'yı maliyet 0'da genişletin. B 4, C 1 alır. Öncelikli kuyruk C'yi 1'de ve B'yi 4'te tutar.

**Tur 2.** C 1 ile en ucuz. C'den geçerek E 3 tutar. Kuyruk şimdi E'yi 3'te ve B'yi 4'te tutar.

**Tur 3.** E 3 ile en ucuz. E'den geçerek D 5, F 8 tutar. Kuyruk B'yi 4'te, D'yi 5'te ve F'yi 8'de tutar.

**Tur 4.** B 4 ile en ucuz. B'den geçerek D 6 tutardı. Bu 5'ten kötü, yani hiçbir şey değişmez. Bir noktayı genişletmek her zaman bir şeyi iyileştirmez. Kuyruk hâlâ D'yi 5'te ve F'yi 8'de tutar.

**Tur 5.** D 5 ile en ucuz. D'den geçerek F 6 tutar. Bu 8'den iyi. Yani F'nin en iyi maliyeti 6'ya düşer. Kuyruk yalnız F'yi 6'da tutar.

**Tur 6.** F, 6 ile en ucuz ziyaret edilmemiş nokta. Hedef bu, yani dururuz.

İyileştirmeleri geriye doğru takip edin, rota A C E D F. Toplam maliyet 6.

Düz BFS bu haritada neyi seçerdi? Bir saniye düşünün. BFS maliyetleri görmezden gelir ve yolları sayar. Cevabı A, B, D, F. Bu yalnız üç yol. Ama o yollar 4, 2 ve 1 tutuyor. Toplam yedi eder. Dijkstra'nın rotası dört yol kullanıyor ve 6 tutuyor. En az adım, en ucuzla aynı şey değil.

BFS, her kenarın 1 tuttuğu aynı yürüyüştür. O durumda düz bir kuyruk en ucuz önce sırasını kendiliğinden verir. Ağırlıklı kenarlar bunu bozar ve öncelikli kuyruk onu geri getirir.

Sınırlar var. Algoritma maliyetlerin negatif olmadığını varsayar. Negatif maliyetli bir kenar "sonradan daha ucuz bir şey çıkamaz" garantisini bozar. O zaman farklı bir algoritmaya ihtiyacınız olur.

Harita uygulamaları ayrıca A* (A yıldız) denen bir fikir kullanır. Aramayı hedefe doğru yönlendirmek için kalan mesafeye dair bir tahmin eklerler. Ve en hızlı mı, en kısa mı, en ucuz mu olduğu tamamen kenarlara hangi sayıları koyduğunuza bağlıdır.

Şunu hatırlayın: En az yol ve en ucuz rota farklı sorulardır. Düz bir kuyruk birincisini cevaplar. Öncelikli kuyruk ikincisini.

## LRU önbelleği: iki yapı birlikte

Şimdiye kadar her yapı kendi başına çalıştı. Gerçek sistemler nadiren tek bir tane kullanır. Çoğu zaman en iyi cevap, birlikte çalışan iki basit yapıdır.

Web uygulamanızdaki her profil görüntüleme veritabanına bir istek gönderiyor. Bu yüzden en son görüntülenen bin profili bellekte tutmaya karar veriyorsunuz. O alan dolunca, en uzun süredir kimsenin bakmadığını atıyorsunuz. Bu bir **LRU önbelleği**, "least recently used" yani en uzun süredir kullanılmayanın kısaltması.

Üç işi var. Birincisi, bir profili müşteri numarasına göre hızlıca bulmak. İkincisi, bir profil her okunduğunda onu en yeni olarak işaretlemek. Üçüncüsü, önbellek dolunca en eski girişi bulup çıkarmak. Bir girişi böyle atmaya tahliye (eviction) denir ve onun da hızlı olması gerekir. Bunlar neredeyse her istekte olur, yani burada hızlı O(1) demek.

Önce akla gelen yapıyı deneyin. Bir hash tablosu size müşteri numarasıyla aramayı yaklaşık bir adımda verir. Ama hash tablosunun sıra kavramı yoktur. En uzun süredir kullanılmayan girişi bulmak için her girişi tarayıp zaman damgalarını karşılaştırırdınız. Bu O(N), her tahliyede bin kontrol.

O zaman bir liste deneyin. Onu en yeni önde, en eski arkada olacak şekilde sıralı tutun. Artık tahliye kolay. Arkadaki öğeyi atarsınız. Ama numaraya göre aramak için onu bulana kadar listeyi dolaşırsınız. Bu O(N). Ve bir öğeyi en yeni olarak işaretlemek onu öne taşımak demek. Önce onu bulmanız gerekir.

Her yapı, diğerinin kötü olduğu şeyde iyi. Çözüm basit. Seçmeyin, ikisini de kullanın.

Bir önceki modüldeki gibi her düğümün iki yönü de gösterdiği bir çift bağlı liste tutun. Onu en yeni önde, en eski arkada olacak şekilde sıralayın. Sonra müşteri numarasından o müşterinin listedeki düğümüne giden bir hash tablosu tutun. Tablo doğrudan profili göstermez. Düğümü gösterir ve düğüm müşteri numarasını ve profili tutar. Tahliyenin tabloyu temizlemek için o numaraya ihtiyacı var.

Artık her işlem kısa. Bir müşteriyi okumak için hash tablosundan düğümünü isteyin. Bu ortalamada O(1). O düğümü bulunduğu yerden koparıp öne bağlayın. Bu sadece birkaç işaretçi değişikliği. Tahliye için arkadaki düğümü alın, numarasını hash tablosundan çıkarın ve koparın. Yeni bir müşteri eklemek için önde bir düğüm oluşturun ve numarasını tabloya koyun. Hiçbir adım listeyi dolaşmıyor.

Yalnız üç tutan bir önbellekle izleyelim. A müşterisi görüntüleniyor, liste sadece A. Sonra B görüntülenip öne geçiyor, liste "B A" okunuyor. Sonra C geliyor ve liste "C B A" okunuyor.

Şimdi A tekrar görüntüleniyor. Hash tablosu bize arkadaki A'nın düğümünü veriyor. Onu öne taşıyoruz. Liste şimdi "A C B" okunuyor.

Şimdi D müşterisi görüntüleniyor ve önbellek dolu. Bir saniye düşünün ve hangisinin atılacağını tahmin edin.

B. Pek çok kişi ilk girdiği için A'yı tahmin eder. Bu saçma bir tahmin değil. Bir kuyruk tam olarak böyle karar verirdi, en eski önce. Ama A az önce kullanıldı, bu yüzden öne taşındı. B arkada duruyor, geldiğinden beri dokunulmamış. B'yi koparıyoruz, B'nin girişini tablodan siliyoruz ve D'yi öne koyuyoruz. Liste "D A C" okunuyor.

Her yapı diğerinin kötü olduğu şeyi kapatıyor.

## Neden çift bağlı liste?

O önbellek bir hash tablosunu bir çift bağlı listeyle eşleştiriyor. Liste neden iki yönlü işaretçilere ihtiyaç duyuyor? Elle çözün.

Diyelim liste en yeniden en eskiye "A C B" okunuyor. C müşterisi tekrar görüntüleniyor ve hash tablosu size C'nin düğümünü veriyor. C'yi öne taşımak için önce onu listeden çıkarmanız gerekiyor. Bu, A'nın C'nin ötesini, doğrudan B'yi göstermesini sağlamak demek.

Soru şu: Her düğümün yalnız ileriyi gösterdiği bir listede, C'nin hemen önündeki düğüm olan A'yı nasıl bulursunuz? Bir saniye düşünün.

Bulamazsınız, doğrudan değil. C yalnız kendinden sonra geleni bilir. Yani listenin önünden, C'yi gösteren düğüme ulaşana kadar yürürdünüz. Bu O(N), önbelleğin kaçınmak için kurulduğu maliyetin ta kendisi.

Şimdi her düğüme bir geri işaretçisi verin. C zaten A'nın arkasında, B'nin önünde olduğunu biliyor. Yani A'yı B'ye yönlendirir, B'yi A'ya geri yönlendirir ve C'yi öne taşırsınız. Bu, hiç yürüyüş olmadan birkaç işaretçi değişikliği. Yani O(1). En eski girişi atmak da aynı şekilde çalışır. Arkadaki düğüm, hangi düğümün yeni arka olacağını zaten biliyor.

İki yapıyı birleştirmenin bir bedeli var ama. Artık her zaman eşleşmesi gereken iki yapınız var. Tabloyu güncelleyip listeyi güncellemezseniz izini sürmesi çok zor hatalar elde edersiniz. Her giriş ayrıca ek bellek taşır. Bu, verinin üstüne bir tablo yuvası artı iki işaretçi.

Pek çok dil sıralı bir map ya da hazır bir LRU yardımcısıyla gelir. Kendinizinkini yazmak yerine onu kullanın. Birini elle kurmak çoğunlukla klasik bir mülakat sorusu. İyi bir soru, çünkü her parçanın neden orada olduğunu anlayıp anlamadığınızı test ediyor.

Son bir uyarı. Boyut sınırı bir ayrıntı değil. Hiçbir şeyi atmayan bir önbellek bir bellek sızıntısı gibi davranır. Yani bir programın tutup hiç geri vermediği bellek. Önbellek, sunucunun belleği bitip çökene kadar büyür.

Yani önbelleği bir hash tablosu ve bir çift bağlı listeden kurun. İkisini eşleşik tutun ve her zaman bir boyut sınırı koyun.

## Veritabanı indeksi

Önbellek en çok kullanılan verinizin biraz kısmını bellekte tutar. Verinizin çoğu çok daha büyük ve daha yavaş bir yerde yaşar. Diskteki bir veritabanında yaşar.

Yavaş bir giriş sayfası, uğraştığım en yaygın performans şikayetlerinden biri. Sayfa bir müşteriyi e-postasına göre arıyor. Küçük bir tabloyla testte anında çalışıyor. 10 milyon müşterili canlı uygulamada aynı istek saniyeler sürüyor. Veritabanına böyle bir isteğe sorgu (query) denir.

Fark, veritabanının satırı nasıl bulduğunda. Yardım olmadan veritabanının tek bir seçeneği var. Tablodaki her satırı okuyup her e-postayı kontrol eder. Buna tam tablo taraması (full table scan) denir ve O(N)'dir. Daha kötüsü, satırlar diskte ya da diskten yüklenmiş sayfalarda yaşar. Disk bellekten çok daha yavaştır, yani her ek okuma gerçek zamana mal olur. Tek bir müşteri için çekilecek çok sayfa var.

Çözüm bir **indeks** (index). İndeks, veritabanına her satırın nerede yaşadığını söyleyen ayrı bir yapıdır. Genellikle bir sütunun değerine göre sıralı tutulur. Bir kitabın arkasındaki dizin gibi çalışır. Sıralı listede bir kelimeye bakarsınız ve o size bir sayfa numarası verir.

Peki hangi şekilde? Sıralı ve log n aramalı bir ikili arama ağacı tahmin edebilirsiniz. Ama bir sorun var. Bir veritabanı diskten sayfa ya da blok denen sabit boyutlu parçalar halinde okur. Bir ikili ağaç düğümü bir anahtar ve iki çocuk tutar. Aşağı yürümek her seviye için pahalı bir sayfa okuması demek. 10 milyon satır için bu yaklaşık 24 seviye ve muhtemelen o kadar sayfa okuması.

Cevap, her düğümü tam bir sayfa yapmak. Bir sayfa yüzlerce anahtar ve yüzlerce çocuk işaretçisi tutabilir. Bu, dengeli ağaçlarda duyduğunuz B ağacı. Her düğüm uzun değil geniş. Çoğu veritabanı B+ ağacı denen yakın bir türevini kullanır ama fikir aynı. Tek bir sayfa okuması aramayı iki değil, yüzlerce kat daraltır.

Kabaca sayılar koyalım. Diyelim her sayfa yaklaşık 500 anahtar tutuyor. 10 milyon arasından bir müşteriyi bulmak kaç sayfa okuması sürer? Bir tahminde bulunun.

Kök sayfa altındaki 500 sayfayı gösterir. O sayfalar 500 × 500 sayfayı gösterir. Bu 250.000 sayfa. Bu alt sayfaların her biri 500 giriş tutar. Bu, üçüncü seviyenin 125 milyon satırı kapsadığı anlamına gelir. Yani bu kaba modelde 10 milyon arasından bir müşteriyi bulmak yaklaşık üç sayfa okuması sürer. Dördüncü bir seviye ekleyin, bir milyarı geçersiniz.

Üst seviyeler sürekli okunur, bu yüzden genellikle bellekte kalırlar. Çoğu zaman yalnız son bir ya da iki okuma gerçekten diske dokunur. Bazı veritabanları satırların kendisini o alt seviyede tutar. Arama yine aynı şekilde çalışır.

Bir B ağacı sıralı olduğu için tam eşleşmelerden fazlasını yapar. "Geçen hafta kaydolan müşteriler" gibi bir sorgu alın. İlk eşleşen tarihi bulur. Sonra tarihler bitene kadar komşu girişler boyunca ileri yürür. Kayıt tarihine göre sıralayan bir sorgu indeksi sırasıyla okuyabilir. Satırları sıralamasına gerek kalmaz.

Bazı veritabanları ayrıca bir hash indeksi sunar. Bir hash tablosu gibi çalışır. Tek bir hash sizi doğrudan girişe götürür. Bir e-posta gibi tam eşleşmeler için harikadır. Aralıkları ya da sıralamayı cevaplayamaz, çünkü hash'leme sırayı dağıtır.

Yani büyük bir tabloda bir arama yavaşsa ilk soru, satırı bir indeksin bulup bulamayacağıdır.

## İndeksleri iyi kullanmak

Bir indeks satır bulmayı hızlandırır. İndeksleri iyi kullanmak birkaç kural ister.

İndeksler bedava değildir. Her biri disk alanı tutar. Her ekleme, güncelleme ya da silme de etkilediği indeksleri güncel tutmak zorundadır. Veritabanı bu işin bir kısmını toplu yapar ama daha fazla indeks yine her yazmada daha fazla iş demek. Yani yazmalar yavaşlar ve her sütunu indekslemek bir hatadır.

Ana kural şu. Aradığınız ve sıraladığınız sütunları indeksleyin, başka pek bir şeyi değil. Bir indeks en çok, bir sorgu tablonun küçük bir dilimini seçtiğinde yardım eder.

Şunu deneyin. Bir sorgu, hesabı aktif olan her müşteriyi istiyor ve her 10 müşteriden 9'u aktif. O sütundaki bir indeks yardım eder mi? Bir saniye düşünün.

Muhtemelen hayır. İndeksi dolaşıp sonra her 10 satırdan 9'unu getirmek, bütün tabloyu bir kez okumaktan daha yavaştır. Yani iyi bir veritabanı indeksi atlar.

Ayrıca sorgunuz bir sütunu bir fonksiyona sardığında dikkat edin. Örneğin karşılaştırmadan önce e-postayı küçük harfe çevirebilir. İndeks asıl değerleri tutar, bu yüzden çoğu zaman yardım edemez. Aynısı "gmail.com ile biten her e-posta" araması için de geçerli. Değerin nasıl başladığını bilmediğinizde sıralı bir liste yardım edemez.

Şimdi pek çok kişinin kafasını karıştıran durum. Bu, birleşik indeks (composite index) denen iki sütun üzerindeki bir indeks. Soyada, sonra ada göre bir indeks düşünün, soyada göre sıralı bir sınıf listesi gibi. Şu üç sorgudan hangisi onu kullanabilir?

Birincisi soyadı García olan herkesi buluyor. İkincisi Anna García'yı soyadı ve adına göre buluyor. Üçüncüsü herhangi bir soyadıyla Anna adlı herkesi buluyor.

İlk ikisi genellikle onu kullanabilir. Liste soyada göre sıralı, yani bütün García'lar bir arada duruyor. İçlerinde Anna'yı bulmak kolay. Üçüncüsü onu normal şekilde kullanamaz. Her Anna, her soyadının altında bir tane olmak üzere bütün listeye dağılmış durumda. Bazı veritabanları yine de bütün indeksi tarayabilir ama bu bir kısayol değil. Birleşik bir indekste sütun sırası önemlidir.

Son olarak tahmin etmeyin. Her büyük veritabanının bir EXPLAIN ya da sorgu planı özelliği vardır. Bir sorgunun indeksinizi kullanıp kullanmadığını ya da tam taramaya dönüp dönmediğini gösterir. Eksik bir indeksin, bir kez eklenince bir sorguyu saniyelerden milisaniyelere indirdiğini gördüm. Onu fark etmenin yolu bu kontrol.

## Doğru yapıyı seçmek

Seçebileceğiniz pek çok yapı var: hash tabloları, ağaçlar, heap'ler ve çizgeler. Her biri farklı bir iş için yapılmış. Asıl beceri doğrusunu seçmek.

Bir tasarım incelemesi düşünün. Bu, takımın bir planı kurulmadan önce kontrol ettiği bir toplantı. Biri soruyor: "Bu veriyi hangi yapı tutuyor ve neden?" "Liste, çünkü hep onu kullanıyoruz" bir cevap değil. "Hash tablosu, çünkü hızlılar" da değil.

İyi bir cevap işten başlar. Kodunuzun en sık sorduğu soruyu söyleyin. Sonra onu o soru için yapılmış yapıyla eşleştirin.

Pratik yapalım. Kodunuzun sorabileceği bir soru vereceğim. Ben söylemeden yapıyı seçin.

İlk soru: Bu kupon kodu geçerli mi? Bir saniye. Bu bir hash kümesi. "Bu burada mı?" sorusunu yaklaşık bir adımda cevaplar.

Sonraki: Bu sayfadaki 40. sıradaki müşteriyi ver. Bu bir dizi. Bir sıraya atlamak bir adım sürer.

Sonraki: 20 ile 50 dolar arasındaki her ürünü en ucuzdan başlayarak göster. Düşünün. Aramaya ve sıraya birlikte ihtiyacınız var. Bu dengeli bir arama ağacı. 20 dolara atlayıp 50'yi geçene kadar sırayla ileri yürüyebilir.

Sonraki: Şu an hangi teslimat en acil? Bu bir heap. En acil öğeyi her zaman tepede tutar.

Sonraki: Son değişikliği geri al. Bu bir yığıt, en yeni önce.

Ve "bu belgeleri gönderildikleri sırayla yazdır" bir kuyruk, en eski önce.

Zaten elimde tuttuğum öğelerin hemen yanına ekle ve çıkar, bir bağlı liste için daha seyrek bir durum.

Sonuncusu: Depodan bu kasabaya herhangi bir kara yolu var mı? Bu bir çizge. Onu depodan dolaşır ve kasabaya ulaşıp ulaşmadığınızı görürsünüz.

Ne yaptığınıza dikkat edin. Hiçbir zaman yapıdan başlamadınız. Sorudan başladınız ve soru yapıyı gösterdi.

Dikkat edilecek üç tuzak var. Birincisi, soruya göre değil alışkanlığa göre seçmek. "Bu burada mı?" için kullanılan bir liste her öğeyi kontrol eder ve bu yine ikinci dereceden döngü. İkincisi, küçük boyutları unutmak. Bir avuç öğe için sırayla kontrol edilen küçük bir dizi bir hash tablosunu yenebilir. Önbellek o taramayı çok hızlı yapar. Üçüncüsü, kendinizinkini kurmak. Dilinizdeki yerleşik yapılar zaten iyi sürümler. Onları kullanın ve her birinin neye mal olduğunu bilin.

## Gerçek senaryolarda seçim

Her yapının neyde iyi olduğunu bilmek bir şey. Gerçek bir tasarım incelemesinde birini seçmek başka. O yüzden önceki alışkanlığı çalışalım. Yapıdan değil, işten başlayın. Bir incelemede yapacağınız gibi bir tanesini çözelim.

50.000 müşteri kaydınız var. Uygulama gün boyu müşterileri e-postayla arıyor. Kayıtlar düzenli güncelleniyor. Ve yönetim ekranı müşterileri kayıt tarihine göre sıralı, her seferinde bir sayfa listeliyor. Durun ve ben seçmeden siz seçin.

En sık işlemle başlayın. E-postayla arama. E-postaya göre anahtarlanmış bir hash tablosu bunu 50.000'e kadar kontrol yerine yaklaşık bir kontrole indirir. Ana yapı bu.

Ama hash tablosunun sırası yok, yani sıralı yönetim ekranına hizmet edemez. Ekran her yüklendiğinde 50.000'in hepsini sıralayabilirsiniz. Bu, sayfa görüntüleme başına n log n. Ya da ikinci bir yapı tutabilirsiniz, eşitlikleri bozmak için müşteri numarasıyla birlikte kayıt tarihine göre anahtarlanmış dengeli bir ağaç. Bu size sıralı yürüyüşü doğrudan verir.

İki yapı daha fazla bellek demek ve her ekleme ya da silmenin her zaman eşleşsinler diye ikisini de güncellemesi gerekir. Yönetim ekranı günde iki kez açılıyorsa istek üzerine sıralamak daha basit ve yeterli. Sürekli açılıyorsa ağaç buna değer. Cevap tablonun kendisinden değil, uygulamanın gerçekte nasıl kullanıldığından gelir.

Bir tane daha. Bir an durup karar verin. Bir destek kuyruğu her zaman sıradaki en yüksek öncelikli talebe hizmet etmeli. Gün boyu yeni talepler geliyor.

Bu, en acil talep tepede olmak üzere öncelikli kuyruk olarak kullanılan bir heap. Her yeni talebi eklemek O(log n). En acili almak O(log n).

Neden bir liste tutup her seferinde sıralamıyoruz? Sıralama n log n ve her gelişte tekrarlardınız. Neden hash tablosu değil? Bir talebi numarasıyla anında bulur ama hepsini taramadan hangi talebin en acil olduğunu söyleyemez. Temsilciler talepleri numarayla da arıyorsa heap'in yanına bir hash tablosu ekleyin. Daha önce olduğu gibi ikisi her zaman eşleşmeli.

Bir tane daha, önce tahmin edin. Bir sosyal uygulama tanıyor olabileceğiniz kişileri öneriyor. Bunlar sizden iki bağlantı uzakta olan ve zaten arkadaşınız olmayan kişiler.

Soru, neyin neye bağlı olduğu. Yani bu bir çizge. Her kişi bir köşe ve her arkadaşlık bir kenar. Komşuluk listeleri her kişinin arkadaşlarını tutar. Bir komşuluk matrisi neredeyse tamamen boş olurdu. Her kişi herkesin yalnız minik bir kısmını tanır.

Sizden başlayarak genişlik öncelikli arama çalıştırın ve 2. derinlikte durun. 1. seviye arkadaşlarınız. 2. seviye onların arkadaşları ve adaylar bunlar.

Sizden ve arkadaşlarınızdan oluşan bir hash kümesi tutun. 2. seviyede ulaştığınız her kişi için önce o kümeyi kontrol edin, böylece kendinizi ya da zaten tanıdığınız birini hiç önermezsiniz. Sonra bir hash tablosunda o kişinin sayısına bir ekleyin. Aynı kişi pek çok ortak arkadaş üzerinden çıkar. Yani sayı, kaç ortak arkadaşınız olduğunu söyler. Her kişi önerilerde yine bir kez görünür, o sayıya göre sıralı.

İki soruluk test hepsini birbirine bağlıyor. Nasıl büyür? Bu Büyük O sütunu. Nerede durur? Bu, verinin önbelleğin sevdiği şekilde bellekte yan yana mı durduğu, yoksa dağınık ve işaretçilerle bağlı mı olduğu. İkisini de cevaplayın, nadiren yanlış seçersiniz.

## Gerçek bir üründe seçimler nerede?

Artık ana veri yapılarını biliyorsunuz. Ama geliştiricilerin çoğunda bir soru kalır. Gerçek bir üründe gerçekten nerede birini seçme fırsatınız olur?

Bir sürprizle başlayın. Gündelik kodun büyük kısmında seçmezsiniz. Uygulamanız bir siparişi kaydettiğinde ya da bir müşteriyi aradığında işi, sizin için seçtiği yapıları kullanan veritabanı yapar. O zaman sizin seçimleriniz gerçekte nerede olur?

Çoğunlukla dört yerde. Zaten bellekteki veri üzerinde çalışan kod var. Popüler veriyi yakında tutan önbellekler var. Sistemin parçaları arasında iş taşıyan kuyruklar var. Ve hangi veritabanı indekslerini ekleyeceğinizi seçmek var.

Bir internet mağazasını dolaşıp o seçimleri özellik özellik bulalım. Her biri için ben cevaplamadan yapıyı tahmin edin.

Birincisi, "son görüntülenenler" şeridi. Bir alışverişçinin baktığı son 10 ürünü gösteriyor, en yeni önce. Bir ürünü tekrar görüntülerseniz öne atlıyor. Şerit dolunca en eskisi düşüyor. Bir tahminde bulunun.

Bunun aynı anda iki işe ihtiyacı var. Şeritte zaten olup olmadığını görmek için bir ürünü hızlı bulması ve uçlardaki öğeleri hızla taşıması ya da düşürmesi gerekiyor. Hiçbir tek yapı ikisini birden yapmaz, bu yüzden ikisini eşleştirirsiniz. Bir hash tablosu ürünü bir adımda bulur. Bir çift bağlı liste sırayı tutar. Yani bir ürünü öne taşımak ya da en eskisini düşürmek de bir adım sürer. Bu çifte LRU önbelleği denir.

İkincisi, ana sayfadaki "en çok satan 10" kutusu. Satışlar gün boyu gelmeye devam ediyor ve kutu her zaman en çok satan 10'u gösteriyor. Ne kullanırdınız? Her satıştan sonra her ürünü sıralamak israf olurdu, çünkü yalnız ilk 10 ile ilgileniyorsunuz. Bir heap tam bunun için yapılmış. En büyük öğesini her zaman bilir ve yeni bir tane eklemek yaklaşık log n adım sürer. Yani bütün kataloğu hiç sıralamak zorunda kalmazsınız.

Üçüncüsü, ödemeden sonraki e-postalar, bir fiş ve bir kargo bildirimi gibi. Bir indirim gününde birkaç dakikada binlerce sipariş geliyor. Hangi yapı? Bir kuyruk. Ödeme her e-posta işini arkaya ekleyip hemen bitiyor. Bir işçi onları önden, en eskiden başlayarak gönderiyor. Kuyruk bütün yoğunluğu tutuyor ve kimse bir yükleme sayfasında beklemiyor.

Dördüncüsü, "bunu alanlar şunları da aldı" satırı. Her ürünü bir nokta olarak düşünün ve insanlar iki ürünü birlikte her aldığında aralarına bir çizgi çekin. Bu ne? Bir veya iki adım dışarıya aranan bir çizge, tıpkı "tanıyor olabileceğin kişiler" gibi. Büyük bir mağazada bu arama genellikle önceden çalışır ve sonuçlar hızlı arama için kaydedilir.

Son bir özellik sipariş geçmişi sayfası. Tarihe göre sıralı, her seferinde 10 sipariş. Burada doğru cevap genellikle bırakın veritabanı yapsın. Müşteri ve sipariş tarihi üzerindeki bir indeks size sıralı sayfaları doğrudan verir. Sizin işiniz doğru indeksi eklemek, kendiniz bir ağaç kurmak değil.

Desene dikkat edin. Gerçek özellikler çoğu zaman birlikte çalışan iki yapıya ihtiyaç duyar. Ve bazen en iyi seçim veritabanının seçmesine izin vermektir.

İşte herhangi bir özellikte kullanabileceğiniz bir kontrol listesi. Birincisi, özelliğin verisiyle ne yaptığını listeleyin. İkincisi, en sık yaptığı işi bulun. Üçüncüsü, o işi bir yapıyla eşleştirin. Dördüncüsü, verinin ne kadar büyüyebileceğini sorun. Ve beşincisi, akıllıca bir şeye geçmeden önce ölçün.

## Modül özeti

Her uygulamanın sorduğu soruların arkasındaki yapılar bu kadar. Bir hash tablosu bir anahtarı bir yuvaya çevirir, yani "bunu gördüm mü?" yaklaşık bir adım sürer. Anahtarlar değişmediği ve çakışmalar seyrek kaldığı sürece hızlı kalır.

Bir arama ağacı şeyleri sıralı tutar ama sıralı veride yalnız kendini dengeleyen bir tanesi hızlı kalır. Bir heap en acil öğeyi her zaman bilir. Ve bir çizge her şeyi her şeye bağlı modeller.

Bir çizgeyi dolaşmak için bir kuyruk halkalar halinde yayılır ve en az adımı bulur, bir yığıt ise derine iner. Yolların maliyetleri farklı olduğunda Dijkstra her zaman en ucuz noktayı genişletir.

Sonra gerçek sistemler yapıları birleştirir, bir LRU önbelleğindeki hash tablosu ve çift bağlı liste gibi. Ve bir veritabanı indeksi geniş, sığ bir ağaçtır. Tam bir taramayı, daha yavaş yazmalar pahasına birkaç sayfa okumasına çevirir.

Gerçek bir üründe seçimlerinizin çoğu önbelleklere, kuyruklara ve hangi indeksleri ekleyeceğinize iner. Yani bir yapı seçerken kodun en çok ne yaptığından ve ne sıklıkla yaptığından başlayın.

Sırada, son modül kodunuz çalışırken ne olduğuna bakıyor.
