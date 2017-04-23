---
layout: post
title:  "Not #5 - Linux'ta Klasör Eşitleme"
date:   2017-01-20 22:30
categories: not
tags: linux, klasör, eşitleme, senkronizasyon
meta: linux, klasör, eşitleme, senkronizasyon
author: ozziest
---

> Bu bir kendime not yazısıdır. Daha sonradan unutma ihtimalime karşın kendimce notlar alıyorum.

Linux'ta en sevdiğim komutlardan biri `scp`.

`scp` komutunun açılımı Secure Copy yani güvenli kopyalamadan geliyor. Bu komut ile **ssh** protokolü üzerinden iki farklı host arasında dosya transferi yapabiliyoruz. Sadece uzak bir bilgisayardaki dosyayı yerele indirmek için değil, yereldekini de uzaktaki bilgisayara yüklemek için kullanabiliyorsunuz.

<pre><code class="language-js">
scp -P 54 -r 192.166.66.66:/home/remote_folder /home/local_folder
</code></pre>

Yukarıdaki örnekte 54 numaralı port üzerinden **ssh** protokolü ile bağlantı kurularak, `192.166.66.66` numaralı IP'ye sahip bilgisayarda bulunan `remote_folder` klasörü, yereldeki `local_folder` ile eşitleniyor.