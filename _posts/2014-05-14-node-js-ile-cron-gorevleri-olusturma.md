---
layout: post
title: "Node.js İle Cron Görevleri Oluşturma"
date: 2014-05-14 21:00
categories: [Turkish, Coding]
keywords: nodejs, cron, task, görev
author: Özgür Adem Işıklı
---

**Cron** görevlerini duyduğunuzu varsayıyorum. ([Detay](http://en.wikipedia.org/wiki/Cron)) Unix gibi işletim sistemlerinde zaman bazlı görev listesi olarak tanımlamak doğru olur. PHP ile geliştirme yaparken, belirlediğimiz zamanlara göre çalışmasını istediğimiz kodların crontab üzerinde tanımlayarak tetiklerdik. Böylece istediğimiz zaman istediğimiz PHP kodunu otomatik olarak çalıştırabilirdik.

Son dönemde geleceği parlak olan **Node.JS** ile uğraşmaktayım. Yine çalışmalarım sonunda görevlenmiş komutlarım olduğunda, bunu Node.JS ile nasıl yapacağımı araştırdım ve Node.JS için hazırlanan çok güzel bir kütüphane ile karşılaştım: Node-Cron ([GitHub](https://github.com/ncb000gt/node-cron))

Bu kütüphane aracılığı ile yazdığınız kodlar içinde cron görevi tanımlayabiliyorsunuz. PHP geliştiricileri için bu alışılmadık bir durum. Çünkü PHP kodunun tetiklenmesi yani çalıştırılması gerekmektedir. Bu nedenle crontab görevleri işletim sistemi üzerinde tanımlanır. Ancak Node.JS sürekli çalışan bir uygulama gibidir. Siz bir kez çalıştırdıktan sonra aynı uygulama yeniden başlatılana kadar ya da bir hata dolayısıyla kapatılana kadar çalışır ve isteklere cevap verir. Node.JS’nin bu yapısından faydalanılarak ve yanına JavaScript‘in asenkron mimarisi eklenerek Node-Cron kütüphanesi geliştirilmiştir.

### Kurulum

NPM (Node Packaged Modules) sayesinde aşağıdaki şekilde kurulur;

<pre><code class="language-bash">
$ npm install cron
</code></pre>

### Kullanım

Aşağıdaki kodu bir kere çalıştırdığınızda cron göreviniz hazır hale getirilir.

<pre><code class="language-javascript">
var CronJob = require('cron').CronJob;
new CronJob('* * * * * *', function(){
    console.log('Bu yazıyı her saniye göreceksiniz.');
}, null, true, "Europe/Istanbul");
</code></pre>

Burada dikkat etmeniz gereken, bu kodu tekrar tekrar çalıştırmamaktır. Kodu her çalıştırdığınızda yeni bir cron görevi oluşturulur. Belirlediğiniz cron zamanına göre de fonksiyon içerisindeki kodlarınız çalışacaktır. Ben kendi uygulamalarımda bunu basit bir değişkenle kontrol ediyorum. Eğer bu kontrolü gerçekleştirmezseniz, tekrar tekrar cron görevi oluşturulmuş olur. Örneğin aşağıdaki gibi bir uygulama yazdığımızı varsayalım;

<pre><code class="language-javascript">
"use strict";
 
var http = require("http");
http.createServer(processRequest).listen(1234, "127.0.0.1");
 
function processRequest (request, response) {
    
    var CronJob = require('cron').CronJob;
    new CronJob('* * * * * *', function(){
        console.log('Bu yazıyı her saniye göreceksiniz.');
    }, null, true, "Europe/Istanbul");
 
    response.writeHead(200, {"Content-Type": "text/html"});
    response.write("Cron görevimiz ayarlandı.");
    response.end();
    
}
</code></pre>

Bu kodları çalıştırmak için konsol üzerinden `node index.js` komutunu verdikten sonra [localhost:1234](http://localhost:1234) adresini tarayıcımız üzerinden çağırmamız gerekir. Daha sonrasında ise cron mesajımız konsol üzerine her saniye yeniden yazılacaktır. Ancak aynı sayfayı bir daha çağırırsak yeni bir cron görevi set etmiş oluruz. Yukarıda da belirttiğim gibi Node.JS, PHP gibi bir iş yapabilmek için bir yerden direktif bekleyen bir sistem değildir. Uygulama çalıştırıldığında, tek thread üzerinde hazır olarak bekler. Yani konsol üzerinden uygulama kapanana kadar ayarlanan değerler, değişkenler aynen korunur. Bu nedenle kodlarımızı aşağıdaki gibi değiştiriyoruz.

<pre><code class="language-javascript">
var http = require("http");
var CronJob = false; // Cron görevimizi henüz set etmedik
http.createServer(processRequest).listen(1234, "127.0.0.1");
 
function processRequest (request, response) {
 
    response.writeHead(200, {"Content-Type": "text/html"});
 
    // Cron görevimizi kontrol ediyoruz
    if (CronJob === false) {
        CronJob = require('cron').CronJob;
        new CronJob('* * * * * *', function(){
           console.log('Bu yazıyı her saniye göreceksiniz.');
        }, null, true, "Europe/Istanbul");
        response.write("Cron görevimiz ayarlandı.");
    } else {
        response.write("Daha önce cron görevimiz çalıştırıldı.");
    }
 
    response.end();
 
}
</code></pre>

Kodlarımızın yeni halinde, genel bir değişkenimiz içinde Cron görevi durumunu tutarak, sadece bir kez görev tanımlama işkemini gerçekleştiriyoruz. Böylece ikinci bir istek yaptığınızda tekrardan Cron görevi oluşturulmuyor.

### Sonuç

Her ne kadar bu yazımda Cron görevlerinin Node.JS ile tanımlanmasını anlatmak istesem de, genel olarak Node.JS çalışma yapısına da değinmem gerekti. Lakin bu örnek aynı zamanda Node.JS’in çalışmasını anlamak açısından da oldukça önemli bir örnektir. Çünkü alışageldiğimiz tarzda, her istek anında yeniden tüm parametrelerin set edilmesi gibi bir işlem Node.JS üzerinde bulunmamaktadır. Bu da Node.JS’nin performans konusunda öne çıkmasını sağlayan bir nedendir.
