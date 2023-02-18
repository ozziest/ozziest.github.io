---
layout: post
title: "Web Projenize Grafik Ekleyin: Google Charts Tools"
date: 2012-10-09 19:00
categories: [Turkish, Coding]
keywords: web, grafik, google chart tools, grafik ekleme
author: Özgür Adem Işıklı
lang: tr
---

Google Developers henüz yeni sayılan yayın hayatında bir çok Api ve kütüphane ile geliştiricilere destek veriyor. Bunların her birine ayrı ayrı değinmek gerek tabii fakat bu yazımda projelerimde kullandığım bir Javascript kütüphanesinden bahsetmek istiyorum. Eğer sizde projelerinizde verileri kullanıcılara grafikler aracılığı ile ulaştırmak istiyorsanız internette çeşitli kütüphaneler ile bunu gerçekleştirebilirsiniz. Bu kütüphanelere bir alternatif de Google aracılığı ile hazırlanmış. Google Chart Tools adı altında sunulan Api aracılığı ile hızlı bir şekilde projelerinizde farklı türlerde grafikler ekleyebiliyorsunuz.

https://developers.google.com/chart/ adresinden Api’ye ücretsiz olarak ulaşabilirsiniz. Bu belkide en büyük avantajı çünkü aynı işlevleri yapan diğer kütüphanelerin lisans ücretleri bulunmakta ya da çeşitli linkler ile kendi reklamlarını yapmaktadırlar. Ancak Google’ın sunduğu bu kütüphane ile herhangi bir reklama maruz kalmadan uygulamanızda grafikler yaratabiliyorsunuz.

Tamamen JS ve HTML5 altyapısını kullandığı için son derece hızlı ve kullanışlı bir kütüphane. Kullanımı ise oldukça basit. Hemen bir örnek vermek gerekirse yandaki grafiği oluşturmak için aşağıdaki kod parçasını projenize eklemeniz yeterli.

<pre><code class="language-js">
google.load("visualization", "1", {packages:["corechart"]});
google.setOnLoadCallback(drawChart);
function drawChart() {
var data = google.visualization.arrayToDataTable([
  ['Task', 'Hours per Day'],
  ['Work',     11],
  ['Eat',      2],
  ['Commute',  2],
  ['Watch TV', 2],
  ['Sleep',    7]
]);
var options = {
  title: 'My Daily Activities'
};
var chart = new google.visualization.PieChart(document.getElementById('chart_div'));
chart.draw(data, options);
}
</code></pre>

Bu kod ile belirlediğiniz div içinde grafiğiniz görüntülenmektedir (Bu örnek için ilgili div ID’si chart_div). Grafikte yer alacak verileri ise Json verisi olarak hazırlamak zorundasınız. data değişkeninin içeriğinde veri yapısı görülmektedir. Eğer bu verileri veritabanından çekerek oluşturmak istiyorsanız dikkat etmeniz gereken verileri Json yapısında hazırlamak sadece. Link aracılığı ile dökümanları ve örnekleri inceleyerek farklı şekillerde grafikler hazırlayabilirsiniz. Şüphesiz en çok ilgi çeken dünya haritası şeklindeki grafik olacaktır. Zira dünya haritasını gösterip, gönderdiğiniz ülke verilerine göre grafik üzerinde bir yoğunluk belirleyebiliyorsunuz ve bu şekilde çalışan bir grafik kütüphanesi piyasada pek bulunmamaktadır.
