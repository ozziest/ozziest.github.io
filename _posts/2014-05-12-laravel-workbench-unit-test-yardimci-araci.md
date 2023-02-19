---
layout: post
title: "Laravel Workbench Unit Test Yardımcı Aracı"
date: 2014-05-12 21:00
categories: [Turkish, Coding]
keywords: laravel, php, unit, test workbench, package, testcase
author: Özgür Adem Işıklı
lang: tr
description: Laravel Workbench inceleme yazısı. Workbench nedir ve nasıl kullanılır sorularının cevaplarını bu makalede bulabilirsiniz.
---

**Laravel** üzerinde geliştirme yaptığınız sırada yeni bir paket oluşturarak çalışmanız çoğu zaman yararınıza olacaktır. Bir işi yapan bir paket geliştirdiğinizde, paketi **composer** ve **packagist** yardımıyla yayınlayabilir ve dilediğiniz projenizde sadece composer üzerine ekleme yaparak yeniden kullanabilirsiniz. Bu nedenle paketlerle çalışmak oldukça önemlidir.

Eğer yeni bir paket oluşturmak isterseniz [Laravel Türkçe Dökümantasyonu](http://laravel.gen.tr/docs/packages)‘ndan yararlanabilirsiniz. Yeni bir paket oluşturduğunuzda eminim ki benim gibi **Unit Test** yazmak için can atacaksınız. :) Çünkü herkes bilir ki hepimiz Unit Test yazmaya can atarız. :) Workbench üzerinde paket geliştirirken Unit Test yazmamıza yardımcı olacak oldukça kullanışlı bir araç bulunmakta: Orchestral/Testbench ([GitHub](https://github.com/orchestral/testbench))

## Kullanım

Bu aracı geliştirme yaptığınız geliştirdiğiniz paketin composer dosyasına dahil ettikten sonra aşağıdaki şekilde kullanabilirsiniz.

<pre><code class="language-php">
class TestCase extends Orchestra\Testbench\TestCase {
	
}
</code></pre>

Provider ve Aliases tanımlamalarını da aşağıdaki şekilde yaptığınızda, testiniz içinde doğrudan sınıflarınızı metotlarına ulaşmanız mümkün olacaktır.

<pre><code class="language-php">
    protected function getPackageProviders()
    {
        return array('Acme\AcmeServiceProvider');
    }
 
    protected function getPackageAliases()
    {
        return array(
            'Acme' =&gt; 'Acme\Facade'
        );
    }
</code></pre>

## Cannot redeclare crypt_random_string() Hatası

Eğer kurulumdan sonra bu hatayı aldıysanız kütüphanenin [buradan](https://github.com/orchestral/testbench#cannot-redeclare-crypt_random_string) kendi çözüm yöntemini deneyebilirsiniz. Ayrıca Laravel Türkiye Forumu‘ndan bu konuyla ilgili yürütülen [tartışmalardan](http://forum.laravel.gen.tr/viewtopic.php?id=547) yararlanabilirsiniz.
