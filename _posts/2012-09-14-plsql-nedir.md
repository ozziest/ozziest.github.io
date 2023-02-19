---
layout: post
title: "PL/SQL Nedir?"
date: 2012-09-14 19:00
categories: [Turkish, Coding]
keywords: plsql, nedir
author: Özgür Adem Işıklı
lang: tr
description: PL/SQL konusunu incelediğim giriş seviyesinde bir makale.
---

Bilmeyenler için önce SQL‘in tanımından başlamakta yarar var. SQL (Structured Query Language, Yapılandırılmış Sorgluama Dili) veri tabanı işlemlerinde verileri yönetmek için kullanılan veritabanı yönetim sistemidir. Bir çok kişi tarafından bir programlama dili olarak da bilinir. Amacı veritabanı üzerinde işlem yapmaktır ve yardımcı bir dildir. Bir program yazamazsınız, yazdığınız program ve veritabanı arasındaki veri alışverişini yönetirsiniz.

PL/SQL(Procedural Language/Structured Query Language) ise Oracle firması tarafından geliştirilmiş Oracle veritabanı sistemlerinde kullanılabilen bir dildir. Prosedürel bir yapısı vardır ve bu nedenle bir çok işlemi program kısmında değil de veritabanı kısmında halledebilirsiniz. Bu dil de değişken tanımlayabilir ve akış kontrolü yapabilirsiniz. Basit bir örnek vermek gerekirse, veritabanından çekeceğiniz bir alanın değerine göre bir başka sorgu çalıştıracaksanız bunu kontrol program kısmında değil, PL/SQL sayesinde veritabanı kısmında halledebilirsiniz. Program ve veritabanı arasından sürekli veri taşımak yerine veriler veritabanında hazırlanır ve programa gönderilir. Binlerce bu şekilde işleminiz olduğu zaman PL/SQL ciddi bir performans artışı sağlayabilir. Standart SQL sorgularına göre tek seferde veritabanına bir çok SQL cümleciği gönderebilirsiniz.

Basit bir PL/SQL kod bloğunu oluşturalım. FOR döngüsüyle ardı ardına çalıştıracağımız SQL cümlecikleri oluşturalım. Aşağıdaki kod bu işlemi gerçekleştirmektedir. PL/SQL olmadan standart SQL kodlarıyla bu işlemi gerçekleştirmek isteseydik programımız içinde ardı ardına SQL komutlarını veritabanına yollayacaktık. Fakat PL/SQL sayesinde bu işlemi sadece veritabanına yaptırıyoruz.

<pre><code class="language-sql">
BEGIN
     FOR sayac IN 0 .. 10 LOOP
          INSERT INTO ogrenciler (ogrenci_no,ogrenci_adi) VALUES (sayac, 'İSİM');
     END LOOP;
END;
</code></pre>
