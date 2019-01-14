---
layout: post
title:  "SPA, Angular, Bower ve Gulp"
date:   2014-12-13 22:00
categories: [Turkish, Coding]
tags: soa, angular, bower, gulp, spa
meta: soa, angular, bower, gulp, spa
author: ozziest
---

Uzun zamandır sürdürmek zorunda kaldığım bir proje üzerine çalışıyorum ve son dönemlerde oldukça fazla sürdürülebilirlik sorunları yaşıyordum. Bir yandan günden güne çöplüğe dönen kodlar, bir yandan yeni özelliklerin eklenmesinde yaşadığım zorluklar alıp başını gidiyordu. Uzunca bir düşünme safhasından sonra, kendimce cesur bir adım atarak proje çekirdeğinde refactoring yapma kararı aldım.

## Klasik Yapı

Bu noktada biraz proje yapımdan söz etmem gerekiyor. Klasik ***web sitesi*** yapısında çalışan bir projeydi. Request ve response arasında yaşam mücadelesi veriyordu. İstek yapılıyor, veritabanından veriler alınıyor/yazılıyor ve sonuç sunucu tarafında derlenerek (HTML ve veri birleştirmesi) kullanıcıya yansıtılıyordu.

## Bağımlılık Yönetimi

PHP altyapısı ile geliştirilen bir sistem olduğu için PHP tarafında kullandığım paketlerin bağımlılıklarını **Composer** ile yönetebiliyordum. Ancak front-end tarafında kullandığım kütüphaneler için de bir bağımlılık yöneticisine ihtiyacım vardı. Bunun için [Bower](http://bower.io) kullanmaya başladım ve kullandığım tüm paketleri Bower ile projeye entegre ettim.

## Script Yükleme Sorunu

Front-end tarafında çok fazla script olduğundan yüklemesi uzun sürüyordu ve ben de kullandığım scriptleri bulduğum bir iki ek uygulama ile birleştiriyordum. Ancak bu işi benim için yapacak araçların olduğunu öğrendim; [Gulp](http://gulpjs.com) ve [Grunt](http://gruntjs.com). Bu her iki araç da **Javascript Görev Yöneticisi** olarak lanse ediliyor. İkisinin de kendi içerisinde eklentileri mevcut ve ilgili eklentiler [NPM](https://www.npmjs.com) aracılığı ile kurularak tanımlayacağınız görevleri gerçekleştirilebiliyor. Tahmin edebileceğiniz gibi birçok plugin mevcut. Ben daha basit kullanımdan ötürü **Gulp** kullanmayı tercih ettim. Kendi Gulp görevlerimi tanımladıktan sonra, Bower ile yüklediğim scriptler üzerinde dilediğim işlemleri otomatik olarak yaptırabiliyorum.

## Sonuçların Derlenmesi Sorunu

Sonuçların sunucu tarafında derlendiğinden yukarıda bahsetmiştim. Refactoring kararını aldığımda, yapacağım değişikliklerdeki en önemli karar bu konuyla ilgili olandı. Sonuçların sunucu tarafında değil, istemci tarafında derlenmesini istiyordum. Bunun için sunucunun bir API olarak çalışmasına karar verdim. Yani bu işlemden sonra elimde tam anlamıyla bir **Single Page Application** olacaktı.

> Eski sistemde de görünürde SPA var gibiydi. Ancak tüm istekler AJAX isteğiydi ve alınan sonuçlar HTML'di. DOM manipülasyonu ile sayfalara yerleştiriliyordu. Bu oldukça zahmetli bir geliştirme süreciydi.  

Arayüzde ise Angular ile veri alışverişini sağlayacaktım. Henüz bu işleme başlayalı bir hafta kadar oldu ve sonuçlar oldukça etkileyici. Sağladığım düzen ve sunucunun rahatlığı proje açısından geleceğe umutla bakmamı sağlıyor.

> Ancak Angular.js her ne kadar çok harika bir framework olsa da, debug konusunda oldukça sıkıntı yaşatıyor. 

## Sonuç

Şuanda *"Keşke daha önce bu işleme başlasaymışım."* diyorum. Refactoring işlemi size ilk başta zaman kaybettirecek gibi gözükse de, bu tarz radikal kararlardan sonra projenizin deyim yerindeyse eli ayağı düzeliyor. Yeni bir projeye başlıyorsanız ya da var olan bir projenize her geçen gün kod yazma isteğiniz azalıyorsa, projenize bu tür araçları dahil etmenizi (naçizane) öneririm. 

> Projenin zamanla çöplüğe dönmesinde, geçmişteki eksik ve yanlış bilgilerimin de şüphesiz payı var. Refactoring demek, aynı zamanda **code review** yapmanızı da sağlamaktadır. (Bir yabancının, geçmişteki sizin.) Böylece geçmişte yaptığınız hataları da görme şansı yakalıyorsunuz.




