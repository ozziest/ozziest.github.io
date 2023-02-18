---
layout: post
title: "GitLab İle CI/CD"
date: 2019-01-16 19:45
categories: [Turkish, Coding]
keywords: gitlab, ci, cd, devops, continuous integration, continuous delivery
author: Özgür Adem Işıklı
post_img: 12.jpg
post_img_link: https://pixabay.com/en/michelangelo-abstract-boy-child-71282
lang: tr
---

> Bu makaleyi iyi bir şekilde özümseyebilmek için temel düzeyde Linux ve Docker bilgisi gerekmektedir.

**_Continuous Integration_** ve **_Continuous Delivery_** kavramları sırasıyla; **_Sürekli Entegrasyon_** ve **_Sürekli Teslimat_** manasına gelmektedir. Söz konusu olan eğer yazılımlarsa, sürekli entegrasyon ve teslimat, kodlanan yazılımların durmaksızın derlenmesi, test edilmesi, entegre edilmesi, sürümlenmesi ve yayınlanmasını anlatmaktadır. Bu işlemlerin tümü el yordamıyla yapıldığında kaybedilecek vaktin azaltılması ve doğabilecek hataların en aza indirgenmesi amacıyla, CI/CD kavramları kelime dağarcığımıza yerleşmiş, modern yazılım geliştirme metodolojileri arasında kendilerine sağlam bir yer edinmiştir.

CI/CD kavramları tek başına var olmaktan ziyade, kendisinden çok daha büyük bir kavramın -DevOps- yalnızca bir alt başlığını oluşturmaktadır. Bu nedenle CI/CD uygulamalarına geçmeden evvel, DevOps kelimesini biraz açarak irdelememiz ve hakkında bilgi sahibi olmamız yerinde olacaktır.

**_Amazon Web Services_** dokümanları üzerinde yazılan tanıma göre **_DevOps_**; organizasyonların yüksek hızda yazılım geliştirme ve sunma kabiliyetlerini arttıran uygulamaların, araçların ve bu **_kültürün_** bir birleşimidir. [1] DevOps ile birlikte, geleneksel metotlar ile uygulama geliştiren süreçlere nazaran daha hızlı bir şekilde ürün geliştirilebilmektedir.

**_Google Trends_** üzerinde biraz araştırma yaptığımızda bu kavramın, yazılımın beşiği sayılabilecek ABD'de 2013 yılından sonra daha çok gündeme gelmeye başlamış olduğunu görüyoruz. Türkiye'de ise 2015 yılından itibaren gösterilen ilgi artmaya başlamış ve ardımızda bıraktığımız 2018 yılında, deyim yerindeyse **_altın çağını_** yaşamıştır. [2]

<a href="https://trends.google.com/trends/explore?date=all,all&geo=US,TR&q=%2Fm%2F0c3tq11,DevOps" target="_blank">
    <img src="/images/posts/13.jpg" />
</a>
<p class="img-description">Resim 1 - Google arama trendleri ABD-Türkiye karşılaştırması</p>

### DevOps Kültürü

AWS tarafından yapılan tanımda da görülebileceği üzere, özellikle seçilmiş olan bir kelime vardır; kültür. DevOps ve alt başlıkları hakkında konuşmaya başladığımız ilk andan itibaren yoğun olarak çeşitli araçlardan bahsederiz. Buna karşın bu işin kurumsal bir kültür haline getirilmesi muhtelif zaruretler içermektedir.

Dünyaca ünlü [Refactoring Improving the Design of Existing Code](https://martinfowler.com/books/refactoring.html) kitabının yazarı [Martin Fowler](https://martinfowler.com/)'ın kişisel blogunda **_DevOpsCulture_** başlıklı bir makale yayınlayan [Rouan Wilsenach](http://rouanw.github.io/), en iyi araçlar kullanılsa dahi, doğru bir kültüre sahip olmadığımızda DevOps kelimesinin sadece modalı bir terim olacağını savunmaktadır. [3] Wilsenach, DevOps'un ana karakteristiğinin geliştirme ve operasyon bölümleri arasında iş birliğinin arttırılması olduğunu belirtmekle birlikte, sektörde aksi yönde görüşlerin de olduğunun unutulmaması gerekir. [4] Bu makaledeki asıl amacımız bu olmadığından, biz bu tartışmaya dahil olmayacak, şimdilik, DevOps'un birkaç yazılım geliştirme aracından çok daha fazlası olduğunu aklımızda tutacak ve bu makalemizde sadece **_GitLab_**'ın CI/CD için sunduğu altyapıya odaklanacağız.

### GitLab

GitLab, yazılım projelerini barındırabileceğiniz, versiyon kontrol sistemi tabanlı ve açık kaynak kodlu bir yazılım projesidir. Buna ilave olarak DevOps alanında da her geçen gün daha iddialı bir konum kazanmaktadır. Kendi tanıtım sayfalarında, doğrudan DevOps döngüsünü entegre eden ilk uygulama olduğunu ve sadece GitLab'ın eşzamanlı DevOps işlemlerine olanak sağladığını özellikle vurgulamaktadır. [5]

Eğer GitLab kullanmazsanız, bir çok farklı uygulamayı bir araya getirerek DevOps süreçlerinizi yönetmeniz de elbette mümkündür. Ancak GitLab **_planlama_** aşamasından, **_monitoring_** aşamasına kadar tüm adımları dahili olarak çözümlemektedir.

### GitLab CI/CD Mimarisi

GitLab CI/CD yapısını incelediğimizde, GitLab, web arayüzleri ve API kullanımı ile CI/CD süreçlerinin veri tabanı üzerinde saklamakta, buna ek olarak süreci yöneten ilave bir araç bulunmaktadır. Süreci yönetme işinde kullanılan bu araca [GitLab Runner](https://docs.gitlab.com/runner/) adı verilmektedir. Bu araç, yukarıda bahsettiğimiz bütünleşik yapının içerisinde düşünülmelidir. GitLab'ın bütünleşik çözümlerinden kast, tüm DevOps kültürü için araçlar geliştiriyor olmalarıdır. Ancak GitLab'ın geliştirdiği her araç ayrı bir uygulama olabilmektedir. Bu şekilde bir tasarıma gidilmesinin amacı, harici başka alternatiflerin de GitLab'a entegre edilebilmesine olanak sağlamaktır.

GitLab Runner, GitLab'tan tamamen izole bir şekilde, Go dili kullanılarak [MIT Lisansı](https://gitlab.com/gitlab-org/gitlab-runner/blob/master/LICENSE) ile geliştirilmiş ve GNU/Linux, macOS, FreeBSD ve Windows üzerine kurulabilmektedir. [6]

GitLab Runner'ı herhangi bir makineye kurabilirsiniz. Söz konusu makine [DigitalOcean](https://www.digitalocean.com/) üzerinde kiralayacağınız basit bir sunucu da olabilir, kendi bilgisayarınız da. GitLab sunucusunun, GitLab Runner ile aynı makine üzerinde bulunmak gibi bir zorunluluğu yoktur.

İlave olarak, GitLab doğrudan ücretsiz hesapların da kullanabileceği **_Shared Runner_**'lar sunmaktadır. Dilerseniz bu paylaşımlı Runner'lardan herhangi birini de kullanmanız mümkündür.

Ayrıca önemli bir diğer avantajı da, kendi GitLab hesabınıza birden fazla GitLab Runner bağlayabilmenizdir. Böylece birden fazla GitLab Runner entegrasyonuyla CI/CD süreçlerinizi yönetebilirsiniz.

<img src="/images/posts/15.png" class="center" />
<p class="img-description">Resim 2 - GitLab Server, Runner ve geliştirici ilişkisi</p>

### GitLab Runner Nasıl Çalışır?

Yukarıda da belirttiğimiz gibi GitLab Runner, GitLab'dan tamamen ayrı bir cihaz üzerine kurulabilir. Daha sonrasında ise GitLab Runner, GitLab ile GitLab API üzerinden haberleşerek bilgi alış verişinde bulunur.

<img src="/images/posts/14.png" class="center" />
<p class="img-description">Resim 3 - GitLab Server ve Runner arasındaki iletişim</p>

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

Kaydetme işlemine başlamadan önce, kaydetme işleminde kullanacağımız **_GitLab CI Token_** değerini, GitLab sunucusun üzerinde bulunan bir proje ya da group üzerinde `Settings -> CI/CD` bölümü altındaki `Runners` sekmesinde içerisinden alabilirsiniz.

<img src="/images/posts/16.jpg" />
<p class="img-description">Resim 4 - GitLab CI token</p>

Elimizde artık bir GitLab CI Token varken, Runner'ın kurulu olduğu makinada aşağıdaki komut çalıştırıldıktan sonra, Runner size sırayla sunucunun adresini, token bilgisini, Runner tanımını, Runner etiketini ve Runner'ın hangi yöntemle çalışacağını soracaktır.

> Biz Docker ile birlikte çalışacağımız için, 5. adımda **_docker_** seçimi yapacağız.

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
docker,another-tag

Please enter the executor: ssh, docker+machine, docker-ssh+machine, kubernetes, docker, parallels, virtualbox, docker-ssh, shell:
docker

Please enter the Docker image (eg. ruby:2.1):
alpine:latest
```

Yukarıdaki tanımlamalardan sonra, token aldığımız bölümde bulunan **_Specific Runners_** bölümünde, henüz tanımladığımız Runner'ı görebiliyor olmanız gerekmektedir.

<img src="/images/posts/17.jpg" />
<p class="img-description">Resim 5 - GitLab Runner tanımı görüntüsü</p>

Böylece artık GitLab sunucumuzun, ilgili proje ya da grubumuz içerisinde kullanabileceği ve iletişimde olduğu bir Runner bulunmaktadır. Bundan sonraki aşamada, GitLab'a yüklediğimiz çeşitli tanımlamalarla birlikte, bu Runner'a ne zaman, nasıl ve neler yapması gerektiğini söyleyeceğiz.

### CI/CD Direktifleri

Şu ana kadar yaptığımız çalışmalarla birlikte, GitLab ile Runner arasındaki ilişkinin nasıl olacağını tanımladık. Bundan sonra yapacaklarımız, herhangi bir projemiz (repository) içerisinde, herhangi bir olay gerçekleştiğinde (commit, push, tag vb.) hangi işlemlerin nasıl yapılacağını GitLab'a bildirmekten ibarettir. Bu işlemleri GitLab'a bildirmek için kullandığımız ortak lisan, [Yaml](https://yaml.org/) formatında yazacağımız `.gitlab-ci.yml` dosyasıdır.

Bunun için projenizin ana dizininde `.gitlab-ci.yml` isimli bir dosya oluşturarak, nasıl bir yapılandırma istediğimizi yazarak işe başlıyoruz.

<pre><code class="language-yaml">
stages:
  - test
  - deploy

jstest:
  stage: test
  image: node:latest
  tags:
    - 'docker'
  script:
    - npm install
    - npm run test
</code></pre>

Yukarıdaki dosyayı incelediğimizde, iki ana bölüm dikkat çekmektedir; `stages` ve `test`.

`stages` bölümünde projemiz içerisinde hangi **_stage_** adımlarının bulunduğunu GitLab'a söylüyoruz. Buraya dilediğimiz adımları yazabilmekle birlikte, genel olarak `test`, `build`, `publish` ya da `deploy` gibi kalıplaşmış stage isimlendirmeleri kullanabiliriz. Her stage içerisinde birden fazla **_görev_** tanımı yapabiliriz. Örneğin **_test_** stage'i içerisinde, JS testleri için ayrı bir **_task_**, Ruby testleri için ayrı bir **_task_** yazabiliriz. Ya da **_build_** aşaması içerisinde farklı platformlar için ayrı ayrı görevler ile build işlemi gerçekleştirebiliriz. Stage tanımlarının sırasına göre, çalışma anında GitLab bize aşağıdaki gibi bir görselleştirme yapacaktır;

<img src="/images/posts/18.jpg" />
<p class="img-description">Resim 6 - GitLab CE Pipeline Detayı</p>

Üstteki resmi incelediğimiz zaman **_Build_**, **_Prepare_**, **_Test_** gibi bir stage sıralaması yapıldığını ve örneğin Prepare stage'i altında **_compile-assets_**, **_setup-test-env_** gibi görevler bulunduğu görebiliriz. Bu tanımlar tamamen projenin ihtiyaçlarına göre bizim tarafımızdan belirlenmektedir.

Yukarıda kendi yazdığımız yapılandırma dosyasına döndüğümüzde, **_jstest_** isimli bir görev tanımını, bu görev tanımının **_test_** isimli stage altına yer alacağını, Docker image'ı olarak **_node_** isimli image'ın son sürümünün kullanılacağını ve bu görevin **_docker_** etiketine sahip herhangi bir Runner tarafından çalıştırılacağını tanımladığımızı görebilirsiniz.

Bu dosya repository içerisinde GitLab'a ulaştığı zaman, GitLab bu dosyayı doğrudan yorumlayarak gerekli işlemleri yapacaktır.

> Bu makale için oluşturduğum örnek repository kodlarını [buradan](https://gitlab.com/iozguradem/gitlab-ci-test) inceleyebilirsiniz.

GitLab üzerinde, repository içerisinde CI/CD menüsü altında çalıştırılan [Pipeline](https://gitlab.com/iozguradem/gitlab-ci-test/pipelines)'larını ve [pipeline içeriklerini](https://gitlab.com/iozguradem/gitlab-ci-test/pipelines/43995855) görebilirsiniz. Hemen aşağıda yazdığımı **_jstask_** isimli görev çalıştığında console üzerinde neler yapılmış görebilmekteyiz.

<img src="/images/posts/19.jpg" />
<p class="img-description">Resim 7 - GitLab CI/CD Pipeline Job</p>

Logları incelediğimizde, Runner çalışmaya başladıktan sonra öncelikle bizim belirttiğimiz **_node:latest_** image'ının [Docker Hub](https://hub.docker.com/) üzerinden alındığını, daha sonra repository içeriğinin **_git clone_** ile ana dizine çıkartıldığını görebiliriz. Sonrasında `$` ile başlayan her bölüm, bizim **_script_** altına yazdığımız komutlardan ibarettir. Özetle; gerekli ortamı hazırlayıp, koşturulması gereken komutları sırasıyla **_.gitlab-ci.yml_** dosyasına yazarak, Runner'ın sırayla neler yapması gerektiğini adım adım belirtmiş oluyoruz. Repository üzerinde herhangi bir hareket olduğunda GitLab bu dosyayı okuyarak ilgili Runner ile iletişime geçer, ve çalıştılması gereken komutları Runner'a bildirdikten sonra arkasına yaslanarak Runner'dan gelen cevapları bekler. Runner yaptığı işlemlerin loglarını tekrar GitLab'a gönderir ve biz de web arayüzünden görevlerin sonuçlarını görebiliriz.

### Son Sözler

GitLab CI/CD entegrasyonu için gerçekleştirdiğimiz işlemlerin, sadece birkaç yazılım aracını kullanmaktan ibaret olmadığına yazının ilk bölümünde bir miktar değinmiştim. CI/CD, DevOps sürecinin yalnızca bir parçasını oluşturmaktadır ve CI/CD entegrasyonuna ek olarak DevOps içerisinde yapılabilecek bir çok farklı eylem bulunmaktadır. Konu hakkında daha fazla araştırma yaparak bu konudaki bilgi dağarcığınızı büyütebilirsiniz.

Ek olarak, bu makalede bu kadar detay olmasının bir diğer sebebi de; kaputun altında neler olduğunun yeterince anlaşılmadığı durumlarda, ezbere yapılan hareketlerin, yazılım geliştirme süreçlerine yarardan çok zarar sağladığını düşünmemdir. Yazılım geliştirme süreçleri nasıl dilleri kullanmaktan ibaret değilse, DevOps da benzer şekilde, sadece araçları kullanmaktan ibaret değildir. DevOps kültürü, doğrudan doğruya geliştirme hızınıza katkıda bulunmasının yanı sıra, dolaylı olarak da, geliştiricilerin yaptıkları ve kısa süre sonra nasıl yaptıklarını unutacakları pek çok işin bir sistematiğe oturtulması demektir.

Tam da bu nedenle geliştirme süreçlerinize günbegün yedirmeniz gereken bir kültür olduğundan, bir çok farklı kaynakta bahsedilmektedir. Aynı zamanda DevOps; **_Yazılım Geliştirme_** süreci içerisinde bulunan bütün ekiplerin, yaptıkları işi basit kodlara ve şemalara dökerek birbirlerine anlatma sürecidir de. Oluşturulacek her yeni proje, yapılacak her iş bu kültür içerisinde düşünülmeli, kurgulanmalı ve uygulanmalıdır.

Bir bilgisayarın yapabileceği bir işi, asla bir insan kendisi yapmamalıdır.

### Referanslar

[[1] What is DevOps?](https://aws.amazon.com/devops/what-is-devops/)

[[2] Google Trends Results](https://trends.google.com/trends/explore?date=all,all&geo=US,TR&q=%2Fm%2F0c3tq11,DevOps)

[[3] DevOpsCulture, Rouan Wilsenach](https://martinfowler.com/bliki/DevOpsCulture.html)

[[4] DevOps is a culture, but here's why it's actually not](https://devopsagenda.techtarget.com/opinion/DevOps-is-a-culture-but-heres-why-its-actually-not)

[[5] GitLab](https://about.gitlab.com/product/)

[[6] GitLab Runner](https://gitlab.com/gitlab-org/gitlab-runner)
