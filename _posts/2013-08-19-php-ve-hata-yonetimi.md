---
layout: post
title:  "PHP ve Hata Yönetimi"
date:   2013-08-19 19:00
categories: [Turkish, Coding]
tags: php ve hata yönetimi, bende çalışıyor
meta: php ve hata yönetimi, bende çalışıyor
author: ozziest
---

Geliştirdiğimiz uygulamalar hiçbir zaman hatasız olmayacak. Ancak hata dediğimiz zaman, bizim bunları ikiye ayırmamız gerekiyor;

- Kullanıcının gerçekleştirdiği hatalı işlemler.
- Yazılımdan kaynaklanan hatalar.

Kullanıcı hataları yanlış bir linki açmaya çalışmak da olabilir, form verileri üzerindeki bilgilerin tarayıcı aracılığı ile düzenlenip gönderilmeye çalışılması da olabilir. Bizim bu ve benzer durumlarda kullanıcı için anlamlı mesajları kullanıcılara iletmemiz gerekiyor.

Bir işlemi ele alalım; kullanıcı kayıt işlemi. Bu işlem gerçekleştirilirken bir metot çağırdığımızı düşünelim. PHP camiasında genellikle bir metotun işini doğru yapıp yapmadığını boolean tipinde döndürdüğü cevaba bakarak anlarız. Ama bunu kesinlikle yapmayın.

Çünkü bir metot ilk geliştirme anında yalnızca iki farklı sonuç döndürüyor olabilir. (true/false) Ancak zamanla siz o metota tekrar müdahale edeceğiniz için (solid prensiplerine hiç takmadığınızı varsayıyorum) durumlar değişebilir. Durumlar değiştikçe ikincil bir çözüm yoluna gidiliyor; hata kodları döndürmek. Eğer başarılı ise 0, e-posta adresi eksik ise 1, e-posta adresi zaten kayıtlı ise 2 gibi hata kodları geriye döndürülüyor. Bu ilkinden daha büyük bir yanlıştır. Çünkü metotu çağırdığınız yerde bu sefer hata kodlarını kontrol etmek ve ona göre kullanıcıya bir mesaj göstermek zorunluluğunuz ortaya çıkacaktır.

Bilmeyenler ve bilip de umursamayanlar için söylüyorum: PHP’de de diğer dillerde olduğu gibi Exception dediğimiz bir kavram var ve oldukça kullanışlı. Bir yerde bir hata söz konusu olacaksa ne true/false ne de hata kodu (1, 2, 3 vb.) geriye döndürmeyin. Exception kullanın. Exception bu gibi işler için var.

Aynı örneğimizi ele alalım. Kullanıcı kaydı anında çıkabilecek her hatada Exception fırlatın ve metotu çağırdığınız yerde Exception oluşması durumunda, Exception mesajını alıp her ne ise kullanıcıya gösterin. Bu saatten sonra dilediğiniz kadar hata mesajı fırlatabilirsiniz. Tekrar tekrar metottan ne dönmüş diye kontrol etmek zorunda kalmazsınız.

Burada başka bir soruna da göz atmak gerekir. Şahsen ben iki türde hata mesajı kullanılıyorum. Birincisi, kullanıcıya göstermem gereken her mesaj için Exception sınıfından genişletilmiş UserException sınıfını ve yazılımsal hatalar için doğrudan Exception sınıfını. Bu benim ne işime yarıyor? UserException’da izleyeceğim yol belli. Bu hata türlerinde “Kullanıcı sıçmıştır”. Bu nedenle kullanıcıya hata ne ise gösterir, kendi işime bakarım. Diğer oluşan her türlü Exception’da “Yazılım sıçmıştır”. Bu durumda da hata ne ise loglarım, kullanıcıya “Bir hata meydana geldi.” gibi yazılımın hatasını gizleyen bir mesaj veririm. Logları sürekli inceleyerek, kullanım anında ortaya çıkan bu hiç beklenmedik durumlardan haberdar olurum ve hepsini ortaya çıktıkça çözerim.

### Sonuç

**@** işareti ile olası hataları gizlemek, true/false gibi boolean tipte değerler döndürmek, hata kodları ile hata durumlarını geriye döndürmek asla yapmamanız gereken hatalar. Bunlardan uzak durun. Yazılımınıza asla güvenmeyin. Mutlaka sizin görmediğiniz hatalar çıkacaktır. Bunları sürekli loglayın ve sürekli olarak logları inceleyin. Kullanıcı bile ne olduğunu anlamadan hatayı çözme şansınız var. Aksi halde müşteri sizi arar, “Kullanıcı kayıt olamıyor” der ve siz saatlerce hatayı ararsınız. Log yoksa elinizde sorun nerededir bilemezsiniz. Sonra cevabınız şu olur;

***“Ama ben de çalışıyor…”***

