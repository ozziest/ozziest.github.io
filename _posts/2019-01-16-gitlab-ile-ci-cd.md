---
layout: post
title:  "GitLab İle CI/CD"
date:   2019-01-16 19:45
categories: [Turkish, Coding]
tags: gitlab, ci, cd, devops, continuous integration, continuous delivery
meta: gitlab, ci, cd, devops, continuous integration, continuous delivery
author: ozziest
---

<a href="https://pixabay.com/en/michelangelo-abstract-boy-child-71282/" target="_blank">
    <img src="/images/posts/12.jpg" class="center" />
</a>

> Bu makaleyi iyi bir şekilde özümseyebilmek için temel düzeyde Linux ve Docker bilgisi gerekmektedir. 

***Continuous Integration*** ve ***Continuous Delivery*** karamları sırasıyla; ***Sürekli Entegrasyon*** ve ***Sürekli Teslimat*** manasına gelmektedir. Söz konusu olan eğer yazılımlarsa, sürekli entegrasyon ve teslimat, kodlanan yazılımların durmaksızın derlenmesi, test edilmesi, entegre edilmesi, sürümlenmesi ve yayınlanmasını anlatmaktadır. Bu işlemlerin tümü el yordamıyla yapıldığında kaybedilecek vaktin azaltılması ve doğabilecek hataların en aza indirgenmesi amacıyla, CI/CD kavramları kelime dağarcığımıza yerleşmiş, modern yazılım geliştirme metodolojileri arasında kendilerine sağlam bir yer edinmiştir.

CI/CD kavramları tek başına var olmaktan ziyade, kendisinden çok daha büyük bir kavramın -DevOps- yalnızca bir alt başlığını oluşturmaktadır. Bu nedenle CI/CD uygulamalarına geçmeden evvel, DevOps kelimesini biraz açarak irdelememiz ve hakkında bilgi sahibi olmamız yerinde olacaktır.

***Amazon Web Services*** dokümanları üzerinde yazılan tanıma göre ***DevOps***; organizasyonların yüksek hızda yazılım geliştirme ve sunma kabiliyetlerini arttıran uygulamaların, araçların ve bu ***kültürün*** bir birleşimidir. [1] DevOps ile birlikte, geleneksel metotlar ile uygulama geliştiren süreçlere nazaran daha hızlı bir şekilde ürün geliştirilebilmektedir.

***Google Trends*** üzerinde biraz araştırma yaptığımızda bu kavramın, yazılımın beşiği sayılabilecek ABD'de 2013 yılından sonra daha çok gündeme gelmeye başlamış olduğunu görüyoruz. Türkiye'de ise 2015 yılından itibaren gösterilen ilgi artmaya başlamış ve ardımızda bıraktığımız 2018 yılında, deyim yerindeyse ***altın çağını*** yaşamıştır. [2]

<a href="https://trends.google.com/trends/explore?date=all,all&geo=US,TR&q=%2Fm%2F0c3tq11,DevOps" target="_blank">
    <img src="/images/posts/13.jpg" class="center" />
</a>

### DevOps Kültürü

AWS tarafından yapılan tanımda da görülebileceği üzere, özellikle seçilmiş olan bir kelime vardır; kültür. DevOps ve alt başlıkları hakkında konuşmaya başladığımız ilk andan itibaren yoğun olarak çeşitli araçlardan bahsederiz. Buna karşın bu işin kurumsal bir kültür haline getirilmesi muhtelif zaruretler içermektedir.

Dünyaca ünlü [Refactoring Improving the Design of Existing Code](https://martinfowler.com/books/refactoring.html) kitabının yazarı [Martin Fowler](https://martinfowler.com/)'ın kişisel blogunda ***DevOpsCulture*** başlıklı bir makale yayınlayan [Rouan Wilsenach](http://rouanw.github.io/), en iyi araçlar kullanılsa dahi, doğru bir kültüre sahip olmadığımızda DevOps kelimesinin sadece modalı bir terim olacağını savunmaktadır. [3] Wilsenach, DevOps'un ana karakteristiğinin geliştirme ve operasyon bölümleri arasında iş birliğinin arttırılması olduğunu belirtmekle birlikte, sektörde aksi yönde görüşlerin de olduğunun unutulmaması gerekir. [4] Bu makaledeki asıl amacımız bu olmadığından, biz bu tartışmaya dahil olmayacak, şimdilik, DevOps'un bir kaç yazılım geliştirme aracından çok daha fazlası olduğunu aklımızda tutacak ve bu makalemizde sadece ***GitLab***'ın CI/CD için sunduğu altyapıya odaklanacağız. 

### GitLab

GitLab, yazılım projelerini barındırabileceğiniz, versiyon kontrol sistemi tabanlı ve açık kaynak kodlu bir yazılım projesidir. Buna ilave olarak DevOps alanında da her geçen gün daha iddialı bir konum kazanmaktadır. Kendi tanıtım sayfalarında, doğrudan DevOps döngüsünü entegre eden ilk uygulama olduğunu ve sadece GitLab'ın eşzamanlı DevOps işlemlerine olanak sağladığını özellikle vurgulamaktadır. [5]

Eğer GitLab kullanmazsanız, bir çok farklı uygulamayı bir araya getirerek DevOps süreçlerinizi yönetmeniz de elbette mümkündür. Ancak GitLab ***planlama*** aşamasından, ***monitoring*** aşamasına kadar tüm adımları dahili olarak çözümlemektedir. 

### GitLab CI/CD Mimarisi

GitLab CI/CD yapısını incelediğimizde, GitLab, web arayüzleri ve API kullanımı ile CI/CD süreçlerinin veri tabanı üzerinde saklamakta, buna ek olarak süreci yöneten ilave bir araç bulunmaktadır. Süreci yönetme işinde kullanılan bu araca [GitLab Runner](https://docs.gitlab.com/runner/) adı verilmektedir. Bu araç, yukarıda bahsettiğimiz bütünleşik yapının içerisinde düşünülmelidir. Bu şekilde bir tasarıma gidilmesinin amacı, harici başka alternatiflerin de GitLab'a entegre edilebilmesine olanak sağlamaktır.

GitLab Runner, GitLab'tan tamamen izole bir şekilde, Go dili kullanılarak [MIT Lisansı](https://gitlab.com/gitlab-org/gitlab-runner/blob/master/LICENSE) ile geliştirilmiş ve GNU/Linux, macOS, FreeBSD ve Windows üzerine kurulabilmektedir. [6] 

GitLab Runner'ı herhangi bir makineye kurabilirsiniz. Söz konusu makine [DigitalOcean](https://www.digitalocean.com/) üzerinde kiralayacağınız basit bir sunucu da olabilir, kendi bilgisayarınız da. GitLab sunucusunun, GitLab Runner ile aynı makine üzerinde bulunmak gibi bir zorunluluğu yoktur.

İlave olarak, GitLab doğrudan ücretsiz hesapların da kullanabileceği ***Shared Runner***'lar sunmaktadır. Dilerseniz bu paylaşımlı Runner'lardan herhangi birini de kullanmanız mümkündür. 

Ayrıca önemli bir diğer avantajı da, kendi GitLab hesabınıza birden fazla GitLab Runner bağlayabilmenizdir. Böylece birden fazla GitLab Runner entegrasyonuyla CI/CD süreçlerinizi yönetebilirsiniz. 

<img src="/images/posts/15.png" class="center" />

### GitLab Runner Nasıl Çalışır?

Yukarıda da belirttiğimiz gibi GitLab Runner, GitLab'dan tamamen ayrı bir cihaz üzerine kurulabilir. Daha sonrasında ise GitLab Runner, GitLab ile GitLab API üzerinden haberleşerek bilgi alış verişinde bulunur.

<img src="/images/posts/14.png" class="center" />

Bu esnek yapı sayesinde, örneğin yeni bir commit geldiğinde, hangi Runner üzerinde, hangi görevlerin çalıştırılabileceğine karar verebilir, commit geldiğinde GitLab sizin direktiflerinize göre GitLab Runner'ı çalıştırır, GitLab Runner da elde ettiği sonuçları GitLab ile paylaşarak sonucu sizin web arayüzünde ya da API cevaplarında görebilmenizi sağlar.

Ek olarak, GitLab Runner görevleri çalıştırırken kurulduğu makinenin komut satırını kullanabildiği gibi Docker desteği de sağlamaktadır. 

> Docker burada müthiş bir kolaylık sağlamaktadır. Misalen; JavaScript uygulamanızı derleyeceğiniz, içerisinde nodejs bulunan Docker container'ı, hemen ardından da .Net Core ile geliştirilen bir uygulamayı derlemek için, içerisinde dotnet bulunan bir Docker container'ı kullanabilirsiniz. Runner'ın kurulu olduğu makinede nodejs ya da dotnet bulunması gerekmez. Sadece Docker olması yeterlidir. 

### GitLab Runner Kurulumu

Yukarıda GitLab Runner'ı çeşitli platformlar üzerine kurabileceğimizden bahsetmiştik. Makalemiz içerisinde biz tercihimizi Ubuntu'dan yana kullandık. 

> Diğer kurulumlar için [Kurulum Dokümanını](https://docs.gitlab.com/runner/install/index.html) inceleyebilirsiniz. 

Ubuntu kurulumu için aşağıdaki satırlar yeterlidir;

```bash
curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh | sudo bash
sudo apt-get install gitlab-runner
```

İlk kurulum tamamlandıktan sonra yapılacak güncellemeler için aşağıdaki komutlar işimizi görecektir;

```bash
sudo apt-get update
sudo apt-get install gitlab-runner
```

### GitLab Runner Entegrasyonu

GitLab Runner kurulduktan sonra, GitLab Runner ile GitLab'ı entegre etmemiz gerekmektedir. Bu işlemle birlikte GitLab Runner, GitLab sunucusunun hangi adresten hizmet verdiğini bilecek ve GitLab da hangi özelliklere sahip Runner'ın nerede bulunduğunu anlamış olacaktır.

> GitLab açık kaynak kodlu bir proje olduğundan, kendi GitLab sunucunuzu da kendi belirlediğiniz bir cihaz üzerine kurabilirsiniz. 

Kaydetme işlemine başlamadan önce, kaydetme işleminde kullanacağımız ***GitLab CI Token*** değerini, GitLab sunucusun üzerinde bulunan bir proje ya da group üzerinde  `Settings -> CI/CD` bölümü altındaki `Runners` sekmesinde içerisinden alabilirsiniz. 

<img src="/images/posts/16.jpg" class="center" />

Elimizde artık bir GitLab CI Token varken, Runner'ın kurulu olduğu makinada aşağıdaki komut çalıştırıldıktan sonra, Runner size sırayla sunucunun adresini, token bilgisini, Runner tanımını, Runner etiketini ve Runner'ın hangi yöntemle çalışacağını soracaktır.

> Biz Docker ile birlikte çalışacağımız için, 5. adımda ***docker*** seçimi yapacağız.

```bash
sudo gitlab-runner register
```

Bilgileri aşağıdaki örnekteki gibi girebilirsiniz;

```bash
Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com )
https://gitlab.com

Please enter the gitlab-ci token for this runner
xxx

Please enter the gitlab-ci description for this runner
[hostame] my-runner

Please enter the gitlab-ci tags for this runner (comma separated):
my-tag,another-tag

Please enter the executor: ssh, docker+machine, docker-ssh+machine, kubernetes, docker, parallels, virtualbox, docker-ssh, shell:
docker

Please enter the Docker image (eg. ruby:2.1):
alpine:latest
```
Yukarıdaki tanımlamalardan sonra, token aldığımız bölümde bulunan ***Specific Runners*** bölümünde, henüz tanımladığımız Runner'ı görebiliyor olmanız gerekmektedir.

<img src="/images/posts/17.jpg" class="center" />

Böylece artık GitLab sunucumuzun, ilgili proje ya da grubumuz içerisinde kullanabileceği ve iletişimde olduğu bir Runner bulunmaktadır. Bundan sonraki aşamada, GitLab'a yüklediğimiz çeşitli tanımlamalarla birlikte, bu Runner'a ne zaman, nasıl ve neler yapması gerektiğini söyleyeceğiz.

### CI Konfigurasyonu

GitLab Runner entegrasyonunu yaptıktan sonra projenizin ana dizininde `.gitlab-ci.yml` isimli bir dosya oluşturarak, nasıl bir CI/CD yapılandırması istediğimizi yazıyoruz.

```yml
image: ruby:2.2

test:
  script:
  - bundle exec rake spec
```

Bu tanımlama dosyasını incelediğimizde, tıpkı Docker tanımlarında olduğu gibi bir `image` anahtarı bulunmaktadır. Bu image anahtarı ile hangi Docker image'ını kullanacağımızı belirliyoruz. 

### Referanslar

[[1] What is DevOps?](https://aws.amazon.com/devops/what-is-devops/)

[[2] Google Trends Results](https://trends.google.com/trends/explore?date=all,all&geo=US,TR&q=%2Fm%2F0c3tq11,DevOps)

[[3] DevOpsCulture, Rouan Wilsenach](https://martinfowler.com/bliki/DevOpsCulture.html)

[[4] DevOps is a culture, but here's why it's actually not](https://devopsagenda.techtarget.com/opinion/DevOps-is-a-culture-but-heres-why-its-actually-not)

[[5] GitLab](https://about.gitlab.com/product/)

[[6] GitLab Runner](https://gitlab.com/gitlab-org/gitlab-runner)