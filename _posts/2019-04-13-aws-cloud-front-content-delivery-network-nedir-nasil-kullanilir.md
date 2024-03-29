---
layout: post
title: "Amazon CloudFront Nedir ve Nasıl Kullanılır?"
date: 2019-04-13 15:00
categories: [Turkish, Coding]
keywords: aws, amazon, cloudfront, s3, cdn, spa
author: Özgür Adem Işıklı
post_img: 20.jpg
post_img_link: https://pixabay.com/photos/dock-ship-container-port-boat-1277744
lang: tr
description: Bu makalede AWS CloudFront hakkındaki genel bilgileri ve projelerinizde nasıl kullanabileceğinizi öğrenebilirsiniz.
---

### 1. Motivasyon

Şu an dünya üzerindeki internet kullanıcısı sayısı 4.39 milyardan daha fazla [[1](https://wearesocial.com/blog/2019/01/digital-2019-global-internet-use-accelerates)]. Üstelik bu bir nefeste rahatlıka telaffuz ettiğimiz rakam, her geçen gün hızla artıyor. 2023 yılından sonra internete bağlı olan cihaz sayısının 50 milyarı geçeceği tahmin ediliyor [[2](https://www.statista.com/statistics/471264/iot-number-of-connected-devices-worldwide/)]. İnternet demek, temelde birbirine bağlı cihazların haberleşmesi, bilgi alış verişinde bulunması demektir. Bu nedenle geliştireceğimiz ya da geliştirmekte olduğumuz uygulamarın, kapsamları da önemli olmakla birlikte, bu denli yüksek potansiyel kullanıcıya hizmet verebilmesi oldukça ciddi bir sorun olmaya başlıyor.

Yazılım geliştirme ekosisteminde ölçeklendirme, yukarıdaki paragrafta bahsettiğimiz bu sorunun çözümü için gerekli olan yöntem ve tekniklerin bütünü olarak nitelendirilebilmektedir. WikiPedia'da kullanılan tanıma göre **_ölçeklendirme; artan iş yüküne cevap verebilmek için mevcut sisteme daha fazla kaynak eklenmesi özelliğidir_** [[3](https://en.wikipedia.org/wiki/Scalability)]. Web sunucuları özelinde mesele ele alındığında, web sunucusunun daha fazla isteğe cevap verebilmesi beklenmektedir. Ölçeklendirmeyi yatay ve dikey (horizontal,vertical) olarak iki farklı türde yapabiliriz. Ya mevcut bir sunucuya direkt olarak daha fazla donanımsal kaynak ekleriz ya da daha aynı sunucunun daha fazla yan yana koyarak isteklere sırayla cevap vermelerini bekleriz.

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

Kısa adıyla **S3**, uzun adıyla **Simple Storage Service**; her türlü dosyalarımızı barındırmak için Amazon tarafından sağlanan alandır. Bir **S3 Bucket** yaratarak, oluşturulan bölüm altına dosyalarımızı yükleyebiliriz. Tıpkı Dropbox gibi **_AWS Console_** üzerinden bir arayüzle yükleme yapabileceğimiz gibi, **_aws cli_** marifetiyle de komut satırı üzerinden ya da doğrudan Amazon API'larını kullanarak programatik olarak da dosyalarımızı **_Bucket_** içerisine yükleyebiliriz. Bu örneğimizde doğrudan Amazon'un arayüzlerini kullanacağız.

Öncelikle **_AWS Console_** üzerindeki hesabınızla oturum açabilirsiniz. Daha sonra **_Services_** menüsü altındaki **_Storage_** bölümü altında, **_S3_** servisini seçin.

**_Create bucket_** butonu ile yeni bir S3 oluşturma penceresine ulaşabilirsiniz. Oluşturacağımız Bucket'a tekil bir isim vermeniz gerekmektedir. Örnek uygulamamız için lokasyon seçeneğimiz önemli değil.

<img src="/images/posts/22.jpg" />
<p class="img-description">Resim 2 - Create Bucket</p>

**_Configure Options_** bölümündeki ayarları olduğu gibi bırakabiliriz. Ancak oluşturacağımız bucket içerisindeki dosyalara tüm kullanıcıların ulaşabilmesi için **_Set Permissions_** ekranındaki ayarların aşağıdaki gibi olması gerekmektedir.

<img src="/images/posts/23.jpg" />
<p class="img-description">Resim 3 - Create Bucket, set permissions</p>

**_Review_** adımında seçtiğimiz yapılandırmanın bir önizlemesi gösterilecektir. **_Create Bucket_** butonuna basarak bucket'ımızı oluşturabiliriz. Artık CloudFront'un kullanabileceği bir S3 Bucket'ına sahibiz.

<img src="/images/posts/24.jpg" />
<p class="img-description">Resim 4 - S3 Bucket List</p>

Yukarıda da bahsettiğimiz gibi dosyaları doğrudan bu ekran üzerinden yükleyebiliriz.

> Örnek uygulama gerçekleştirdiğimiz için aşağıdaki adımlarda manuel yüklemeleri detaylıca anlatacağız ancak DevOps teknikleri kullanılarak dosyaların programatik olarak yüklenmesi daha sağlıklı olacaktır.

**_ozziest-test-s3_** ismini verdiğimiz bucket'a tıkladığımızda, **_Upload_** butonun göreceksiniz. Bir önceki adımda oluşturduğumuz `index.html` isimli dosyayı seçip ilerleyebiliriz. **_Set Permissions_** adımında aşağıda gösterildiği gibi dosya için public erişim vermemiz gerekmektedir. Aksi takdirde dosyayı son kullanıcılar görüntüleyemez.

<img src="/images/posts/25.jpg" />
<p class="img-description">Resim 5 - Upload to Bucket, Set Permissions</p>

**_Set Properties_** ve **_Review_** ekranlarında herhangi bir değişiklik yapmamız gerekmiyor. **_Upload_** butonuna tıklayarak dosya yükleme işlemini tamamlayabiliriz.

> Amazon'un S3 üzerindeki dosyalarınız için sizden saklama ve erişim ücreti talep edeceğini lütfen unutmayın.

Ancak tüm dosyalarımızın bir web uygulaması gibi sunulabilmesi için son bir ayar daha yapmamız gerekiyor. Bucket içerisindeyken **_Properties_** sekmesine ulaşarak **_Static Website Hosting_** seçeneğini aktif hale getirmemiz gerekiyor.

<img src="/images/posts/29.jpg" />
<p class="img-description">Resim 6 - S3 Bucket, Static Website Hosting</p>

#### 5.3. CloudFront Yapılandırması

CloudFront yapılandırması için **_Services_** menüsü altında bulunan **_Networking&Content Delivery_** bölümünde, CloudFront'u açabiliriz. **_Create Distribution_** butonu aracılığı ile yeni bir dağıtım oluşturmak istediğimizi belirtiyoruz. Dağıtım tipini seçmemiz istediğinizde, **_Web_** bölümü altında bulunan **_Get Started_** butonuna tıklıyoruz.

**_Origin Settings_** altında bulunan **_Origin Domain Name_** altında oluşturduğumuz S3 Bucket'ını görebilirsiniz. Ek olarak **_Default Root Object_** alanına, doğrudan index.html dosyamızın gösterilmesi gerektiğiniz söylemeliyiz. **_Create Distribution_** butotuna tıklayarak dağıtım oluşturma işlemini tamamlayabiliriz.

<img src="/images/posts/26.jpg" />
<p class="img-description">Resim 7 - CloudFront, Origin Settings</p>

Artık CloudFront sayfasında dağıtımımızı görebiliyoruz. **_In Progress_** olarak gözüken dağıtımımız tüm lokasyonlarda yayına hazırlanıyor demektir.

<img src="/images/posts/27.jpg" />
<p class="img-description">Resim 8 - CloudFront, In Progress</p>

Bu aşamadan sonra **_Domain Name_** altında belirtilen adres üzerinden dosyalarımıza ulaşabiliriz.

<img src="/images/posts/28.jpg" />
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
