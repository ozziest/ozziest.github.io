---
layout: post
title:  "#4 C#'da Self Signed Sertifika Oluşturma"
date:   2016-09-28 18:00
categories: [Turkish, Notes]
tags: csharp, self signed, sertifika
meta: csharp, self signed, sertifika
author: ozziest
post_img: notes.jpg
post_img_link: https://pixabay.com/en/books-pages-story-stories-notes-1245690
---

> Bu bir kendime not yazısıdır. Daha sonradan unutma ihtimalime karşın kendimce notlar alıyorum.

C#'da kendinden imzalı bir serifika oluşturmak zorunda kaldığımda aşağıdaki kod bloğundan yararlandım.

<pre><code class="language-php">
public static byte[] CreateGeneralCert(SecureString password)
{
    using (CryptContext ctx = new CryptContext())
    {
        ctx.Open();
        X509Certificate2 cert = ctx.CreateSelfSignedCertificate(
            new SelfSignedCertProperties
            {
                IsPrivateKeyExportable = true,
                KeyBitLength = 512,
                Name = new X500DistinguishedName("cn=localhost"),
                ValidFrom = DateTime.Today.AddDays(-1),
                ValidTo = DateTime.Today.AddYears(1),
            });
        return cert.Export(X509ContentType.Pfx, password);
    }
}
</code></pre>
