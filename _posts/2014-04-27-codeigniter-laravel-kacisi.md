---
layout: post
title: "CodeIgniter'dan Laravel'e Kaçış"
date: 2014-04-27 19:00
categories: [Turkish, Coding]
keywords: php, laravel, codeigniter
author: Özgür Adem Işıklı
---

Yaklaşık bir buçuk yıl kadar CodeIgniter (CI) ile bir proje geliştirmekteydim. Şartlar CI ile projeye başlamak için gayet uygundu. Tahmin edersiniz ki yapacağımız işi maddi kaygılar belirlediğinden, başladığınız projenin nereye gideceğini ve nerede son bulacağını bilemiyoruz.

## CI ile Basit Bir Proje

Son derece basit bir projeye başlamıştık ve bir kaç haftada bitmesi gerekiyordu. Ancak müşteri talepleri ve maddi gelirimizi her zaman müşterinin sağlıyor oluşu projeyi sürekli başka boyutlara taşıyordu. Bu nedenle 2 hafta olarak düşündüğümüz proje 1 buçuk yıl sürdü. Tabii olarak CI ile bu kadar uzun bir süre aynı projeyi geliştiriyorsanız proje klasörleriniz birbirine girmeye başlıyor. Bir yerden sonra ipin ucunu kaçırabiliyorsunuz çünkü müşterinin her zaman acelesi var ve her zaman bir şeyler “ACİL!” olarak talep ediliyor. Zaten CI da öyle çok derli toplu kod geliştirebileceğiniz bir framework değil.

## CI ile Boğulmak

CI ile proje geliştirken yaşadığım en büyük sıkıntılar şunlar olmuştu;

- Harici olarak hazır kütüphane kullanmanın zorluğu. (2.1.4 sürümünde composer bulunmuyor)
- Standart olarak ORM yapısının olmayışı. Bu nedenle ekip olarak çalıştığımızda yaşadığımız veritabanı karmaşası.
- Standart olarak Template Engine olmayışı.
- Bir süre sonra çöplüğe dönüşen kod yığınları.
- Neyse ki söz konusu projemize ağırlık vermeyi bıraktık ve başka projelere yönelebildik. Bu nedenle yeni projemizde CI’dan kaçış işlemlerimizi başlatmış olduk ve Laravel ile çalışmaya başladık.

## Laravel’e İlk Bakış

1 haftalık çalışma sonundaki edindiğimiz izlenimlere göre kendisini çok sevme sebeplerimiz şöyle;

- Composer
- Composer (Önemini anlatabilmek için tekrar ediyorum. :) )
- ORM yapısı
- Kullanışlı route yönetimi
- Migration ve Şema oluşturma işlemleri. (Ekip halindey çalışırken muazzam bir rahatlık.)

Öncelikli olarak bu sebepler yazdığımız koddan daha fazla keyif almamızı sağladı. Özellikle Composer ile hazır paketleri kullanabilmekteki kolaylık, çok hızlı bir şekilde kod geliştirmeye olanak veriyor. Eğer siz de sadece CI ile geliştirme yapıyorsanız ve biraz üzerinizde bir çekimserlik var ise framework değiştirme konusunda, kesinlikle tereddüt etmeyin. CI’dan kurtulmanız gerekiyor. Çünkü bir noktadan sonra size destek değil, köstek oluyor.
