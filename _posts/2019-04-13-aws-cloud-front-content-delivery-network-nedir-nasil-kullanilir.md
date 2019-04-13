---
layout: post
title:  "Amazon CloudFront Nedir ve Nasıl Kullanılır?"
date:   2019-04-13 15:00
categories: [Turkish, Coding]
tags: aws, amazon, cloudfront, s3, cdn, spa
meta: aws, amazon, cloudfront, s3, cdn, spa
author: ozziest
---

<a href="https://pixabay.com/photos/dock-ship-container-port-boat-1277744/" target="_blank">
    <img src="/images/posts/20.jpg" class="center" />
</a>

### 1. Motivasyon

Şu an dünya üzerindeki internet kullanıcısı sayısı 4.39 milyardan daha fazla [[1](https://wearesocial.com/blog/2019/01/digital-2019-global-internet-use-accelerates)]. Üstelik bu bir nefeste rahatlıka telaffuz ettiğimiz rakam, her geçen gün hızla artıyor. 2023 yılından sonra internete bağlı olan cihaz sayısının 50 milyarı geçeceği tahmin ediliyor [[2](https://www.statista.com/statistics/471264/iot-number-of-connected-devices-worldwide/)]. İnternet demek, temelde birbirine bağlı cihazların haberleşmesi, bilgi alış verişinde bulunması demektir. Bu nedenle geliştireceğimiz ya da geliştirmekte olduğumuz uygulamarın, kapsamları da önemli olmakla birlikte, bu denli yüksek potansiyel kullanıcıya hizmet verebilmesi oldukça ciddi bir sorun olmaya başlıyor.

Yazılım geliştirme ekosisteminde ölçeklendirme, yukarıdaki paragrafta bahsettiğimiz bu sorunun çözümü için gerekli olan yöntem ve tekniklerin bütünü olarak nitelendirilebilmektedir. WikiPedia'da kullanılan tanıma göre ***ölçeklendirme; artan iş yüküne cevap verebilmek için mevcut sisteme daha fazla kaynak eklenmesi özelliğidir*** [[3](https://en.wikipedia.org/wiki/Scalability)]. Web sunucuları özelinde mesele ele alındığında, web sunucusunun daha fazla isteğe cevap verebilmesi beklenmektedir. Ölçeklendirmeyi yatay ve dikey (horizontal,vertical) olarak iki farklı türde yapabiliriz. Ya mevcut bir sunucuya direkt olarak daha fazla donanımsal kaynak ekleriz ya da daha aynı sunucunun daha fazla yan yana koyarak isteklere sırayla cevap vermelerini bekleriz. 

Ölçeklendirmedeki asıl sorun maliyettir. Bir startup şirketi olarak yeni uygulama geliştirdiğinizi düşünün. Uygulamanızın potansiyel kullanıcısı 1 milyar. Bu potansiyel kullanıcıların hiç bir zaman uygulamanızı kullanmama ihtimali de olmasına karşın, gerçekleştiğinde onlara cevap verebilmeniz gerekmektedir. Eğer tüm ölçeklendirme sorunlarını kendi başımıza çözmeye kalkarsak, çok ciddi yatırımlar yapmak zorundayız. Tam da bu noktada yardımımıza bulut servis sağlayıcılar (Cloud Services Providers) koşmaktadır. 

### 2. Cloud Services Providers

Amazon kimileri için bir e-ticaret sitesinden ibaret olsa da, bizim için ölçeklenebilirlik sorunlarımıza çözüm bulan güçlü bir yardımcımızdır. Kendi e-ticaret ihtiyaçları için kendi sunucularına ciddi rakamlarla yatırımlar yapmış ve ciddi boyuttaki işlem yüklerine cevap verebilmektedir. Bize sağladığı güzellik de kendi imkanlarını bizimle paylaşmasıdır. Tabii olarak bu paylaşım için bir miktar kira talep etmektedir. Ancak güzel tarafı artan taleplerinize göre fiyatlandırma (On Deman Pricing) uygulamasıdır. AWS kullandığınızda, yukarıdaki örneğimizdeki gibi potansiyel kullanıcısı 1 milyar olan bir uygulama geliştirdiğiniz zaman, ilk zamanlarda yüksek miktarlarda yatırım yapmanıza gerek kalmamaktadır. Kullanıcı sayısınız ve doğal olarak iş yükünüz düşükken, daha düşük ücretler ödemeniz bulut servis sağlayıcılarla mümkün olabilmektedir.

> Amazon (AWS), bulut servis sağlayıcısı sunan tek şirket değildir. Google, Microsoft, IBM ve hatta AliBaba da bulut servis sağlayıcıların sundukları bir çok hizmeti sunmaktadır. Ancak bu yazımızda konumuz Amazon CloudFront olduğu için diğer sağlayıcılar göz ardı edilmiştir.

### 3. Amazon CloudFront

Amazon CloudFront, uygulamalarımız için hızlı, güvenli ve programlanabilir CDN (Content Delivery Network) hizmeti sunmaktadır. Gökhan Şengün'ün yaptığı tanıma göre;

> CDN, bir web sitesinin içeriğini en düşük ağ ve işlem gecikmesi ile yani en hızlı bir şekilde kullanıcılara ulaştırmak üzere coğrafi olarak farklı bölgelerde konumlandırılmış sunucu kümesine verilen isimdir. [[4]](https://medium.com/@gokhansengun/cdn-nedir-ve-neden-kullan%C4%B1l%C4%B1r-71f6ffce6133)

Elbette kendimiz de standart bir sunucu ya da sunucular kiralayıp, CDN hizmetini kendimiz de yapılandırabiliriz. Ancak yükümüz arttığında daha çok sunucu kiralamalı ve daha sunucuların yönetimleriyle daha çok ilgilenmeliyiz. Bu da bize maliyet olarak yansıyacaktır. 

Bir başka önemli konu ise uygulamamızın yükünün her zaman aynı olmayacağıdır. Örneğin mesai saatleri içerisinde daha yoğun istek alabilirsiniz. Eğer bir e-ticaret uygulamanız varsa Black Friday gibi indirim günlerinde normalin üzerinde istek almanız kuvvetle muhtemeldir. Eğer CDN sunucularımızı kendimiz yönetiyor olsaydık, maliyetlerimizi minimal tutmak için çok ciddi eforlar sarf etmemiz gerekecekti. Amazon CloudFront bizi tüm bu dertlerden kurtarmaktadır. 

### 4. Nasıl Çalışır?

<img src="/images/posts/21.png" class="center" />
<p class="img-description">Resim 1 - Amazon CloudFront Schema</p>

Temel olarak dosyalarımızı bir kaynağa koymamız gerekmektedir. Daha sonra Amazon CloudFront bu kaynaktaki dosyaları, farklı lokasyonlardaki sunuculara yerleştirir ve bir domain üzerinden son kulanıcıya sunmaya başlar. Siz yeni dosyalar ekledikçe ya da dosyalarınızı güncelledikçe, bu dosyaları ilgili lokasyonlarda günceller. İstek yapan kullanıcının lokasyonuna göre en hızlı yanıt verebilecek sunucu yanıt döner. Ne kadar çok istek alırsanız alın, Amazon sizin için yanıt verme işlemini otomatik olarak gerçekleştirir. Bunun yanında oldukça kullanışlı bir Cache mekanizması da sunmaktadır. 

Tüm bunların karşılığında gerçekleştirilen istek başına bir ücret talep etmektedir. 

### 5. Uygulama

Gerçekleştireceğimiz bir örnekle kullanımı biraz daha yakından inceleyeceğiz. Sırasıyla yapacaklarımız;

- Basit bir web uygulaması geliştirmek. (index.html, hello world!)
- Dosyalarımızı koyacağımız S3 Bucket yapılandırmasını yapmak.
- Amazon CloudFront entegrasyonunu tamamlamak.

#### 5.1. Web Uygulaması

Aşağıdaki şekilde basit bir web uygulamamız olduğunu varsayalım;

```html
<html>
    <body>
        <h1>Hello World!</h1>
    </body>
</html>
```

#### 5.2. S3 Bucket Yapılanmdırması

Kısa adıyla **S3**, uzun adıyla **Simple Storage Service**; her türlü dosyalarımızı barındırmak için Amazon tarafından sağlanan alandır. Bir **S3 Bucket** yaratarak, oluşturulan bölüm altına dosyalarımızı yükleyebiliriz. Tıpkı Dropbox gibi ***AWS Console*** üzerinden bir arayüzle yükleme yapabileceğimiz gibi, ***aws cli*** marifetiyle de komut satırı üzerinden ya da doğrudan Amazon API'larını kullanarak programatik olarak da dosyalarımızı ***Bucket*** içerisine yükleyebiliriz. Bu örneğimizde doğrudan Amazon'un arayüzlerini kullanacağız.

Öncelikle ***AWS Console*** üzerindeki hesabınızla oturum açabilirsiniz. Daha sonra ***Services*** menüsü altındaki ***Storage*** bölümü altında, ***S3*** servisini seçin.

***Create bucket*** butonu ile yeni bir S3 oluşturma penceresine ulaşabilirsiniz. Oluşturacağımız Bucket'a tekil bir isim vermeniz gerekmektedir. Örnek uygulamamız için lokasyon seçeneğimiz önemli değil.

<img src="/images/posts/22.JPG" class="center" />
<p class="img-description">Resim 2 - Create Bucket</p>

***Configure Options*** bölümündeki ayarları olduğu gibi bırakabiliriz. Ancak oluşturacağımız bucket içerisindeki dosyalara tüm kullanıcıların ulaşabilmesi için ***Set Permissions*** ekranındaki ayarların aşağıdaki gibi olması gerekmektedir.

<img src="/images/posts/23.JPG" class="center" />
<p class="img-description">Resim 3 - Create Bucket, set permissions</p>

***Review*** adımında seçtiğimiz yapılandırmanın bir önizlemesi gösterilecektir. ***Create Bucket*** butonuna basarak bucket'ımızı oluşturabiliriz. Artık CloudFront'un kullanabileceği bir S3 Bucket'ına sahibiz.

<img src="/images/posts/24.JPG" class="center" />
<p class="img-description">Resim 4 - S3 Bucket List</p>

Yukarıda da bahsettiğimiz gibi dosyaları doğrudan bu ekran üzerinden yükleyebiliriz. 

> Örnek uygulama gerçekleştirdiğimiz için aşağıdaki adımlarda manuel yüklemeleri detaylıca anlatacağız ancak DevOps teknikleri kullanılarak dosyaların programatik olarak yüklenmesi daha sağlıklı olacaktır. 

***ozziest-test-s3*** ismini verdiğimiz bucket'a tıkladığımızda, ***Upload*** butonun göreceksiniz. Bir önceki adımda oluşturduğumuz `index.html` isimli dosyayı seçip ilerleyebiliriz. ***Set Permissions*** adımında aşağıda gösterildiği gibi dosya için public erişim vermemiz gerekmektedir. Aksi takdirde dosyayı son kullanıcılar görüntüleyemez.

<img src="/images/posts/25.JPG" class="center" />
<p class="img-description">Resim 5 - Upload to Bucket, Set Permissions</p>

***Set Properties*** ve ***Review*** ekranlarında herhangi bir değişiklik yapmamız gerekmiyor. ***Upload*** butonuna tıklayarak dosya yükleme işlemini tamamlayabiliriz. 

> Amazon'un S3 üzerindeki dosyalarınız için sizden saklama ve erişim ücreti talep edeceğini lütfen unutmayın. 

Ancak tüm dosyalarımızın bir web uygulaması gibi sunulabilmesi için son bir ayar daha yapmamız gerekiyor. Bucket içerisindeyken ***Properties*** sekmesine ulaşarak ***Static Website Hosting*** seçeneğini aktif hale getirmemiz gerekiyor.

<img src="/images/posts/29.JPG" class="center" />
<p class="img-description">Resim 6 - S3 Bucket, Static Website Hosting</p>

#### 5.3. CloudFront Yapılandırması

CloudFront yapılandırması için ***Services*** menüsü altında bulunan ***Networking&Content Delivery*** bölümünde, CloudFront'u açabiliriz. ***Create Distribution*** butonu aracılığı ile yeni bir dağıtım oluşturmak istediğimizi belirtiyoruz. Dağıtım tipini seçmemiz istediğinizde, ***Web***  bölümü altında bulunan ***Get Started*** butonuna tıklıyoruz. 

***Origin Settings*** altında bulunan ***Origin Domain Name*** altında oluşturduğumuz S3 Bucket'ını görebilirsiniz. Ek olarak ***Default Root Object*** alanına, doğrudan index.html dosyamızın gösterilmesi gerektiğiniz söylemeliyiz. ***Create Distribution*** butotuna tıklayarak dağıtım oluşturma işlemini tamamlayabiliriz. 

<img src="/images/posts/26.JPG" class="center" />
<p class="img-description">Resim 7 - CloudFront, Origin Settings</p>

Artık CloudFront sayfasında dağıtımımızı görebiliyoruz. ***In Progress*** olarak gözüken dağıtımımız tüm lokasyonlarda yayına hazırlanıyor demektir. 

<img src="/images/posts/27.JPG" class="center" />
<p class="img-description">Resim 8 - CloudFront, In Progress</p>

Bu aşamadan sonra ***Domain Name*** altında belirtilen adres üzerinden dosyalarımıza ulaşabiliriz.

<img src="/images/posts/28.JPG" class="center" />
<p class="img-description">Resim 9 - CloudFront in action</p>

### 6. Sonuç

Artık Amazon CloudFront, S3 üzerinden sunduğumuz tüm dosyaları kullanıcılara ne kadar istek gelirse gelsin ulaştıracaktır. Gerçekleştirdiğimiz CloudFront yapılandırmasında varsayılan ayarları kullandık. Buna göre en iyi erişim seçeneğini kullanarak, dosyalarımız tüm lokasyonlarda barındırılıyor. Ancak tüm bu işlemlerin bir maliyeti olduğunu ve hem S3 hem de CloudFront için ayrı ayrı ücretlere tahsil edileceğini unutmamak gerekiyor. Fakat nihayetinde dosyalarımızı son kullanıcılara ölçeklenebilir bir şekilde aktarmanın rahatlığını yaşıyoruz.


### Referanslar

[[1] Digital 2019: Global Internet Use Accelerates](https://wearesocial.com/blog/2019/01/digital-2019-global-internet-use-accelerates)

[[2] Internet of Things (IoT) connected devices installed base worldwide from 2015 to 2025 (in billions)
](https://www.statista.com/statistics/471264/iot-number-of-connected-devices-worldwide/)

[[3] Scalability](https://en.wikipedia.org/wiki/Scalability)

[[4] CDN Nedir ve Neden Kullanılır?](https://medium.com/@gokhansengun/cdn-nedir-ve-neden-kullan%C4%B1l%C4%B1r-71f6ffce6133)

[[5] Amazon S3](https://aws.amazon.com/s3/)

[[6] Amazon CloudFront](https://aws.amazon.com/cloudfront/)