---
layout: post
title:  "Küçük İş Parçacıkları"
date:   2013-08-12 19:00
categories: [Turkish, Coding]
tags: küçük iş parçacıkları, solid, separation of concerns
meta: küçük iş parçacıkları, solid, separation of concerns
author: ozziest
---

Merhabalar,

PHP’de class kullanmayan artık yok gibidir. Fonksiyonel programlamaya göre avantajlarını sayacak değilim yazı boyunca. Sadece zaman zaman benimde farkında olmadan içine düştüğüm bir hatadan bahsetmek istiyorum bugün.

Örneğin bir blog kodluyoruz ve yönetim panelinde içerik girerken kayıt işlemini AJAX kullanarak yaptığımızı varsayalım. Arayüzde JS aracılığı ile verilerimizi sunucuya gönderdikten sonra, sunucudan tekrar tarayıcıya bir cevap gelmesini bekleriz. Bu aşamada sunucu tarafında eğer ki siz, verileri aldıktan sonra, kontrolünü, kayıt işlemini ve oluşturulacak cevabı (json, xml ya da basit 0-1 türünden bir veri de olabilir.) aynı method içinde yapıyorsanız ciddi bir sorun var demektir. Buradaki sorun, methodun birden fazla iş yapmasıdır.

Eğer kendimizi tekrar etmek istemiyorsak, yaptığımız her iş parçacağını ayrı ayrı methodlara, hatta yerine göre sınıflara bölmeliyiz. Bir çok framework bizim için bu işi ya yapıyor ya da bizi bu şekilde davranmaya zorluyor. Ancak bu tüm frameworkler için geçerli değil. Örneğin CodeIgniter sizi zorlamaz ve eğer siz dikkat etmezseniz, blog içeriği kaydeden methodda aynı anda bir çok işi yaparsınız ve kodunuzun yeniden kullanılabilirliği azalır.

Bir noktadan sonra kullandığınız controllerın hiç bir yönetim işlemi yapmadığının farkına varabilirsiniz. Üstelik binlerce satır da kodlamış olabilirsiniz. Bazen bu hataya ben düşebiliyordum fakat şuanda daha çok dikkat ediyorum. Basit bir anlayışla, **iş yapan bir method eğer içinde kontrol ifadeleri barındırıyorsa yanlış yolda olduğunuzu söyleyebilirim. İş yapan parçacıkların sadece kendi işlerini yapmalarını ve en önemlisi minimum işi yapmalarını sağlayın.**

Edit: [http://en.wikipedia.org/wiki/Separation_of_concerns](http://en.wikipedia.org/wiki/Separation_of_concerns)

