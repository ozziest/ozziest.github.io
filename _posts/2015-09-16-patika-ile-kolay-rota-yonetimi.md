---
layout: post
title: "Patika ile Kolay Rota Yönetimi (PHP)"
date: 2015-09-16 21:00
categories: [Turkish, Coding]
keywords: patika, php, routing, package, routes
author: Özgür Adem Işıklı
post_img: coding.jpg
post_img_link: https://pixabay.com/en/rocket-launch-rocket-take-off-nasa-67643
lang: tr
---

Bugün [Patika](http://ozguradem.net/patika) isimli PHP tabanlı rota yönetim kütüphanesini yayınladım. Bu blog yazısında elimden geldiğince paketi tanıtmaya çalışacağım.

#### Neden?

PHP'de bir çok farklı ve oldukça başarılı **routing** kütüphaneleri bulunuyor. Bunlar hakkında daha önce [Fast Route Kütüphanesi](php/2015/04/17/fast-route-kutuphanesi) ve [Symfony/Routing ile Rotalarınızı Yönetin](php/2015/02/04/symfony-routing-ile-rotalarinizi-yonetin) başlıkları altında yazılar yayımlamıştım. Ancak bu her iki güzide kütüphanede de rotaları el ile tanımlıyor olmak bazı durumlarda beni çok yoruyordu.

Özellikle API tasarlarken, elimden geldiğinde hafif ve rotalara göre doğrudan beni Controller sınıflarına iletecek bir yapıya ihtiyacım vardı. Önce bu yapıyı kendim kurdum, kullandım ve daha sonra bunun bir paket olmasının güzel olabileceğini düşündüm. Önümüzdeki günlerde gerçekten bir ihtiyaç mı değil mi göreceğiz. :)

#### Nasıl?

Paketin kurulumları ve konfigürasyonunu [dokümantasyon](http://ozguradem.net/patika) üzerinden görmeniz mümkün. Çalışmaya hazır hale getirdikten sonra **Patika** istek yapılan URL'yi çözümleyerek, bu URL'yi bir namespace'e dönüştürüyor. Eğer namespace altında bir sınıf varsa bu sınıfı parametrelerle birlikte çağırıyor. Eğer yoksa Controller ya da Method bulunamadı diyerek 2 farklı Exception altında hata veriyor. Böylelikle rota yazmanıza gerek kalmıyor.

#### Örnek URL Yapıları ve Çözümlemeleri

###### 1. Basit Rota

<pre><code class="language-php">
GET localhost/users/all
# Namespace: App\Controllers\Users
# Method: all
# Arguments: []
</code></pre>

##### 2. Uzun Rota

Bu bölümde rotayı PHP'nin izin verdiği ölçüde uzatma imkanımız var.

<pre><code class="language-php">
GET localhost/admin/manager/users/all
# Namespace: App\Controllers\Admin\Manage\Users
# Method: all
# Arguments: []
</code></pre>

##### 3. Parametreli

<pre><code class="language-php">

GET localhost/admin/manage/users/get/1 
# Namespace: App\Controllers\Admin\Manage\Users
# Method: get
# Arguments: [1]
</code></pre>

<pre><code class="language-php">
namespace App\Controllers\Admin\Manage;

class Users {

    public function get($id)
    {
        echo $id;
    }

}
</code></pre>

##### 4. Çoklu Parametreli

<pre><code class="language-php">
GET localhost/admin/manage/users/getByArgument/1/2/3/foo/bar 
# Namespace: App\Controllers\Admin\Manage\Users
# Method: get
# Arguments: [1, 2, 3, 'foo', 'bar']
</code></pre>

<pre><code class="language-php">
namespace App\Controllers\Admin\Manage;

class Users {

    public function get($one, $two, $three, $foo, $bar)
    {
        echo $id;
    }

}
```
</code></pre>
