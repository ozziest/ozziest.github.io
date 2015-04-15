---
layout: post
title:  "Döküman Tabanlı Geliştirme"
date:   2014-07-18 22:39:28
categories: diğer
tags: php, döküman, ddd, test
meta: php, döküman, ddd, test
author: ozziest
---

Son 6 ayımı **Laravel** üzerine aktif olarak çalışarak geçirdim. Beni **test yazma** ve **SOLID Prensipleri** gibi birçok nimeti kullanmaya zorladığı için kendisine minnettarım. Özellikle paket tabanlı çalışmanın, kodların tekrar tekrar kullanılabilmesi açısından önemli olduğunu düşünmeye başladım bu süreç içerisinde. Ancak en büyük kazancım; tamamen spontane bir şekilde gelişen döküman yazma özelliğim oldu.

[TDD](http://en.wikipedia.org/wiki/Test-driven_development) ve [BDD](http://en.wikipedia.org/wiki/Behavior-driven_development) gibi kavramlara bir şekilde aşina olduğumuzu düşünüyorum. Ancak ben hiç bir zaman iyi bir test yazarı olamadım. Sonradan farkettim ki ne SOLID’i  ne de ne yapmak istediğimi adam akıllı biliyorum.

Biz geliştiricilerde gördüğüm; ustalastıkça kod yazmadan önce daha çok düşünüyoruz. Gerek ofisimizdeki tahtalar, gerekse kağıtlar kalemler aracılığı ile aklımızdakilerin görsel bir iskeletini çıkartıyoruz. Ancak çoğu zaman “Hangi sınıfın hangi metodu hangi iş için çalışacak?”, “Bir hata durumunda o metot nasıl davranacak?” gibi şeyleri bu planlara dahil etmiyoruz. Onun yerine kodlama ya da test yazma aşamasında bunları düşünüyoruz. Zihminizdekileri doğrudan koda dökmek tasarım çıkmazlarına sebep verebiliyor. İşte bu noktada **DDD (Document Driven Development)** bizim yardımımıza koşuyor.

Ben artık genel planlarımı yaptıktan sonra, henüz tek satır kod yazmadan tasarlayacağım sınıfın dökümanını oluşturuyorum. Hangi parametreleri yollayacağım, ilgili metotdan dönecek istisnalar neler olacak vb. durumların hepsini dökümanı yazarken çok detaylı bir şekilde açıklamış oluyorum. Daha sonrasında eğer **deadline** için vaktim varsa dökümana göre testlerimi yazıyorum. En son aşamada ise dökümandaki gibi çalışacak kodları yazıyorum. Örneğin bir döküman yazdığımızı düşünelim ve aşağıdaki gibi Kullanıcı silme işleminin nasıl yapılacağını kodla örnekleyelim;


<pre><code class="language-php">
try
{
	// Find the user using the user id
	$user = Sentry::findUserById(1); 
	// Delete the user
	$user-&gt;delete();
}
catch (Cartalyst\Sentry\Users\UserNotFoundException $e)
{
	echo 'User was not found.';
}
</code></pre>


Bunun gibi bir döküman hazırladıktan sonra, tam olarak neye ihtiyacımız olduğu zaten ortaya çıkmış oluyor.  Ondan sonra tek yapmamız gereken ilgili testleri yazmak ve çalışan sınıflarımızı kodlamak. Planlamada sağlanan bu kolaylığın yanı sıra yazdığımız sınıfın, paketin ya da kod parçacığının nasıl çalıştığı anlatan bir döküman elimizin altında oluyor. Şahsen bu gibi bir dökümanla neleri test edebileceğimi daha iyi kavramış oluyorum. Zira test yazmayı öğrenirken en çok zorlandığım şey, ilgili kod bölümünün nesini test edeceğimi bilmemekti. Belki de ben bu konuda çok beceriksizimdir. Eğer siz de benim gibi test yazmakta zorlanan beceriksizlerdenseniz DDD işinize yarayacaktır.