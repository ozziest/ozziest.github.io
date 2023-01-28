---
layout: post
title: "MySQL Sorgularında Satır Sayısı"
date: 2012-09-27 19:00
categories: [Turkish, Coding]
keywords: mysql, satır sayısı
author: Özgür Adem Işıklı
---

MySQL ile çalışırken yazdığınız SQL sorgularının sonuçlarına otomatik olarak satır sayısı alanı da ekleyebilirsiniz. Aşağıdaki SQL kodunda bu işlemin nasıl yapıldığı gösterilmektedir.

<pre><code class="language-sql">
SELECT 	@rownum := @rownum + 1 AS sira_no, adi, soyadi
FROM      ogrenci, (SELECT @rownum := 0) r
</code></pre>

Bu sorguda öğrenci tablosundan adi ve soyadi alanları listelenmekte ve bu alanların başına sıra numarası yani satır sayısı eklenmektedir.
