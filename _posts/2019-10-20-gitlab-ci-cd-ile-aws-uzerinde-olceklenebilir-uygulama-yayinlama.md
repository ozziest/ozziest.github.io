---
layout: post
title:  "GitLab CI/CD İle AWS Üzerinde Ölçeklenebilir Uygulama Yayınlama"
date:   2019-10-20 15:00
categories: [Turkish, Coding, Presentation]
tags: gitlab, ci, ci, aws, scaling, runner
meta: gitlab, ci, ci, aws, scaling, runner
author: ozziest
---

<a href="https://pixabay.com/photos/container-port-loading-stacked-3118783/" target="_blank" title="">
    <img class="center" src="/images/posts/31.jpg" class="center" />
</a>

> Bu makaleye konu olan sunumu [MaviDurak-IO](https://kommunity.com/mavidurakio) ve [Sakarya Coders](https://www.meetup.com/Sakarya-Coders/) işbirliği ile [15 Ekim](https://kommunity.com/mavidurakio/events/gitlab-cicd-ile-aws-uzerinde-olceklenebilir-uygulama-yayinlama)'de yapılan bir etkinlikte gerçekleştirdim. Hazır bu kadar belgeyi bir araya getirmişken, üzerinde biraz daha emek vererek bir blog yazısı yazmak mantıklı geldi. Bu makaledeki ilk iki bölüm, daha önceden yazdığım [Amazon CloudFront Nedir ve Nasıl Kullanılır?](https://ozguradem.net/turkish/coding/2019/04/13/aws-cloud-front-content-delivery-network-nedir-nasil-kullanilir/) başlıklı makalemden aynen kopyalanmıştır.

### 1. Motivasyon [1]

Şu an dünya üzerindeki internet kullanıcısı sayısı 4.39 milyardan daha fazla [[1](https://wearesocial.com/blog/2019/01/digital-2019-global-internet-use-accelerates)]. Üstelik bu bir nefeste rahatlıka telaffuz ettiğimiz rakam, her geçen gün hızla artıyor. 2023 yılından sonra internete bağlı olan cihaz sayısının 50 milyarı geçeceği tahmin ediliyor [[2](https://www.statista.com/statistics/471264/iot-number-of-connected-devices-worldwide/)]. İnternet demek, temelde birbirine bağlı cihazların haberleşmesi, bilgi alış verişinde bulunması demektir. Bu nedenle geliştireceğimiz ya da geliştirmekte olduğumuz uygulamarın, kapsamları da önemli olmakla birlikte, bu denli yüksek potansiyel kullanıcıya hizmet verebilmesi oldukça ciddi bir sorun olmaya başlıyor.

Yazılım geliştirme ekosisteminde ölçeklendirme, yukarıdaki paragrafta bahsettiğimiz bu sorunun çözümü için gerekli olan yöntem ve tekniklerin bütünü olarak nitelendirilebilmektedir. WikiPedia'da kullanılan tanıma göre ***ölçeklendirme; artan iş yüküne cevap verebilmek için mevcut sisteme daha fazla kaynak eklenmesi özelliğidir*** [[3](https://en.wikipedia.org/wiki/Scalability)]. Web sunucuları özelinde mesele ele alındığında, web sunucusunun daha fazla isteğe cevap verebilmesi beklenmektedir. Ölçeklendirmeyi yatay ve dikey (horizontal,vertical) olarak iki farklı türde yapabiliriz. Ya mevcut bir sunucuya direkt olarak daha fazla donanımsal kaynak ekleriz ya da daha aynı sunucunun daha fazla yan yana koyarak isteklere sırayla cevap vermelerini bekleriz. 

Ölçeklendirmedeki asıl sorun maliyettir. Bir startup şirketi olarak yeni uygulama geliştirdiğinizi düşünün. Uygulamanızın potansiyel kullanıcısı 1 milyar. Bu potansiyel kullanıcıların hiç bir zaman uygulamanızı kullanmama ihtimali de olmasına karşın, gerçekleştiğinde onlara cevap verebilmeniz gerekmektedir. Eğer tüm ölçeklendirme sorunlarını kendi başımıza çözmeye kalkarsak, çok ciddi yatırımlar yapmak zorundayız. Tam da bu noktada yardımımıza bulut servis sağlayıcılar (Cloud Services Providers) koşmaktadır. 

### 2. Cloud Services Providers [1]

Amazon kimileri için bir e-ticaret sitesinden ibaret olsa da, bizim için ölçeklenebilirlik sorunlarımıza çözüm bulan güçlü bir yardımcımızdır. Kendi e-ticaret ihtiyaçları için kendi sunucularına ciddi rakamlarla yatırımlar yapmış ve ciddi boyuttaki işlem yüklerine cevap verebilmektedir. Bize sağladığı güzellik de kendi imkanlarını bizimle paylaşmasıdır. Tabii olarak bu paylaşım için bir miktar kira talep etmektedir. Ancak güzel tarafı artan taleplerinize göre fiyatlandırma (On Deman Pricing) uygulamasıdır. AWS kullandığınızda, yukarıdaki örneğimizdeki gibi potansiyel kullanıcısı 1 milyar olan bir uygulama geliştirdiğiniz zaman, ilk zamanlarda yüksek miktarlarda yatırım yapmanıza gerek kalmamaktadır. Kullanıcı sayısınız ve doğal olarak iş yükünüz düşükken, daha düşük ücretler ödemeniz bulut servis sağlayıcılarla mümkün olabilmektedir.

> Amazon (AWS), bulut servis sağlayıcısı sunan tek şirket değildir. Google, Microsoft, IBM ve hatta AliBaba da bulut servis sağlayıcıların sundukları bir çok hizmeti sunmaktadır. Ancak bu yazımızda konumuz Amazon CloudFront olduğu için diğer sağlayıcılar göz ardı edilmiştir.

### 3. Problem

Bu makalemiz üzerinde, öncelikle bir problem tanımı yapacağız ve daha sonra bu problemi AWS ve GitLab CI/CD araçlarını kullanarak nasıl çözebileceğimizi göreceğiz;

- NodeJS ile geliştirilmiş bir API’ye sahibim. (Bu makale içerisinde bu API'dan **mavi-api** olarak bahsedeceğiz.)
- Bu uygulamayı bir sunucu üzerinde yayınlamak istiyorum.
- Kaç kullanıcı tarafından kullanılacağını bilmiyorum. Kullanıcı sayısı yüz de olabilir, bir milyon da.
- Çok fazla param yok.
- Yetenekli sistem mimarlarına sahip değilim ve sadece küçük bir geliştirici ekibim var.
- Ekibin deployment ve sunucu konfigürasyonları ile çok fazla vakit kaybetmesini istemiyorum.

Yukarıda tanımladığımız problemin birden fazla çözümü mevcut. Bizim burada anlatacağımız çözüm yöntemi tek bir çözüm yönteminden ibaret olacaktır. Ancak bizim kullandığımız yöntem ve araçlardan çok daha farklı çözüm yollarının da olabileceğini okuyucu aklından çıkarmamalıdır. Tüm çözüm yöntemlerini tek bir makale üzerinde anlatmak oldukça zor bir iştir. Ancak bunun yanında, ben de, tüm çözüm yollarına hakim olan bir sistem mimarı değilim. Bu nedenlerle, biz bu makale içerisinde sadece AWS-GitLab CI/CD araçları ile oluşturulabilecek çözümlerden sadece bir tanesini anlatacağız. Buradaki amacımız sadece ve sadece bu tarz bir problemle karşılan bir ekibe fikir vermekten ibarettir.

### 4. Terminoloji.

Nasıl bu problemi çözeceğimizi göstermeden evvel, öncelikle bazı kelimelerin, teknolojilerin ve kavramların bilinmesinde yarar bulunmaktadır. Yapmaya çalıştığımız iş birçok farklı parçayı ve teknolojiyi içermektedir. Biz burada, sadece bu teknolojilerin genel özelliklerini ve ne amaçla kullandıklarına değinecek ve daha ileri düzeyde bilgiler için sizlere bazı ileri okuma linkleri vereceğiz.

#### 4.1. AWS Nedir?

Dünyanın en kapsamlı ve yaygın kullanılan bulut platformudur. Hızlı büyüyen startup’lar, büyük kuruluşlar ve önde gelen devlet kurumlarının dahil olduğu milyonlarca müşteri; altyapılarını güçlendirmek, daha çevik olmak ve maliyetleri azaltmak için AWS’ye güvenmektedir. [2]

[mobilhanem](https://www.mobilhanem.com) üzerinde Salih Kardan tarafından AWS üzerinde yazılan bir yazıda şöyle bir pasaj geçmektedir;

> *"AWS Servislerini kullanarak birçok yükten kendinizi kurtarmış oluyorsunuz. Sunucuların güvenliği, internet ulaşımı, elektriği vs gibi etkenleri hiç düşünmeden, uygulamanızı geliştirmeye odaklanabilirsiniz. AWS kullanarak uygulamalarınızı kısa süre içerisinde ayağa kaldırabilirsiniz ve kullandığınız kadar ödeme (pay as you go) ile harcamalarınızı azaltabilirsiniz."* [3]

Buradaki tanımlamaya dikkat ederseniz, bizim problem tanımımızda yer alan bir çok sorunun AWS tarafından çözülebildiği özellikle vurgulanmaktadır.

#### 4.2. GitLab Nedir?

GitLab'ın sitesinde yer alan tanım şu şekilde;

> *GitLab yazılım geliştirme ekiplerinin bir yazılımı ortaklaşa geliştirebilmeleri için açık kaynak kodlu bir proje olarak başlatıldı. Endüstriyel bir standart haline gelen GitLab, tüm yazılım geliştirme süreçleri için ortak bir uygulama sunma amacını güdüyor. GitLab uygulamanızı yönetmek, planlamak, oluşturmak, doğrulamak, paketlemek, yayınlamak, konfigüre etmek, izlemek ve güvenli hale getirmek için ihtiyacınız olan her şeyi size tek bir çatı altında sunuyor.* [4]

Burada özellikle dikkat etmemiz gereken nokta **yayınlama** fonksiyonudur. GitLab, ürettiği çeşitli araçlarla birlikte, uygulamamızı yayınlama aşamasında yardımcı olabileceğini söylüyor. Biz de yazının ilerleyen bölümlerde GitLab'ın sağlamış olduğu bu araçlardan yararlanacağız.

#### 4.3. DevOps ve CI/CD Nedir?

DevOps; organizasyonların yüksek hızda yazılım geliştirme ve sunma kabiliyetlerini arttıran uygulamaların, araçların ve bu kültürün bir birleşimidir. [5]

Continuous Integration (CI) ve Continuous Delivery (CD) kavramları sırasıyla; Sürekli Entegrasyon ve Sürekli Teslimat manasına gelmektedir. CI/CD, kodlanan yazılımların durmaksızın derlenmesi, test edilmesi, entegre edilmesi, sürümlenmesi ve yayınlanması sürecine karşılık gelmektedir. [6]

Buradaki iki tanımdan da anlaşılacağı üzere, eğer bir uygulamanın yayınlanması işinin geliştirici ekibi üzerinden yük oluşturmasını istemiyorsak, DevOps kültüründen ve CI/CD araç ve yöntemlerinden yararlanmamız gerekiyor. Biz de GitLab'ın bize sunduğu bu araçlardan bir tanesi olan [GitLab Runner](https://docs.gitlab.com/runner/)'dan yararlanacağız.

GitLab Runner, GitLab’tan tamamen izole bir şekilde, Go dili kullanılarak MIT Lisansı ile geliştirilmiş ve GNU/Linux, macOS, FreeBSD ve Windows üzerine kurulabilen ve bizim tarafımızdan tanımlanan CI/CD pipeline'larını işletmekten ve sonuçları API üzerinden GitLab'a sunmaktan sorumlu olan bir araçtır. [6]

GitLab Runner’ı herhangi bir makineye kurabilirsiniz. Söz konusu makine DigitalOcean üzerinde kiralayacağınız basit bir sunucu da olabilir, kendi bilgisayarınız da. GitLab sunucusunun, GitLab Runner ile aynı makine üzerinde bulunmak gibi bir zorunluluğu yoktur.

İlave olarak, GitLab doğrudan ücretsiz hesapların da kullanabileceği Shared Runner‘lar sunmaktadır. Dilerseniz bu paylaşımlı Runner’lardan herhangi birini de kullanmanız mümkündür. [6] Biz bu makalemizde, GitLab'ın bizim kullanımımıza sunduğu Shared Runner'lardan istifade edeceğiz.

#### 4.4. Docker Nedir?

Docker'ın kendi sitesinde yer alan tanıma göre Docker; organizasyonların uygulmalarını derleyebileceği, paylaşabileceği ve  çalıştırabileceği tek bağımsız konteynır platformudur. [7] Ancak bu tanım ne yazık ki daha önce virtualization ya da conterization kelimelerini hiç duymamış pek çok insan için bir anlam ifade etmeyecektir. Bu nedenle biz de bu tanımı biraz daha genişletmek amacını güdüyoruz.

[Ahmet Emre Aladağ](https://www.emrealadag.com/)'ın kendi blogunda yayınladığı [Docker Nedir?](https://www.emrealadag.com/docker-nedir.html) başlıklı makalesine baktığımızda ise Docker tanımı şu şekilde yapılıyor;

> Docker, yazılım geliştiriciler ve sistemciler için geliştirilen açık kaynaklı bir sanallaştırma platformudur. Docker ile Linux, Windows ve MacOSX üzerinde Linux ve Windows sanal containerler(makineler) çalıştırabilirsiniz. Bu platform sayesinde web sistemlerinin kurulumunu, testini ve dağıtımını kolaylıkla gerçekleştirebilirsiniz. [8]

Biz ise tüm bu tanımları birleştirmek için Docker'ın şöyle bir tanımını yapacağız;

*Docker; 2008 yılında Linux çekirdeğine eklenen Linux Containers (LXC) teknolojisi üzerinde çalışan, yazılım ve sistem geliştiriciler tarafından kullanılabilen, klasik sanallaştırma yöntemlerine göre daha hızlı bir şekilde sanallaştırma imkanı sağlayan, uygulamalarımızı işletim sistemiyle ve ihtiyacı olan diğer bileşenlerle birlikte pakatleyebileceğimiz, dağıtabileceğimiz ve çalıştırabileceğimiz açık kaynaklı bir sanallaştırma platformudur.* [9]

Son yaptığımız tanımdan da anlaşılabileceği üzere, bu makalede, **mavi-api** uygulamamızı Docker yardımıyla işletim sistemiyle birlikte paketleyecek, dağıtacak ve çalıştıracağız.

### 5. Mimari Tasarım

Uygulamamızı bir sunucu üzerinde yayınladığımızda, genelde şu şekilde bir mimari tasarım kullanırız.

<img class="center" src="/images/posts/gitlab-ci-aws/01.png">

Burada kullanıcı, doğrudan tek bir makine üzerinde yayınladığımı bir uygulamaya erişir. Kullanıcı sayısı arttığında, doğru orantılı olarak sunucu üzerindeki CPU ve RAM kullanım miktarlarımızda artacaktır. Biz de buna paralel olarak daha fazla CPU ve RAM ekleyerek, artan yükü karşılamaya çalışırız. Bu yaptığımız işleme *Dikey Ölçekleme* adı verilir. [10]

Ancak bu tarz bir ölçeklemede çeşitli sınırlar vardır. Belirli bir noktadan sonra daha fazla CPU ve RAM eklememiz olanaksız hale gelir. Bu nedenle, *Yatay Ölçekleme* mimarisi kullanılır.

<img class="center" src="/images/posts/gitlab-ci-aws/02.png">

Bu yaklaşımda, kullanıcı ilk olarak bir *Load Balancer (Yük Dengeleyici)* tarafından karşılandır. Daha sonra bu kullanıcı istekleri, arka tarafta bulunan **N** sayıdaki herhangi bir makineye iletilebilir. Bu şekilde, oldukça fazla bir yükü karşılayabilirsiniz. 

*Ancak doğadaki hiçbir şey sonsuz güzellikte değildir ve hiçbir zaman hiçbir çözüm tüm problemleri sonsuza kadar çözemez.*

Bu şekilde bir mimari kurduğumuzu ve e-ticaret uygulamamızı bu mimari uygulamamızda çalıştırdığımızı düşünelim. Gündüz saatlerinde yükümüzün çok fazla olmasına rağmen, gece saatlerinde bu yük azalacaktır. Ancak sistemimizi yükün az olduğu zamana göre kuramayız. Bu nedenle geceleri çok fazla ihtiyacımız olmayan onlarca, belki de yüzlerde makineyi sistemimize dahil etmiş olacağız. Bir de, *Black Friday* gibi aşırı yoğunlukların yaşandığı dönemlerde, bu sisteme kolay bir şekilde yeni makineler ilave edemeyebiliriz. 

Sorunlar bununla da bitmez. Uygulamamızın yeni bir sürümü çıktığında, tüm makinelerde tek tek güncellenmesi çok zaman alacaktır. Ek olarak, tüm makinelerin güncellemelerinin yapılması ve çeşitli bakımlar da hem personel, hem zaman ve doğal olarak maliyet olarak karşımıza çıkar. 

En başta yaptığımız problem tanımını hatırlayın; biz küçük bir ekibiz ve çok fazla kaynağımız yok. Belki bir ihtimal bir milyon kullanıcı gelebilir diyerek bu kadar devasa masraflara katlanamayız.

Biz de, problemi aşağıda kurduladığımız şekilde çözeceğiz;

<img class="center" src="/images/posts/gitlab-ci-aws/03.png">

Kurduladığımız mimariyi şu şekilde açıklayabiliriz;

- Geliştirici yeni bir sürüm hazırladığında bunu GitLab'a gönderir. (Git push)
- GitLab CI/CD yapılandırması, bu sürümü GitLab Runner üzerinde Docker ile paketler ve işletim sistemiyle birlikte sanallaştırılan bu sürüm, AWS üzerinde bulunan ECR servisine gönderilir.
- AWS üzerinde bulunan ECS'de, ECR üzerinde muhafaza ettiğimiz bu paketlenmiş uygulamayı, nasıl bir makinede çalıştırmak istediğimizi tanımlarız (Task Definition).
- ECS üzerinde, nasıl bir cluster yapısına sahip olmak istediğimizi ve ne şartlarda bu cluster'a yeni bir makine ekleneceğini, ne şartlarda var olan makinelerin kaldırılacağını tanımlarız.
- AWS üzerinde bir *Load Balancer* oluşturur ve burada topladığımız trafiği ECS üzerinde oluşturduğumuz Cluster'a yönlendiririz.

#### 6. Adım Adım Gerçekleştirme

Buraya kadar yeterince, hatta gereğinden fazla konuştuk. Şimdi tüm adımları teker teker gerçekleştireceğiz.

Öncelikle, [mavi-api](https://gitlab.com/iozguradem/mavi-api) dosyalarınızı lokal geliştirme ortamına indirebilirsiniz. 

<pre><code class="language-bash">$ git clone git@gitlab.com:iozguradem/mavi-api.git
$ cd ./mavi-api
$ yarn
$ yarn run start
</code></pre>

> Burada kolaylık olsun diye, benim daha önceden yazdığım uygulamadan yararlanabilirsiniz. Ya da kendiniz de basit bir uygulama yazabilirsiniz. Ancak kendi bilgisayarınza kopyaladığınız bu uygulamayı, benim repo'ma göndermeyeceğiniz için, .git/config içerisinden repository adres değişikliği yapmanız gerekecektir.

##### 6.1. Dockerize

Uygulamamızı Docker ile paketlenebilir yapmak istiyoruz. Ana dizinde bir `Dockerfile` oluşturarak, aşağıdaki içerikleri kopyalayabilirsiniz;

<pre><code class="language-docker">FROM node:10
WORKDIR . /app
COPY package*.json ./
RUN npm install
COPY . .
CMD [ "npm", "start" ]
</code></pre>

Daha sonra komut satırı üzerinde aşağıdaki komutu çalıştırdığınızda, uygulamanızın işletim sistemiyle birlikte paketlendiğini göreceksiniz;

> Aşağıdaki komutu çalıştırabilmek için bilgisayarınızda Docker'ın kurulu olması gerekiyor.

<pre><code class="language-bash">$ sudo docker build -t mavi-api .
Sending build context to Docker daemon  6.675MB
Step 1/6 : FROM node:10
10: Pulling from library/node
9a0b0ce99936: Pull complete 
db3b6004c61a: Pull complete 
f8f075920295: Pull complete 
6ef14aff1139: Pull complete 
0bbd8b48260f: Pull complete 
524be717efb1: Pull complete 
aad1e8812bc2: Pull complete 
13fb072986db: Pull complete 
627a2df19018: Pull complete 
Digest: sha256:f4b6f471cdd4b66b27eef899e7f8423ecd9fbfc863b2cb7a59978a7f64c8e0c3
Status: Downloaded newer image for node:10
 ---> a68faf70e589
Step 2/6 : WORKDIR . /app
 ---> Running in 613cacb07016
Removing intermediate container 613cacb07016
 ---> 058cb5d51668
Step 3/6 : COPY package*.json ./
 ---> 979bc91bf122
Step 4/6 : RUN npm install
 ---> Running in 9cc328e20ff9
npm WARN presentation@1.0.0 No description
npm WARN presentation@1.0.0 No repository field.

added 50 packages from 37 contributors and audited 126 packages in 1.68s
found 0 vulnerabilities

Removing intermediate container 9cc328e20ff9
 ---> 63be9c72bcf0
Step 5/6 : COPY . .
 ---> 505564f5b223
Step 6/6 : CMD [ "npm", "start" ]
 ---> Running in 95157a63dbe8
Removing intermediate container 95157a63dbe8
 ---> 0d2273eca2d3
Successfully built 0d2273eca2d3
Successfully tagged mavi-api:latest
</code></pre>

*"Successfully tagged mavi-api:latest"* mesajını gördüğünüzde, uygulamanız başarılı bir şekilde dockerize edilmiş demekdir. Aşağıdaki komutla birlikte, kolaylıkla uygulamayı kendi bilgisayarınızda çalıştırabilirsiniz;


<pre><code class="language-bash">$ sudo docker run -p 8181:80 mavi-api
</code></pre>


Tarayıcı üzerinden `localhost:8181` adresine gittiğinizde, uygulamanın çalışan halini görebilirsiniz.

<img class="center" src="/images/posts/gitlab-ci-aws/04.png">

#### 6.2. AWS Login

Eğer henüz bir AWS hesabınız yoksa hemen oluşturabilirsiniz. Eğer hali hazırda bir AWS hesabınız varsa **console** oturup açarak, AWS servislerine ulaşabilirsiniz. 

> Bu aşamadan sonra yapacağımız işlemler AWS'de ücrete tabi olabilir. Eğer herhangi bir ücret ödemek istemiyorsanız, yapmayınız. Eğer yapıyorsanız da, bu tamamen sizin sorumluluğunuzdadır.

AWS üzerinde oturum açtıktan sonra sizi bu şekilde bir ekran karşılayacaktır;

<img class="center" src="/images/posts/gitlab-ci-aws/05.png">

#### 6.3. AWS ECR Repository Oluşturma

Oturum açtıktan sonra, ECR servisinin sayfasına gidiniz. Daha önce hiç ECR içinde repository oluşturmamışsanız, *Create a Repository* bölümü altından *Get Started* diyebilirsiniz. Amacımız yeni bir repository oluşturmak. Açılan ekranda size repository adını girmeniz beklenmektedir. Bilgileri aşağıdaki gibi doldurarak **Create Repository** butonuna tıklayabilirsiniz;

<img class="center" src="/images/posts/gitlab-ci-aws/06.png">

Daha sonra, AWS size sahip olduğunuz repository listesini gösterecektir. Bu liste üzerinde, az önce oluşturduğumuz **mavi-api-repository** ismindeki repository'i görebiliyor olmanız gerekiyor. Dikkat ederseniz **URI** sütunu altında, bu repository'nin bir yolu olduğunu belirtilmiş. Bu yolu, dockerize edilmiş uygulamamızı AWS'e gönderirken kullanacağız. Ancak şimdilik bu bölümde işimiz tamam.

<img class="center" src="/images/posts/gitlab-ci-aws/07.png">

#### 6.4. AWS Permissions (IAM)

Şu an bir repomuz var ve biz buna dışarıdan bir Docker image'ı göndereceğiz. Ancak her önüne gelenin buraya bir şey göndermesi saçma olacağından, bir yetkilendirme yapacağız. Bunun için AWS arayüzleri üzerinden, **IAM** servisini bularak işe başlıyoruz. **Users** sekmesi altından, **gitlab-user** adında yeni bir kullanıcı oluşturacağız.

<img class="center" src="/images/posts/gitlab-ci-aws/08.png">

Ancak *Set Permissions* adımında, *"Attach existing policies directly"* sekmesi altından, resimde görünen iki tane yetkiyi bu kullanıcıya vermeniz gerekiyor. Bu makeledeki işlemleri GitLab'a gerçekleştirebilmek için her iki yetkiye de ihtiyacımız var.

<img class="center" src="/images/posts/gitlab-ci-aws/09.png">

Kullanıcı oluşturulduğunda, bu kullanıcının erişim bilgilerini AWS size **yalnızca bir defaya mahsus olmak üzere** gösterecektir. Bizim bu bilgilerimize GitLab'ın sahip olması gerekiyor. Kesinlikle koda yazabileceğimiz bir bilgi olmadıklarından, sadece ve sadece GitLab üzerinde tanımlayacağımız **Ortak Değişkenlerine (Environment Variables)** yazacağız. Bunun için, GitLab üzerinde oluşturduğunuz repository'deki CI/CD ayarlarına gideceğiz.

<img class="center" src="/images/posts/gitlab-ci-aws/10.png">

Bu ekranda iki tane ortam değişkeni tanımlayacağız;

- AWS_ACCESS_KEY_ID
- AWS_SECRET_ACCESS_KEY

Bu değerleri, yeni bir kullanıcı oluşturduktan sonra AWS size verecektir. AWS'den aldığımız bu değerleri, GitLab'a vereceğiz.

<img class="center" src="/images/posts/gitlab-ci-aws/11.png">

Bu aşamadan sonra GitLab, AWS'e erişebilecektir. Bunu nasıl yapacağımızı bir sonraki adımda görüyoruz.


#### 6.5. GitLab CI/CD

GitLab'da, varsayılan olarak oluşturulan her repository içerisinde GitLab'ın sağlamış olduğu *Shared Runner*'lar kullanılabiliyor. Ancak bunlara ne gibi işlemler yaptırmak istediğimizi, GitLab'a tarif etmek zorundayız. Bunun için, projenin ana dizininde, `.gitlab-ci.yml` isminde bir dosya oluşturmamız gerekiyor. Bu dosya içerisini aşağıdaki gibi doldurabilirsiniz;

<pre><code class="language-yaml">stages:
  - deploy

services:
  - docker:19.03.0-dind

deploy-api:
  stage: deploy
  image: docker
  tags:
    - 'docker'
  only:
    refs:
      - master
  script:
    - apk add --update python python-dev py-pip build-base 
    - pip install awscli --upgrade --user
    - ~/.local/bin/aws --version
    - $(~/.local/bin/aws ecr get-login --no-include-email --region eu-central-1)
    - docker build -t api .
    - docker tag api:latest 459672519579.dkr.ecr.eu-central-1.amazonaws.com/mavi-api-repository:latest
    - docker push 459672519579.dkr.ecr.eu-central-1.amazonaws.com/mavi-api-repository:latest
</code></pre>

> Buradaki bazı kavramları anlamayabilirsiniz. Eğer bu dosya ne amaçla kullanılıyor, hangi keyword'ler ne için yazılıyor öğrenmek istiyorsanız, daha önceden hazırladığım [GitLab ile CI/CD](https://ozguradem.net/turkish/coding/2019/01/16/gitlab-ile-ci-cd/) başlıklı makalemi kullanabilirsiniz.

Bizim burada odaklanacağımız ana bölüm `script` keyword'ü altında yazan kodlar. Bu kodların hepsi bir Docker konteynerı içerisinde çalıştırılacak. Biz öncelikle, **python** programlama diline ve onun paket yöneticisi olan **pip**'e sahip olmak istiyoruz. Daha sonra, AWS'e hazırlayacağımız docker image'ını gönderebilmek için **aws-cli** aracını kuruyoruz. 4. satırda ise bir login denemesi gerçekleştiriyoruz (Login denemesi anında, Ortam Değişkenlerine eklediğimiz AWS anahtarları kullanılacaktır). Eğer herhangi bir hata çıkmazsa 5. satırda uygulamamızı dockerize edeceğiz. 6. satırda ise bu image'a **latest** etiketini vereceğiz. 7. satırda ise bu image'ı, ECR üzerinde hazırladığımız repository'mize göndereceğiz.

> Lütfen dikkat, ECR listesi üzerindeki URI ile bizim burada yazdığımız yol uyuşmak zorundadır.

Eğer bu kodu GitLab'a push'larsak, GitLab üzerinden bulunan CI/CD burada yazdığımız görevleri Shared Runner'a gönderecek ve çalıştırılmasını isteyecektir. Çalıştığında neler oluyor diye merak ediyorsanız, [buradaki](https://gitlab.com/iozguradem/mavi-api/-/jobs/320547684) loglardan çıktıları görebilirsiniz. Ancak sonuç olarak, bizim GitLab'a derlemesini söylediğimiz image'ımızı, ECR üzerinde görebiliyor olmamız gerekiyor. Eğer siz de AWS'in ECR servisinde oluşturduğumuz repository altında bir image görüyorsanız, her şey yolunda demektir.

<img class="center" src="/images/posts/gitlab-ci-aws/12.png">

#### 6.6. Task Definition

Artık ECR üzerinde muhafaza ettiğimiz bir Docker image'ı mevcut. Şimdi ise bu image'ın nasıl çalıştırılacağını tanımlayacağız. Bunun için AWS üzerinden ECS servisini bulacağız. Bu servisin detayında, **Task Definitions** bölümü altında gideceğiz ve yeni bir *Task Definition* tanımı yapacağız. Task Definition oluştururken, **Fargate** teknolojisini seçmemiz gerekiyor. Biz elimizden geldiğince bütün işi AWS'e yıkmak istiyoruz ve **Fargate** bunu bizim için gerçekleştiriyor. [11]

Oluşturma ekranının ikinci aşamasında, genel bilgileri ve rolleri seçiyoruz. Bu aşamada her şeyi varsayılanlarda bırakabiliriz. Ancak dikkat etmeniz gereken, Memory ve CPU özelliklerini seçtiğimiz bölüm. Burada, her bir makine için ne kadarlık bir CPU ve Ram kullanacağımızı seçiyoruz. Bu değerleri dilediğiniz gibi seçebilirsiniz. Ancak ne kadar yüksek değerler seçerseniz, her bir makinenizin maliyeti o kadar fazla olacaktır.

<img class="center" src="/images/posts/gitlab-ci-aws/13.png">

Aynı sayfanın hemen alt bölümünde, **Add Container** bölümü bizim için önemli. Az önce ECR üzerinde muhafaza etmeye başladığımız Docker image'ını, burada kullanacağımızı belirtmemiz gerekiyor. Bunun için *Add Container* butonuna tıklıyoruz ve sağ tarafta minik bir modal içerisinde aşağıdaki gibi bir tanım gerçekleştiriyoruz;

<img class="center" src="/images/posts/gitlab-ci-aws/14.png">

Buradaki tanımlar bizim için yeterlidir. Diğer tüm özellikleri varsayılan değerleriyle bırakabiliriz. **Add** butonuna tıklayarak, bu container tanımımızı *Task Definition* içerisine dahil edebiliriz. Daha sonra tek yapmamız gereken **Create** butonuna tıklamak ve artık her bir makinemizin özelliklerinin nasıl olması gerektiğini tanımlamış olduk.

<img class="center" src="/images/posts/gitlab-ci-aws/15.png">

#### 6.7. Load Balancer

Bu adımda, kullanıcıdan gelen istekleri karşılamak için bir **Load Balancer** oluşturacağız. Ama önce, sabit bir IP'mizin olmasını istiyoruz. Bu aşama şart değil ama görmüşken bunu da görebiliriz. Bunun için **EC2** servisine gidiyoruz ve **Elastic IPs** bölümünde, **Allocate New IP Address** diyerek yeni bir IP adresi alıyoruz.

<img class="center" src="/images/posts/gitlab-ci-aws/16.png">

Bu IP adresimizi Load Balancer'a tahsis edeceğiz. Load Balancer oluşturmak için, **Load Balancers** sekmesini kullanıyoruz. **Create New Load Balancer** menüsü üzerinden, yeni bir **Network Load Balancer** oluşturacağız. İlk adımda bir isim veriyoruz ve hangi portları dinleyeceğimizi seçiyoruz. SSL için 443 numaralı portu da dinleyebiliriz ancak biz şimdilik sadece 80 numaralı portu seçeceğiz. Daha sonra hangi *Availability Zones* üzerinde konuşlanacağımıza karar veriyoruz. Bu aşamada az önce ayırdığımız statik IP adresini de kullanabiliriz. Aşağıdaki gibi yapılandırmamızı yaptıktan sonra, bir sonraki aşamaya geçebiliriz. 

<img class="center" src="/images/posts/gitlab-ci-aws/17.png">

Eğer 443 numaralı SSL portunu da dinliyor olsaydık, 2. adımda çeşitli ayarlar yapmamız gerekirdi. Ancak biz şimdilik bu aşamayı geçiyoruz. 3. adımda bir **Target Group** ayarı yapmamız bekleniyor. Biz şimdilik sadece bir isimlendirme yapıyoruz. ECS tarafında bir Cluster oluştururken target group ECS tarafından oluşturulup yönetilecek. Bu nedenle sadece bir isimlendirme yaparak, bu haliyle Load Balancer'ı oluşturabiliriz.

<img class="center" src="/images/posts/gitlab-ci-aws/18.png">

Daha sonra hem bir tane Load Balancer hem de Target Group oluşturulmuş olacak. Her iki listeden de bunları görebilirsiniz.

<img class="center" src="/images/posts/gitlab-ci-aws/19.png">

#### 6.8. Cluster Oluşturma

Şimdi ise her şeyi birleştirmenin zamanı geldi. ECS üzerinde öncelikle bir Cluster oluşturacağız. Daha sonra da bu Cluster içerisinde bir servis oluşturacağız. Cluster oluşturmak için ECS bölümüne gidebiliriz. **Create Cluster** butonuna tıkladığımızda, bize Cluster şeması soracaktır. Yine burada **Fargate** seçimi yapmamız önemli. İkinci aşamada sadece bir isimlendirme yapmamız yeterli olacaktır. Geriye kalan her şey varsayılan ayarlarında kalabilir.

<img class="center" src="/images/posts/gitlab-ci-aws/20.png">

#### 6.9. Servis Oluşturma

Daha sonra, Cluster detayına gittiğimizde, henüz herhangi bir servisin olmadığını görmekteyiz. Bu adımda yeni bir servis oluşturacağız. Bu adım en önemli adımımız çünkü tüm auto scaling yapılandırmasını burada kurguluyoruz. Tüm parçaların bir araya geldiği nokta burası. Ayrıca bir servis oluşturduktan sonra, bazı ayarlar daha sonradan değiştirilebilse de, bazıları değil. Bu nedenle yapılacak bir hata, tekrar servisin baştan oluşturulmasıyla sonuçalanabilir.

Cluster detay sayfasında, **Services** sekmesindeki **Create** butonuna tıklayarak, servis oluşturma sihirbazımıa ulaşabiliriz.

<img class="center" src="/images/posts/gitlab-ci-aws/21.png">

Birinci adımda, **Configure Service** bölümü altında, çok temel servis ayarlarını yapıyoruz. Yine her zaman yaptığımız gibi **Fargate**'i seçiyoruz. Önceki adımlarda oluşturduğumuz **Task Definition**'ı bu bölümde seçiyoruz. Task definition'ın farklı sürümleri olabilir. Üzerinde yapılan her bir değişiklik bir sürümü temsil eder. Biz son sürümünü seçiyoruz. **Service Name** bölümüne *mavi-api-service* yazıyoruz. **Number of tasks** bölümünde, bu serviste kaç tane task çalıştırılacağını soruyor. Biz buraya şimdilik **1** yazabiliriz. Ancak 3. aşamada yapacağımız auto scaling ayarları sonrası buraya yazdığımız değer önemsiz hale gelecek. Geriye kalan her şeyi varsayılan haliyle bırakabiliriz ve sonraki adıma geçebiliriz.

<img class="center" src="/images/posts/gitlab-ci-aws/22.png">

**VPC and security groups** bölümünde, Load Balancer'ımız ile aynı VPC ve subnet üzerinde yer almaya dikkat ediyoruz. **Security groups** bölümünü olduğu gibi bırakabiliriz. Bu bölümdeki ayarları bizim için AWS'in yapılandırmasında şu an için yarar var.

<img class="center" src="/images/posts/gitlab-ci-aws/23.png">

**Load balancing** bölümünde, daha önceden hazırladığımız Load Balancer'ımızı servisimize bağlayacağız. Bunun için **Network Load Balancer**'ı seçiyoruz. Hemen aşağısında, daha önceden oluşturduğumuz **mavi-api-load-balancer**'ı görebiliyor olmamız gerekiyor. **Add Load Balancer**'ı tıkladığımızda, Load Balancer ile servis entegrasyonu için son ayarlara geçebiliriz. **Production listener port** bölümündeki alanda **8080** portunu yazabiliriz. Bu veriyi daha sonra kaldıracağız. **Target group name** bölümüne ise **mavi-api-target-group** yazabiliriz. **Health check protocol** olarak da **HTTP** seçmemiz gerekiyor. Bu şekilde, her bir task ayapa kalktığında, AWS HTTP üzerinden bir istek göndererek 200 yanıtının dönmesini bekleyecek, eğer beklenen yanıt belirli bir periyod boyunca gelirse, ilgili task **"Sağlıklı"** olarak adlandırılacak ve Load Balancer bu task'a trafik yönlendirmeye başlayacaktır. Bu işlemlerden sonra Load Balancer ayarlarımız şu şekilde gözükmelidir.

<img class="center" src="/images/posts/gitlab-ci-aws/24.png">

Bu adımda son olarak **Enable service discovery integration** bölümündeki checkbox'ı kaldırıyoruz. Bu özelliğe ihtiyacımız olmayacak.

Üçüncü adımda, **Set Auto Scaling (optional)** başlığını göreceksiniz. İşte burası, ECS'in kalbidir. Bu bölümde, ne gibi şartlarda yeni makineler oluşturularak servise eklenecek, ne gibi şartlarda makine sayısı düşürülecek belirleyebiliyoruz.

*"Configure Service Auto Scaling to adjust your service’s desired count"*, seçeneğini seçtiğimizde hemen aşağıda bir takım bilgiler girilmesi isteniyor. Bilgileri aşağıdaki gibi doldurabiliriz. Burada, en az kaç task'ımızın olması gerektiğine, kaç tane task çalıştırılmasını arzuladığımızı ve en fazla kaç task çalıştırabileceğimizi seçebiliyoruz.

<img class="center" src="/images/posts/gitlab-ci-aws/25.png">

**Automatic task scaling policies** bölümünde, çalışan task sayısını neye göre arttırıp azaltacağımızı belirliyoruz. Ben bu örneğimizde ortalama bellek kullanımını takip edeceğimizi belirttim. Eğer bellek kullanımı %20'ü geçerse yeni bir task oluşturulması gerektiğini, daha aşağısına inerse servis içerisindeki bir task'ın kaldırılması gerektiğini belirttim.

<img class="center" src="/images/posts/gitlab-ci-aws/26.png">

Son adıma geldiğimizde AWS bize bir özet gösterecektir. **Create Service** butonuna tıklayarak servisi oluşturabiliriz. Tüm tanımlar bittikten sonra çalışan taskları görebiliriz.

<img class="center" src="/images/posts/gitlab-ci-aws/27.png">

#### 6.10. Load Balancer Ayarı

Son olarak, Load Balancer üzerinde ufak bir ayar yapmamız gerekecek. Load Balancer listesine gittiğimizde, Listener sekmesinde aşağıdaki gibi bir yapı görüyor olacağız;

<img class="center" src="/images/posts/gitlab-ci-aws/28.png">

Burada, iki farklı target group görüyoruz. Bizim amacımız dinlediğimiz 80 portunu **mavi-api-target-group** target group'una yönlendirmek. Edit menüsü üzerinden bunu yapıyoruz.

<img class="center" src="/images/posts/gitlab-ci-aws/29.png">

Daha sonra 8080 portundaki dinlemeyi silebiliriz. Aynı şekilde **Target Groups** altında yer alan ve artık kullanılmayan **target-group**'u da silebiliriz. Target Groups'un altında yer alan **mavi-api-target-groups**'u seçtiğimizde, hemen altında beliren **Targets** sekmesi içerisinde, ECS tarafından oluşturulan task'ları görebiliriz. Oluşturulan her task, Health Check'den geçirilecek ve sağlıklı olanlara Load Balancer trafiği yönlendirilecek.

<img class="center" src="/images/posts/gitlab-ci-aws/30.png">

Eğer her şey yolundaysa, Load Balancer'a bağladığınız IP adresinizi kullanarak uygulamanıza artık erişebilirsiniz;

<img class="center" src="/images/posts/gitlab-ci-aws/31.png">

#### 6.11 Auto Scaling Tetikleme

Benim geliştirdiğimiz uygulamada, memory mikrarını arttırmak için özel olarak tasarladığım bir route var; `http://3.124.14.233/image`. Bu route'a gönderdiğiniz her istekten sonra, bir resim dosyası açılıyor ve içeriği bir diziye atılıyor. Her istekte dizi sürekli büyüdüğü için memory kullanımı da artıyor. Bu URL'e istek göndererek kullanılan RAM miktarını arttırdığımızda, belirlediğimiz *Auto Scaling Policy* gereği, yeni bir task daha ECS tarafından otomatik olarak oluşturulacaktır.

> Bizim hazırladığımız protokole göre, 5 dakika boyunca bu *yoğun bellek kullanımı* senaryosu devam etmek zorunda.

<img class="center" src="/images/posts/gitlab-ci-aws/32.png">

Yukarıdaki resimde de gördüğünüz gibi, bellek beklediğimizden üst seviyeye çıktığında, sisteme yeni task'lar ECS tarafından otomatik olarak ilave ediliyor. Ayrıca bu olay hakkında **Events** sekmesi altında da log oluşturulduğunu görebilirsiniz.

<img class="center" src="/images/posts/gitlab-ci-aws/33.png">

#### 6.12. Yeni Versiyon Yayınlama

Buraya kadar yaptığımız işlemler bir seferlik işlemlerdi. Ancak bir nokta eksik kaldı, o da otomatik olarak yeni bir sürümün yayınlanması. Bu iş için GitLab'ın CI/CD teknolojisinden yararlanacağız.

AWS bize komut satırından kullanılabilecek *aws-cli* aracını sağlıyor. Zaten bu araç vasıtası ile GitLab üzerinden uygulamamızı dockerize ederek ECR'e pushladık. Aynı aracı kullanarak, dilediğimiz servis için yeniden deployment yapılmasını isteyebiliyoruz. `.gitlab-ci.yml` dosyasındaki script bölümünün son satırına, aşağıdaki komutu da yazarsak, yeni image ECR'a yüklendikten sonra oluşturduğumuz servis için yeni bir deployment yapılması gerektiğini ECS'e söyleyebiliriz.

<pre><code class="language-yaml">stages:
  - deploy

services:
  - docker:19.03.0-dind

deploy-api:
  stage: deploy
  image: docker
  tags:
    - 'docker'
  only:
    refs:
      - master
  script:
    - apk add --update python python-dev py-pip build-base 
    - pip install awscli --upgrade --user
    - ~/.local/bin/aws --version
    - $(~/.local/bin/aws ecr get-login --no-include-email --region eu-central-1)
    - docker build -t api .
    - docker tag api:latest 459672519579.dkr.ecr.eu-central-1.amazonaws.com/mavi-api-repository:latest
    - docker push 459672519579.dkr.ecr.eu-central-1.amazonaws.com/mavi-api-repository:latest
    - ~/.local/bin/aws ecs update-service --cluster mavi-api-cluster --service mavi-api-service --force-new-deployment
</code></pre>

Lütfen son satırdaki komuta dikkat edin. Son komutta, ECS üzerindeki *mavi-api-cluster*, içerisinde bulunan *mavi-api-service* servisi için yeni bir deployment yapılması gerektiğini söylüyoruz. Bu komut çalıştırıldıktan sonra, ECS yeni sürümden taskları otomatik olarak oluşturur. Ancak eski versiyonları kapatmaz. Ne zaman ki yeni sürümler sağlıklı olarak işaretlenir, o zaman eski sürümler tek tek kaldırılır. Dolayısıyla son kullanıcı hiç bir şey fark etmez.

### Sonuç

Zaten yazı hali hazırda epey uzun olduğu için çok fazla şey yazmak istemiyorum. Sonuç ortada, görüyorsunuz. Suya sabuna dokunmadan, devase bir sistem kurduk. Üstelik son derece düşük maliyetle. Bir milyon kullanıcı da gelse, sistemimiz ona göre kendisini ayarlayabilir. Ancak bir milyon kullanıcının bir dakika içerisinde gelmesi hala bir sıkıntı. Bu yüzden Task Definition içerisinde oluşturulan makinelerin CPU ve RAM miktarlarının çok da düşük seçilmemesi gerekiyor. Her şeye rağmen, sonuç yine de harika.

Ancak oluşturduğumuz hemen hemen her şeyi ayrı ayrı faturanıza yansıyayacağınızı unutmayın. Lütfen her bir ürünün fiyatlandırma detayını kontrol edin. Eğer uygulamayı kendi hesabınızda denediyseniz, denemeleriniz bittikten sonra hepsini silmenizi öneririm. Aksi takdirde kullanmadığınız servislere gereksiz faturalar ödeyebilirsiniz.


### Referanslar

[[1] Amazon CloudFront Nedir ve Nasıl Kullanılır?](https://ozguradem.net/turkish/coding/2019/04/13/aws-cloud-front-content-delivery-network-nedir-nasil-kullanilir/)

[[2] AWS ile bulut bilişim?](https://aws.amazon.com/tr/what-is-aws/)

[[3] AWS Dersleri – AWS Nedir?](https://www.mobilhanem.com/aws-dersleri-aws-nedir/)

[[4] What is GitLab?](https://about.gitlab.com/what-is-gitlab/)

[[5] What is DevOps?](https://aws.amazon.com/devops/what-is-devops/)

[[6] GitLab İle CI/CD](https://ozguradem.net/turkish/coding/2019/01/16/gitlab-ile-ci-cd/)

[[7] Docker: The Modern Platform for High-Velocity Innovation](https://www.docker.com/why-docker)

[[8] Docker Nedir?](https://www.emrealadag.com/docker-nedir.html)

[[9] Docker Bölüm 1: Nedir, Nasıl Çalışır, Nerede Kullanılır?](https://gokhansengun.com/docker-nedir-nasil-calisir-nerede-kullanilir/)

[[10] Yatay vs Dikey Ölçeklenebilirlik](http://www.ilterismutlu.com/yatay-vs-dikey-olceklenebilirlik-horizontally-vs-vertically-scalable-scalability/)

[[11] AWS Fargate](https://aws.amazon.com/fargate/)
