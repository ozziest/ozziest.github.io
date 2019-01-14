---
layout: post
title:  "#6 Windows Servisi Olarak Web API Geliştirme"
date:   2017-04-12 20:00
categories: [Turkish, Notes]
tags: windows, service, rest api, web api, csharp, dotnet
meta: windows, service, rest api, web api, csharp, dotnet
author: ozziest
---

> Bu bir kendime not yazısıdır. Daha sonradan unutma ihtimalime karşın kendimce notlar alıyorum.

### Giriş

Windows Hizmetleri, Windows işletim sistemi üzerine koşan ve Hizmetlet ekranı aracılığı ile yönetebildiğimiz bir ekran. Arka planda çalışan ve bir arayüze sahip olmayan bu uygulamaları, dilerseniz el yordamıyla, dilerseniz otomatik olarak başlatabiliyorsunuz.

Benim de bir ihtiyaç üzerine arka planda sürekli olarak çalışacak bir REST API çalıştırma zaruretim oluştu. Bu notu da üzerinde unutmamak adına alıyorum.

### Gereksinimler

Söz konusu Windows Servisi olduğu için, Windows işletim sistemine sahip olan bir bilgisayar ve Visual Studio gereksinimlerimizi oluşturuyor.

### Uygulama

- Visual Studio ile yeni proje oluşturma ekranınan `Windows Service` seçilir.
- [ASP.NET Web API Self Host](https://www.nuget.org/packages/AspNetWebApi.SelfHost/) paketi Nuget üzerinden kurulur.
- OnStart bölümü aşağıdaki şekilde güncellenerek Web API'nin ayağa kalkması sağlanır;

<pre><code class="language-csharp">

private HttpSelfHostServer server;
private HttpSelfHostConfiguration config;

protected override void OnStart(string[] args)
{
    config = new HttpSelfHostConfiguration("http://localhost:9191");
    config.Routes.MapHttpRoute(
        "DefaultApi",
        "api/{controller}/{action}"
    );
    config.Formatters.Remove(config.Formatters.XmlFormatter);

    server = new HttpSelfHostServer(config);
    server.OpenAsync().Wait();
}
</code></pre>

- OnStop metodu aşağıdaki şekilde güncellerek durdurma anında Web API'nin de durması sağlanır;

<pre><code class="language-csharp">
protected override void OnStart(string[] args)
{
    server.CloseAsync().Wait();
    server.Dispose();
}
</code></pre>

- Bu adımdan sonra aşağıdaki gibi Controller dosyaları oluşturarak Web API geliştirebilirsiniz;

<pre><code class="language-csharp">
public class ServiceController: ApiController
{

    [HttpGet]
    public Dictionary<string, dynamic> Information()
    {
        return new Dictionary<string, dynamic>()
        {
            { "name", "MyService" },
        };
    }
}
</code></pre>

- `installutil MyService.exe` komutu verilerek ilgili uygulamanın Hizmetler bölümüne kaydı gerçekleştirilir.
- `installutil /u MyService.exe` komutu ile dilendiği zaman uygulamanın Hizmetler üzerindeki kaydı silinir.

### Olası Hatalar

Bağımlılığı bulunan `Newtonsoft.Json` paketinin sürümü biraz olarak geliyor. Ben derleme aşamasında bu nedenle ciddi sıkıntılar yaşadım. `Newtonsoft.Json` paketinin sürümü yükselttiğinizde olası sorunları ortadan kaldırabilirsiniz.

### Sonuç

Her ne kadar Windows'u sevmesem de, işim gereği bu tarz geliştirmeler yapmak zorunda kalabiliyorum. Eğer olur da Windows üzerinde bir hizmet olarak Web API çalıştırmak isterseniz, bu yöntemi kullanabilirsiniz.
