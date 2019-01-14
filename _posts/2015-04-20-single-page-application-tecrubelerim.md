---
layout: post
title:  "Single Page Application Tecrübelerim"
date:   2015-04-20 21:30:28
categories: [Turkish, Coding]
tags: single page application, spa, web uygulaması, angular, javascript, tecrübe
meta: single page application, spa, web uygulaması, angular, javascript, tecrübe
author: ozziest
---

Bundan dört ay önce [SPA, Angular, Gulp ve Bower](/web/2014/12/13/spa-angular-bower-gulp.html) başlıklı bir blog yazısı kaleme almıştım. O dönemde web sitesinden web uygulamalarına daha yeni yeni geçiş yapıyordum. Şuanda da üst düzey bir uzman olmasam da, bu konuda epey yol katettiğimi ve tecrübe kazandığımı söyleyebilirim. Bu blog yazısında da elimden geldiğince ve aklıma geldiğince düşüncelerimi anlatmaya çalışacağım.

> Yazıyı yazdıktan sonra düzenleme; daha çok kendime notlar gibi bir yazı ortaya çıktı. 
> Belki vakit kaydetmek istemeyebilirsiniz. İçerisinde muazzam önemli, yeni bilgiler yok. :)

### Hızlı Siteler

Bir **SPA** geliştirdiğiniz zaman kesinlikle en büyük avantajınız sunucunuzun hızının artması olacaktır. En basit haliyle sunucu ve istemci arasındaki veri trafiği azalmaktadır. Bunun yanında **render** işlemi (verilerin HTML kodlarına arasına yerleştirilmesi) istemci tarafında yapıldığı için sunucuya ekstra bir rahatlama daha sağlanmaktadır. Yani **SPA** kullandığınızda **Twig** ya da **Blade** gibi sunucu tarafından çalışan **Template Engine**yapılarına ihtiyacınız kalmamaktadır. 

### Hızlı Geliştirme

**SPA** geliştirmeye başladığım zaman, klasik (eski) usüllere göre yaptığım geliştirmeye nazaran daha hızlı ilerleyebileceğimi hiç tahmin etmezdim. Ancak çok daha hızlı bir geliştirme söz konusu oldu benim için. Bunda **AngularJS**'in marifeti çok büyük. Verilerin tarayıcı üzerinde **render** edilerek listelenmesi pek fark yaratmıyordu ancak veri güncelleme ve ekleme bölümlerinde mükemmel bir hız avantajı sağlıyor. Bir noktadan sonra projeniz akıp gidiyor. 

### AngularJS

***"AngularJS SPA'lar için biçilmiş kaftan!"*** diyebilmem için muadilleriyle de proje geliştirmiş olmam lazım. Ancak henüz o kadar tecrübem olmadı. Ancak çok eleştirilen bir yönüne değinmek istiyorum; **Separation Of Concerns.**

Bu kavrama en yalın haliyle; ***"işlerin birbirinden ayrılması"*** diyebiliriz. **AngularJS**'in eleştiri aldığı bölüm de tam olarak bu noktada yatıyor. Çünkü **HTML**'lerin içine verilerin nasıl yerleştirileceğini belirtiyorsunuz ve bunu da HTML etiketlerine benzer etiketler kullanarak yapıyorsunuz. Dolayısıyla bir yerden sonra **HTML** kodları arasında **if** bloklarınız oluyor. Bu birçok geliştirici için sorunlu bir kavram ve işlerin ayrılması gerektiği ilkesine ters düşüyor. 

Ancak ben işleri neye göre ayırdığımıza odaklanmak istiyorum. **Back-End** tarafı veritabanı ile aramızdaki ilişkiyi ve güvenliği sağlayan bir katman. İstemeyi bildikten sonra (oturum, güvenlik, yetki vb.) bize her türlü bilgiyi verir. **JavaScript-AngularJS**'in bulunduğu katman ise bizim kullanıcıya sunulacak verileri sunucudan usülünce istediğimiz katman. Benim için bu katmanın aldığı tek görevi bu. 

Asıl sorun yaratan nokta ise verilerin kullanıcıya nasıl gösterileceği. Buna klasik yöntemde sunucu tarafında, genellikle **view** dosyalarında karar verilirdi ve siz ne kadar mantığı ve gösterim şeklini ayırmaya çalışırsanız çalışın, yine de **if** bloklarını **view** dosyalarında kullanırdınız. 

Her iki yöntemi kullanmış biri olarak şunu söyleyebilirim ki; **AngularJS**'in kullandığı yöntem daha mantıklı geliyor. Geliştirme aşamasında bunun rahatlığını da yaşıyorum. Eğer veri sunucudan düzgün geliyor ama düzgün görüntülenmiyorsa, hata nereden kaynaklanıyor biliyorum. **JavaScript** tarafından bu kontrolleri yapmak ise çok daha kötü sonuçlar doğuruyor. Bunu **jQuery** zamanında çok acı şekilde tecrübe ettim. Bence en önemlisi; tüm uygulamanızda aynı kararlılığı kullanmanız. Verilerin kullanıcıya nasıl sunulacağını sadece **HTML** dosyalarına bıraktıysanız, hep orada kalsın ve asla bir başka yerden müdahale etmeyin. Çünkü bu daha büyük sorunlar çıkaracaktır. 

### SEO Optimizasyonu

Eskiden bu bir sorundu ancak bu bölümü çok uzun tutmayacağım. Çünkü [PhantomJS](http://phantomjs.org/) ile böyle bir sorun kalmıyor.

### Sonuç

Artık klasik yöntemden uzaklaşmak gerekiyor. Doğrudan web uygulamaları tasarlamak ve hızla yol almak şuan için en iyi çözüm. Eğer **AngularJS**'i beğenmiyorsanız, başka bir **Front-End Framework**'ünü tercih edebilirsiniz, hiç sorun değil. Ama bırakın istemciler kendi bellek ve işlemcilerini sizinle paylaşsın. 