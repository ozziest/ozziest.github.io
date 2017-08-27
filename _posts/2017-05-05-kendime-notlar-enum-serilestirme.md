---
layout: post
title:  "Not #8 - Enum Veri Tiplerinin Serileştirilmesi"
date:   2017-05-05 20:00
categories: not
tags: csharp, enum, serialization
meta: csharp, enum, serialization
author: ozziest
---

> Bu bir kendime not yazısıdır. Daha sonradan unutma ihtimalime karşın kendimce notlar alıyorum.

### Giriş

Web programlama tecrübelerime PHP ile başlayıp JS ile devam ettiğimden, C# kullanmaya başladığım dönemde, bilmeme rağmen göz ardı ettiğim Enum sabitlerine bugünlerde hayranlık duyuyorum. Her ne kadar aramızdaki duygusal bağ gereksiz olsa da, geliştirme ve refactoring anında sağladığı avantajlar yabana atılacak türden değil.

Geçtiğimiz günlerde Enum tipindeki verileri bir Web API projesinde, string karşılıkları olarak serileştirme ihtiyacı hissetim. Bunun için hazırlanmış çok basit bir Attribute yardımıyla bu işi kolaylıkla halledebiliyoruz.

### Uygulama

<pre><code class="language-csharp">
public enum UserType
{
    ADMIN,
    MODERATOR,
    STANDARD
}

public class User
{
    
    [JsonConverter(typeof(StringEnumConverter))]
    public UserType type;

}
</code></pre>