---
layout: post
title: "Sonuçlanmayan Ajax İsteklerinin Çözümü"
date: 2012-09-24 19:00
categories: [Turkish, Coding]
keywords: ajax, timeout sorunu, timeout çözümü
author: Özgür Adem Işıklı
lang: tr
description: Ajax isteklerinde meydana gelen timeout sorunun çözümü nasıl olabilir?
---

Ajax istekleri demek sistemin daha hızlı çalışması ve sistem kaynaklarının etkili kullanılması demektir. Ajax’la ilgili bir önceki yazımda Zaman Aralıklı Ajax İsteği Hataları‘nı anlatmıştım ve çözüm yollarından bahsederek sisteminizi nasıl stabil hale getirebileceğinizden bahsetmiştim. Şimdi ise bir başka olası ajax hatasından bahsetmek istiyorum; sonuçlanmayan ajax isteklerinden sonra yapılacak işlemler.

Bir ajax isteğinizde bir sorgu çalıştırıp sonrasında gelen sonucu ekrana yazan bir bölümünüz olsun. Eğer bağlantı kopması, sunucunun cevap vermemesi vb. nedenlerle ajax isteği sonuçsuz kalırsa kullanıcı tarafında gereksiz bir bekleme oluşacaktır. Bu gibi olası hataları engellemek için JQuery‘nin ajax isteklerinde timeout ve error özelliklerinden yararlanabilirsiniz. Timeout parametresiyle ajax’ın sonucu için ayrılacak maksimum süreyi belirleyebilir ve olası hata durumlarında error fonksiyonu oluşturarak hataya göre işlem yapabilirsiniz.

<pre><code class="language-js">
$.ajax({method: "post",
        url: "ornek.php",
        dataType: "json",
        timeout: 5000,
        success: function(){
            alert('sonuçlandı');
        },
        error: function( objAJAXRequest, strError ){
            alert("Hata! Tip: " + strError);
        }
});
</code></pre>

Yukarıdaki kod bloğunda ajax çağrısının timeout süresi 5 sn. olarak belirleniyor ve bu süre zarfında eğer istek sonuçlanmazsa yani bir hata oluşursa error fonksiyonu çalıştırılıyor. Fakat JQuery’nin eski bazı sürümlerinde timeout’dan hatasından sonra hata yakalama fonksiyonu çalışmamaktadır. Bu noktaya dikkat etmeniz ve JQuery versiyonunuzu güncel gerekmektedir.
