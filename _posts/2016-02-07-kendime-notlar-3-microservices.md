---
layout: post
title:  "#3 Microservice Notlarım"
date:   2016-02-07 19:00
categories: [Turkish, Notes]
tags: microservice, nedir, nasıl, avantajlar
meta: microservice, nedir, nasıl, avantajlar
author: ozziest
---

> Bu bir kendime not yazısıdır. Daha sonradan unutma ihtimalime karşın kendimce notlar alıyorum. 

Mikro servisler birbirleriyle çalışan küçük, otonom yapılardır. Peki "Ne kadar küçük?" soruna cevap olarak Jon Eaves diyor ki: ***"Bir mikroservis iki hafta içerisinde yeniden yazılabilir bir şey olmalıdır. Bir diğer basma kalıp düşünceye göre; Yeterince küçük olması gerektiğinde daha küçük değil."***


- Teknoloji Çeşitliliği
	- Tüm microservisler farklı teknolojiler kullanılarak geliştirilebilir.
	- İşe uygun olan teknolojiler seçilebilir.
	- Yeni bir teknoloji denemek istediğimiz, sadece düşük riskli bir microservice üzerinde bu teknolojiyi deneyebilir ve sonuçlarını gördükten sonra kullanma ya da kullanmama kararı alabilirsiniz.
	- Monolithic uygulamalarda yeni bir teknoloji denemesi pek mümkün olmaz ve zamanla sistem bakım maliyetleri artmaya başlar.

- Esneklik
	- Eğer bir microservice patlarsa, diğer bölümler çalışmalarına devam edebilirler.
	- Eğer monolithic bir servis patlarsa, tüm sistem patlar. Biz de bunun olmaması için birden fazla makine ile hatayı tolere etmeye çalışırız.

- Ölçeklenebilirlik:
	- Monolithic uygulamalarda her şeyi hep birlikte ölçeklemek zorundasınız.
	- Microservice mimarisinde, sadece ihtiyacı olan bölümleri kendi içerisinde ölçekleyebilirsiniz. 

- Yayınlama
	- Monolithic uygulamalarda bir satır değişiklik nedeniyle tüm sistemin yayınlanması (deployment) gerekir. Bu tüm sistemin testlerinin tekrardan çalıştırılması, tüm farklı makinalarda yer alan uygulamaların güncellenmesi vs. gibi oldukça uzun bir sürece maal olabilir.
	- Tüm uygulamanın aynı anda güncellenmesi oldukça büyük bir riski de beraberinde getirir. 
	- Microservislerde sadece tek bir mikroservisi geri kalan yapıdan tamamen bağımsız olarak güncelleyebiliriz. Böylece risk azalmış olur. Güncellenen mikroserviste bir hata varsa bile, sadece o mikroservisin (örneğin fotoğraf servisi) işlevleri duracağından (sistemin geri kalanı çalışmaya devam edeceğinden) sorunumuz daha az olacaktır.

- Organizasyonel İşbirliği
	- Takımlar ve codebase büyüdükçe hatalar çoğalma eğilimindedir. 
	- Aynı proje üzerinde çalışan büyük bir takım sürekli bir birini rahatsız etmek zorunda kalır.
	- Bu nedenlerle küçük takımlar daha üretkendir.
	- Microservisler bize daha küçük takım ve codebase avantajı sağlarlar.

- Değiştirilebilirlik
	- Eğer büyük bir organizasyonun parçasıysanız, yıllar önceden kalma ama hala çalışan bir uygulamayla karşılaşabilirsiniz. Uygulamalanın zamanı çoktan dolmuştur ama kimse ona dokunmak istemez çünkü çok büyük ve risklidir.
	- Microservislerin değiştirilmesi kolay olduğundan (2 haftada yeniden yazma süresi) gerekirse tamamen baştan yazabilirsiniz. Bu nedenle riskleri azdır.
	- Ne sıklıkla bir gün içinde 100 satırdan daha fazla kodu silmenize rağmen içinizde kuşku olmadan gününüze devam edebilirdiniz?
	- Microservislerle çalışırken yeniden yazma ya da değiştirilebilme bariyerleri monolithic uygulamalara göre oldukça azdır.
