---
layout: post
title:  "Adonis Web Framework İncelemesi"
date:   2016-02-21 13:30
categories: javascript
tags: adonis, nodejs, web framework, adonis nedir, laravel for nodejs
meta: adonis, nodejs, web framework, adonis nedir, laravel for nodejs
author: ozziest
---

[Adonis](http://adonisjs.com), **NodeJS** ile daha az kod yazarak uygulama geliştirmeyi amaçlayan ve MVC mimarisine göre oluşturulmuş bir framework'dür. Diğer NodeJS web frameworklerine göre öne çıkan başlıca özellikleri **ES6** ile geliştirme yapmayı amaçlaması ve **PHP** dili için geliştirimiş [Laravel](https://laravel.com)'in mimarisine benzer olmasıdır. 

### Laravel Benzerliği

Adonis dokümanlarında da yer aldığı gibi; Laravel'in özellikleri kasıtlı olarak framework bünyesinde uygulanmaya çalışılmaktadır. Benzerlikleri anlamak için temel bazı tanımlamaları hem Laravel'de hem de Adonis bünyesinde incelememiz yeterli olacaktır.

#### Routing

<pre><code class="language-php">
Route::get('user/{id}', 'UserController@showProfile');  // Laravel
</code></pre>

<pre><code class="language-js">
Route.get('user/{id}', 'UserController.showProfile')  // Adonis
</code></pre>

#### Controllers

<pre><code class="language-php">
// Laravel
namespace App\Http\Controllers;

class UserController extends \App\Http\Controllers\Controller {

    public function showProfile($id)
    {
        return 'showProfile';
    }
}
</code></pre>

<pre><code class="language-js">
// Adonis
class UserController {

  *showProfile (request, response) {
    response.send('showProfile')
  }

}

module.exports = HomeController
</code></pre>

#### Request

<pre><code class="language-php">
$request->input('email');   // Laravel
</code></pre>

<pre><code class="language-js">
request.input('email')  // Adonis
</code></pre>

#### Response

<pre><code class="language-php">
response()->json(['name' => 'foo']);   // Laravel
</code></pre>

<pre><code class="language-js">
response.json({ name: "foo" })  // Adonis
</code></pre>

#### ORM

Adonis'de de tıplı Laravel'in Eloquent ORM'si gibi bir [Lucid](http://adonisjs.com/docs/2.0/lucid) adında bir ORM kütüphanesi bulunur.

<pre><code class="language-php">
// Laravel
namespace App;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    
}
</code></pre>

<pre><code class="language-js">
// Adonis
const Lucid = use('Lucid')

class User extends Lucid {

}
</code></pre>

Adonis'de de tıplı Eloquent ORM'de yaptığınız gibi ilişkiler tanımlayabilir ya da şartlara bağlı sorgulamalar yapabilirsiniz. Ancak yazının çok uzamaması için, ilgili bölümleri dahil etmiyorum. Link aracılığı ile daha detaylı bilgi alabilirsiniz: [İlişkiler](http://adonisjs.com/docs/2.0/lucid-relations), [Sorgular](http://adonisjs.com/docs/2.0/queries)

#### Konsol Komutları

Adonis tıpkı Laravel gibi bazı görevleri gerçekleştirmek için bir komut satırı desteğiyle birlikte gelmektedir.

<pre><code class="language-bash">
$ ./ace make:controller Home
$ ./ace make:model User
</code></pre>

### Avantajlar

- NodeJS tabanlı olması nedeniyle, NodeJS'in PHP'ye göre olan avantajlarının hepsinden yararlanabilirsiniz. (Hızlı, daha hızlı büyüyen üçüncü parti kütüphane havuzu vb.)

- Laravel'e benzedeği için daha önce Laravel kullanan geliştiriciler oldukça hızlı adapte olabilirler.

- ES6 destekli olduğu için, yazdığınız kodların ExpressJS ya da diğer NodeJS frameworklerine göre göreceli olarak daha düzgün olması muhtemeldir. (Ancak sonuçta bir insan geliştirme yapacak, mükemmellik garantisi söz konusu olamaz.)

### Dezavantajlar

- Geliştirdiğiniz uygulamanın test edilmesi için kullanılabilecek yardımcı araçlar Laravel'de olduğu gibi yoktur.

- Henüz belirli bir community oluşmamıştır. Resmi dokümantasyonun yanında ek kaynaklar sınırlıdır.

- Aktif olarak geliştirme yapan developer sayısı oldukça azdır. GitHub'da bulunan [çekirdek repoda](https://github.com/adonisjs/adonis-framework/network/members) sadece 20 adet **fork** alınmıştır. Görebildiğim kadarıyla sadece [Harminder Virk](https://github.com/thetutlage) tarafından geliştirilmektedir.

### Sonuç

Benim görüşlerime göre ***gelecek vaadeden***, ancak ***zamana*** ve ***desteğe*** ihtiyacı olan bir framework denemesidir. Eğer zaman içerisinde ihtiyacı olan desteği diğer developerlardan bulabilir ve geliştirmeler ivme kazanırsa, NodeJS topluluğu tarafından sahiplenilecektir. 

### Kaynaklar

- [Adonis Resmi Site ve Dokümantasyon](http://adonisjs.com)
- [Adonis Core Repo](https://github.com/adonisjs/adonis-framework)
- [Adonis Application Repo](https://github.com/adonisjs/adonis-app)
- [Screen Casts](https://www.youtube.com/playlist?list=PLWmIA5YpCsizOMoM3tH5NSp1sHmdzVLvW)

