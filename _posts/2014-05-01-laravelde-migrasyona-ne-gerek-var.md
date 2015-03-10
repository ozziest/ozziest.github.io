---
layout: post
title:  "Laravel'de Migrasyona Ne Gerek Var?"
date:   2014-05-01 19:00
categories: php laravel
tags: php, laravel, migrasyon, veritabanı, versiyon, taşıma, migration
meta: php laravel migrasyon veritabanı versiyon taşıma migration
author: ozziest
---

## Alışkanlıklar

*“Biz yıllardır phpmyadmin ile veritabanı dizayn etmiş adamız. Eski köye yeni adet getiriyorlar. MVC falan, bunlar bir takım dış mihrakların uydurduğu şeyler. Kendi framework’ümde gül gibi kod yazıyorum. Bir kere daha güvenli. Bir de alışkanlık var tabi. Öğrenmek için ayıracak vakit yok.”*

Bu duyduklarınız size bir yerlerden tanıdık gelebilir. Şuan bir web uygulaması geliştirirken hiç bir yardımcı araçtan yararlanmıyor olabilirsiniz. Ama buna bir son vermek zorundasınız. Bilmemek ayıplanamaz ama sürekli bilmiyor olmak ve yeni şeylere karşı çıkmak ağır bir saplantıdır.

## Ekip Olarak Geliştirme Yapmak

Ekip olarak bir web uygulaması geliştirdiğimizi düşünelim. 4 geliştiricimiz ve 1 test elemanımız var. Her geliştirici kendi lokalinde çalışmalarını sürdürüyor. Tabii bir de canlı (production) üzerinde çalışan bölüm var. Geliştirmenin her anında veritabanı üzerinde değişiklikler yaptığınızı düşünün. Bunu hem diğer geliştirici arkadaşlarınıza hem de canlıya nasıl bildireceksiniz? İşte bu sorun nedeniyle veritabanı sürümleme kavramı ortaya çıkmıştır. Çeşitli frameworkler çeşitli çözüm yolları bulmuştur. (CodeIgniter umursamamıştır.) Laravel’de ise bu sorunu migrasyon (migration – göç) ile çözülmüştür. El yordamıyla tablo yapısı güncellemek yerine, yapıyı kod ile oluşturur ve repoya gönderirsiniz.

Laravel ile veri tabanının yapısı üzerinde değişiklikler yapmak için [Şema Oluşturucu](http://laravel.gen.tr/docs/schema) kullanılmaktadır. Şema oluşturucu ile yeni tablo oluşturabilir, var olan tablonun yapısı üzerinde değişiklikler yapabilirsiniz. Migrasyon dediğimiz kavram ise veri tabanının yapısı oluştururken her bir değişikliğin adım adım tanımlanmasından ibarettir.

Örneğin; bir kullanıcı tablosu oluşturacağımızı düşünelim. Hemen Laravel konsol uygulamasında aşağıdaki kodu çalıştırırız.

{% highlight bash %}
php artisan migrate:make kullanicilar_tablosunu_olustur
{% endhighlight %}

Bu kod bize “kullanicilar_tablosunu_olustur” adında bir migrasyon oluşturur. Migrasyon app/database/migrations klasörü altında başında zaman damgası eklenerek oluşturulmuştur. Migrasyon içerisinde up ve down olarak iki tane metodumuz bulunmaktadır. Up metodu, migrasyon çalıştırıldığında yapılacak, Down metodu ise migrasyon geri alındığında yapılacak değişiklikleri tanımladığımız bölümdür. Bu bölümlerin içinde şema oluşturucu ile tablo yapısını tanımlarız.

{% highlight php %}
public function up()
{
   Schema::create('users', function($table)
   {
      $table->increments('id');
      $table->string('name', 255);
   });
}
{% endhighlight %}

Yukarıdaki kod örneğinde users adında bir tablo oluşturulur. Tablonun id ve name olmak üzere iki tane alanı vardır. Bu tanımlamayı yaptıktan sonra artık bu migrasyonun nasıl bir tablo yapısı istediğini biliyoruz. Bu kodu ekledikten sonra aşağıdaki kod ile migrasyonlar çalıştırılır.

{% highlight bash %}
php artisan migrate
Temsili Migrasyon Çalıştırılması
Temsili Migrasyon Çalıştırılması
{% endhighlight %}

Siz migrasyonları ne kadar çalıştırırsanız çalıştırın, Laravel her seferine aynı kodları tekrardan çalıştırmaz. Migrasyonların daha önce çalıştırılıp çalıştırılmadığını kendi kontrol eder ve böylelikle veri tabanınız güncel kalır. 5 kişilik bir ekipte isteyen geliştirici istediği değişikliği bu yolla yapabilir. Tek yapılması gereken pull yapıldıktan sonra (versiyon kontrol sistemi kullandığınızı varsayıyorum) migrasyonların çalıştırılmasıdır. Gerisini Laravel’e bırakın.

Ayrıca hazır olarak alıp kullandığınız bazı kütüphaneler de veritabanı yapılarını migrasyonlar içerisinde tanımlamışlardır. (Bknz: Sentry Kütüphanesinin 4. Kurulum Adımı)

