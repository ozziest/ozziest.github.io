---
layout: post
title:  "TT #03 - Ne Demek Artık Çalışmıyor?"
date:   2015-03-22 18:00
categories: [Turkish, Presentation]
tags: php, test, hata, yazılım, unit test, birim test, kabul testi, entegrasyon testi
meta: php, test, hata, yazılım, unit test, birim test, kabul testi, entegrasyon testi
author: ozziest
---

> Bu blog yazısı, geçtiğimiz Cumartesi (21 Mart 2015) günü [Mavidurak-IO](http://mavidurak.github.io) ve [Ahir Ar-Ge Danışmanlık AŞ](http://ahir.com.tr)'nin gerçekleştirdiği **Tech Talk #03** etkinliğinde gerçekleştirdiğim sunumun özetini içermektedir.

Hatalar, programlamanın kaçınılmazıdır. Yazdığınız ya da yazmadığınız her satır kod bir hataya neden olabilir. Biz ne kadar mükemmel olduğunu düşünsek de, şuan dünyada var olma nedenimiz dahi geçmişte yapılmış bir hatadır.

Hatalardan tamamen kurtulamayız, ancak doğru zamanda ve doğru yerde tespit edebiliriz. 

Peki doğru zaman nedir? Doğru zaman kesinlikle milyonlarca insanın izlediği bir TV programında Windows 95'i tanıttığınız an değildir. Evet bu yaşandı. Bizzat Bill Gates'in de yer aldığı programda, tüm dünya o meşhur mavi ekranla tanıştı. Ne büyük felaket değil mi? Elbette bu Microsoft'un sonu olmadı ancak ucu Internet Explorer'a kadar uzanan kötü bir şöhreti de beraberinde getirdi.

Bu nedenle doğru zaman; tek başınıza, kendi kendinizle yüzleştiğiniz anlardır. ***Asla bir başkasının size hatanızı söylemesine fırsat vermeyin.***

## Kendinizle Yüzleşmek

Eğer geliştirdiğiniz bir yazılım için kendi kendinizle yüzleşmezseniz, son kullanıcıya yayına çıktığınızda, muhtemelen durumunuz kaza anında yola savrulan kişilerden farklı olmayacaktır. İşler bu noktaya gelmeden alınan her türlü önleme, yani kendinizle yüzleşmeye **"Test Geliştirme"** adını veriyoruz. Sadece ben değil, tüm dünya bu gerekliliği yıllardır uygulamaya gayret ediyor. 

## Biz Robot Değiliz

Evet, geliştirdiğimiz kodları kontrol etmeliyiz. Ama nasıl? Ufak bir proje için belki oturum açma ve kapatma işlemini kontrol etmek zaman kaybına neden olmayacaktır. Ancak onlarca, belki yüzlerce tablodan oluşan bir sistemi her değişiklikten sonra nasıl kontrol edebilirsiniz? Asla bu kadar boş vaktiniz olmayacak ve asla bir robot gibi bıkmadan, usanmadan çalışamayacaksınız.

Ancak biz geliştiriciler robot olmasak da, robot yapabiliriz. 

Peki robot derken neyi kast ediyorum? Kesinlikle, içerisinde motorlar barındıran ve sizin yerinize tuşlara basan bir robot değil. Kullanıcının, geliştirdiğiniz sistem üzerinde yapabileceği her eylemi ***"taklit"*** edebilecek bir robot. Burada taklit(mock) kelimesi, test dünyasında oldukça sihirli bir kelimedir. Geliştireceğimiz robotun birebir kullanıcı gibi davranmasını beklemeyiz. Sadece kullanıcıyı taklit etmesini ve yapabileceği her işi bizim yerimize tekrar tekrar yapmasını bekleriz. Siz hiç yaptığı işten sıkılan, başı ağrıyan, dikkati dağılan bir robot gördünüz mü? 

Test geliştirme işte bu noktadan hareketle yola çıkmaktadır; geliştirdiğimiz yazılıma kullanıcılar gibi değerler gönderen ve yazılım bu değerlere karşılık doğru tepkiler veriyor mu kontrol eden testler geliştiririz. Yani yazdığımız kod için doğru çalışıp çalışmadığını kontrol eden başka kodlar yazarız. 

Test geliştirme dünya üzerinde o kadar yaygındır ki her dilin kendisine özgü test kütüphaneleri vardır. Yazının sonunda görebildiklerinizin (bazı dillere göre test kütüphaneleri) yanına yüzlercesini daha ekleyebilirsiniz. Bu size, yola çıkmak için kolaylık sağlamaktadır. Her biri kendi içinde oldukça güzel dökümanlara ve örneklere sahiptir. Üzerinde çalıştığınız projede kullanacağınız dil için en uygun test kütüphanesini kullanabilirsiniz.

## Kırmızı ve Yeşil

Her test sürecinde aşina olmamız gereken bazı ortak başlıklar vardır. Bunların başında renkler gelir; bu renkler kırmızı ve yeşildir. Geliştireceğimiz her yazılım bizim için kırmızıdır. Testi önce yazarız, sonra o testi geçecek yazılımı geliştiririz. (Bu metoda "Test Güdümlü Geliştirme - TDD" adı verilir.) Yani tüm kırmızıları yeşile çevirmeye çalışırız. Büyük şirketlerde test ekibi tamamen yazılım ekibinden ayrıdır ve test geliştirilenlerin üst düzey yazılım bilgisine ihtiyaçları yoktur. Test kütüphaneleri bu nedenle çok basit tasarlanır ve temel programlama bilenlerin kullanabilecekleri şekilde hazırlanır.

> Ne yazık ki ülkemizde bazı büyük şirketlerde test yazılmadığını öğreniyoruz, gerçekten üzücü.

## Yöntemler

Ayrıca testler de kullandıkları yöntemlere göre bazı farklılıklar gösterir. Unit Test, Integration Test, Acceptable Test vb. Bunlar hakkında çok daha detaylı bilgilere internet üzerinden ulaşabilirsiniz ancak benim yazımın konusu tüm türleri ayrı ayrı incelemek olmadığından, bu türlere detaylı olarak girmeyeceğim. 

## Yazdık ve Bitti mi?

Bu testleri yazdığımızda işimiz bitmez. Eğer sürekli geliştirilecek bir sistem üzerinde çalışıyorsak, test yazma, çalıştırma, refactoring işlemlerini tekrar tekrar yapmamız gerecektir. Örneğin yarım saatte bir tüm testleri yeniden çalıştırabilirsiniz. İşlemlerin tekrarlanmasına **"Continious Integration"** adını veriyoruz. Deyim yerindeyse; ***ömür biter, test bitmez.***

Ancak tüm bunlara rağmen bir çok geliştirici test yazmayı fuzuli bulmaktadır. Kimilerine göre bu bir zaman kaybıdır. İlk başta öyledir, yaklaşık 15-20% arasında bir zamanınızı buna ayırmanız gerekecektir. Bu; geliştireceğiniz ürünün piyasaya çıkış süresini uzatacaktır. Ancak daha sonradan çıkabilecek hataları çok kısa bir sürede bulup, daha etkili müdahalelerde bulunacağız için bu zamanı telafi etmeniz mümkündür. Şahsi görüşüm; ***kullanıcıya hata göstermeyen yazılım, çoğu durumda zamandan daha önemlidir.***

## Kaynaklar

* [PHPUnit](http://phpunit.de)
* [CodeCeption](http://codeception.com/)
* [Unit Testing](http://en.wikipedia.org/wiki/Unit_testing)
* [Integration Testing](http://en.wikipedia.org/wiki/Integration_testing)
* [Acceptance Testing](http://en.wikipedia.org/wiki/Acceptance_testing)