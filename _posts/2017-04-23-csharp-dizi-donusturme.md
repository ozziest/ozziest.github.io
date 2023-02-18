---
layout: post
title: "#7 C# Liste Dönüştürme"
date: 2017-04-23 12:50
categories: [Turkish, Notes]
keywords: csharp, list, convert
author: Özgür Adem Işıklı
post_img: notes.jpg
post_img_link: https://pixabay.com/en/books-pages-story-stories-notes-1245690
lang: tr
---

> Bu bir kendime not yazısıdır. Daha sonradan unutma ihtimalime karşın kendimce notlar alıyorum.

### Giriş

Çok basit olan bu işlemi bir çok farklı yöntemle gerçekleştirebilirsiniz. Ben bu yazıda gördüğümde çok beğendiğim yöntemi; `ConvertAll` metodunu örneklendireceğim.

### Uygulama

Aşağıdaki yapıda bir listemiz olduğunu varsayalım;

<pre><code class="language-csharp">
public static void Main()
{
    List<Dictionary<string, dynamic>> items = new List<Dictionary<string, dynamic>>() 
    {
        new Dictionary<string, dynamic>() 
        {
            { "id", 1},
            { "name", "Foo Bar" }
        }
    };
}
</code></pre>

Bu listeyi; aşağıdaki sınıfa göre dönüştürmek istiyoruz.

<pre><code class="language-csharp">
public class User
{
    public int id { get; set; }
    public string name { get; set; }
}
</code></pre>

Bunun için aşağıdaki formu kullanabiliriniz;

<pre><code class="language-csharp">
public static void Main()
{
    List<Dictionary<string, dynamic>> items = new List<Dictionary<string, dynamic>>() 
    {
        new Dictionary<string, dynamic>() 
        {
            { "id", 1},
            { "name", "Foo Bar" }
        }
    };

    List< User > users = items.ConvertAll(
        new Converter<Dictionary<string, dynamic>, User>(MyConverter)
    );
}

public static User MyConverter (Dictionary<string, dynamic> input)
{
    return new User()
    {
        id = input["id"],
        name = input["name"]
    };
}
</code></pre>

Burada; List tipinde olan `items` değişkeninin `ConvertAll` metodunu, yeni bir `Converter` göndererek çağırıyoruz. Bu `Converter`'ın giriş değerininin `Dictionary<string, dynamic>`, dönüş değerinin ise `User` sınıfının bir örneği olmasını belirtiyoruz. Son olarak da bu şartları sağlayan dönüştürücü metodumuzun hangisi olduğunu belirtiyoruz. Bizim örneğimizde bu metot `MyConverter` isimli metot. `ConvertAll` metodu sırayla tüm elemanlar için gönderdiğimiz metodu çağırarak oluşan yeni liste değişkenini bize geriye gönderiyor.
