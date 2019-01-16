---
layout: post
title:  "#9 CSharp İle Generic Sınıf İşlemleri"
date:   2017-05-05 23:00
categories: [Turkish, Notes]
tags: csharp, generic, class, instance
meta: csharp, generic, class, instance
author: ozziest
---

<a href="https://pixabay.com/en/books-pages-story-stories-notes-1245690/" target="_blank">
    <img src="/images/posts/notes.jpg" class="center" />
</a>

> Bu bir kendime not yazısıdır. Daha sonradan unutma ihtimalime karşın kendimce notlar alıyorum.

### Giriş

C# üzerinde sevdiğim bir başka özellik, zarif kullanımları açısından ***Generic Classes*** kavramı. Bu tarz sınıflar belirli işlemlerin gerçekleştirilmesini, belirli sabit veri türlerinden bağımsız olarak yapmaya yarıyor. Bu yazı üzerinde de unutmak istemediğim bazı kullanımları not almayı istiyorum.

### Uygulama

<pre><code class="language-csharp">
public class Test 
{

    public void Process< T >()
    {
        Type type = typeof(T);
        MyCustomType instance = (MyCustomType)Activator.CreateInstance(type);
    }

    public void Run()
    {
        Process< MyCustomType >();
    }

}
</code></pre>

- **Process** metodu tanımında nasıl Generic Class alabileceğimizi ve metodun içerisinde de (Generic Class konusundan bağımsız olarak) nasıl bir type için yeni bir instance oluşturabileceğimizi görüyoruz.
- **Run** metodu içerisinde de Generic Class tanımı içeren bir metodu nasıl çağırabileceğimizin örneği bulunmaktadır.
