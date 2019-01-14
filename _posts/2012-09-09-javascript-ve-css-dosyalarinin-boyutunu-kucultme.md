---
layout: post
title:  "JavaScript ve CSS Dosyalarının Boyutunu Küçültme"
date:   2012-09-09 19:00
categories: [Turkish, Coding]
tags: js, css, minift, dosya sıkıştırma, js sıkıştırma, css sıkıştırma
meta: js, css, minift, dosya sıkıştırma, js sıkıştırma, css sıkıştırma
author: ozziest
---

Bir çok web sitesinin kodlarını incelediğimizde artık JS dosyalarının isimlerinde min eklentisi görürüz. Bu eklenti o JS dosyasının sıkıştırılmış olduğunu bize gösterir. Kodları incelediğimizde birbiri içine sıkışmış kodlar bulunur ve gayet düzensizdir. JS kodlarının bu şekilde bir yapıya sokulmasının amaçları vardır. Bu yazımızda bu amaçları ve bu işlemi nasıl yaptığımızı inceleyeceğiz.

Öncelikle geçmişte sadece JS değil CSS kodlarımız da çok büyük dosyalar olmuyordu. Ancak gelişen web teknolojileri sayesinde çok daha fazla CSS ve JS kodu yazıyoruz. Özellikle JQuery ve AJAX’ın artık web’in olmazsa olmazları olduğu günümüzde bilhassa JS işlemleri uygulamamızın her sayfasında bulunmakta. Proje dağınıklığı ve hız açısından ise tüm JS kodları tek bir JS dosyasında (ya da gruplara bölünmüş şekilde farklı dosyalarda) toplanmakta. Sonuç olarak boyutu büyüyen dosyalar demek; web uygulamamızın daha geç yüklenmesi demektir. Bu sorunu aşmak için JS ve CSS dosyalarımız sıkıştırılır. Kaynak kodları görüntülendiğinde ise kullanıcılar karışık kodlarla karşılaşırlar. Zaten bir diğer avantajı da budur. Siz kodlarken değişken isimlerini kullanıldıkları yere göre verirsiniz ama JS kodlarının sizin yazdığınız gibi düzenli olması çok da sağlıklı değildir. Her ne kadar bu sıkıştırma işleminin zıttı olan çözme işlemi için de araçlar olsa da yani tam bir güvenlik asla sağlamasa da; sıkıştırma işlemi web uygulamalarında tercih edilirler.

Aşağıda bu işlemleri yapabileceğiniz farklı siteler bulunmakta.

JS Sıkıştıma İşlemi: [javascriptcompressor.com](http://javascriptcompressor.com/)

JS Çözme İşlemi: [jsbeautifier.org](http://jsbeautifier.org/)