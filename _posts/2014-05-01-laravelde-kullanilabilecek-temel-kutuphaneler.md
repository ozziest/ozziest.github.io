---
layout: post
title:  "Laravel'de Kullanılabilecek Temel Kütüphaneler"
date:   2014-05-01 21:00
categories: [Turkish, Coding]
tags: php, laravel, paket, kütüphane, library, package
meta: php laravel paket kütüphane library package
author: ozziest
---

Sentry kütüphanesi ile oturum işlemlerini kolayca yönetebilirsiniz. Kayıt olma, oturum açma, şifremi unuttum vb. gibi temel işlevlerinin yanında kullanıcı grupları oluşturarak, her gruba ayrı ayrı yetkiler verebilirsiniz. Kurulumu composer ile gerçekleştirilebilir ve hızlı bir şekilde uygulamanıza adapte edebilirsiniz. Temel veri tabanı yapısı için migrasyonları da hazırdır.

[https://cartalyst.com/manual/sentry/introduction](https://cartalyst.com/manual/sentry/introduction)

## Bootstrap Form Oluşturucu: Bootforms

Bootforms kütüphanesi Form kütüphanesini (GitHub) temel alarak geliştirilmiştir. Form kütüphanesi ile hızlı bir şekilde form oluşturabiliyorken, Bootforms kütüphanesi ile bootstrap tasarım çatısına uygun olarak formlar üretebilmektesiniz. Uygulamalarınızda eğer bootstrap çatısını kullanıyorsanız çok seveceğiniz bir araç.

<pre><code class="language-php">
{{ BootForm::openHorizontal(3, 9) }}
  {{ BootForm::text('First Name', 'first_name') }}
  {{ BootForm::text('Last Name', 'last_name') }}
  {{ BootForm::text('Date of Birth', 'date_of_birth') }}
  {{ BootForm::email('Email', 'email') }}
  {{ BootForm::password('Password', 'password') }}
  {{ BootForm::submit('Submit') }}
{{ BootForm::close() }}
</code></pre>


Yukarıdaki kod öğreği ile bootstrap temelinde bir Horizontal Form oluşturabilmektesiniz. Bu kütüphane de Laravel 4’e composer ile hızlıca eklenebilmektedir.

[https://github.com/adamwathan/bootforms](https://github.com/adamwathan/bootforms)

## Laravel 4 Debugbar

Bu kütüphane PHP Debug Bar ile entegre olarak çalışmaktadır. PHP Debug Bar, tıpkı Firebug gibi uygulamanızın web arayüzünde bir panel oluşturularak debug işlemlerinize yardımcı olmak için tasarlanmıştır. Laravel 4 Debugger ise bu kütüphanin hızlı bir şekilde projenize entegre edilmesidir diyebiliriz. Entegre işlemi tamamlandıktan sonra, uygulamanız arasına serpiştirdiğiniz kodlarla Debug Bar’a mesajlar verdirebilirsiniz.

<pre><code class="language-php">
Debugbar::info($object);
Debugbar::error("Error!");
Debugbar::warning('Watch out..');
Debugbar::addMessage('Another message', 'mylabel');
 
// Try-Catch block
try {
    throw new Exception('foobar');
} catch (Exception $e) {
    Debugbar::addException($e);
}
</code></pre>

[https://github.com/barryvdh/laravel-debugbar](https://github.com/barryvdh/laravel-debugbar)

## Laravel-Guard

Bu kütüphane ile assets bileşenleriniz (js, css vb.) anında derlenir ve boyutları ufaltılarak tek bir bileşen haline getirilir. Onlarca CSS dosyanız olsa dahil kullanıcıya tek bir bütünleşik ve boyutu ufaltılmış (minified) CSS dosyası gönderilir. Böylece uygulamanızda performans artışı sağlamış olursunuz.

[https://github.com/JeffreyWay/Laravel-Guard](https://github.com/JeffreyWay/Laravel-Guard)

