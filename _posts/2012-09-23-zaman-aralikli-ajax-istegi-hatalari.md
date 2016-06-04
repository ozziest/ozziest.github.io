---
layout: post
title:  "Zaman Aralıklı Ajax İsteği Hataları"
date:   2012-09-23 19:00
categories: js
tags: ajax, periyodik ajax istekleri
meta: ajax, periyodik ajax istekleri
author: ozziest
---

Ajax günümüz web sitelerinin olmazsa olmazlarındandır. Bir çok veriyi kaydederken ya da sorgulama yaparken sayfayı tamamen yenilemek yerinde sadece ilgili bölgede işlem yaparak sonuca hızlı bir şekilde ve sistemi yormadan ulaşmamızı sağlamaktadır.  Bu yöntem sayesinde web sayfalarımız çok daha hızlı ve kullanışlı olmaktadırlar. Gmail’in ve Facebook’un bu kadar çok kullanılmasının sebepleri arasında Ajax yöntemi yer almaktadır.

Ancak bu yöntem kullanılırken gözden kaçan ciddi bir hatadan bahsetmek istiyorum sizlere. Örneğin belirli bir zaman aralığında bir bölgedeki verilerin yenilenmesini istiyoruz. Bu belli bir akış (feed) olabilir ya da o an sistemde var olan kullanıcılar olabilir. Javascript kullanarak belirli bir zaman aralığında Ajax isteği yapmak bu talebimizi karşılayacaktır. Ancak burada dikkat edilmesi gereken çok ciddi bir durum var ki gözden kaçırdığınızda kendi web sitenizi ulaşılmaz/kullanılamaz hale getirebilirsiniz.

Aslında riskli yapı çok basit. Siz 5 saniye aralıklarla sistemde bir Ajax isteği gerçekleştiriyorsunuz. Arka planda kullandığınız dile göre PHP ya da ASP kodlarınız çalışıyor ve bir sonuç üretip size döndürüyor. Bazı durumlarda sisteminiz aşırı yüklenmeden dolayı yavaş çalıştığından cevap süresi sizin zaman döngünüzden fazla olursa, ilk isteğiniz cevaplanmadan ikinci bir istekte bulunmuş oluyorsunuz. Bir kullanıcı çok fazla sorun olmaz elbette ancak web sitenizi yüzlerce hatta binlerce kullanıcının kullandığını düşündüğünüzde ardı ardına yığılmış binlerce istek birikmiş olabilir. Bu istekler çözümlenmezse sisteminiz yavaşlar ve bir süre sonra cevap veremez hale gelir. Bu bir çok hacker’ın kullandığı “servis dışı” yapma metodudur aslında ve siz kendi web sitesinizi ulaşılamaz hale getirmiş olursunuz.

#### Çözüm!

Siz siz olun web sitenizin cevap hızına ne kadar güvenirseniz güvenin periyodik olarak kullanılan ajax sorgularından elinizden geldiğince uzak durun. Eğer kullanmak zorundaysanız, zaman aralığı yerine sonuca göre tekrar ajax çağrısı yapabilirsiniz. Bu bir ajax çağrısı sonuçlandığında otomatik olarak yeniden sorgulama başlatılması demektir ve sizi olası kilitlenmeden kurtarır. Ajax çağrılarınız zamana göre değil, sisteminizin performansına göre bir çalışma hızı belirlemiş olur.  Aşağıda çağrı sonuçlandığında tekrar kendisini çağıran bir JQuery yapısı görülmektedir.


<pre><code class="language-js">
function ajax_cagrisi() {
    $.ajax({
        type: 'POST',
        url: 'ajax.php',
        data: 'param=deger',
        success: function(cevap){
            //    sonuca göre yapılacak işlemler
            ajax_cagrisi();
        }
    });
}
</code></pre>

