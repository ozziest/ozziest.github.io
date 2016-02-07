---
layout: post
title:  "CodeIgniter Session Kütüphanesi"
date:   2013-09-26 19:00
categories: php
tags: codeigniter, session, library, cookie, hata
meta: codeigniter, session, library, cookie, hata
author: ozziest
---

Merhaba,

Bugün biraz CI Session kütüphanesinden bahsetmek istiyorum. Nasıl kullanılacağını bir çok yerden bulabilirsiniz. ([http://ellislab.com/codeigniter/user-guide/libraries/sessions.html](http://ellislab.com/codeigniter/user-guide/libraries/sessions.html)) Benim bahsetmek istediğim bu kütüphanenin çalışma yöntemidir.

İsmi her ne kadar session olsada, veriler Session’da değil Cookie’lerde tutulmaktadır.

> The Session class stores session information for each user as serialized (and optionally encrypted) data in a cookie.

Bu nedenle oturum yönetimini bu kütüphane ile gerçekleştirdiğinizde oturum verileriniz deyim yerindeyse kabak gibi ortadadır. CI bunun için size iki güvenlik alternatifi sunar; verileri aynı anda database’de kaydetme ve verileri şifreleme. Ancak dikkat edilmesi gereken nokta; CI’da varsayılan olarak bu iki yöntem de kapalıdır kütüphanede. Eğer bu işlemleri config dosyasından aktif hale getirmezseniz büyük sıkıntılar yaşayabilirsiniz.

