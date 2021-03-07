---
layout: post
title:  "Yazılımda Test Türleri ve Arasındaki Farklar"
date:   2017-04-23 13:30
categories: [Turkish, Coding]
tags: software, tests, unit test, acceptance test, integration test
meta: software, tests, unit test, acceptance test, integration test
author: ozziest
post_img: 07.jpg
post_img_link: https://pixabay.com/en/eyeglasses-exam-optometry-vision-2003188
---

### 1. Giriş

***Testing*** kavramı yazılım tarihi kadar eski, zaman içerisinde gelişmiş ve bir çok alt dallara ayrılmış, kendi içerisinde oldukça teorik bilgi barındıran bir kavramdır. Her ne kadar ülkemizdeki bilgi birikimi testing konusunda oldukça yetersiz olsa da, İngilizce olarak yazılmış bir çok farklı kitap ve kaynağa ulaşmanız mümkündür. Bu kaynakları göz ucuyla taradığınızda dahi, testing kavramının pratiğinden önce ne kadar gelişmiş ve detaylı bir teorisinin bulunduğunu anlayabilirsiniz. Ben de bu makale ile naçizane bilgilerimi, dilim döndüğünce ve kelimelerim elverdiğince anlatmaya çalışacağım. 

Bu makale ile testing kavramının özünü açığa çıkarmak ve testing terminolojisine dokunarak test türleri üzerinde karşılaştırmalı bir yazı yazmak niyetindeyim. Makale okuyucularının makale sonunda, test türleri hakkında yeterli teorik bilgiye sahip olmasını amaçlamaktayım.

Bu makale ile öğrenilebilecek teorik bilgileri, pratik test yazımı konusunda bir rota oluşturabilmesi açından önemli buluyorum. Rotası olmayan bir yolculuğun size zaman kaybettirmesinin yanı sıra, doğru hedefe ulaşamaması da her zaman için ihtimaller dahilindedir.

### 2. Test Nedir?

Yazılım sektöründe kullanılan ***test*** kelimesi henüz TDK literatüründe kendine yer bulamamış olsa da, yalın bir tabir olması açısından aşağıdaki ifadeyi kullanabiliriz;

> ***Test***; bir amacı gerçekleştirmek için kodlanan bir yazılımın ya da yazılımının bir bölümünün, amaçlanan hedefi gerçekleştirip gerçekleştirmediğinin kontrol edilmesidir.

Bu tanımı baz alırsak, geliştirdiğimiz her kodun bir şekilde test edildiği sonucuna ulaşırız. En bağlangıç seviyesinde yazılım geliştirenlerimiz bile, yazdıkları ufacık bir kod parçasının doğru çalışıp çalışmadığını el yordamıyla kontrol etmektedir. Bu bağlamda ***test etme*** eylemi sürekli uyguladığımız bir eylemdir. 

Ancak söz konusu eylem, üretilen yazılımdaki kod sayısı arttıkça uygulanması zor bir hale gelecektir. 10 satır kodun el yordamıyla test edilmesi oldukça kolay olabilir. Ancak binlerce satır, birbiri ile bağlantılı bir kod örüntüsünün, eklenen ya da yeniden düzenlenen her satırdan sonra el yordamıyla kontrol edilmesi imkansıza yakındır. Bu nedenle yazılım dünyasında ***test*** kelimesini kullandığımızda, el yordamıyla yapılan kontroller konumuzun **dışındadır**. 


Yazılımda "***test***" dediğimizde anlamamız gereken tanım şudur;

> ***Test***; bir amacı gerçekleştirmek için kodlanan bir yazılımın ya da yazılımının bir bölümünün, amaçlanan hedefi gerçekleştirip gerçekleştirmediğinin ***kod aracılığıyla*** kontrol edilmesidir.

Özetle; geliştirdiğiniz kodun kontrolünü yapmak amacıyla yazdığınız diğer kodlar için biz ***test*** kavramını kullanıyoruz. ***Test*** kelimesini kullandığımızda anlamamız gereken tanım budur.

### 4. Test Türleri

***Testing*** terminolojisine giriş yaptığınızda, farklı test türlerinin olduğunu görürüz. Bunun sebebi bu kavramı geliştiren usta yazılımcıların sizin için işi zorlaştırmak istemesi değildir. Kategorize etme işlemi zaman içerisinde, kendiliğinden oluşan doğal bir olaydır. Test türleri amaç değil, sonuçtur. Bu bölüm altında, makalemizin ana konusunu da oluşturan bu test türlerine değineceğiz.

Test türleri seviyeler, tipler ve süreçler bakımından bir ön kategoriye sahiptirler. Test türlerinden bahsetmenin tek yolu, öncelikle bu türleri dahi bir ön kategorize işlemin tabi tutmakta geçmektedir. [1] Her bir test türü başlı başına bir makale hatta bir kitap konusu olabilir. Ancak bizim burada ne hepsini açıklayabilecek gücümüz, ne de zamanımız var. Bu nedenle yazılım geliştiriciler için en çok söz konusu olan dört test türü üzerinde duracağız;

- Unit Tests (Birim Test)
- Integration Tests (Entegrasyon Testi)
- Functional Tests (Fonksiyonel Test)
- Acceptance Tests (Kabul Testi)

Buradaki dört farklı tür kendi içerisinde bir tanıma sahiptir ve her birinin farklı amaçları ve yazım yöntemleri vardır. Doğal olarak her biri için farklı araçlar söz konusu olabilmektedir. Test yazma ihtiyacı hissettiğiniz zaman, test yazma amacınızı doğru olarak belirleyemezseniz ne tür bir test yazacağınıza karar veremezsiniz. Doğru test türünde kullanmadığınızda ise test yazım amacınızı gerçekleştiremezsiniz. Dolayısıyla doğru test türünü karar vermek amaçlarınıza ulaşmak açısından önemlidir.

##### 4.1. Unit Tests

***Unit Test*** ya da Türkçe’ye çevrilmiş haliyle ***Birim Test***; her ne kadar çok sık duymasak da tarihi epey eskiye, 70’lerde ***Alan Kay*** önderliğinde geliştirilen Smalltalk programalama dilinen kadar uzanan bir terimdir. Bilgisayar bilimlerinde birim test; başka bir bloğu çağıran ya da kullanan bir kod bloğunun (genellikle metot) kendi görevini doğru olarak yapıp yapmadığının, ilişkide olduğu diğer bloktan ayrılarak test edilmesi işlemidir. Eğer görev doğru olarak yapılmazsa birim testler başarısız olur. Birim olarak adlandırdıklarımız genellikle bir metot ya da bir fonksiyondur. [2]

Buradaki anahtar faktör; test etmek istediğimiz ufacık kod parçasının, diğer kod parçalarından tamamen yalıtıralarak test edilmesidir. Amacımız; ufacık kod parçasının kendi işini doğru olarak yapıp yapmadığının kontrol edilmesidir. Bu kod parçasının diğer kodlarla olan ilişkisi birim testin konusu değildir. Bu nedenle birim test yazabilmenin ön koşulu; test edilecek kod parçasının diğer kodlardan yalıtılmış olmasıdır. Eğer test etmek istediğiniz bölüm, diğer bölümlere sıkı sıkıya bağlı ise bu bölüm için unit test yazmanız pratikte pek mümkün değildir.

Bu nokada, bahsi geçen düşünceleri kavramlaştırmamız gerekmektedir. ***Kod parçası*** olarak adlandırdığımız bölüm genellikle bir ***metot*** ya da bir ***fonksiyondur***. Bu metotun diğer kodlarla olan ilişkisine ***bağımlılık (coupling)*** adı verilmektedir. Eğer iki farklı kod parçası birbirine sıkı sıkıya bağlı ise ***Highly Coupled***, zayıf bir şekilde bağlı ise ***Loosely Coupled*** söz konusudur. [3] 

Bağımlılık kavramı anlaşılmaksınız birim testi yazmak söz konusu olmadığından, örneklerle bu kavramı pekiştirmemiz gerekmektedir. Aşağıdaki güzel Highly Coupled örneği verilmiştir;

<pre><code class="language-csharp">
public void Create(string name, string surname)
{
    UserModel model = new UserModel();
    model.name = name;
    model.surname = surname;
    model.Save();
}
</code></pre>

Bu örnekte oluşturduğumuz `Create` metodu, `UserModel` sınıfı ile sıkı sıkıya bağımlıdır ve kendi işini yapabilmek için `UserModel` sınıfını tanımak zorundadır. Bu şekilde yazılmış bir metodun birim testinin yazılması zordur. Çünkü yazacağınız test sadece `Create` metodunu değil, `UserModel` içerisindeki `Save` metodunu da test edecektir. 

Bu metodu eğer aşağıdaki şekilde yeniden düzenlersek sıkı sıkıya oluşmuş olan bu bağlantıyı, zayıf bir bağlantıya dönüştürmüş oluruz;

<pre><code class="language-csharp">
public void Create(UserModel model, string name, string surname)
{
    model.name = name;
    model.surname = surname;
    model.Save();
}
</code></pre>

Buradaki `Create` metoduna `UserModel` sınıfı dışarıdan gönderiliyor. Böylece siz bu `Create` metodunun birim testini yazarken, sahte (***mock object***[4]) bir `UserModel` sınıfı gönderebilirsiniz. Birim test yazarken kontrol edeceğimiz noktalar, ilgili modele `name` ve `surname` bilgilerinin aktarılması ve `Save` metodunun çağrılmasıdır. Eğer `Create` metodu bu işleri yapıyorsa, birim testimizden geçebilmektedir. Böylelikle `Create` metodunu, diğer kodlardan bağımsız olarak test edebilmiş oluruz ki bu; birim testin özünü oluşturmaktadır.

Birim testler basit olmalı ve oldukça küçük bir bölümün üstlendiği işi doğru olarak yapıp yapmadığını kontrol etmelidir. Bunun yanında iyi bir birim test aşağıdaki özellikleri de üzerinde taşımalıdır;

- Otomatikleştirilebilmeli ve kolaylıkla tekrar tekrar çalıştırılabilmelidir.
- Kolaylıkla yazılabilmelidir.
- Bir kez yazıldığında özellik kullanıldıkça kalmalıdır.
- Herhangi biri çalıştırabilmelidir.
- Çalıştırması bir düğmeye basmak kadar kolay olmalıdır.
- Kontrolleri çabucak (mümkünse saniyeler içinde) gerçekleştirmelidir.

Eğer aşağıdaki soruların hepsine **Evet** cevabını veremiyorsanız, muhtemelen yazdığınız şey birim test değildir;

- 2 ay önce yazdığım unit testleri çalıştırıp sonuçları alabilir miyim?
- Takımımın herhangi bir üyesi 2 ay önce yazdığım testleri çalıştırıp sonuçları alabilir mi?
- Yazdığım tüm unit testler bir kaç dakika içinde sonuçlanabilir mi?
- Tüm unit testleri dilediğim zaman bir butona basarak çalıştırabilir miyim?
- Temel bir birim testi bir kaç dakika içinde yazabilir miyim?

Eğer yukarıdaki sorulardan herhangi birine verdiğiniz cevap **Hayır** ise; yazdığınız şeyin bir test olduğu gerçeğine karşın, kesinlikle birim test değildir. 

##### 4.2. Integration Tests

Aracınız bozulduğunda hangi parçanın bozulduğunu nasıl anlayabilirsiniz? Motor bir çok farklı parçanın birleşiminden oluşmaktadır ve tüm parçalar bir arada, uyum içinde çalışırlar. Hata bunlardan herhangi birinde ya da birden fazlasından meydana gelmiş olabilir. Tüm parçalar bir araya gelerek asıl sonucu meydana getirir: arabanın yürümesi. Arabanın yürümesini entegrasyon testi olarak düşünebilirsiniz. Eğer test hatalıysa, parçaların birlikteliği hatalı demektir. Testler sorunsuz bir şekilde çalıştıysa, tüm parçalar sorunsuz çalışıyor demektir.

Yazılımlarda da buna benzer süreç işlemektedir. Kullanıcı arayüzünü kullanarak yaptığınız testler entegrasyon testleridir. Bir butona tıkladığınızda beklediğiniz sonucu alamıyorsanız, yazılımı oluşturan tüm parçalar (bir takım olarak) hatalıdırlar. Buradaki sorun tam olarak hangi parçanın hatalı olduğunun bulunmasının zor olmasıdır. Çünkü bir butona bastığınızda hataya yol açacak onlarca, hatta yüzlerde parça olabilir.

Bu nedenle entegrasyon testi demek: iki ya da daha fazla bağımlı modülün bir grup olarak çalışmasının test edilmesi demekdir.Kısaca entegrasyon testi; birim testlerle test edilebilen kodların bir arada çalışmasını kontrol eder. Birim testler ise sadece ilgili biriminin diğer birimlerden bağımsız olarak çalışmasını kontrol eder.

> Yukarıdaki üç paragraf ***The Art of Unit Testing: with examples in C#*** ismli kitaptan, bazı bölümler çıkartılarak ve bazı bölümler yeniden düzenlenerek alınmıştır.

Örnek olarak birim testlerini yazdığınız bir kod parçasının, ürettiği veriyi bir noktada diske yazdığını düşünelim. Birim test yazdığınızda, dosya sisteminden bağımsız olarak bu metodun veriyi doğru bir şekilde ürettiğini ve verileri dosyaya yazmaya çalıştığını test edersiniz. Ancak dosya izinleriniz doğru bir şekilde yapılandırılmamış olabilir. Bu noktada birim testleriniz doğru bir şekilde çalışsa da, gerçek ortamda istenilen işi yapamayabilir. Entegrasyon testleri aracılığı ile gerçek ortama en yakın ortamı oluşturarak, bir bütün olarak metodu test edebilirsiniz. Bu nedenle birden fazla bölümün ortaklaşa çalışmasını test etme olanağını birim testte değil, entegrasyon testinde bulabilirsiniz. 

Birim testlerde hatayı daha rahat bulabilirsiniz. Ancak entegrasyon testler daha fazla kodu test ettiği için daha geneldir. Bu nedenle sürdürülebilirliği ve çalıştırılması birim testlere göre daha zordur. Çünkü test ortamının gerçek ortama yakın bir şekilde tasarlanması zaman almaktadır. 


##### 4.3. Functional Tests

Fonksiyonel testler, belirli bir veri grubunun sonuçlarını, bir sonuç kümesiyle karşılaştırarak kontrol eder. Fonksiyonel testler ara sonuçlar ya da yan etkiler ile ilgilenmezler. Sadece sonuç odaklı çalışırlar. X işlemi yapıldıktan sonra, sonucun ne olduğu ile ilgilenmezler. [5]

Yukarıdaki paragraf biraz karışık gelebilir. Bu nedenle biraz örneklemek yerinde olacaktır. Örneğin bir oturum açma işleminin testini yazdığımızı düşünelim. Oturum aç butonuna tıkladığımızda, sadece bizi anasayfaya yönlendirmesini test ediyorsak ve girdiğimiz kullanıcı bilgilerinin doğru olup olmadığının önemli olmasını istemiyorsak bu bir fonksiyonel testtir. Eğer bu bilgilerin de doğru olup olmadığının kontrolünü istiyorsak, o halde bu bir entegrasyon testidir. 

Birbirine çok yakın olarak gözüken bu iki test türünün temel farkı; birinde (fonskyionel test) yan etkiler (veritabanında kayıt oluşup oluşmaması gibi) önemsizdir. Diğerinde (entegrasyon testi) ise bu yan etkiler önemlidir. Çünkü sistemin bir bütün olarak çalışıp çalışmadığı kontrol edilmektedir. Genellikle bir resme tıkladığınız zaman, resmin bir modal üzerinde açılması, profil linkine tıkladığınızda sayfanın değişmesi gibi testler fonsksiyonel testler olarak isimlendirilirler.

##### 4.4. Acceptance Tests

Acceptance Tests ya da Türkçe karşılığıyla **Kabul Testleri**, son kullanıcı senaryolarının testlerini ifade etmektedir. Özünde yine entegrasyon testleri olarak kabul edilebilse de, bir kullanıcı hikayesinin test edilmesini ifade etmektedir. Örneğin kod yardımıyla yine tarayıcı açılarak bir kullanıcı hikayesi, örneğin yeni bir kullanıcı oluşturularak, kullanıcıya yetkiler atanması işlemi test edilir. Test senaryoları İngilizce'ya yakın bir dilde kodlanır ve kabul testi yazmak için geliştirilmiş kendine özgü araçları vardır. 

Kabul testlerindeki amaç; yazılımın sunulacağı kullanıcı grubunun yazılımı çalışıyor olarak kabul edebilmesi için gerekli olan işlemlerin gerçekleştirilebilip gerçekleştirilemeyeceğinin kontrolüdür. Sistemin neresinde hatalar olduğunun tespitinden ziyade, ***"Sistem kendisinden beklenilen işlemleri yapamıyor."*** demek için kullanılmaktadır.

### 5. Hangi Test Türünü Seçmeliyim?

- Eğer amacınız geliştirme anında hızlı bir şekilde hata bulmak ise; **unit test**.
- Eğer amacınız sistemi bir bütün (veritabanı ya da dosya sistemi, yani yazılımın çalıştığı ortam ile birlikte) olarak test etmek ise; **integration test**.
- Eğer amacınız ortamdan ve yan etkilerden bağımsız olararak sadece bir fonksiyonun test edilmesi ise; **functional test**.
- Eğer amacınız yazılımdan beklenen işlemlerin yapılıp yapılamadığını kontrol etmek ise; **acceptance test**.

Örneklerle açıklamak gerekirse;

- Kullanıcının kaç gündür kayıtlı olduğunu hesaplayan fonksiyonun testi; **birim test**.
- Kullanıcının veri tabanına kaydedilebilmesinin testi; **entegrasyon testi**.
- Bir resme tıklandığında modal açılması; **fonksiyonel test**.
- Yeni bir kullanıcı kaydedilmesinin arayüz üzerinden testi; **kabul testi**.

### 6. Sonuç

Herkes yazılım üretebilir ve üretilen her yazılımda hatalar her ne kadar dikkat edilse de olacaktır. Hataları en aza indirmek ve sürdürülebilir yazılımlar üretmek için testlere ihtiyaç duyulmaktadır. Hangi testleri hangi durumda geliştirmek gerektiği konusuna bu makale üzerinde -elimden geldiğince- değinmeye çalıştım. Umarım test türleri hakkında fikir sahibi olmanız açısından yardımcı olabilmiştir.

Eğer yazıyı beğendiyseniz ve yararlı olduğunu düşünüyorsanız ***paylaşarak*** bu yazının daha fazla kişiye ulaşmasını sağlayabilirsiniz. Eksik gördüğünüz yerler var ise değerli ***yorumlarınızla*** katkıda bulunabilirsiniz. 

### 7. Kaynakça

- [1] [Software Testing](https://en.wikipedia.org/wiki/Software_testing)
- [2] [The Art of Unit Testing: with examples in C#](https://www.amazon.com/Art-Unit-Testing-examples/dp/1617290890)
- [3] [Coupling](https://en.wikipedia.org/wiki/Coupling_(computer_programming))
- [4] [Mock Object](https://en.wikipedia.org/wiki/Mock_object)
- [5] [Stackoverflow - What is the difference between integration testing and functional testing?](http://stackoverflow.com/a/17276889)