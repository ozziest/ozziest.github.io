---
layout: post
title: "Kredi Kartı Numarasından Banka Öğrenme İşlemi"
date: 2015-09-12 19:00
categories: [Turkish, Coding]
keywords: kredi kartı numarası, banka bilgisi öğrenme, kredi kartı, kart tipi
author: Özgür Adem Işıklı
lang: tr
description: Kredi kartından nasıl banka numarasını öğrenebileceğinizi gösteren bir makale.
---

Bu sefer teknik bir yazı yerine, kullanışlı bir bilgi vermek amacıyla yazıyorum. :)

Geçtiğimiz günlerde kredi kartından (banka kartı da olabilir) kart tipini ve ilgili kartın ait olduğu bankayı öğrenip öğrenemeyeceğimi merak etmiştim. Biraz araştırmadan sonra bingo; **binlist** adı verilen bir yapı ile karşılaştım.

### BIN/IIN List Nedir?

Kredi kart numaralarının ilk altı karakteri **Issuer Identification Number (IIN)** olarak adlandırılmaktadır. Daha önceden bu adlandırma işlemi **Bank Identification Number (BIN)** olarak yapılıyormuş. Bu ilk 6 numara aracılığı ile kartın tipi ve ait olduğu banka bilgilerini bilme imkanımız oluyor.

### Binlist.net

Bu işlemi yapabileceğimiz ilk kaynağımız [binlist.net](http://www.binlist.net). Doğrudan site üzerinden size tanınan arayüzle ya da ücretsiz olarak sunulan **_(günlük 10.000 istek limitli)_** API aracılığı ile sorgulama yapabiliyorsunuz. Kart tipinde genelde sorun yaşanmasa da, bazen banka bilgileri eksik gelebiliyor. Ancak eğer siz bu bilgileri biliyorsanız [binlist-data](https://github.com/binlist/binlist-data) reposu üzerine katkıda bulunarak banka bilgilerine yenilerini ekleyebilirsiniz.

Hatta eğer projelerinizde AngularJS kullanıyorsanız, aşağıdaki servis aracılığı ile hızlıca sorgulama yapabilirsiniz.

<pre><code class="language-js">
app.factory('$bin', function ($http) {

  var service = {};

  service.get = function (code, callback) {
    code = code.replace('-', '').substr(0, 6);
    $http.get('http://www.binlist.net/json/' + code)
      .then(function (response) {
        callback(response.data);
      }, function () {
        callback(false);
      });
  };

  return service;

});
</code></pre>

### Gist Üzerindeki Dosyalar

Bir diğer kaynağımız **_Türkiye_** için hazırlanmış olan BIN listeleri. Bulabildiklerimi aşağıda paylaşıyorum.

- [CSV - berkayunal](https://gist.github.com/berkayunal/1595676)
- [SQL - ismailbaskin](https://gist.github.com/ismailbaskin/2489587)
- [SQL - kemalaydin](https://gist.github.com/kemalaydin/b3de747b5436ec7a40c9)
