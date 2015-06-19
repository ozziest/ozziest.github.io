---
layout: post
title:  "PHP ve Annotation Kullanımı"
date:   2015-06-06 18:00
categories: php git
tags: php, annotation, php ve annotations, kullanımı, örnek
meta: php, annotation, php ve annotations, kullanımı, örnek
author: ozziest
---

## Genel Bilgiler

Annotation kavramı veriler hakkında ön bilgiler veren başka veri blokları, **notlar** olarak adlandırılır. Normalde bir özellikleri yoktur. PHP ile yazılım geliştirme aşamasında da annotation'lar doküman blokları içerisinde sıkça kullanılır ama yazılıma etki etmezler. Ancak bazen bu durum farklılaşabilir. 

> JAVA gibi bazı dillerde annotation dilin bir parçasıdır ve programın çalışmasına doğrudan etki eder. Ancak konumuz PHP. :)

## PHP'deki Kullanım

Normal şartlar altında aşağıdaki gibi doküman bloklarına yazılan kodlar PHP tarafından yorumlanmaz ve programın akışına herhangi bir katkı sağlamaz. 

<pre><code class="language-php">
/**
 * This method gets the record by id
 *
 * @param  integer 		$id 
 * @return object
 */
public function find($id)
{
	// code samples
}
</code></pre>

Ancak PHP'de [ReflectionClass]() olarak adlandırılan bir takım sınıflarla bir sınıf için instance oluşturmadan, o sınıf hakkında bilgiler alabilirsiniz ve buna doküman blokları da dahildir;

<pre><code class="language-php">
/** 
* A test class
*
* @param  foo bar
* @return baz
*/
class TestClass { }

$rc = new ReflectionClass('TestClass');
var_dump($rc->getDocComment())
</code></pre>

Böylece herhangi bir sınıfın, herhangi bir metodunu çağırmadan önce döküman bloguna yazılan annotation'la program akışlarını değiştirebilirsiniz. 

## Neden?

"Neden böyle bir şeye ihtiyaç duyulur?" sorusunu duyuyor gibiyim. Buna [Symfony Framework](http://symfony.com/) içerisinde kullanılan [Doctirine](http://www.doctrine-project.org/) ORM modelleriyle cevap vermek güzel olabilir;

<pre><code class="language-php">
/**
 * @ORM\Entity
 * @ORM\Table(name="product")
 */
class Product
{
    /**
     * @ORM\Column(type="integer")
     * @ORM\Id
     * @ORM\GeneratedValue(strategy="AUTO")
     */
    protected $id;

    /**
     * @ORM\Column(type="string", length=100)
     */
    protected $name;
}
</code></pre>

Bir kez bu ***ön yorumlama*** işlemiyle çok kullanışlı sınıflar oluşturmanız mümkün. Ancak buna rağmen annotation kullanmanın zararlı olduğunu düşünenler de var. Tek bir doğru olamayacağı için her iki tarafa da kulak vermek ve uygulamaları görmek en akılcı yol olabilir.

## Kolay Taraf

Ancak siz böyle bir yapıyı uygulamalarınızda oluşturmak isterseniz, **Reflection** sınıfları ile uzun uzadıya uğraşmanıze gerek yok. Bu işi oldukça basit bir şekilde halleden bir paketimiz bile var; [Annotations](https://github.com/marcioAlmada/annotations). Üstelik şuanda **PHP-7** desteğiyle ilgili bir ***branch*** oluşturularak çalışmalara başlanmış bile. 

İlgili paketi kullanarak aşağıdaki gibi hızlıca annotation yorumlayabilirsiniz;

<pre><code class="language-php">
$annotations = $reader->getClassAnnotations('FooController');
$annotations->get('name')     // > string(3) "Foo"
$annotations->get('accept')   // > array(3){ [0] => "json" [1] => "xml" [2] => "csv" }
</code></pre>

Paket ile ilgili çok daha detaylı bilgileri dokümanı üzerinden öğrenebilirsiniz; [https://github.com/marcioAlmada/annotations](https://github.com/marcioAlmada/annotations)


