---
layout: post
title:  "#2 CSharp String İçeriği Dosya Olarak İndirme"
date:   2016-01-25 19:00
categories: [Turkish, Notes]
tags: csharp, dosya indirme, string, byte array
meta: csharp, dosya indirme, string, byte array
author: ozziest
post_img: notes.jpg
post_img_link: https://pixabay.com/en/books-pages-story-stories-notes-1245690
---

> Bu bir kendime not yazısıdır. Daha sonradan unutma ihtimalime karşın kendimce notlar alıyorum. 

Elimizde olan `string` bir veriyi, web üzerinden dosya olarak indirilecek şekilde göndermek için aşağıdaki kodu kullanabiliriz;

<pre><code class="language-js">
public FileStreamResult Download()
{
	string content = "My Data Content";
    return File(
    	new MemoryStream(
    		Encoding.UTF8.GetBytes(content)
    	), 
    	"application/text", 
    	"file.txt"
    );
}	
</code></pre>