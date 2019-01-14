---
layout: post
title:  "Test Yazmak ile  Başım Dertte!"
date:   2014-09-15 22:39:28
categories: [Turkish, Coding]
tags: php, test, unit test, codeception, acceptance
meta: php, test, unit test, codeception, acceptance
author: ozziest
---

Anlatmak istediklerimi anlatmadan önce, lütfen aşağıdaki kod bloğunu satır satır inceleyiniz;

<pre><code class="language-php">
$I = new AcceptanceTester($scenario);
$I->wantTo('create wiki page');
$I->amOnPage('/');
$I->click('Pages');
$I->click('New');
$I->see('New Page');
$I->fillField('title', 'Hobbit');
$I->fillField('body', 'By Peter Jackson');
$I->click('Save');
$I->see('page created'); // notice generated
$I->see('Hobbit','h1'); // head of page of is our title
$I->seeInCurrentUrl('pages/hobbit'); 
$I->seeInDatabase('pages', array('title' => 'Hobbit'));
</code></pre>

Bu kod bloğu benim gördüğüm en basit, en sade ve en anlaşılır test bloğu. Böyle yazılmış test satırlarını gördüğünde içimi büyük bir test yazma heyecanı kaplıyor. Satırı satırına ne yapılmak istendiğini sizin de anladığınızı düşünüyorum. Bu oldukça basit bir kabul (Acceptance) testi. 

### Kimin İçin Test?

Biz yazılım geliştiriciler CV'lerimize değer katmak (yani daha çok para kazanmak) için her türlü bilgiyi öğrenmeye çalışıyoruz. Testlerde bunlardan biri. Çoğumuz sıkılarak ve gereksiz bularak testleri geçiştiriyoruz. Ya zamanımız olmuyor, ya müşteri bizden iş bekliyor. Testi biraz da işin egosuna kaçarak yazıyor, sağda solda anlatıyoruz. (Bunları öncelikle kendime bir özeleştiri olarak söylüyorum.)

Ama bu aşamada sesli düşünmek zorundayım. **DRY-Don't Repeat Yourself** felsefesi gayet açıktır; kendimizi tekrar etmemeliyiz. Bunun için yaptığımız işlemin doğruluğunu kontrol etmek için tarayıcıyı açıp adımları tek tek kendimiz yapmak yerine, bunları yukarıdaki gibi kodlamalı ve bilgisayara yaptıracağımız her işi kullanışlı hale getirmeliyiz. Testlerin var olma amacı bu kadar basittir.

### Birim Test Sorunsalı

Benim için birim testler bir sorun ve hala istediğim noktada değilim. Bu konuda zamanımın çoğunu tanınmış projeler için yazılmış testleri inceleyerek geçiriyorum. Ama iş kendi testlerimi yazmaya geldiğinde, nedense işler istediğim gibi gitmiyor. Yüzlerce, binlerce satır kod yazmak hiç yormuyor ama iki satır test kodu yazmak beni öldürüyor. Tam başlayacağım bir çay alıyorum. Dışarıyı seyrediyorum. Kendime bulabildiğim kadar başka iş buluyorum.  

Bu gidişe bir dur demek amacıyla; hergün kod [kata](http://tr.wikipedia.org/wiki/Kata)ları yazma kararı almış bulunuyorum. Çok basit bir senaryom var. Her oluşturacağım sınıftan önce, o sınıfın test bölümünü oluşturmak. Hedefim içlerini doldurmak değil, sadece test sınıfını oluşturup, testleri isimlendirmek. Belirli bir süre bunu tekrar ettikten sonra, en azından başlangıç aşamasındaki üşengeçliğimi yenebilmeyi umuyorum. Böylece beynimin içinde *"Bırak testi, kodlamaya başla."* diye bağıran **cahili** bir nebze yumuşatabilirim.

### Test Araçları 

Size araçların özelliklerini tek tek anlatmayacağım. **Laracast** üzerindeki test videolarının büyük bir çoğunluğunu izledim ve uyguladım diyebilirim. Ama en çok hoşuma giden **Codeception** oldu. Yukarıda örnek verdiğim kabul testi de, kendi örneklerinden bir tanesi. Lütfen siz de inceleyin. Bağlantı [burada!](http://codeception.com/)

### Gelişme Dönemi

Bu belgeyi elimden geldiğince güncelleyecek ve *test yazma denemelerimi* sizlere aktaracağım. 

#### 1) Her Sınıftan Önce Test Sınıfı

* `Amaç`: Bir sınıf oluşturmadan önce birim testi yapacak olan sınıfı oluşturacağım. Testleri isimlendireceğim. İçlerini doldurmak gibi bir zorunluluğum yok.

Son iki haftalık süreci kendime verdiğim sözleri tutmaya çalışarak geçirdim. Kabul etmeliyim ki oldukça zorlandım. İnsanın alışkanlıklarını bir anda terk etmesi tahmin ettiğimizden daha güç oluyor. Amatörce gitar çaldığım dönemlerde yanlış öğrendiğim bir tekniğin, ileride nasıl gelişmeme engel olduğunu görmüştüm. Yazılım geliştirme için de aynı geçerli. Doğrudan kod yazmayı öğrendiğimiz için test sürecine geçmek kolay olmuyor. 

İlk günlerde planıma sadık kalamadım. Sonraları bloga yazmam gerektiğinü düşününce, sadece sınıfları oluşturdum ve test metotlarını görmezden geldim. Sınıfları oluşturmak biraz alışkanlık aldığındaysa içi boş test metotları oluşturdum. Ama bu işlemi tüm test türleri için yapmadım. Sadece repository sınıflarını hedef aldım. 

Şuan itibari ile aldığım karara uymayı başardım ve oluşturduğum her repository için bir test sınıfı ve içi boş test metotlarım var. Tabii burada kendime yeni bir hedef koyarak her test sınıfı için en önemli gördüğüm bir test metodunu yazmaya çalışacağım.  

#### 2) Her Test Sınıfı İçin Bir Tane Test Hedefi

* `Amaç`: Her sınıf için bir test sınıfı ve içi boş test metotlarının yanı sıra, ilgili sınıfta bir tane testin yazılmasını hedefliyorum.  

> Bu amacı yazdıktan sonra araya giren başka projeler nedeniyle epey bir 
> süre blogumu güncelleyemedim. Tekrardan işlerin rayına oturmasıyla birlikte 
> belirlediğim 2 haftalık süre sonundaki gelişmeleri aktarıyorum.

İlk amaca uygun olarak, her sınıftan önce oluşturduğum boş test sınıfları için en az bir tane test metodu ilkemi gerçekleştirmeye koyuldum. Programlamaya test yazarak başlamadığımız için karşıma çıkan sorun; testi yazarken kafamın karışmasıydı. Eğer ortada bir çalışan bir metot varsa, onu nasıl test edeceğimi kestirebiliyordum. Ancak ortada hiç birşey yokken bu durumu aşmak oldukça zordu.

Bu zorluk tahmin ettiğimden daha uzunca bir süre sürdü. Öyleki; önce testi yazdıktan sonra asıl kodu yazmaya başladığımda, test yazarken düşünmediğim bir çok detay ortaya çıkıyordu. Bunun nedeni her zaman kodu önce yazmamdan kaynaklanıyordu. 

Yine bu süreçte dikkatimi çeken bir başka durum; test sürekli kod yazdıktan sonra güncellendiği için, bir noktadan sonra testi tamamen bırakıp sadece asıl kodu yazmaya odaklanmamdı. Yani günün sonunda yine başa dönüyordum ve önce asıl kod, sonra test yazılmış oluyordu. Bunu kırmak için çalışma sistemimi biraz daha değiştirmeye ve daha çok asıl kod öncesi pratik yapmaya karar verdim. Bu çalışmaları da iş dışında gerçekleştireceğim. Takdir edersiniz ki her işin bir teslim süresi var ve test yazımı zaman olarak yarardan çok zarar sağlamaya başlamış durumda.

#### 3) Gün Sonunda Test Pratikleri

* `Amaç`: Gün sonunda kendi belirleyeceğim bir senaryoya göre sadece test yazmak. Test yazım anında hiç bir şekilde asıl koda girmeden, teste tekrar dönmeyecek şekilde testi yazmaya çalışmak.

Çok uzunca bir süredir bu seriye bir ara vermiştim çünkü işler istediğim gibi gitmiyordu ve ciddi aksaklıklar yaşadım. Son iki haftadır yeni yeni toparlıyorum ve bununla birlikte elde ettiğim çözümleri toplu olarak yazacağım. 

## Sonuç

Öncelike test olayını çözdüm diyebilirim. Henüz çok kapsamlı olmasa da CodeCeption'ın sağladığı avantajlar sayesinde çok hızlı, sıkılmadan ve pratik bir şekilde ürettiğimiz yazılımların testlerini geliştirebiliyorum. Özellikle refactoring işlemlerinden sonra testlerin ne kadar büyük bir nimet olduğunu anlıyorsunuz. 

Bu süreçte PHP tarafında önerilen en iyi seçeneklerin hepsini denedim diyebilirim. PHPUnit, PHPSpec ve CodeCeption bunlardan bazılarıydı. PHPSpec Birim test yazma konusunda oldukça yetenekli ancak kod geliştirirken beni yorduğunu düşünüyorum. PHPUnit diğerlerine göre daha bir çaresiz bırakıyor sizi. Çünkü test yazmayı istemek başka şey, ne yazacağınızı bilmek başka şey. Bu konuda PHPUnit'in daha çok dökümana ihtiyacı olduğunu düşünüyorum. 

CodeCeption, PHPUnit'i de içerisinde barındırdığı için doğrudan birim testlerde de kullanabiliyorsunuz. Bu mükemmel bir avantaj ve kendi içinde var olan kabul testi bölümü mükemmel bir dizayna sahip. PHP dışında geliştirdiğiniz sistemlerin testi için de kullanabileceğiniz güzel bir yapısı var. Üst düzey programlama yetenekleri gerektirmiyor ve çok hızlı bir şekilde test yazmanıza olanak sağlıyor. 

Henüz ürettiğim yazılımlar için "%100 Test" amacımın hakkını veremesem de, bu oranı %60'lara çıkarmış bulunuyorum. Ülkemizde bulunan koca koca şirketlerde bile testin adının pek anılmadığını düşündüğümüzde, bu oran bir startup şirketinde çalışan benim için hiç de fena değil. 

Önümüzdeki dönemde CodeCeption hakkında çok detaylı bir makale hazırlamak istiyorum. Umarım fırsat bulabilirim.






