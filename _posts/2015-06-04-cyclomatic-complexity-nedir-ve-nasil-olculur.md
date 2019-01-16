---
layout: post
title:  "Cyclomatic Complexity Nedir ve Nasıl Ölçülür?"
date:   2015-05-28 20:00
categories: [Turkish, Coding]
tags: cyclomatic complexity, kod karmaşası, php, ölçüm, phploc
meta: cyclomatic complexity, kod karmaşası, php, ölçüm, phploc
author: ozziest
---

<a href="https://pixabay.com/en/rocket-launch-rocket-take-off-nasa-67643/" target="_blank">
    <img src="/images/posts/coding.jpg" class="center" />
</a>

**[Cyclomatic Complexity](http://en.wikipedia.org/wiki/Cyclomatic_complexity)** kavramı yazılım camiasına ait bir terimdir ve geliştirilen kodların karmaşasının ölçülmesi olarak ifade edilir. Ölçüm yapılırken akış diyagramındaki bağımsız bölümler ve karar yapıları göz önünde bulundurulur. Ne kadar çok karar yapısı bulunuyorsa, kod karmaşası o kadar artık. Kod karmaşasının arttığı durumlarda üzerinde çalışılan projenin sürdürülebilir olması güçleşir, hata ile karşılaşma oranında artış gözlemlenir. Bu nedenle kod karmaşasının minumumda tutulması her zaman için önerilir.

## Nasıl Ölçülür?

Bu ölçümü yapan ve teorinin formüllerine bağlı olarak çalışan hazır araçlar vardır. Ben ağırlıklı olarak PHP tarafında kod geliştiren bir geliştirici olduğum için bu işi yapan ve yanında başka raporlamar da gerçekleştiren [PHPLOC](https://github.com/sebastianbergmann/phploc) isimli bir araç kullanıyorum. Kendisi [PHPUnit](https://github.com/sebastianbergmann/phpunit)'i de geliştiren [Sebastian Bergmann](https://github.com/sebastianbergmann) tarafından geliştirilmiş bir araç. 

## PHPLOC Kurulumu

Aşağıdaki adımları izleyerek kurulumu gerçekleştirebilirsiniz.

<pre><code class="language-bash">
$ wget https://phar.phpunit.de/phploc.phar
$ chmod +x phploc.phar
$ mv phploc.phar /usr/local/bin/phploc
</code></pre>

## Kullanım 

Proje dosyalarınızın bulunduğu klasörü parametre olarak göndererek sonuçları incleyebilirsiniz;

<pre><code class="language-bash">
$ phploc src

phploc 2.0.4 by Sebastian Bergmann.

Directories                                          3
Files                                                8

Size
  Lines of Code (LOC)                             1858
  Comment Lines of Code (CLOC)                     560 (30.14%)
  Non-Comment Lines of Code (NCLOC)               1298 (69.86%)
  Logical Lines of Code (LLOC)                     289 (15.55%)
    Classes                                        260 (89.97%)
      Average Class Length                          37
      Average Method Length                          9
    Functions                                        5 (1.73%)
      Average Function Length                        5
    Not in classes or functions                     24 (8.30%)

Complexity
  Cyclomatic Complexity / LLOC                    0.67
  Cyclomatic Complexity / Number of Methods       7.86

Dependencies
  Global Accesses                                    2
    Global Constants                                 2 (100.00%)
    Global Variables                                 0 (0.00%)
    Super-Global Variables                           0 (0.00%)
  Attribute Accesses                                48
    Non-Static                                      48 (100.00%)
    Static                                           0 (0.00%)
  Method Calls                                      96
    Non-Static                                      91 (94.79%)
    Static                                           5 (5.21%)

Structure
  Namespaces                                         4
  Interfaces                                         0
  Traits                                             0
  Classes                                            7
    Abstract Classes                                 0 (0.00%)
    Concrete Classes                                 7 (100.00%)
  Methods                                           28
    Scope
      Non-Static Methods                            28 (100.00%)
      Static Methods                                 0 (0.00%)
    Visibility
      Public Method                                 10 (35.71%)
      Non-Public Methods                            18 (64.29%)
  Functions                                          1
    Named Functions                                  0 (0.00%)
    Anonymous Functions                              1 (100.00%)
  Constants                                          1
    Global Constants                                 1 (100.00%)
    Class Constants                                  0 (0.00%)
</code></pre>

## Yorumlama

Gösterilen sonuçlar arasında yer alan **Complexity** bölümünde sayısal değerler göreceksiniz. Bunlar koda ve metotlara göre kod karmaşasını gösteren değerlerdir. Aşağıdaki listeye göre kodunuzun karmaşasını saptayabilirsiniz;

* `0-10`: Basit kod yapısı, yüksek sürdürülebilirlik.
* `11-20`: Kabul edilebilir karmaşıklık, orta sürdürülebilirlik.
* `21-50`: Yüksek karmaşıklık, düşük sürdürülebilirlik. 
* `50-100`: Test adilemez, çok düşük sürdürülebilirlik.
* `>100`: Felaket

## Karmaşıklığı Azaltma

Karmaşıklık bir çok farklı faktöre bağlı olsa da, genel olarak aşağıdaki adımlar uygulanarak azaltılabilir;

* [SOLID Prensipleri](http://en.wikipedia.org/wiki/SOLID_%28object-oriented_design%29)'ne uygun kod geliştirmek.
* [Tasarım Desenleri](http://en.wikipedia.org/wiki/Software_design_pattern)'ni aktif kullanmak.
* [Basit Tasarım](http://en.wikipedia.org/wiki/KISS_principle)'dan yana olmak. 
* Bol bol [Refactoring](http://en.wikipedia.org/wiki/Code_refactoring) yapmak.

