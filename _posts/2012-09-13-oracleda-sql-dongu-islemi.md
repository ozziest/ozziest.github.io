---
layout: post
title: "Oracle’da SQL Döngüsü"
date: 2012-09-14 19:00
categories: [Turkish, Coding]
keywords: oracle, sql döngüsü
author: Özgür Adem Işıklı
lang: tr
description: Oracle üzerinde yazdığınız SQL betiklerinde nasıl döngü kurabileceğinizi öğrenebilirsiniz.
---

Oracle üzerinde belirli bir veriye göre ardı ardına SQL kodu çalıştırmak istediğiniz döngü kullanmak zorundasınızdır. Tabi döngü kullanmak demek PL/SQL’e giriş yapmak demektir. Aşağıdaki kod parçası bu işi gerçekleştirmekte. Genel olarak bakıldığında öğrenci tablosundaki tüm öğrenci_no alanlarına göre notlar tablosuna her öğrenci için sözlü notu ekliyor. Siz de bu kodu istediğini şekilde düzenleyerek kullanabilirsiniz.

<pre><code class="language-sql">
begin for veri in (Select ogrenci_no from ogrenci) loop
    insert into notlar (ogrenci_no, not_turu, not) values (veri.ogrenci_no, 'Sozlu' , 100) ;
end loop;
end;
</code></pre>
