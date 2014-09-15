---
layout: post
title:  "Test Yazmak ile  Başım Dertte!"
date:   2014-09-15 22:39:28
categories: php test 
---

### Önizleme 

Anlatmak istediklerimi anlatmadan önce, lütfen aşağıdaki kod bloğunu satır satır inceleyiniz;

{% highlight php %}
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
{% endhighlight %}

Bu kod bloğu benim gördüğüm en basit, en sade ve en anlaşılır test bloğu. Böyle yazılmış test satırlarını gördüğünde içimi büyük bir test yazma heyecanı kaplıyor. Satırı satırına ne yapılmak istendiğini sizin de anladığınızı düşünüyorum. Bu olukça basit bir kabul (Acceptance) testi. 

### Kimin İçin Test?

Biz yazılım geliştiriciler CV'lerimize değer katmak (yani daha çok para kazanmak) için her türlü bilgiyi öğrenmeye çalışıyoruz. Testlerde bunlardan biri. Çoğumuz sıkılarak ve gereksiz bularak testleri geçiştiriyoruz. Ya zamanımız olmuyor, ya müşteri bizden iş bekliyor. Testi biraz da işin egosuna kaçarak yazıyor, sağda solda anlatıyoruz. (Bunları öncelikle kendime bir özeleştiri olarak söylüyorum.)

Ama bu aşamada sesli düşünmek zorundayım. **DRY-Don't Repeat Yourself** felsefesi gayet açıktır; kendimizi tekrar etmemeliyiz. Bunun için yaptığımız işlemin doğruluğunu kontrol etmek için tarayıcıyı açıp adımları tek tek kendimiz yampak yerine, bunları yukarıdaki gibi kodlamalı ve bilgisayara yaptıracağımız her işi kullanışlı hale getirmeliyiz. Testlerin var olma amacı bu kadar basittir.

### Birim Test Sorunsalı

Benim için hala birim testler bir sorun ve hala istediğim noktada değilim. Bu konuda zamanımın çoğunu tanınmış projedeler için yazılmış testleri inceleyerek geçiriyorum. Ama iş kendi testlerimi yazmaya geldiğinde, nedense işler istediğim gibi gitmiyor. Yüzlerce, binlerce satır kod yazmak hiç yormuyor ama iki satır test kodu yazmak beni öldürüyor. Tam başlayacağım bir çay alıyorum. Dışarıyı seyrediyorum. Kendime bulabildiğim kadar başka iş buluyorum.  

Bu gidişe bir dur demek amacıyla; hergün **kod kataları** yazma kararı almış bulunuyorum.* Çok basit bir senaryom var. Her oluşturacağım sınıftan önce, o sınıfın test bölümünü oluşturmak. Hedefim içlerini doldurmak değil, sadece test sınıfını oluşturup, testleri isimlendirmek. Belirli bir süre bunu tekrar ettikten sonra, en azından başlangıç aşamasındaki üşengeçliğimi yenebilmeyi umuyorum. Böylece beynimin içinde *"Bırak testi, kodlamaya başla."* diye bağıran **cahili** bir nebze yumuşatabilirim.

### Test Araçları 

Size araçların özelliklerini tek tek anlatmayacağım. **Laracast** üzerindeki test videolarının büyük bir çoğunluğunu izledim ve uyguladım diyebilirim. Ama en çok hoşuma giden **Codeception** oldu. Yukarıda örnek verdiğim kabul testi de, kendi örneklerinden bir tanesi. Lütfen siz de inceleyin. Bağlantı [burada!](http://codeception.com/)

### Gelişme Durumum

Bu belgeyi elimden geldiğince güncelleyecek ve test yazmaya çalışma *test yazma serüvenimi* sizlere aktaracağım. 

###### Her Sınıftan Önce Test Sınıfı

* Amaç: Bir sınıf oluşturmadan önce birim testi yapacak olan sınıfı oluşturacağım. Testleri isimlendireceğim. İçlerini doldurmak gibi bir zorunluluğum yok.

* Sonuç: Pek yakında.  

[Kata](http://tr.wikipedia.org/wiki/Kata)









