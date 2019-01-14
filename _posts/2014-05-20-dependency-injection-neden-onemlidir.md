---
layout: post
title:  "Dependency Injection Neden Önemlidir?"
date:   2014-05-20 21:00
categories: [Turkish, Coding]
tags: yazılım, geliştirme, dependency, injection, bağımlılık
meta: yazılım, geliştirme, dependency, injection, bağımlılık
author: ozziest
---

**Dependency Injection** Türkçe’ye çevrildiğinde genelde **Bağımlılık Enjeksiyonu** gibi garip bir kavram ortaya çıkıyor. Bu kavramın zor anlaşılmasındaki esas sorun, her tür teknik bilgiyi birebir çevirmedeki inadımız diyebiliriz. Hatta Enjeksiyon kelimesinin de Fransızca olduğunu düşünürsek, el birliği ile oldukça başarısız bir Türkçeleştirme çalışması yaptığımız aşikardır.

Çevirideki zorlukları bir kenara bırakıp esas konumuza dönersek, bu işin temelinde sınıflarımızın bağımlılığını görürüz. Uygulamamız içerisinde tüm sınıflar bir iş yaparlar. Yaptıkları işlere sorumlulukları diyebiliriz. Örneğin Kullanıcı sınıfı kullanıcı işlemlerini yapar ve içerisinde kullanıcı işlemlerini gerçekleştirecek metotlar barındırır.

<pre><code class="language-php">
class User {
 
   public function insert($name)
   {
      echo 'Kullanıcı kaydedildi.';
   }
 
}
</code></pre>

Ancak bu basit sınıf bir yerden sonra bize yetmez. Yukarıdaki örneğimizi geliştiririz ve yeni kullanıcı kaydı esnasında kullanıcıya e-posta gönderilmesini de isteriz. Başka sınıflar içerisinden de e-posta gönderilebilmesi için de ortak kullanacağımız bir Email sınıfı tanımlarız.

<pre><code class="language-php">
class Email {
 
   public function send()
   {
       echo 'E-Posta gönderildi';
   }
 
}
</code></pre>

Daha sonra User sınıfında aşağıdaki gibi kullanırız.

<pre><code class="language-php">
class User {
 
   public function insert($name)
   {
      echo 'Kullanıcı kaydedildi.';
      $email = new Email;
      $email-&gt;send();
   }
 
}
</code></pre>


Yukarıdaki kodumuzu yazdıktan sonra işlerimiz sarpa sarar. Çünkü **User** sınıfının çalışabilmesi için **Email** sınıfını tanımasına ihtiyacı vardır. Siz User sınıfını alıp başka bir yerde de kullanmak isterseniz, Email sınıfını da kopyalamanız ya da e-posta gönderilen satırları yeniden düzenlemeniz gerekecektir.

Sadece bu da değil. Siz yukarıdaki sınıfı geliştirdiniz ve sistemi yayına aldınız diyelim. **Müşteriler** çok harika isteklerle gelmeye bayılırlar. :) Dediler ki “Adı Ahmet olanlara e-posta, diğerlerine SMS gönderelim.” Bu durumda sizin User sınıfını değiştirmeniz gerekecektir. Ancak bu işlem, sınıfa hem **yeni bir bağımlılık** hem de yeni **if-else** yapıları eklemek demektir. User sınıfı haddinden fazla şey bilmeye başlayacaktır. İlk önce oldukça basit görünen sınıf, müşteriden gelen bir taleple bir anda bambaşka boyutlara taşınabilir.

Bir diğer önemli husus ise Unit Test yazımında karşımıza çıkacaktır. Siz bu User sınıfını test ederken Email sınıfını da çalışır hale getirmek zorundasınız. Eğer Email sınıfında bir hata varsa bu User sınıfını da etkiler. Bu da test yazmanızı zorlaştırır. Email sınıfını taklit de edemezsiniz. Çünkü User ve Email sınıfları artık içli dışlı olmuşlardır. Bu sorunu ortadan kaldırmak için Email sınıfımızın ne olduğunu User sınıfının bilmemesi gerekmektedir.


<pre><code class="language-php">
class User {
 
   private $notification;
 
   public function __construct(Notification $notification)
   {
      $this-&gt;notification = $notification;
   }
 
   public function insert($name)
   {
      echo 'Kullanıcı kaydedildi.';
      $this-&gt;notification-&gt;send();
   }
 
}
</code></pre>

User sınıfımızı yukarıdaki gibi değiştirdik. Dikkat ederseniz sınıfımız içinde new komutu ile bir sınıf oluşturulmuyor. User sınıfının oluşturulması anında bir Email sınıfını dışarıdan Notification aracı olarak kullanmak için gönderebiliriz. Böylelikle User sınıfı Email sınıfını kendisi çağırmıyor, çalışacağı sınıfın ne olduğunu hiç mi hiç bilmiyor. Aşağıdaki şekilde User sınıfını, Notification aracı olarak  kullanabiliriz.

<pre><code class="language-php">
$user = new User(new Email);
</code></pre>

Böylelikle User sınıfına çalışacağı sınıfı biz göndermiş oluyoruz. User sınıfı, bu sınıf bağımlılığından kurtuluyor. Müşteriden gelen değişiklik isteğini düşündüğümüzde, bu yapı bizim için oldukça uygundur. User sınıfını new komutu ile oluşturmadan önce müşteri adını kontrol eder ve ona göre gerekli olan sınıfı kendisine göndeririz. User sınıfı ise kullanıcıya göndereceği mesajı e-posta ile mi yoksa SMS ile mi gönderdiğini asla bilmez. O sadece send() metodunu çağırır. Yani User sınıfı sadece kendi işini yapar. Böylece oldukça basit bir sınıfımız olmuş olur. (KISS, Keep it simple, stupid)

Bu yapıya, bağımlılığı dışarıdan enjekte etme yöntemi denilebilir. Test yazarken ise Email ya da SMS sınıfı, içi boş bir send() metodu ile birlikte taklit edilerek User sınıfı test edilebilir. User sınıfı test edilirken, kendisine gönderilen kütüphaneyi kullanacaktır. Dolayısıyla test anında sadece User sınıfını birlikte çalıştığı sınıflardan ayırarak (izole ederek, yalıtarak) test edebilme olanağımız da olacaktır.