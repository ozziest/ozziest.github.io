---
layout: post
title:  "#1 JS'de Closure Kullanımı"
date:   2016-01-13 18:00
categories: [Turkish, Notes]
tags: js, closure, private data object, closure nedir
meta: js, closure, private data object, closure nedir
author: ozziest
---

> Bu bir kendime not yazısıdır. Daha sonradan unutma ihtimalime karşın kendimce notlar alıyorum. 

Her ne kadar **Closure** kelimesinin **Kaplam** olarak çevirilerine daha önce denk gelmiş olsam da, açıkçası bu kelimeyi tam olarak kendi yazılım lügatımıza aktarabileceğimizi sanmıyorum. [1] Çeviri yönünden ele alındığında oldukça zor bir kelime ve belki de bu yüzden bir çok farklı kaynaktan tam olarak anlamaya çalıştığım bir yapı oldu.

Çeviri tartışmasını bir yana bırakırsak ve nedir bu **Closure** irdelemek istersek şöyle bir cümle kurabiliriz;

> JavaScript dünyasında genellikle olay yakalayıcılarda (event handler) ve ***callback***'lerde **veri mahremiyeti** amacıyla ***Closure*** yapıları kullanıyor. [2]

Bu noktada ***"JavaScript'de neyin mahremiyetinden bahsediyoruz?"*** dediğinizi duyar gibiyim. Bu nedenle biraz kod görelim.

### Veri Mahremiyeti

<pre><code class="language-js">
var User = (name) => {
    
    return {
        get: () => name,
        getFullname: (surname) => name + surname
    };

};

var john = User("John");

console.log(john.name);  // undefined
console.log(john.get()); // John
</code></pre>


Burada oluşturduğumuz `User` isimli objemiz `name` isimli bir parametre alıyor. Ancak bu name ***Closure*** sayesinde dışarıdan ulaşılabilir değil. Bu nedenle `name` değişkeni üzerinden veriyi doğrudan almaya çalıştığımızda geriye ***undefined*** dönüyor. Veriye ulaşabilmemiz mümkün olmadığı için değiştirebilmemiz de mümkün olmuyor. Bu nedenle `User` objesine bağlı ve dışarıya açılan (bizim ulaşabileceğimiz) bir `get` fonksiyonu oluşturuyoruz. Bu fonksiyonun özelliği kendi kapsamının (scope) daha üzerindeki kapsama da (User kapsamına) ulaşabilmesi. Böylelikle sakladığımız veriyi, kendi yazdığımız fonksiyonlarla dilersek dışarı açabiliyoruz.

Bu Closure kullanımı veri mahremiyeti amacıyla kullanım şeklini temsil ediyor.

### Paketleme

***Paketleme*** kelimesi benim uydurduğum (5 dakika evvel) bir kavram. Ancak anlatacağım yapının da genel geçer bir adını bulamadım. Yine öncelikle örneği gösterecek, sonra açıklamasını yapacağım.

<pre><code class="language-js">
const message = (text) => () => text;
const errorMessage = message("An error occurred!");
console.log(errorMessage()); // An error occurred!
</code></pre>

Buradaki örneğimizde genel bir `message` sabiti oluşturuyoruz. Bu sabiti dışarıdan `text` isimli parametreye bir veri göndererek çağırdığımızda sonuç olarak bize bir **fonksiyon** gönderiyor.

Örnekteki amacımız; sabit ve her yerde kullandığımız bir mesajı fonksiyonlaştırmak. 2. satırda elde ettiğimiz `errorMessage` aslında bir fonksiyon ve içerisindeki mesajımız dışarıdan değiştirilemiyor. Fonksiyonu çağırdığımızda ise içerisinde artık değişmez olarak duran mesajımız geriye dönüyor.

### Ön Çağırım

Herhangi bir işi gerçekleştirmek üzere kullandığımız bir fonksiyonun olduğunu varsayalım. Uygulamanızın her yerinden çağırdığınız bu fonksiyona çoğu zaman bazı parametreleri sabit olarak kullanarak çağırıyoruz. Böyle bir durumlarda aşağıdaki yapıyı kullanabiliriz;


<pre><code class="language-js">
const partialApply = (fn, ...fixedArgs) => {
    return function (...otherArgs) {
        return fn.apply(this, fixedArgs.concat(otherArgs));
    };
};

const add = (a, b) => a + b;
const add10 = partialApply(add, 10);

console.log(add10(5));
</code></pre>

Burada yer alan `partialApply` fonksiyonu ilk parametre olak bir başka fonksiyonu alıyor ve devamında dinamik olarak birden fazla parametre gönderilmesine izin veriyor. Sonuç olarak ise geriye bir başka fonskiyon döndürüyor. Bu geriye gönderilen fonksiyon çağırıldığında ise, ilk etapta gönderilen fonksiyon, ilk ve ikinci çağırımda gönderilen parametrelerle birlikte çağırılıyor. Böylece asıl fonksiyonumuz için bir kolay çağırım fonksiyonu elde etmiş oluyoruz. Çok mu karışık? Biraz da çalışma anını adım adım inceleyelim.

`const add = (a, b) => a + b` satırında yer alan `add` fonksiyonu bizim asıl fonksiyonumuz ve iki parametre alıyor. Bir alt satırda yer alan `partialApply(add, 10)` çağırımımızdan geriye bir fonksiyon geliyor, biz bu fonksiyonu `add10` sabiti içerisinde muhafaza ediyoruz. Son satırda `add10(5)` şeklinde elimize gelen bu en son fonksiyonu çağırıyoruz. 

Bu kodla birlikte `partialApply`den bize gelen ve `add10` içerisinde tuttuğumuz fonksiyonı çalıştırmış oluyoruz. Bu fonksiyon içerisinde ilk etapta gönderdiğimiz `add` fonksiyonunu, ilk parametreler ve son parametrelerle birlikte çalıştırmış oluyoruz.

Buradaki amacımız şu; sürekli 10 ile bir sayısı toplamak istediğimizde, 10 ile toplama işlemini yapacak ama aslında gerçek toplama işlemini fonksiyonunu kullanan bir fonksiyon oluşturmuş oluyoruz. Her seferinde `add(10, 5)` yazmak yerine `add10(5)` yazarak aynı işi yapmış oluyoruz.

### Sonuç

Şuan fıkrasına gülünmeyen adam gibiyim belki ama bu benim cehaletimden kaynaklanıyor da olabilir. Bu bir ***kendime not*** yazısı olmasına rağmen neden gerek olduğunu anlatmaya çalışacağım.

Buradaki örnekler basit örnekler ve öğrenmeyi kolay kılması açısından kasıtlı olarak basit seçildiler. Ancak gerçek uygulamalarda işler karışabiliyor ve siz bazı verilerin bazı yerlerden erişilmesini istemeyebiliyorsunuz. Bunu uygulama güvenliği için değil, geliştirme güvenliği amacıyla yapmak istiyorsunuz. Çünkü hangi veriyi değiştirmemeniz gerekeceğini zamanla unutacak ve değiştireceksiniz. 

### Kaynaklar

[[1] JavaScript Kapsam Ve Kaplamları Anlamak](http://www.karalamalar.net/javascript-kapsam-ve-kaplamlari-anlamak/)

[[2] Master the JavaScript Interview: What is a Closure?](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-closure-b2f0d2152b36)
