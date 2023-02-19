---
layout: post
title: "Fast Route Kütüphanesi"
date: 2015-04-17 08:30:28
categories: [Turkish, Coding]
keywords: fast router, php, router, rota, url, packages
author: Özgür Adem Işıklı
lang: tr
description: Fast Route kütüphanesi hakkında yazdığım bir tanıtım yazısı.
---

Geçtiğimiz aylarda [Symfony/Routing İle Rotalarınızı Yönetin](/php/2015/02/04/symfony-routing-ile-rotalarinizi-yonetin.html) başlıklı bir yazı yayınlamıştım. O yazıda Symfony/Routing kütüphanesi ile PHP projelerinizde nasıl rotalama işlemi gerçekleştirebileceğinizden bahsetmiştim. Ancak bu yazıda, bir başka rotalama kütüphanesine değineceğim; [Fast Route](https://github.com/nikic/FastRoute)

**Symfony/Routing** kesinlikle harika bir kütüphane. Ancak gerçekleştirilen bir [benchmark testi](https://github.com/tyler-sommer/php-router-benchmark)'ne denk gelmem [Fast Route](https://github.com/nikic/FastRoute) kütüphanesi ile tanışmamı sağladı. Benchmark testine göre en hızlı rotalama kütüphanesi [Fast Route](https://github.com/nikic/FastRoute). Bunun arkasında yatan neden için [Fast Route](https://github.com/nikic/FastRoute) kütüphanesi yaratıcısı [Nikita Popov](http://nikic.github.io), kütüphaneyi **"Regex Temelinde Çalışan Bir Kütüphane"** olarak tanımlıyor. Bu konuyla ilgili oldukça detaylı olarak bir blog yazısı hazırlanmış. İlgili blog yazısına buradan ulaşabilirsiniz; [Fast request routing using regular expressions](http://nikic.github.io/2014/02/18/Fast-request-routing-using-regular-expressions.html)

## Kullanım

Kütüphane **Composer** ile hızlı bir şekilde kurulabiliyor ([Packagist](https://packagist.org/packages/nikic/fast-route)) ve kullanımı da oldukça basit;

<pre><code class="language-php">
require '/path/to/FastRoute/src/bootstrap.php';

$dispatcher = FastRoute\simpleDispatcher(function(FastRoute\RouteCollector $r) {
    $r->addRoute('GET', '/user/{name}/{id:[0-9]+}', 'handler0');
    $r->addRoute('GET', '/user/{id:[0-9]+}', 'handler1');
    $r->addRoute('GET', '/user/{name}', 'handler2');
});

$routeInfo = $dispatcher->dispatch($httpMethod, $uri);
switch ($routeInfo[0]) {
    case FastRoute\Dispatcher::NOT_FOUND:
        // ... 404 Not Found
        break;
    case FastRoute\Dispatcher::METHOD_NOT_ALLOWED:
        $allowedMethods = $routeInfo[1];
        // ... 405 Method Not Allowed
        break;
    case FastRoute\Dispatcher::FOUND:
        $handler = $routeInfo[1];
        $vars = $routeInfo[2];
        // ... call $handler with $vars
        break;
}
</code></pre>

Eğer basit ama hızlı bir rota yapısı oluşturmak isterseniz bu kütüphane tam size göre. Repository üzerine gönderilen [commitleri](https://github.com/nikic/FastRoute/commits/master) incelediğimizde ise yazıyı yazdığım anda hala aktif bir kütüphane olduğunu söyleyebiliriz.
