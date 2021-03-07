---
layout: post
title:  "PHP To C#"
date:   2015-12-10 20:00
categories: [Turkish, Coding]
tags: php, c#, c sharp, geçiş, ön izleme, inceleme, karşılaştırma
meta: php, c#, c sharp, geçiş, ön izleme, inceleme, karşılaştırma
author: ozziest
post_img: coding.jpg
post_img_link: https://pixabay.com/en/rocket-launch-rocket-take-off-nasa-67643
---

Uzunca bir süredir **PHP** ile birlikte uygulamalar geliştiriyor olsam da, şartlar beni geçtiğimiz günlerde **.NET** ile bir uygulama geliştirmeye götürdü. **PHP** ile belirli bir seviyeye gelmiş uygulama, sonrasında **C#** ve **.NET** platformu bileşenleri kullanılarak geliştirildi. Ben de bu yazıda bir yandan **PHP** ile **C#**'ı, bir yandan da **.NET** platformunu irdelemek istedim. Eğer bir gün olur da **PHP**'den sıkılırsanız, nelerle karşılaşabileceğinizi dilim döndüğünce anlatmak istiyorum.

> .NET dünyasında çok yeni olduğum için bu yazıyı lütfen teknik bir yazı olarak değerlendirmeyin. **Önizleme** daha doğru bir tabir olacaktır.

### C# İsimlendirme Tutarlılığı

Açıkçası **PHP** gibi kendi içinde oldukça tutarsız isimlendirmeleri olan bir dilden çıkıp da **C#**'a geçmek sizi oldukça rahatlatacaktır. Bir tarafta `strpos` ve `str_replace` gibi kötü isimlendirmeler, diğer tarafta `Convert.ToInt32` ve `Convert.ToBoolean` gibi yapılar söz konusu. Bu tabii olarak tatlı ***tatlı*** bir detay. 

### Tip Güvenli Syntax

Her şeyden önce, **Tip Dayatması** tabirini daha çok sevdiğimi belirtmek isterim. :) 

**Tip Dayatma** desteği **PHP7** ile birlikte artık kullanılabilir durumda. Ancak **PHP**'nin omuzlarında geçmişe yönelik uyumluluk yükü olduğu için, bu özelliğin **PHP** projelerinde çok fazla kullanılabileceğini sanmıyorum. Zira **PHP**'de bu özellik sizin tercihinize bırakılmış durumda. Özgürlük her zaman güzeldir ama developer yeterince bilgili değilse, özgürlük kaosa dönüşecektir.

**C#**'ta da tip dayatmasından kurtulma şansınız `dynamic` ile bir nebze mümkün olsa da, genel olarak bir zorlama söz konusudur. Ne kadar veri tipleri arasındaki tutarlılığa sabit kalırsanız, uygulama o kadar kararlı seyredecektir.

### Geliştirme Ortamı

**PHP** ile bir uygulama geliştirmek isterseniz oldukça ciddi bir özgürlüğünüz var ve en basit text editörden, oldukça gelişmiş IDE'lere kadar geniş bir yelpazeye sahipsiniz. İster ücretsiz olsun ister ücretli, kesinlikle kendinize uygun bir geliştirme ortamı bulabilirsiniz.

**C#** ile uygulama geliştirecekseniz tek çıkar yol var: **Visual Studio**. Evet **Visual Studio Code**'dan haberim var ve **Microsoft**'un open source hamlelerinden sonra diğer platformlar için geliştirdiği bazı araçları da denedim ancak **C#** gibi bir dil ve **.NET** gibi bir framework ve bileşenlerini `henüz` kaldırabilecek durumda olduklarını düşünmüyorum. Bu nedenle **.NET** dünyasında sizin handikapınızı oluşturacak şey **Visual Studio**'dur, çünkü lisans ücreti ödemek zorundasınızdır. 

> Düzeltme: [Serkan İnce](http://twitter.com/serkanince444) uyardı; **Visual Studio Community** sürümü tamamen ücretsizmiş. Bu ek bilgiyle birlikte Visual Studio'yu bir avantaj olarak değerlendirmek gerekiyor.

### İşletim Sistemi

**PHP** ile uygulama geliştirken işletim sistemi size sorun çıkarmaz ancak **Visual Studio Code** harici diğer **VS** sürümleri **Windows** dışında çalışmadığı ve bir üst bölümde de belirttiğim gibi **Windows** harici platformlar için geliştirilenler henüz (bence) yeterli olmadığı için geliştirme aşamasında **Windows**'a muhtaç kalırsınız. Uzun lafın kısası; ***büyük geçmiş olsun***.

### Debugging

Öncelikle aktif olarak bir **XDebug** kullanan bir developer olduğumu belirtmek isterim. Kullandığım geliştirme ortamlarına eklentiler vasıtasıyla **XDebug**'ı entegre eder, geliştirmeye sonra devam ederdim. Ancak **Visual Studio** kullandığınızda haşır neşir olacağınız debugger, **PHP** dünyasında ek paket ve eklentiler yardımıyla kullanacaklarınıza göre kat ve kat daha kullanışlı olacaktır.

### Testing

**PHP** ile uygulama geliştirken [CodeCeption](http://codeception.com) ve [PHPUnit](https://phpunit.de/) yardımıyla fırsat buldukça `Unit Test`, sıklıkla (nasıl olsa SPA geliştiriyoruz diyerek) `Acceptance Test` yazardım. Bunların çok büyük yararı vardı çünkü uygulamada refactoring yaptığım zaman, nerelerin bozulduğunu çok net görebilirdim. Ancak **Visual Studio** ve **C#**'ın derlenen bir dil olması marifetiyle yapacağınız bir değişikliğin olumsuz sonuçlarını (büyük oranda) hemen görürsünüz. Bu nedenle herhangi bir test yazma girişimim olmadı. Belki de şuan tamamen cahilce konuşuyorum. İlerleyen dönemlerde eğer pişman olursam, bu bölümü muhakkak güncelleyeceğim.

### Güvenlik

**PHP** ya da **C#** daha güvenlidir demek pek mantıklı değil. Çünkü **PHP** geliştiriyici daha özgür bıraktığından, temel güvenlik önlemleri konusunda geliştirilen uygulama yeterli olamayabilir. Buna rağmen **PHP** dünyasında herhangi bir **modern** framework kullanılarak geliştirilen uygulama ile **C#** ile geliştirilmiş bir web uygulaması arasında bir fark oluşabildiğini henüz görmedim. Söz konusu temel güvenlik önlemleri ise her iki tarafta da aradığınızı bulabilirsiniz. Eğer aradığınız şeyi bilmiyorsanız, her iki tarafta da güvenlik açığı olan uygulamalar geliştirebilirsiniz.

### Kapalı Kod, Açık Kod

Geliştiriğiniz bir uygulamanın kodlarını müşterinize vermek istemiyorsanız **.NET** dünyasının daha uygun olduğunu düşünüyorum. Çünkü çalışan bir uygulamayı **DLL** olarak teslim edebilirsiniz. **PHP** tarafında da kod şifreleme teknik ve araçları var ama hem bunların kırılabildikleri söylentisi hem de gerçekten iyi olanların ücretli olmaları **PHP**'ye open source avantajlarını bir nebze kaybettiriyor.

### Performans

Henüz performans konusunda kıyaslama yapabilecek kadar bir bilgi oluşturamadım. Zaten bu iş oldukça teknik bir iş ve yorum yapmak yerine sayısal verilerle konuşmak doğru olandır. 

### Sonuç

Her ne kadar hoşnut olmasam da kapitalist bir dünyada yaşıyoruz ve iyi bir şey ortaya çıkarmak istiyorsanız, bütçenizi ona göre ayarlamanız gerekiyor. **PHP** ile hiç bir şey yapılmaz demek değil bu. Elbette ki bir çok kullanılacağı senaryo vardır. Nerelerde ve hangi şartlarda kullanılabileceği tamamen ayrı bir yazı konusu olduğu ve henüz o kadar tecrübeli olduğumu düşünmediğim için bunu sonraya bırakıyorum. Ancak şuan yeni bir projeye başlayacak olsam ve bu projeyi kısıtlı bir bütçeyle, tek başıma yürütecek olsam **PHP** ile yola çıkardım. Çünkü hala en iyi bildiğim şey PHP ve daha iyi sonuçlar alabileceğime inanıyorum. Ancak sizin ekibiniz ve bütçeniz yeterli ise; ***para var, huzur var.*** 

> Düzenleme: Buradaki bütçede hem developer maliyetini hem de Visual Studio lisansını kastetmiştim. [Serkan İnce](http://twitter.com/serkanince444)'nin uyarısıyla birlikte Visual Studio Community sürümünün ücretsiz olduğu gerçeği, bu cümlede sadece developer maliyetlerini bırakıyor.

