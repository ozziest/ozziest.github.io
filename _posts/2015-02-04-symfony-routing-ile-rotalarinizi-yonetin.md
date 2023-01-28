---
layout: post
title: "Symfony/Routing İle Rotalarınızı Yönetin"
date: 2015-02-04 22:00
categories: [Turkish, Coding]
keywords: symfony, routing, rota, http-foundation, link
author: Özgür Adem Işıklı
---

Genellikle birçok projemizde bir yönlendirme (link) yapısına ihtiyacımız olur. Bunu kimi zaman `.htaccess` dosyalarıyla çözeriz, kimi zaman da doğrudan **PHP** dosyalarının adını linkmiş gibi veririz. Fakat tüm bunlar amatör işi çözümlerdir.

Yeni bir projeye başlayacaksanız mutlaka **framework** kullanın. Eğer illa kendinize bir framework yazmak istiyorsanız, olabildiğince hazır çözümlerden yararlanın. Bu yazımızda projelerinizde ya da eğer yazacak kadar deliyseniz kendi frameworkünüzde kullanabileceğiniz bir paketi tanıtacağım; [Symfony/Routing](https://github.com/symfony/Routing).

## Kimler Kullanıyor?

Bu paket **Symfony Framework**'ü için geliştirilmiştir. Ancak o kadar güçlü bir tasarıma sahiptir ki; başka frameworkler de bu paketi yönlendirme işlemleri için kullanmaktadır. Bu frameworkler arasında oldukça tanıdık bir isim daha var; **Laravel**.

## Nasıl Kullanacağım?

Paketi kullanmanın en iyi yolu, paketi **Composer** ile birlikte projenize dahil etmektir;

<pre><code class="language-javascript">
{
    "require": {
        "symfony/routing": "3.0.*@dev",
        "symfony/http-foundation": "3.0.*@dev"
    }
}
</code></pre>

Bu işlemden sonra `composer update` komutunu kullanarak bağımlılıkların projenize dahil olmasını sağlayabilirsiniz.

> `http-foundation` benim vereceğim örnekte kullanılacağı için ben dahil ettim. Bu paket yalnızca benim işlerimi biraz daha kolaylaştırmaktır.

## Örnek Kullanım

**Symfony/Routing** paketi temel olarak aşağıdaki 3 adımda çalışmaktadır;

- Rota koleksiyonu (RouteCollection) oluşturulur.
- Rotalar uygulama içerisinden koleksiyona aktarılır.
- Mevcut kullanıcı isteği rota koleksiyonunda var mı kontrol edilir, yoksa exception fırlatılır.

### Koleksiyon Oluşturma

Aşağıda koleksiyon oluşturma işlemini görebilirsiniz;

<pre><code class="language-php">
// Sınıflar çağırılır
use Symfony\Component\Routing\RouteCollection;

// Koleksiyon oluşturma işlemi
$routes = new RouteCollection();
</code></pre>

### Rota Tanımlama

Bu adımdan sonra rotalarımızı tutacağımız koleksiyonumuz artık hazırdır. Bundan sonra rotaları aşağıdaki gibi ekleyebiliriz;

<pre><code class="language-php">
// Sınıflar çağırılır
use Symfony\Component\Routing\RouteCollection;
use Symfony\Component\Routing\Route;

// Koleksiyon oluşturma işlemi
$routes = new RouteCollection();

// Yeni bir rota oluşturulur
$route = new Route('/welcome', ['controller' => 'WelcomeController']);
// Rota koleksiyona dahil edilir.
$routes->add('welcome', $route);
</code></pre>

Rota oluşturmak işlemini kısaltmak için aşağıdaki gibi kullanmanız da mümkündür;

<pre><code class="language-php">
// Rota oluşturulur ve koleksiyona dahil edilir.
$routes->add('welcome', new Route('/welcome', ['controller' => 'WelcomeController']));
</code></pre>

### Rota Kontrolü

Rota kontrolü için kullanıcı hangi url'yi çağırıyor kontrol ettirmemiz gerekmektedir. Bunu birçok farklı yol izleyerek yapabilirsiniz. Ancak amacımız az kodla çok iş yapmak olduğundan, Symfony tarafından hazırlanan [HttpFoundation](https://github.com/symfony/HttpFoundation) paketindeki `Request` sınıfını kullanmamız yerinde bir tercih olacaktır.

<pre><code class="language-php">
// Sınıflar çağırılır
use Symfony\Component\Routing\RouteCollection;
use Symfony\Component\Routing\Route;
use Symfony\Component\Routing\RequestContext;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\Routing\Matcher\UrlMatcher;

// Koleksiyon oluşturma işlemi
$routes = new RouteCollection();

// Yeni bir rota oluşturulur
$route = new Route('/welcome', ['controller' => 'WelcomeController']);
// Rota koleksiyona dahil edilir.
$routes->add('welcome', $route);

// Yeni bir istek nesnesi oluşturulur
$context = new RequestContext();
// Bu nesnenin içeriği Request sınıfı ile doldurulur
$context = $context->fromRequest(Request::createFromGlobals());		
// Url Karşılaştırma sınıfı oluşturulur
$matcher = new UrlMatcher($routes, $context);
// Mevcut url karşılaştırılır
try {
	$parameters = $matcher->match($context->getPathInfo());
} catch (ResourceNotFoundException $e) {
	echo '404!';
}
</code></pre>

Bu işlem rota karşılaştırma işlemidir. Son satırda eğer rota varsa rota tanımlama aşamasında kaydedilen parametreler bize geri döner. Eğer yoksa bir Exception fırlatılır; `ResourceNotFoundException`. Biz de try-catch blogu ile bu hatayı yaklayamaya çalışıyoruz.

Hepsi bu kadar. Yalnızca dakikalar içerisinde oldukça kullanışlı bir yönlendirme işlemimiz oldu. Ancak bu makalede anlatılanlar Routing paketinin son derece yalın bir kullanımıdır. Aşağıdaki kaynaklardan daha detaylı bilgiler edinebilirsiniz;

- [Symfony/Routing](https://packagist.org/packages/symfony/routing)
- [Symfony/HttpFoundation](https://packagist.org/packages/symfony/http-foundation)
- [Symfony Routing API](http://api.symfony.com/2.3/Symfony/Component/Routing.html)
- [Symfony HttpFoundation API](http://api.symfony.com/2.3/Symfony/Component/HttpFoundation.html)
