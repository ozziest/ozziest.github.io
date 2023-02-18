---
layout: post
title: "ROS - Robot Operating System Nedir?"
date: 2015-03-10 22:20
categories: [Turkish, Coding]
keywords: ros, robot, operating, system, robotik, c++, python
author: Özgür Adem Işıklı
lang: tr
---

Öncelikle ROS'u kısaca tanımlamaya çalışacağım;

- Robotlar için bir işletim sistemidir.
- Paketler için bir koleksiyondur ve yazılım derleme aracı olarak kullanılmaktadır.
- Dağıtılmış işlemler/makineler arası iletişim ve yapılandırma için bir mimaridir.
- Sistemin çalışma ve analiz aşamaları için geliştirme aracı olarak kullanılabilir.
- Dil bağımsız bir mimariye sahiptir. Aynı robot üzerinden birçok farklı dil kullanılabilir. (c++, python, lisp, java)

## ROS Ne Değildir?

- Gerçek bir işletim sistemi değildir.
- Bir programlama dili değildir.
- Geliştirme ortamı/IDE değildir.

## ROS Dağıtımları

- ROS Fuerte Turtle
- ROS Electric Emys
- ROS Diamondback
- ROS C Turtle
- ROS Box Turtle

## ROS ve Komponentleri

### ROS Core

ROS Master,

- Merkezi XML-RPC sunucusudur.
- Bağlantıların iletişimi sağlamaktadır.
- Sabit konfigürasyon parametreleri ya da diğer keyfi verileri kaydeder.
- Aslında, insanların anlayabileceği dilde mesajları barındırran ağ tabanlı bir yapıdır.

### ROS Stacks & Packages

ROS kodu iki farklı seviye içerisinde gruplanmıştır;

- Packages
- Stacks

Packages, ROS'a çok küçük bir bağımlılıklar bağlanmış yazılım koleksiyonlarıdır. Stacks ise bu ufak parçaların bir araya gelerek oluşturduğu dağıtıma verilen isimdir.

#### Packages

- Kodlarınızı, build yapılandırmasınızı ve launch dosyalarınızı barındıran bir dizindir.
- Birden fazla `node` içerebilir.
- İçerisinde `manifest.xml` adında bağımlılıkların tanımlandığı bir konfigürasyon dosyası barındırır.
- Sadece paketin amacı ile ilgili kodları içerir.

#### Nodes

- Bazı fonksyionları yerine getiren yapılardır.
- Diğer `Topic` ve `Services`ler ile haberleşebilir.
- Node isimleri benzersizdir.
- Node yapısı, iyi ayrılmış, skale edilebilir bir yapının oluşturulması için vardır.

#### Build System

- Kodların nasıl derleneceğinin belirtildiği `CMakelist.txt` dosyasına ihtiyaç duymaktadır.
- `rosmake` komutu paket ve bağımlılıkları derler.
- Eğer bağımlılıklar indirilmemişse, indirilir ve kurulur.
- Aynı anda birden fazla paket derlenebilir. ROS önce bağımlılıkları çözer.

## Komut Satırı

ROS aşağıdaki bazı komutları içerisinde barındırır;

- `roscd`: ROS paketi dizinine geçer.
- `rosls`: Paket içeriğini listeler.
- `rosmake`: Paketi derler.
- `roscreate-pkg`: Yeni bir paket oluşturur.
- `rosdep`: Paket bağımlılıklarını listeler.
- `roscp`: Paketler arası kopyalama işlemi gerçekleştirir.
- `rostest`: Test dosyalarını çalıştırır.

## İletişim Yolları

### Topic (Pub/Sub)

- Asenkron iletişimler için kullanılır.
- İçerisinde güçlü mesaj tipleri barındırır.
- Callback fonksyionlara sahiptir, bu sayede mutli-thread çalışabilir.
- İstek/cevap etkileşimleri için uygun değildir.
- Çoktan-Çoka bağlantılar için uygundur.

### Service (Higher Priority)

- Standart fonksiyonlar gibi, senkron olarak kullanılır.
- Güçlü mesaj tiplerini içerisinde barındırır.
- Bire-bir iletişim için uygundur.
- Birden fazla istemciler olabilir.
- Servis çağrıları boyunca Topic Callback fonksyionları herhangi bir sorun teşkil etmez.

### Actions

- Topic yapısının üzerine inşaa edilmiştir.
- Uzun işlemler gerçekleştirilmesi için kullanılır.
- İptal edilebilir.

## Messages

- Node iletişimi mesaj gönderimi üzerine kuruludur.
- Mesajlar, özel tipli alanlardan oluşan bir veri yapısından ibarettir.
- ROS içerisinde birçok hazır mesaj standartı vardır ve özel olarak buna yenileri eklenebilir.
