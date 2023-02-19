---
layout: post
title: "Test Yolculuğu: PHP, PHPUnit, Mockery"
date: 2016-05-25 13:30
categories: [Turkish, Coding]
keywords: php, phpunit, mockert, testing, test yazımı
author: Özgür Adem Işıklı
post_img: coding.jpg
post_img_link: https://pixabay.com/en/rocket-launch-rocket-take-off-nasa-67643
lang: tr
description: PHP projelerinde test yazmak ve bir test yapısı kurmak hakkında bir makale.
---

Yazılım bloglarında tonla **_"Neden Test Yazmalıyız?"_** başlıklı makaleler bulabilirsiniz. Bu nedenle bu soruyu cevaplamayacağım. Eğer burayı okuyorsanız **test** kavramı hakkında en azından bir kaç şey duymuşsunuzdur diye düşünüyorum.

Ben bu yazıda bir çok farklı test türü olmasına rağmen **PHP için Unit Test** geliştirme konusunu irdelemek istiyorum. Ancak bunu yaparken herhangi bir Framework'e bağlı kalmadan, işin mantığına ve neyi neden yaptığımıza da **_ince ince_** değinmeği uygun buluyorum. Dolayısıyla bu makale sizin için sıkıcı geldiği an okumayı bırakmanızı **_öneririm_**.

### Nasıl?

Unit Test yazmak için [PHPUnit](https://phpunit.de/manual/current/en/installation.html) ve [Mockery](https://github.com/padraic/mockery) kütüphanelerinden faydalanıyoruz.

- `PHPUnit`: Unit test yazmak için özel olarak geliştirilmiş bir kütüphane. Profesyonel PHP projelerinin vazgeçilmezi.
- `Mockery`: Sınıfları taklit etmemize yarayan kütüphane, kalpazan (benim tabirim).

### Motivasyon

Test yazacak motivasyonu kendinizde bulamıyorsanız **_Adolf Hitler_**'in şu meşhur sözünü hatırlayın;

> Bir gün yazmadığım her Unit Test için bana küfür edeceksiniz.

### Test Driven Development

**_"Kodu yazmadan önce testini yaz"_** dediğimiz bu yöntemde, asıl zor olan **_ne yazacağımızı_** bilmemektir. Bu nedenle önce ne yapacağımızı anlamaya çalışmak zorundayız. Bu yüzden ben test yazımından önce, doküman yazımını salık veririm.

> Bu konu hakkında daha önce yazılmış olan makaleleri inceleyebilirsiniz: [README Driven Development - Fatih Kadir Akın](https://medium.com/@fkadev/readme-driven-development-6b2082b493b7#.i5rsbp1xg), [Doküman Tabanlı Geliştirme - Özgür Adem Işıklı](http://ozguradem.net/di%C4%9Fer/2014/07/18/dokuman-tabanli-gelistirme/)

### Readme First!

Örnek olarak bir **UserRepository** sınıfı -biraz basit tutmak istiyorum- yazacağız. Bu sınıfın dokümantasyonunu yazarken tek yampanız gereken: **_en kolay kullanım formunu yazmak_** olmalıdır. Son derece basit değil mi?

<pre><code class="language-php">
# Doküman Örneği

$userRepo = new UserRepository();
$user = $userRepo->create('foo@bar.com', 'Foo Bar');
echo $user->email;  // foo@bar.com
echo $user->name;   // Foo Bar
</code></pre>

### Adım Adım Test Yapısı Oluşturma

- Yukarıdaki dokümanımızı ana dizine "Readme.md" olarak kaydedin. ([Md Uzantısı Nedir?](https://en.wikipedia.org/wiki/Markdown))
- Yeni bir composer yapılandırma dosyası oluşturun: `$ composer init` ([Bu nedir?](http://www.cangelis.com/php-composer-nedir-nasil-kullanilir/))
- **PHPUnit**'i **global** olarak kurun: `composer global require phpunit/phpunit`
- **PHPUnit**'i projeye ekleyin: `composer require --dev phpunit/phpunit`
- **Mockery** kütüphanesini projeye ekleyin: `composer require --dev mockery/mockery`
- Ana dizinde `src` ve `tests` klasörlerinizi oluşturun.
- PHPUnit için ana dizinde `phpunit.xml` isimli bir yapılandırma dosyası oluşturun ve aşağıdaki içeriği kaydedin;

<pre><code class="language-xml">
&lt;?xml version="1.0" encoding="UTF-8"?>
&lt;phpunit backupGlobals="false"
         backupStaticAttributes="false"
         bootstrap="vendor/autoload.php"
         colors="true"
         convertErrorsToExceptions="true"
         convertNoticesToExceptions="true"
         convertWarningsToExceptions="true"
         processIsolation="false"
         stopOnFailure="true"
         syntaxCheck="false">
    &lt;testsuites>
        &lt;testsuite name="Application Test Suite">
            &lt;directory>./tests/&lt;/directory>
        &lt;/testsuite>
    &lt;/testsuites>
    &lt;php>
        &lt;env name="APP_ENV" value="testing"/>
    &lt;/php>
&lt;/phpunit>
</code></pre>

### Neyi Neden Yaptık?

Asıl proje dosyalarımızı `src`, testlerimizi de `tests` klasöründe barındıracağımızı tahmin etmişsinizdir.

Oluşturduğumuz `phpunit.xml` dosyası, PHPUnit'e test yapılandırmamızın nasıl olduğu hakkında genel bilgiler vermek için kullanılmaktadır ve şart değildir. Ancak yapılandırma dosyamızın ana dizinde durması bizim için bir rahatlık olacaktır. Dikkat ederseniz son bölümde (&lt;php> etiketinin içinde) `APP_ENV` isimli bir ortam değişkeni tanımladık. Bu tarz parametreler kullanarak uygulamınızı test ortamına özel olarak çalışacak şekilde geliştirebilirsiniz.

### Çalıştırma

Şuanda herhangi bir adım atlanmadıysa konsol üzerinde aşağıdaki komutu çalıştırdığınızda phpunit sorunsuz bir şekilde -inşallah- bize yanıt verecektir.

<pre><code class="language-bash">
$ phpunit

PHPUnit 5.2.12 by Sebastian Bergmann and contributors.
Time: 147 ms, Memory: 10.50Mb
No tests executed!
</code></pre>

> Evet, henüz hiç bir testimiz olmadığından PHPUnit bize hiç bir test çalıştırılmadığını söyledi.

### Örnek Test

Şimdi test dizinimizin altına `UserRepositoryTest.php` isimli bir dosya oluşturuyor ve aşağıdaki içeriği kopyalıyoruz.

<pre><code class="language-php">
class UserRepositoryTest extends PHPUnit_Framework_TestCase {

    public function testSimple()
    {
        $this->assertTrue(true);
    }

}
</code></pre>

Tekrardan konsol üzerinden `phpunit` komutunu çalıştırdığımızda bu sefer sonuç aşağıdaki gibi olacaktır:

<pre><code class="language-bash">
PHPUnit 5.2.12 by Sebastian Bergmann and contributors.
.                                                                   1 / 1 (100%)
Time: 134 ms, Memory: 10.75Mb
OK (1 test, 1 assertion)
</code></pre>

> PHPUnit bize toplamda 1 test çalıştırdığını ve 1 **doğrulamanın** başarılı bir şekilde sonuçlandığını gösteriyor. Yeşil rengi gördünüz mü? Bu bize herşeyin yolunda olduğunu gösterir.
> Testimizi biraz incelediğimizde şunları görürüz;

- `UserRepositoryTest` isimli sınıf bizim test sınıfımızı temsil ediyor.
- Tüm test sınıflarımızı `PHPUnit_Framework_TestCase` sınıfından genişletiyoruz. (Bu sınıfı bize PHPUnit sağlıyor)
- Sınıfın içerindeki metotlara ayrı ayrı testler yazabiliyoruz.
- Test metodu olan metotların isimlerini `test` ile başlatıyoruz. Aksi halde PHPUnit o metotları görmezden gelecektir.
- PHPUnit'in bize sağladığı **doğrulama** metotlarını kullanarak (`assertTrue()`) kontrollerimizi gerçekleştirebiliyoruz. Bu doğrulama metotlarının tamamı [PHPUnit Dokümanı](https://phpunit.de/manual/current/en/appendixes.assertions.html) üzerinde yer almaktadır.

### Gerçek Testimizi Yazalım

Test sınıfımızın sorunsuz olarak çalıştığını gördüğümüzde gerçek test kodlarını yazıyoruz. Bunu yaparken aslında daha önceden yazdığımız dokümana sadık kalıyoruz;

<pre><code class="language-php">
public function testSimple()
{
    $userRepo = new UserRepository();
    $user = $userRepo->create('foo@bar.com', 'Foo Bar');
    $this->assertEquals($user->email, 'foo@bar.com');
    $this->assertEquals($user->name, 'Foo Bar');
}
</code></pre>

Dikkat ederseniz açıklama satırı olarak **_"Burada bu değeri görebilmem gerekiyor."_** dediğim bölümler benim asıl test edeceğim unsurları içeriyor. Bu testi yaparken PHPUnit'in bize sağladığı `assertEquals` metodunu kullanıyoruz ve beklediğimiz ve dönen değeri karşılaştırıyoruz.

Yine konsol üzerinden `phpunit` komutunu çalıştırdığımızda `FatalError` ile karşılaştığımızı göreceksiniz.

<pre><code class="language-bash">
PHPUnit 5.2.12 by Sebastian Bergmann and contributors.

PHP Fatal error:  Class 'UserRepository' not found in /home/makale/tests/UserRepositoryTest.php on line 7
Fatal error: Class 'UserRepository' not found in /home/makale/tests/UserRepositoryTest.php on line 7
</code></pre>

Hemen bu sorunu aşmak için `src` klasörü altına `UserRepository.php` isimli dosyayı aşağıdaki gibi oluşturuyoruz.

<pre><code class="language-php">
class UserRepository {
}
</code></pre>

Ancak `phpunit` komutunu çalıştırdığınız zaman yine aynı hatanın oluştuğunu göreceksiniz. Buradaki sorun ilgili sınıfın test bölümünden ulaşılabilecek şekilde `include` işlemine tabi tutulmamasıdır. Ancak biz `include` işlemini profesyoneller gibi composer tarafından bize sağlanan **autoload** bölümüne yaptıracağız. ([Autoload Nedir?](https://getcomposer.org/doc/04-schema.md#autoload))

### Autoload İşlemi Tanımlaması

Bu işlem için ana dizinde bulunan `composer.json` dosyasına aşağıdaki eklemeyi yapıyoruz;

<pre><code class="language-js">
"autoload": {
    "classmap": ["src"]
}
</code></pre>

Bu eklemeden sonra konsol üzerinde `composer dump-autoload` komutunu uygulayarak otomatik olarak yüklenecek sınıfların yeniden belirlenmesini istiyoruz.

> Autoload işleminde ben kolay olması açısından tüm `src` klasörünün taranmasını istedim. Ancak bu pek doğru bir yaklaşım değil. Bu tarz otomatik yüklemeler için **Namespace** kullanmanızı öneririm.

### Test Yazmaya Devam

Bu işlemden sonra `phpunit` komutunu çalıştırdığınızda ilgili fonksiyonun `UserRepository` içerisinde olmadığıyla ilgili hatayı göreceksiniz. Bu nedenle aşağıdaki eklemeyi yapıyoruz.

<pre><code class="language-php">
public function create($email, $name)
{
}
</code></pre>

`phpunit` komutumuzu çalıştırmamız sonrasında aşağıdaki gibi **kırmızı** bir ekranla karşılaşırız ve beklenen ve alınan değerlerin uyuşmadığını PHPUnit bize söyler.

<pre><code class="language-bash">
PHPUnit 5.2.12 by Sebastian Bergmann and contributors.

F                                                                   1 / 1 (100%)
Time: 165 ms, Memory: 10.75Mb

There was 1 failure:
1) UserRepositoryTest::testSimple
Failed asserting that 1 matches expected null.
/home/ubuntu/workspace/makale/tests/UserRepositoryTest.php:9

FAILURES!

Tests: 1, Assertions: 1, Failures: 1.
ozziest:~/workspace/ma
</code></pre>

### Gerçek Dünyaya Dönmek

Testimizi yeniden yeşile çevirmeden önce gerçek dünyaya dönelim. Test yazmak bu kadar basit değil. Örneğimizde veritabanı işlemi için `Eloquent` isimli bir başka kütüphane (Laravel kullananlar bilir) kullandığımızı düşünelim. `Eloquent`'ın bize sağladığı modeli kullanarak ilgili kaydı yaptımızı varsayalım.

> Neden böyle bir varsayım yapıyorum? Çünkü her (fucking) test yazma örneğinde size başka bir yerle bağlantısı olmayan bir sınıf anlatılır. Siz de bağımsız sınıfla olayı anladığınızı düşünürsünüz. Ancak kendi projenizin başına geçtiğinizde kod size, siz koda bakarsınız. Oysaki test yazabilmek için **_bağımsızlığını ilan etmiş sınıflara_** sahip olmanız bir **zorunluluktur.**

Bu nedenle `UserRepository.php` içerisindeki kodumuzu `Eloquent`'ın bize gösterdiği şekliyle `User` modeli üzerinden kaydedecekmişiz gibi tasarlayalım.

<pre><code class="language-php">
use App\Models\User;

class UserRepository {
    
    public function create($email, $name)
    {
        $user = new User();
        $user->email = $email;
        $user->name  = $name;
        $user->save();
        
        return $user;
    }
    
}
</code></pre>

`UserRepository` sınıfımız burada adı geçen `App\Models\User` isimli sınıfa **sıkı sıkıya** bağlı oldu. Olayı bu noktaya getirdikten sonra hep birlikte bir kez daha tekrarlayalım:

> **Sıkı sıkıya başka sınıflara bağlı olan sınıflar için unit test yazamazsınız!**

Yazsanız bile kırk takla atmak zorunda kalırsınız. Bizim buradaki amacımız `User` sınıfını değil, `UserRepository` sınıfını test etmek. Aksi halde sadece bir **_unit_**'i, **_yazılımın basit bir parçasını_** test etmiş olmayız. Yazdığımız şey de **Unit Test** olmaz. Dolayısıyla bu bağımlılıktan kurulmamız gerekiyor.

> Bu konu ayrıca [SOLID Prensipleri](http://tarikkaygusuz.com/post/solid-prensipleri)'yle de doğrudan doğruya alakalı olan bir konudur. SOLID prensipleri hakkında öğreneceğiniz her şey test yazımında size yardımcı olacaktır.

### Dependency Injection

Bu gibi durumlar için ortaya atılan bir terimdir **dependency injection**. Yani ilgili bağımlılıkların dışarıdan gönderilmesi işidir. Böylece asıl sınıf, çalışacağı sınıfı dışarıdan alır. Peki ama nasıl?

<pre><code class="language-php">
use Illuminate\Database\Eloquent\Model;

class UserRepository {
    
    private $model;
    
    public function __construct(Model $model)
    {
        $this->model = $model;
    }
    
    public function create($email, $name)
    {
        $this->model->email = $email;
        $this->model->name  = $name;
        $this->model->save();
        return $this->model;
    }
    
}
</code></pre>

Bağımlılıkları dışarıdan almanın en iyi yolu yapıcı metodu (`construct()`) kullanmaktır. Böylece `UserRepository` sınıfının ilgili model sınıfı ile ilgili bağlantısını kesmiş oluruz. Eğer dikkat ederseniz dışarıdan aldığımız sınıf için `Illuminate\Database\Eloquent\Model` sıfını dayatırız (bkz: [Type Hinting](https://www.sitepoint.com/type-hinting-in-php/)).

> Bu noktada kafa karışıklığı olabilir. Sınıf bağımlı olmasın dedik ve bağımlılığı dışarıdan aldık ama yine de bir tip dayatma işlemi gerçekleştirdik. Böyle yapmamızın nedeni ufak bir kalpazanlıkla harika şeyler yapacak oluşumuz. Biraz daha sabırla okumaya devam edin.

### Mockery İle Kalpazanlık

Konsol üzerinden `phpunit` komutunu çalıştırdığınızda aşağıdaki hatayı göreceksiniz;

<pre><code class="language-bash">
There was 1 error:

1) UserRepositoryTest::testSimple
Argument 1 passed to UserRepository::__construct() must be an instance of Illuminate\Database\Eloquent\Model, none given, called in /home/ubuntu/workspace/makale/tests/UserRepositoryTest.php on line 7 and defined
</code></pre>

Burada yapmak zorunda olduğumuz; testimizde `UserRepository` sınıfından yeni bir `instance` alırken bizden talep edilen bağımlılığı dışarıdan göndermemiz. Ama amacımız sadece ve sadece `UserRepository` sınıfını test etmek olduğundan, gerçekte var olan `App\Models\User` sınıfını **gönderemeyiz**. Bu sınıfı **taklit** eden bir başka sınıf kullanmamız gerekir. Bu iş için `Mockery` kütüphanesinden yararlanırız;

<pre><code class="language-php">
$userMock = Mockery::mock('Illuminate\Database\Eloquent\Model');
$userRepo = new UserRepository($userMock);
</code></pre>

Yukarıdaki kodun ilk satırında verdiğimiz namespace türünde bir taklit sınıfı oluşturulur. Bu işlemi bu kadar kolay yapmamızı sağlayan `Mockery` kütüphanesidir. Biz de oluşturduğumuz bu yalancı sınıfı `UserReposiytory` sınıfına göndeririz. Tekrar konsol üzerinde `phpunit` komutunu çalıştırdığınızda bu sefer çıkacak olan hata şudur;

<pre><code class="language-bash">
There was 1 error:
1) UserRepositoryTest::testSimple
BadMethodCallException: Method Mockery_0__Illuminate_Database_Eloquent_Model::save() does not exist on this mock object
</code></pre>

Hatayı incelediğimizde taklit sınıfımızın `save` isimli bir metoda sahip olmadığını görürüz. Özetle: `UserRepository` sınıfımız gönderdiğimiz yalancı sınıfı bir güzel yedi. Hatta şuan ortada gerçekten (elle tutulur, somut) bir `User` sınıfı dahi yok. :)

Bundan sonrası `Mockery`'nin bize sağladığı güzellikleri kullanma;

<pre><code class="language-php">
public function testSimple()
{
    $userMock = Mockery::mock('Illuminate\Database\Eloquent\Model')
                       ->shouldReceive('save')->times(1)
                       ->mock();
    
    $userRepo = new UserRepository($userMock);
    $user = $userRepo->create('foo@bar.com', 'Foo Bar');
    $this->assertEquals($user->email, 'foo@bar.com');
    $this->assertEquals($user->name, 'Foo Bar');
}
</code></pre>

Taklit sınıfını oluşturduğumuz bölüme bakarsanız, `shouldReceive` metodu ile taklit sınıfımızda `save` isimli bir metodun olması gerektiğini ve bu metodun da **1 defa** çağırılması gerektiğini belirtiyoruz. Taklit sınıfınıza dilediğiniz kadar metot ekleyebilirsiniz. `Mockery` bu konuda oldukça yardımcı oluyor ve ihtiyacınız her şey [Mockery Dokümanı](http://docs.mockery.io/en/latest/) üzerinde detaylıca anlatılmış durumda.

Son bir kez `phpunit` komutunu çalıştırdığınızda aşağıdaki güzeller güzeli ekranla karşılaşırsınız;

<pre><code class="language-bash">
PHPUnit 5.2.12 by Sebastian Bergmann and contributors.

.                                                                   1 / 1 (100%)

Time: 133 ms, Memory: 11.50Mb
OK (1 test, 2 assertions)
</code></pre>

Bu noktada eğer dilerseniz `UserRepository` sıfındaki `create` metodunun içeriğini temizleyerek ya da bozarak, testlerin düzgün çalışıp çalışmadığını dahi test edebilirsiniz.

### Lafı Çok Mu Uzatıyoruz?

İlk başta olay böyle gelebilir, kabul. Bunun en önemli sebebi örnek olarak **_basit_** bir şey seçme zaruretimizin oluşunu söyleyebiliriz. Ancak gerçek hayatta sınıflar daha komplikedir. Tam da bu noktada **Laravel**'in kendi testlerinden bir bölüm incelemek size biraz ipucu verebilir. ([bkz](https://github.com/laravel/framework/blob/5.2/tests/Auth/AuthGuardTest.php))

### İşleri Otomatize Etmek

Ben işin en çok bu kısmından zevk alıyorum. `Laravel` gibi projelerin GitHub sayfalarında yer alan yeşil renkli kutucuklar hiç dikkatinizi çekti mi? [[build-passing]](https://github.com/laravel/laravel)
Projede yapılan her değişiklikten sonra tüm testleri elle çalıştırmak zahmetli olabileceğinden, yapılan her değişiklik (commit) sonrası testlerinizi bir çok farklı ortamda (PHP 5.5, PHP 5.6, PHP7, PHP:hhvm) sizin için çalıştıran araçlar vardır. Bunların en meşhurlarından biri [Travis-CI](https://travis-ci.org). Bu işleri otomatize etmenin terminolojideki genel adı da **Continuous Integration**'dır.

`Travis` sadece private repolar için ücretlidir. Public olan repolarda sınırsızca kullanabilirsiniz. Tek yapmanız gereken projenize bir `travis.yml` dosyası dahil ederek Travis-GitHub entegrasyonunu yapmak.

> Bu bölüm ayrı bir blog yazısı olduğu ve ben zaten hali hazırda bu yazıyı yeterince uzattığım için bu bölüme değinmiyorum. Ancak dileyenler Laravel'in testleri ne durumda inceleyebilirler. ([bkz](https://travis-ci.org/laravel/framework))

### Sonuç

- Proje büyüdükçe ve bir çok farklı yapıdan oluştukça test yazmak zaruri bir hal alır.
- Bir çok farklı test türü vardır: Unit, Acceptance, Integration vb.
- Test türleri için farklı farklı araçlar vardır: [PHPUnit](http://phpunit.de), [Mockery](http:/docs.mockery.io), [CodeCeption](http://codeception.com/), [Selenium](http://www.seleniumhq.org/), [PHPSpec](http://www.phpspec.net/en/latest/)
- Unit Test yazabilmek için sınıflarımızın **Unit** olması gerekmektedir. Gereksiz bağımlılıklar temizlenmeli/dışarıdan dahil edilmelidir.
- SOLID prensipleri test yazma sürecinin olmazsa olmazıdır ve öğrenilmesi elzemdir.
- Taklit sınıflar işin önemli bir kısmını oluştururlar.
- Continuous Integration önemlidir ve her fırsat bulunduğu an uygulanmalıdır.

### İleri Okumalar

- [Why is Software Testing Important to a business?](http://www.softwaretesting.com.au/Why_is_Software_Testing_important.php)
- [Types of software Testing](http://www.softwaretestinghelp.com/types-of-software-testing/)
- [Unit Testing Tutorial Part I: Introduction to PHPUnit](https://jtreminio.com/2013/03/unit-testing-tutorial-introduction-to-phpunit/)
- [S.O.L.I.D: The First 5 Principles of Object Oriented Design](https://scotch.io/bar-talk/s-o-l-i-d-the-first-five-principles-of-object-oriented-design)
- [Wiki: Inversion Of Control](https://en.wikipedia.org/wiki/Inversion_of_control)
- [Book: PHPUnit Essentials](http://shop.oreilly.com/product/9781783283439.do)
