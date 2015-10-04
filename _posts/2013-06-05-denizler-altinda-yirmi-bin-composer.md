---
layout: post
title:  "Denizler Altında Yirmi Bin Composer"
date:   2013-06-05 19:00
categories: php
tags: php, paket yöneticisi, composer nedir, neden kullanılır
meta: php, paket yöneticisi, composer nedir, neden kullanılır
author: ozziest
---

> Önemli Not: Bu yazı temel Composer bilgisi olanlar içindir. Eğer Composer’ı hiç duymadıysanız [buraya](https://web.archive.org/web/20140816200545/https://getcomposer.org/) ve [buraya](https://web.archive.org/web/20140906052958/http://www.cangelis.com/php-composer-nedir-nasil-kullanilir/) tıklayarak hakkında detaylı bilgi edinebilirsiniz.

Composer’la tanışmam yaklaşık 2 sene öncesinde gerçekleşti. 1 yıldır da aktif olarak Composer kullanıyorum. Ancak son dönemde farkettiğim üzere; benim Composer’ı kulanma amacım sürekli başkalarının geliştirdiği paketleri kullanmaya yönelikti. SOLID prensiplerini istisnasız geliştirdiğim her yazılımda kullanma kararımla birlikte, Composer’ın ve yazılım dünyasında paketlerin kilit rol oynadığını fark ettim.

### Nedir Bu Paket?

5 yıllık bir web developer olduğunuzu varsayalım. Bu süre içerisinde PHP ile sayısız proje geliştirdiniz. Her yaptığınız projenizde bir oturum açma/kapatma, kullanıcı kaydı ya da belki de yetkilendirme vardı. Bazı sistemlerde içerik yönetimi, bazılarında ise ortam kütüphanesi ya da galeri vardı. Ancak 5 yıl yazılım dünyası için çok uzun bir süre ve bir çok şey değişiyor. Belki ilk projenizde framework kullanmıyordunuz fakat bugüne gelene kadar bir kaç tanesini aktif olarak kullandınız. Eğer yarın başlayacağınız bir projenizde hazır bir paket kullanmayacaksanız ve basit bir oturum açma işlemini bile yeniden kodlayacaksanız gerçekten çok maceracı olmalısınız. Eğer elinizin altında oturum yönetimi için kullanılacak hazır kodlar varsa bunu başka projelerinizde de kullanmanız kadar güzel bir şey olamaz. Ancak bunun için tüm kodlarınız ortak standartlar üzerine inşaa edilmesi gerekiyor.

PHP standart oluşturulabilmesi açısından epey yol aldı. Laravel ve Symfony2 gibi harika frameworklere sahibiz. Özellikle Laravel, Symfony2′den aldığı paketlerle çok başarılı bir iş çıkardı ortaya. (Laravel ile uğraşmamış olanlar için, Laravel Symfony2 ile bazı paketleri ortak kullanmaktadır.) İşte bu noktada paketlerin önemi ortaya çıkıyor.

Şöyle düşünün, yönlendirme işlemleri her php projesinde hatta her framework’de ortak olarak olması gereken bir özellik. Peki biz neden her projemizde aynı yönlendirme paketini kullanmayalım? İşte bu noktada Laravel ve Symfony2 arasındaki uyum büyük gelecek vaad ediyor.

### Paket Tabanlı Geliştirme

Artık PHP ile bir web uygulaması geliştiriyorsanız, tekrar tekrar aynı şeyleri kodlamanıza gerek yok. Hatta bir çok şeyi kodlamanıza bile gerek yok. Şuanda GitHub üzerinde yayınlanan binlerce paket sizin kullanmanız için bekliyor. Fazladan uğraşmamanız için tek yapmanız gereken Laravel ya da Symfony2 gibi frameworkler ile birlikte çalışmanız. Gerisi sadece paket eklemek ve kullanmakla alakalı bir takım angarya işler. Şöyle düşünün; oturup sıfırdan yazdığızda 1-2 haftanızı alacak bir oturum ve yetkilendirme paketini hemen alıp 2 dakika içinde kullanmaya başlayabilirsiniz. Üstelik tüm geliştirmelerinden de yararlanabilirsiniz. Belki zamanla siz de bu paketlere katkıda bulunursunuz kim bilir?

Bu madalyonun yalnızca bir yüzü. Diğer yüzünde ise sizin yazmak zorunda kaldığınız kodlar var. Yine Laravel üzerinden gidersek, yönlendirmelerinizi yapıp, Controller, View ve Model‘lerinizi oluşturup dilediğiniz işi yerine getiren kodları getirdiğinizi varsayalım. Peki bu tür kodları da paket olarak geliştiremez misiniz? Yani demek istediğim; Evrak Takip Sistemi gibi bir bölümü modül olarak, başka projelerinizde de kullanmak üzere ve hızlı adapte edilir bir şekilde kodlayamaz mısınız? Kesinlikle kodlayabilirsiniz ve bence kodlamalısınızda!

### Nasıl Paket Geliştireceğim?

Bu bölümü Laravel üzerinden örneklendireceğim. Laravel’in dökümantasyonunda Paket Geliştirme bölümünde paket nasıl hazırlanır çok detaylı bir şekilde anlatılmakta. Yeni bir paket oluşturmak için bu dökümandan yararlanabilirsiniz.

Ancak paketinizi geliştirirken workbench klasörü içerisinde geliştirme yapmaktasınız. Bir başka projenizde bunu kullanabilmek için paketinizi yayınlamanız gerekmektedir. Burada karşınıza iki yol çıkıyor; ya GitHub/BitBucket gibi bir sitede public bir repo oluşturarak Packagist üzerinden paketinizi herkese açık olarak yayınlarsınız ya da Satis ile paketinizi gizlersiniz.

Birinci yol genel kullanım şeklini ifade etmektedir. İnternette bu konuyla ilgili oldukça fazla döküman bulabilirsiniz. Bu nedenle ayrıntılı olarak değinmeyeceğim. Bilmeniz gereken tek şey; Laravel ile bir paket oluşturduğunuz zaman, o paketi başka projelerinizde yönlendirmelerinden migrasyonlarına kadar composer yardımı ile kurup kullanabilirsiniz.

Lakin şirket içinde kalması gereken bazı paketleriniz de olabilir ve bunu kimseyle paylaşmak istemeyebilirsiniz. Bu durumda paketi gizlemek en mantıklı hareket olacaktır. Paketi gizlediğinizde ise Packagist seçeniğini kullanamazsınız. Bunun için Satis kullanmanız gerekmektedir. Satis kullanımı ile ilgili ayrıntılı döküman bu adreste mevcuttur. Satis ile kendi gizli repolarınızı composer ile kullanabilirsiniz.

### Paket Kullanmanın En Güzel Yanı

Yukarıda bahsettiğim Evrak Takip Sistemi’ni paket tabanlı olarak geliştirmediğimizi düşünelim. Daha sonra başka bir şirket projemizde aynı işleri yapan bölümler bize lazım olduğunda kopyala yapıştır ile paketi projemizde kullanabiliriz. Aradan X zaman geçer, birinci projemizde Evrak Takip Sistemi’nde bir hata tespit ederiz ve gerekli olan 3 farklı dosyadaki toplamda 20 satırı güncelleriz. Bu durumda aynı kodları kullanan diğer projenizde de o 3 dosyayı tek tek açıp kopyala/yapıştır yapmanız gerekecektir. Hadi iki farklı proje değilde, 10 farklı projede bu modülü kullandığınızı düşünelim. Size kolay gelsin. :)

Eğer aynı sistemi paket tabanlı olarak tasarlarsanız, hatayı tespit ettiğinizde günceller ve reponuza dosyaları atarsınız ve daha sonra diğer projelerde composer update komutu çalıştığı zaman otomatik olarak kodlarınız güncellenir.

Elbette her projede ortak olarak çalışabilecek bir paket tasarlamak düz kod yazmaktan daha zor. Bunun için SOLID prensiplerini çok iyi bilmek ve kod yazarken uygulayabilmek gerekiyor. Eğer bu konuda benim gibi üst düzey bir geliştirici değilseniz, yine de korkmayın. Zaman içerisinde öyle paketler geliştireceksiniz ki, bir başka yerde neden kullanılamadığını anladığınızda SOLID prensiplerini daha iyi anlayacaksınız.

Yine de bu yöntemi kullandığınızda, ilk başta kaybettiğiniz zamanı telafi edebiliyorsunuz. Kendi deneyimlerinden yola çıkararak söylemem gerekirse, bir paketi 2-3 projede kullandığınızda %50-60 zaman tasarrufu sağlamanız mümkün.

