---
layout: post
title:  "Laravel İle Hiyerarşik Kategori Yönetimi"
date:   2014-04-29 19:00
categories: php laravel
tags: php laravel hiyerarşi kategori nested list baum
---

Yazılım dünyasında sık karşılaşılan problemlerden biri de hiyerarşik yapıya sahip kategorilerdir. Bu yapıyı kurabilmek için iki türlü model kullanılmaktadır;

* Tümleşik Yapı Kalıbı (The Adjacency List Model)
* Yerleşim Yapı Kalıbı ( Nested Set Model)

## Tümleşik Yapı Kalıbı (The Adjacency List Mode)

Bu model üzerindeki yapıda bilgiler aynı tablo üzerinde, kategorilerin birbirlerinin ID’lerini referans göstermeleriyle muhafaza edilmektedir. Ancak bu şekilde verileri kaydettiğinizde, hiyerarşik yapıyı listelerken kendi kendini çağıran fonksiyonlar kullanmak zorunda kalırsınız. Çünkü hangi kategorinin altında kaç tane daha kategori var, bu alt kategorilerin altında başka kaç kategori var tek tek kontrol etmeniz gerekmektedir. Üstelik derinliğin nereye kadar gideceğini hiç bilmediğinizden bu yapı sizi ya çıkmaza götürür ya da ciddi bir performans kaybı yaşarsınız. Hele hele bu yapıda oluşturulan bir tablo başka bir tablo ile bağlantılı ise ve JOIN ile birlikte veri almanız gerekiyorsa sorununuz daha çok büyüyecektir. Bu nedenle bu yapıyı çok fazla önerilmemektedir.

## Yerleşim Yapı Kalıbı ( Nested Set Model)

Bu yapıda ise küme yerleşimi şeklinde verileri kaydettiğinizi düşünebilirsiniz. Sağ taraftaki görselde kategorilerileriniz nasıl yerleştirileceği şematik olarak anlatılmaktadır. Burada önemli olan husus, her bir kategorinin nereden nereye kadar bir kampsamı olduğunun da kaydedilmesidir. Bunun için “sol” ve “sağ” doğrultuda iki alan daha veritabanına kaydedilmektedir.

![Nested Set Modek](http://upload.wikimedia.org/wikipedia/commons/thumb/4/41/NestedSetModel.svg/400px-NestedSetModel.svg.png)

Örneğin “Clothing” in kapsamına baktığımızda 1-22 arasındadır. Ancak “Slack” 4-5 arasındadır. Bu türde kaydettiğiniz verileri tek SQL cümleciği üzerinde BEETWEEN anahtarıyla sorgulatabilirsiniz. Çünkü tüm alanların kapsamının nereden başlayıp nereden biteceği bellidir.

Konumuzun özü Nested Set Model olmadığı için daha fazla bilgi vermeyeceğim. Daha ayrıntılı bilgi için WikiPedia‘dan yararlanabilirsiniz.

## Laravel 4 Üzerinde Nested Set Model Kullanım

Laravel üzerinde Yerleşim Yapı Kalıbı (Nested Set Model) kullanmak oldukça basittir. Baum isimli kütüphaneyi projenize dahil ederek hızlıca kullanıma başlayabilrisiniz. (GitHub) Nested Set Model’in algoritması özellikle kayıt ekleme ve silme işlemlerinde göreceli olarak karışık olduğundan, bu kütüphane ile eklenilen kayıtların kapsamlarını siz düşünmezsiniz. Tek yapmanız gereken hangi kategorinin neyin altında olduğunu belirlemek. Sol kapsamın ve sağ kapsamın neler olacağını Baum bizim yerimize kendisi belirlemektedir.

Örnek Kullanım

{% highlight php %}
$root = Category::create(['name' => 'Yazılım']); 
$php = Category::create(['name' => 'PHP']); 
$php->makeChildOf($root);
{% endhighlight %}


