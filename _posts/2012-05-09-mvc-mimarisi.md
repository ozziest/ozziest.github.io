---
layout: post
title:  "MVC Nedir?"
date:   2012-05-09 19:00
categories: yazılım
tags: mvc, mvc nedir, mvc mimarisi
meta: mvc, mvc nedir, mvc mimarisi
author: ozziest
---

### MVC Nedir?

MVC Model-View-Controller kelimelerinin baş harflerinden oluşan bir mimaridir. Büyük boyutlarda uygulama geliştirirken verilerin hazırlanması ve kullanıcıya sunulması işlemlerini birbirinden ayrı tutarak çalışan yapılardır. Bu mimarinin avantajlarından biri geliştirici açısından daha hızlı kod yazmak ve karışıklığı önlemektir. Programcı ve tasarımcının aynı anda proje üzerinde çalışmasına da olanak sağlar. Çıkışı çok eski zamanlara dayansa da günümüzde git gide yaygınlaşmaktadır.

### MVC Katmanları

Model, View(Görünüm) ve Controller(Kontrol) kısmından oluşan bu mimaride her katmanın kendine ait görevi vardır. Kontrol bölümü kullanıcının işlemlerini yönlendiren, verileri çağıran/hazırlayan kısımdır. Model kısmı ise veri hazırlarken kullandığımız yapılardır. Tipik fonksiyon mantığına benzer yapıdadır. Görünüm kısmı ise oluşturduğumuz sonucu kullanıcıya gösterdiğimiz katmandır.

### Adım Adım MVC

Örneğin bir kullanıcı sitemizin adını adres satırına yazarak sitemizi çağırdı. MVC mimarisinde ana kontrol olarak belirlediğimiz controller bölümü çalışır. Ana sayfada kullanıcıya verilecek bilgileri hazırlar. Örneğin bir blog sitesi olduğunu ve ana sayfada son girilen yazıları görüntüleyeceğimizi düşünelim. Kontrol bölümü Model bölümünden yer alan son kayıtları bize döndüren bölümü çalıştırarak bir sonuç alır. Bu sonucu istediği gibi işler(eğer isterse, genelde sonuç işlenmiş halde modelden geri döndürülür) ve sonucu View katmanından hazırladığımız bölüme gönderir. Yani kontrol katmanın görevi; kullanıcının isteğine göre Model katmanından sonuç alma ve sonucu View katmanı aracılığı ile kullanıcıya göstermektir.

### PHP Çatıları ve MVC

PHP geliştiricilerinin sık sık kullandıkları PHP çatılarının hemen hepsinde MVC mimarisi kullanılmaktadır. phpframeworks.com adresinden MVC mimarisini kullanan PHP çatılarını daha detaylı inceleyebilir siz de çalışmalarınızda MVC mimarisini kullanabilirsiniz.