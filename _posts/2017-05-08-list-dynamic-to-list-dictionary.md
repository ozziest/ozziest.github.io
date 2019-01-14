---
layout: post
title:  "#10 - From Dynamic List To Dictionary List In C#"
date:   2017-05-08 20:00
categories: [English, Notes]
tags: csharp, list, dynmaic, dictionary, convert
meta: csharp, list, dynmaic, dictionary, convert
author: ozziest
---

> In case I forget later, this is a simple note for me.

### Introduction

I love to play with ConvertAll method which List object has. Today, I have noticed that how I hate dynamic type variables. First of all, C# is a strongly typed language and these kind of languages have good benefits such as writing unbreakable application. Of course if you don't use dynamic type variables. When I realized that, I have converted my dynamic list to dictionary list and this my note about it.

### Example

<pre><code class="language-csharp">
using System;
using System.Collections.Generic;
using System.Reflection;

namespace ConsoleApplication
{
    class Program
    {
        static void Main(string[] args)
        {

            List< dynamic> source = new List< dynamic>()
            {
                new { id = 1, name = "Marlon Brando" }
            };

            List< Dictionary< string, dynamic>> result = source.ConvertAll(
                new Converter< dynamic, Dictionary< string, dynamic>>(DynamicToDictionaryConverter)
            );
        }

        public static Dictionary< string, dynamic> DynamicToDictionaryConverter(dynamic item)
        {
            if (IsDictionary(item))
            {
                return (Dictionary< string, dynamic>)item;
            }            
            
            Dictionary< string, dynamic> newItem = new Dictionary< string, dynamic>();
            PropertyInfo[] props = item.GetType().GetProperties();
            foreach (PropertyInfo prop in props)
            {
                newItem[prop.Name] = item.GetType().GetProperty(prop.Name).GetValue(item, null);
            }
            return newItem;
        }

        public static bool IsDictionary(object o)
        {
            if (o == null) return false;
            return o is IDictionary &&
                   o.GetType().IsGenericType &&
                   o.GetType().GetGenericTypeDefinition().IsAssignableFrom(typeof(Dictionary<,>));
        }

    }
}
</code></pre>