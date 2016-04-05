---
layout: post
title:  "Hazır Kütüphane Kullanma Nedenlerim"
date:   2014-08-20 22:39:28
categories: diger 
tags: php, package, kütüphane
meta: php, package, kütüphane
author: ozziest
---

Son günlerde dikkatimi çeken ve oldukça önemli bulduğum bir konu var; hazır kütüphane kullanımı. Kimi geliştiriciler bunu mantıksız buluyor. Daha da vahimi, hazır kütüphane kullananların yazılım yeteneklerini yetersiz görüyorlar. Bu kocaman bir saçmalık. Aşağıdaki alt başlıklarda bunun nedenlerini açıklamaya çalışacağım.

## 1. Zaman

Eğer öğrenci değilseniz, zamanınız yoktur. Birçok geliştirici tarafından kullanılan yaygın bir kütüphane zaman konusunda size yardımcı olacaktır. Kazandığınız bu zamanda projenize daha güzel özellikler ekleyebilirsiniz.

## 2. Dokümantasyon

Eğer hazır kütüphane kullanmak yerine kendiniz bir kütüphane yazarsanız, yazdığınız bu kütüphaneyi sizden sonra gelecek geliştiricilerin de kullanabilmesi için kullanım kılavuzu hazırlamanız gerekir. Tanıdığım geliştiricilerin yarısından daha fazlası, hayatı boyunca hiç kullanım kılavuzu hazırlamamıştır. Bu yüzden kendiniz bir paket geliştiriyorsanız ve kılavuz hazırlamıyorsanız, ürettiğiniz yazılımın sürdürülebilirliği düşük olacaktır.

## 3. Stabilite

Eğer kendi yazdığınız kodların cidden kusursuz olduğunu düşünüyorsanız, büyük bir yanılgı içerisindesinizdir. Egolarınızı bir kenara bırakın ve gerçek dünyaya dönün. Hiçbir kod kusursuz değildir. Ne kadar test yazarsanız yazın bu durumu azaltabilir ama asla yok edemezsiniz. Hazır kütüphane kullandığınızda, kulandığınız kütüphane birçok geliştirici tarafından kullanıldığından geri dönütler oldukça fazla olacak ve ilgili kütüphanenin kararlılığı yükselecektir. Bu da ürettiğiniz yazılımın daha sorunsuz bir şekilde işlemesine neden olacaktır.

## 4. Güncellik

Kendiniz bir kütüphane ürettiğinizde, o an için en güncel yazılım metotlarını uygulamış olabilirsiniz. Ancak hiçbir dil yerinde saymıyor ve günden güne gelişiyor. Buna paralel olarak, yazdığınız kütüphaneyi de güncellemeniz gerekecektir. Bu da 1. maddede sözü geçen zaman sorununa yol açacaktır. Ancak hazır bir kütüphane, siz katkı sağlamasanız bile muhtemelen geliştirilmeye devam edecektir. Siz de bu değişikliklerden faydalanabilirsiniz.

## 5. Kullanıcı İstekleri

Kimi geliştiriciler bazı kütüphanelerin taleplerinden çok daha fazlasını yaptığını düşündükleri için kullanmazlar. Örneğin siz sadece oturum yönetimi için hazır kütüphane kullanmayı saçma bulursunuz. İlkten haklısınızdır da. Ama eğer bir müşteriye iş yapıyorsanız, bir sonraki adımda neye ihtiyacınız olduğunu bilemezsiniz. Müşteri bir telefonla gelişmiş bir yetkilendirme sistemi talebinde bulunabilir. Hazır kütüphaneler genellikle kapsamlı olur ve gelecekte başınıza açılabilecek sorunları önceden çözebilirler.

## 6. Farklı Bakış Açıları

Çok kaliteli yazılımcılar tanıyorum ama en büyük sorunları birlikte çalışamamak. Tek başına harikalar yaratan fakat bir ekip içerisinde çalışamayan geliştiriciler her zaman için sorundur. Çünkü kimi projeler tek kişilik değildir. Hazır kütüphaneler ile çalışmak sizi iletişime zorunlu kılar. Kütüphane üzerindeki bir hayatı tespit ettiğinizde bununla ilgili issue(sorun) bildirimi yapmak bile başlı başına bir adımdır. Başkalarının yazdığı kodları görmek, ufkunuzu genişletecektir. Sırf bu yüzden yazılmış büyük kütüphaneleri incelediğim oluyor.

## 7. Maliyet

Eğer kendi projenizi yapmıyorsanız ve ürettiğiniz yazılım satılarak size para kazandıracaksa, yani amacınız paraysa, maliyetleri göz ardı edemezsiniz. Zaman, maliyetle doğru orantılıdır. Hazır kütüphane size daha çok para kazandırır.

## Sonuç

Bunlar ilk bakışta aklıma gelen başlıklar ve zamanla çoğaltılabilir. Bu nedenlerledir ki; egonuzu bir kenara bırakın. Hazır kütüphane sizin **yetersiz** olduğunuzu göstermez. Bulunduğunuz şartlar göre **zekice** bir adım attığınızı gösterir.
