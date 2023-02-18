---
layout: post
title: "Versiyon Kontrol Sisteminde (Git) Branch Nerelerde Kullanılır?"
date: 2014-02-24 19:00
categories: [Turkish, Coding]
keywords: php, git, vcs, branch
author: Özgür Adem Işıklı
lang: tr
---

Versiyon Kontrol Sistemleri (VCS) proje geliştirirken olmazsa olmazlarımızın başında geliyor artık. Bir noktadan sonra “Eskiden biz nasıl kod geliştiriyor muşuz?” demeden edemiyoruz. Uzunca bir süredir ben de her projemde ilk olarak versiyon kontrol sistemi (benim için Git) kuruyorum. Ancak özellikle tek başıma çalıştığım geliştirmelerde hiç branch oluşturma ihtiyacı duymuyordum.

## Sorun

Örnek üzerinde gidelim. Bir web uygulaması geliştirdiniz. Testlerini yaptınız ve production ortamına uygulamanızı attınız. Örnek uygulamamızın iki temel bölümden oluştuğunu varsayalım;

- 1- Oturum yönetimi

- 2- Oturum açan kullanıcı rolüne göre çalışan dört farklı modül; A, B, C, D

Daha sonra siz A modülüne çok güzel bir özellik eklemek istediniz. Çalışmaya başladınız ve epey bir kodladıktan sonra production üzerinde çalışan uygulamanızın oturum yönetiminde çok kritik bir hata tespit ettiniz. Hemen bu hatayı düzeltmeniz gerekiyor. Eğer master haricinde bir branch oluşturmadıysanız yani doğrudan master üzerinde çalışıyorsanız hatayı düzeltip master’a gönderirseniz, henüz tamamlamadığınız A Modülü’ndeki değişiklikleri de production’a atmak zorunda kalırsınız.

## Tanımlama

Buna benzer bir durum başıma geldikten sonra şunu fark ettim;

- Aynı anda, farklı bir çok bölüm üzerinde çalışma gerçekleştirebilirsiniz.

- Müşteri talepleri, kritik ve hemen düzeltilmesi gereken hatalar vs. gibi nedenlerle o an geliştirmekte olduğunuz bölümü yarım bırakmak zorunda kalabilirsiniz.

İşte bu durumda tam olarak şunu isteyeceksiniz;

_“Ben A Modülü’nü geliştireyim ama aynı anda B Modülü’de geliştirilebilsin. A bittiğinde ben bunu sunucuya atayım lakin B’nin geliştirmesi tamamlanmadığı için o sunucuya gitmesin.”_

## Çözüm

Çözüm tek kelimeyle; **branch!**

Hiç bir zaman master kalıp üzerinde çalışmayın. En azından “development“, “bug” vb. gibi kalıplarınız olsun. Tüm geliştirmelerinizi bu kalıplar üzerinde tamamlayın. Eğer modüler bir yapınız varsa, geliştirme yaptığınız her modül için de bir branch oluşturabilirsiniz. İş vereninizin ya da müşterinizin ne zaman “Bu işi bırak, şuna başla” diyeceği belli olmaz.
