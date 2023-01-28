---
layout: post
title: "Product Launch: ikinciel.dev"
date: 2022-01-22 19:00
categories: [Turkish, Coding]
keywords: ikinciel.dev, product, launch, coding, github
author: Özgür Adem Işıklı
post_img: 38.jpg
post_img_link: https://pixabay.com/photos/rocket-launch-rocket-take-off-67643/
---

5 Aralık 2021'de, detaylara çok fazla takılmadan, oldukça hızlı bir şekilde geliştirdiğimiz [ikinciel.dev](https://ikinciel.dev/) projesini aşağıdaki tweet aracılığı ile Twitter üzerinden duyurduk.

<blockquote class="twitter-tweet tw-align-center"><p lang="tr" dir="ltr">🎉🎉🎉<br><br>Biz yazılımcı olarak artık ikinci el elektronik cihazları satışa sunabileceğiniz bir yer var;<a href="https://t.co/bo0LYe7aFf">https://t.co/bo0LYe7aFf</a> <a href="https://twitter.com/ikincieldev?ref_src=twsrc%5Etfw">@ikincieldev</a> <br><br>Sadece aktif GitHub hesabı olanlar login olarak ilan oluşturabilecek ve ilan sahibinin iletişim bilgisine ulaşabilecek. 🥳🥳🥳</p>&mdash; Özgür Adem Işıklı (@iozguradem) <a href="https://twitter.com/iozguradem/status/1467488966656024578?ref_src=twsrc%5Etfw">December 5, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Sonrası; tam bir **kaos!** Bu yazımda [ikinciel.dev](https://ikinciel.dev/) projesinin hikayesini anlatacağım.

## Fikir Aşaması

**ikinciel.dev** projesini gündeme almadan çok daha önceleri, beynimi kemiren bir sorun vardı. Yıllarca API geliştirmiş bir yazılımcı olarak, API geliştirme işinin gereğinden fazla zahmetli, düzensiz ve yavaş olduğunu düşünüyordum. Her ne kadar [GraphQL](https://graphql.org/) API dünyasında farklı bir paradigma olarak adından söz ettiriyorsa da, bir başka yol olabilme ihtimali aklımda bir türlü çıkmıyordu. Bu düşünceler zamanla kafamın içerisinde evrildi ve bir umut ışığı belirdi. Ancak ben, uzunca bir süre kafamda oluşan bu yeni paradigmaya karşı direndim ve kodlamaktan kaçındım. Kaçmamın nedeni, teoride güzel gibi duran bu fikri kodlamaya başladıktan belirli bir süre sonra, bir şekilde tıkanacak ve çıkmaz yola girecektim.

Ancak aylar geçmesine rağmen düşünceler beni bırakmadı ve sonunda [AdonisJS](https://adonisjs.com/) frameworkü temelinde fikrimin ilk çalışmasını yaparak [AdonisX](https://adonisx.github.io/) isminde yayınladım. Ancak _AdonisJS_'in fantastik major sürümlerinden sonra, projeyi AdonisJS ekosisteminden tamamen çıkarabileceğimi fark ettim ve [Axe API](https://axe-api.github.io/) ismiyle, [Express.js](https://expressjs.com/) ve [knex.js](https://knexjs.org/) üzerine inşaa ettim. Düşünce aşamasında gözümü korkutan tüm problemleri çözmüş, gerçek hayatta kullanılabilir bir API framework'ü geliştirmiştim.

Yine de bu noktada önemli bir eksik vardı; **production tecrübesi**. Bu nedenle proje arayışına koyuldum ve yıllardır arkadaşım olan [Furkan Başaran](https://twitter.com/frknbasaran) ile birlikte çalışabileceğimiz projeler üzerine düşünmeye başladık. O dönemde, döviz kurları aşırı artmış, çevremizdeki pek çok yazılımcı önümüzdeki yıllardaki potansiyel elektronik cihaz ihtiyaçlarını göz önünde bulundurarak fazladan alım yapmıştı. Bu, yazılımcıların elinde pek çok ikinci el ürün biriktiği anlamına geliyordu. Adını burada anmak istemediğim, kangrene dönmüş bazı ikinci el satış sitelerinde de ciddi bir güven problemi olduğunun farkındaydık. Bu nedenle sadece yazılımcıların kullanabileceği bir ikinci el ilan platformunun mevcut boşluğu harika bir şekilde doldurabileceğini düşündük. Benim için de [Axe API](https://axe-api.github.io/)'yi test edebileceğim güzel bir gerçek hayat senaryosu olacaktı. Fikirde mutabık kaldık ve böylece **ikinciel.dev** projesinin temelleri atılmış oldu.

## Geliştirme Aşaması

Geliştirme aşamasına çok zaman ayırmak istemiyorduk. Bu nedenle en az özellik ile yola çıkma kararı aldık ve **sadece bir hafta sonunda** bitirecek şekilde bir özellik listesi hazırladık. Sözümüze sadık kalarak, pek çok hataya rağmen projeyi fikrimizi anlatacak düzeye getirdik. Hataların farkındaydık ama bütün bir hafta sonunu ayırmak bile bizi yeterince yormuştu ve bir an önce duyurularını yapmak istiyorduk. Zaten düşüncemize göre, bir kaç beğeni ve RT sonrasında, yaklaşık 10 kadar üye edindikten sonra [ikinciel.dev](https://ikinciel.dev/) kimsenin uğramayacağı bir proje olabilirdi.

Tüm bunlara rağmen hızlı geliştirme, en az özellik ve tolere edilebilir hatalar silsilesiyle projeyi hazırladık. Ek olarak, back-end tarafında **Axe API**'yi de kullandık. Böylece beni proje geliştirmeye iten nedenler karşılık buldu.

## Twitter Duyurusu

Duyuru için Twitter üzerindeki yoğun zamanlardan birisini seçmedik. Hatta bunu hiç düşünmedik. İşimiz bittiği anda, doğrudan ben bir tweet yazarak projeyi duyurdum. Furkan tweeti okuduğunda, ufak bir yazım hatası olduğu konusunda beni uyardı. Ancak ben umursamadım ve tweeti silip yeniden yazmaya üşendim.

Tüm bunlara rağmen, ilk anlardan itibaren müthiş bir beğeni gelmeye başladı. Ama daha da önemlisi, projeye inanan pek çok yazılımcı, tweeti alıntılayarak **ikinciel.dev**'in ne kadar harika bir proje olduğunu yaymaya başladı. İlk 20-30 dakikalık güzel tepkilerden sonra sönümleneceğini düşünmemize rağmen giderek arttı. Gün bittiğinde 800'ün üzerine yeni kullanıcımız, binlerce beğenimiz, yüzlerce RT ve onlarca ilanımız olmuştu. Ancak bununla birlikte, pek çok çalışmayan nokta, özellik talebi ve geri bildirim vardı.

Topluluğun gösterdiği ilgi bizi şok etmişti ama yapacak işlerimiz vardı. Hiç vakit kaybetmeden gördüğümüz hataları düzeltmeye başladık. Buna paralel olarak, bütün geri bildirimleri cevaplamaya çalıştık. Eksiksiz tüm önerileri not aldık. Elimizdeki imkanlar dahilinde yaptığımız her geliştirmeyi duyurduk ve topluluğun geri bildirimine sunduk.

Bir yandan bunlar olurken, bir yanda da projeye katkıda bulunmak isteyen pek çok arkadaş edindik. Hemen hepsiyle görüşmeler gerçekleştirdik, fikirlerini aldık ve nasıl birlikte çalışabileceğimize odaklandık. Bugün itibari ile hala birlikte çalıştığımız arkadaşlarımızın varlığı bizi çok mutlu ediyor.

## Gelecek Planları

İşimiz çok!

Sadece daha fazla özelliğe odaklanmıyoruz. Daha fazla özellik eklemektense, **ikinciel.dev**'i mümkün olan en sade ama bir o kadar da kullanışlı bir yapıda tutmak istiyoruz. Bu süreçte daha fazla ortaklık kurmayı amaçlıyor, global bir ürüne dönüşmenin çalışmalarını yapıyoruz. Çok düşünmeden hızlıca kod yazdığımız dönemi geride bıraktığımıza ve artık daha fazla düşünerek, daha fazla geri bildirim dinleyerek, daha fazla metrik ölçerek, topluluk için en faydalı adımları bulmaya çalıştığımıza inanıyoruz.

Son olarak; [ikinciel.dev](https://ikinciel.dev/), bir yan projeden çıkan başka bir yan proje olması dolayısıyla, [Axe API](https://axe-api.github.io/) ile yeterince ilgilenememiş oldum. Buna rağmen bugün itibari Axe API ile harika çalışan bir altyapı sağlamanın gururunu yaşıyorum. İlerleyen dönemlerde Axe API alakalı daha fazla makale yazacağım.
