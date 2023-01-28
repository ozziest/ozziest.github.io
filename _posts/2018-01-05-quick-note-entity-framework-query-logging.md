---
layout: post
title: "#11 Entity Framework Query Logging"
date: 2018-01-05 20:00
categories: [English, Notes]
keywords: csharp, entity framework, logging, ef, database, query
author: Özgür Adem Işıklı
post_img: notes.jpg
post_img_link: https://pixabay.com/en/books-pages-story-stories-notes-1245690
---

> In case I forget later, this is a simple note for me.

### Introduction

Although Entity Framework is not my favorite ORM library, I have to use it when I develop a .Net application. This is a **_quick note_** about how can you create a logging with the queries created by EF.

### Example

<pre><code class="language-csharp">
using System;
using System.Collections.Generic;
using System.Reflection;

namespace ConsoleApplication
{

    public class LogHandler
    {

        public void Write(string content)
        {
            // do something you want
        }

    }

    class Program
    {
        static void Main(string[] args)
        {
            MyDBContext db = new MyDBContext();
            // You can send logs to console
            db.Database.Log = Console.Write;    

            // Or you can use a log handler object.
            LogHandler logger = new LogHandler();
            db.Database.Log = logger.Write;
        }

    }
}
</code></pre>
