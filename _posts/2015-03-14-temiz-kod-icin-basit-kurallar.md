---
layout: post
title:  "Temiz Kod İçin Basit Kurallar"
date:   2015-03-15 18:00
categories: php
tags: php, temiz kod, basit kurallar, laracast
meta: php temiz kod basit kurallar laracast
author: ozziest
---

Geçtiğimiz Cumartesi günü (14 Mart 2015) [Mavidurak-IO](http://mavidurak.github.io) ve [Ahir Danışmanlık](http://ahir.com.tr) işbirliği ile düzenlenen [Tech Talk #02]() etkinliğinde "Temiz Kod İçin Basit Kurallar" başlıklı bir sunum gerçekleştirdim. Sunumdan ziyade -görece- kötü kod örnekleri göstererek, nasıl daha temiz kod yazabileceğimizi anlatmaya çalıştım. Bu blog yazısı da, sunumda verdiğim örneklerin dökümanlanması amacıyla hazırlandı. 

> Bahsi geçen sunumu, [Laracast](http://laracast.com) üzerinde izlemiş olduğum bir seriden öğrendim ancak tüm seriyi değil, sadece 25 dakikaya sığdırabildiklerimden bahsettim.  Siz serinin tamamını görmek istiyorsanız, **Laracast** üzerindeki "Simple Rules For Simpler Code" başlıklı seriye göz atabilirsiniz. 

## İsimlendirme 

İlk olarak değineceğimiz başlık isimlendirme. Ana önerimiz; ***"Her şeye verdiğiniz isimleri elinizden geldiğinde anlaşılır tutun."***


### Kısaltmalar 

Hemen aşağıdaki kötü isimlendirme örneklerine göz atabilirsiniz;

{% highlight php %}
class Trnslatr {
    

}

class UserRepo {
    

}
{% endhighlight %}

Bu örnekleri incelediğimizde, kısaltmalar kullanıldığını görüyoruz. Yaptığımız kısaltmayı nasıl bir mantıklı yaptığımızı daha sonra hatırlamayabiliriz. Bu nedenle aşağıdaki gibi kısaltma kullanmadan yazacağımız kodlar çok daha **şık** duracaktır;

{% highlight php %}
class Translator {
    

}

class UserRepository {
    

}
{% endhighlight %}

### X ve Y gibi değişkenler

Bir diğer kötü örnek `$x` ya da '$y' gibi sıklıkla kullanılan değişkenlere örnek olarak verilebilir;

{% highlight php %}
foreach ($people as $x) 
{
    echo $x->name;
}
{% endhighlight %}


Bu kullanımda kodumuz çok fazla okunaklı olmamaktadır. Bunun yerine aşağıdaki gibi bir kullanım daha uygun olacaktır;

{% highlight php %}
foreach ($people as $person) 
{
    echo $person->name;
}
{% endhighlight %}

`$x` ve `$y` gibi değişkenleri sadece koordinatları belirttiğimiz anlarda kullanmak bizim yararımıza olacaktır.

### Uzun Metot İsimleri

Aşağıdaki örnekte detaylı bir metot ismi görülmektedir;

{% highlight php %}
class User {

    public function userComments()
    {

    }

}
{% endhighlight %}

Bu kullanım ilk başta güzel gibi gözüksede, ilgili sınıfı muhtemelen aşağıdaki gibi kullanacağızdır; 

{% highlight php %}
$user->userComments();
{% endhighlight %}


Buradaki sorun tekrardan sınıf isminin metotta kullanılmasıdır. Doğrudan aşağıdaki gibi hazırlanacak metotlar daha okunaklı kodumuzun olmasına yardımcı olabilir;

{% highlight php %}
class User {

    public function comments()
    {

    }

    public function getComments()
    {

    }

}
{% endhighlight %}


## If-Else Kullanımı ve Indent'ler

Algoritma geliştirirken if-else olmazsa olmazlarımızdandır. Ancak çoğu durumda `else` bir fazlalıktır. Hemen aşağıdaki örneğimizi inceleyebiliriz;

{% highlight php %}
public function store()
{
    $inputs = Input::all();
    $validation = Validator::make($inputs, ['name' => 'required']);

    // 0. seviye indent
    if (date('l') !== 'Friday')
    {
        // 1. seviye indent
        if ($validdation->passes())
        {
            // 2. seviye indent
            User::create($input);
            return Redirect::home();
        }
        else
        {
            return Redirect::back();
        }
    }
    else
    {
        throw new Exception("Cuma'ya gittik, geleceğiz.");
    }

}
{% endhighlight %}

Örneğimizde sadece Cuma günü dışında kayıt alıyoruz. Tabiki kullanıcının gönderdiği verilerin eksiksiz olmasını da kontrol ediyoruz. Ancak burası basit gibi gözüksede, ileride karışmaya çok müsayit bir yapıdadır. Başka şartlar da eklenirse, içinden çıkılmaz bir hale gelebilir. Bu nedenle aşağıdaki adımlara dikkat ederek kodumuzu yeniden düzenliyoruz;

* Else kullanımından kaçın,
* 1. seviyenin daha ilerisindeki intent'lere girme.
* Asıl işlemi en sona sakla


{% highlight php %}
public function store()
{
    $inputs = Input::all();
    $validation = Validator::make($inputs, ['name' => 'required']);

    if (date('l') === 'Friday')
    {
        throw new Exception("Cuma'ya gittik, geleceğiz.");
    }

    if ($validdation->fails())
    {
        return Redirect::back();
    }

    User::create($input);
    return Redirect::home();

}
{% endhighlight %}

Kodumuz bu hale geldikten sonra, bizim için çok temiz ve okunaklı olacaktır. 

# Sonuç

Tüm bunlar oldukça basit kurallardır ve **kesinlikle** mutlaka uyulması gerekli değildir. Sadece benim katıldığım görüşleri içermektedir. Ne kadar basit kod yazarsanız, üzerinde çalıştığınız proje o kadar sürüdürülebilir olacaktır. 

Mutlu kodlamalar...







