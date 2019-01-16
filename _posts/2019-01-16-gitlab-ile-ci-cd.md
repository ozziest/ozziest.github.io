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

> Bu makaleyi daha iyi bir şekilde anlayabilmek için temel Linux ve Docker bilgisi gerekmektedir. Eğer bu konulara aşina değilseniz, öncelikle bu konuları öğrenmeniz daha yararlı olacaktır.

***Continuous Integration*** ve ***Continuous Delivery*** kavramları, DevOps kültürünün yalnızca bir parçasını oluşturmaktadır. ***Amazon Web Services*** dokümanları üzerinde yazılan tanıma göre de ***DevOps***; organizasyonların yüksek hızda uygulama geliştirme ve sunma kabiliyetlerini arttıran uygulamaların, araçların ve bu ***kültürün*** bir birleşimidir. [1] DevOps ile birlikte geleneksel metotlara göre uygulama geliştiren süreçlere göre daha hızlı bir şekilde ürün geliştirilebilir. 

***Google Trends*** üzerinde biraz araştırma yaptığımızda bu kavramın, yazılımın beşiği sayılabilecek ABD'de 2013 yılından sonra daha çok gündeme gelmeye başlamış olduğunu görüyoruz. Türkiye'de ise 2015 yılından itibaren gösterilen ilgi artmaya başlamış ve 2018 yılında deyim yerindeyse altın çağını yaşamıştır. [2]

<a href="https://trends.google.com/trends/explore?date=all,all&geo=US,TR&q=%2Fm%2F0c3tq11,DevOps" target="_blank">
    <img src="/images/posts/13.jpg" class="center" />
</a>

Bu yazımızda DevOps'u detaylı olarak incelemekten ziyade, hakkında temel bilgileri edindikten sonra ***GitLab***'ın bize bu konuda sunduğu ***CI/CD*** araçlarına odaklanacağız.

### DevOps Kültürü

AWS tarafından yapılan tanımda da görülebileceği üzere, özellikle seçilmiş olan bir kelime vardır; kültür. DevOps hakkında konuşmaya başladığımız ilk andan itibaren yoğun olarak çeşitli araçlardan bahsederiz. Buna karşın bu işin kurumsal bir kültür haline getirilmesi muhtelif zaruretler içermektedir.

Dünyaca ünlü [Refactoring Improving the Design of Existing Code](https://martinfowler.com/books/refactoring.html) kitabının yazarı [Martin Fowler](https://martinfowler.com/)'ın kişisel blogunda ***DevOpsCulture*** başlıklı bir makale yayınlayan [Rouan Wilsenach](http://rouanw.github.io/), en iyi araçlar kullanılsa dahi, doğru bir kültüre sahip olmadığımızda DevOps kelimesinin sadece modalı bir terim olacağını savunmaktadır. [3] Wilsenach, DevOps'un ana karakteristiğinin geliştirme ve operasyon bölümleri arasında iş birliğinin arttırılması olduğunu savunmaktaıdr. Ancak bunun yanında aksi görüşlerin de olduğunun unutulmaması gerekir. [4] Bu makaledeki asıl amacımız bu olmadığından biz bu tartışmaya dahil olmayacak, bunun yerine GitLab'ın DevOps için sunduğu altyapıya odaklanacağız. 

### GitLab

GitLab açık kaynak olmasının yanı sıra, DevOps alanında her geçen gün daha iddialı bir konuma gelmektedir. Kendi tanıtım sayfalarında, doğrudan DevOps döngüsünü entegre eden ilk uygulama olduğunu, sadece GitLab'ın eşzamanlı DevOps işlemlerine olanak sağladığını vurgulamaktadır. [5]

Eğer GitLab kullanmazsanız, bir çok farklı uygulamayı bir araya getirerek DevOps sürecinizi yönetmeniz de elbette mümkün. Ancka GitLab ***planlama*** aşamasından, ***monitoring*** aşamasına kadar tüm süreçleri dahili olarak çözümlemektedir. Bu aşamalarından bir bölümü de CI/CD süreçleri oluşturmaktadır. 

### GitLab CI/CD

GitLab CI/CD yapısını incelediğimizde, GitLab web arayüzleri ve API kullanımı ile CI/CD süreçlerinin veri tabanı üzerinde tutulduğunu, süreci yöneten ek bir aracın olduğunu görüyoruz. Süreci yöneterek build oluşturma işinde kullanılan bu araca [GitLab Runner](https://docs.gitlab.com/runner/) adı verilmektedir. 

GitLab Runner, GitLab'tan tamamen izole bir şekilde, Go dili kullanılarak [MIT Lisansı](https://gitlab.com/gitlab-org/gitlab-runner/blob/master/LICENSE) ile geliştirilmiş ve GNU/Linux, macOS, FreeBSD ve Windows üzerine kurulabilmektedir. [6] Bunun tabii bir sonucu olarak farklı dillerde geliştirilen uygulamalar da bu GitLab Runner kullanılarak build işlemine tabi tutulabilmektedir.

GitLab Runner'ı [DigitalOcean](https://www.digitalocean.com/) üzerinde kiralayacağınız basit bir sunucuya kurabileceğiniz gibi, kendi bilgisayarınıza da kurabilirsiniz. Ancak GitLab doğrudan ücretsiz hesapların da kullanabileceği ***Shared Runner***'lar da vermektedir. Ayrıca önemli bir diğer avantajı da, kendi GitLab hesabınıza birden fazla GitLab Runner bağlayabilmenizdir. Böylece birden fazla GitLab Runner entegrasyonuyla CI/CD süreçlerinizi yönetebilirsiniz. 

### GitLab Runner Nasıl Çalışır?

Yukarıda da belirttiğimiz gibi GitLab Runner, GitLab'dan tamamen ayrı bir cihaz üzerine kurulabilir. Daha sonrasında ise GitLab Runner, GitLab ile GitLab API üzerinden haberleşerek bilgi alış verişinde bulunur.

<img src="/images/posts/14.png" class="center" />

Bu esnek yapı sayesinde, örneğin yeni bir commit geldiğinde, hangi Runner üzerinde, hangi görevlerin çalıştırılabileceğine karar verebilir, commit geldiğinde GitLab sizin direktiflerinize göre GitLab Runner'ı çalıştırır, GitLab Runner da elde ettiği sonuçları GitLab ile paylaşarak sonucu sizin web arayüzlerinden görebilmenizi sağlar.

Ek olarak, GitLab Runner görevleri çalıştırırken kurulduğu makinenin komut satırını kullanabildiği gibi Docker desteği de sağlamaktadır. 

### GitLab Runner Kurulumu

Öncelikle GitLab Runner'ı herhangi bir bilgisayara kurmamız gerekmektedir. Ben örnek gösterim için tercihimi Ubuntu'dan yana kullandım. Diğer kurulumlar için [Kurulum Dokümanını](https://docs.gitlab.com/runner/install/index.html) inceleyebilirsiniz. 

Ubuntu kurulumumuz son derece basit;

```bash
curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh | sudo bash
sudo apt-get install gitlab-runner
```

İlk kurulumlardan sonra güncellemek için aşağıdaki komutlar işimizi görecektir;

```bash
sudo apt-get update
sudo apt-get install gitlab-runner
```

### GitLab Runner Entegrasyonu

GitLab Runner kurulduktan sonra, GitLab Runner için ilk yapılandırmamız yapmamız gerekmektedir. Bu yapılandırma ile birlikte GitLab Runner, GitLab'ın nereye kurulacağını ve nasıl çalışacağını artık biliyor olacaktır. 

> GitLab açık kaynak kodlu bir proje olduğundan, kendi GitLab sunucunuzu da kendi belirlediğiniz bir cihaz üzerine kurabilirsiniz. 

Aşağıdaki komut çalıştırıldıktan sonra, Runner size sırayla bazı bilgiler soracaktır.

```bash
sudo gitlab-runner register
```

Bilgileri aşağıdaki örnekteki gibi girebilirsiniz;

> GitLab CI Token değerini, ilgili proje ya da grup ayarlarında bulunan `Settings -> CI/CD` bölümü altındaki `Runners` sekmesinde içerisinden alabilirsiniz. 

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

Yukarıdaki verilerde dikkat ettiyseniz, Runner'ın `docker` desteğiyle birlikte çalışacağı söylenmiş durumda.

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