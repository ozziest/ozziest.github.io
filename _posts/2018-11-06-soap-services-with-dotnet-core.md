---
layout: post
title: "How To Use SOAP Services With Dotnet Core"
date: 2018-11-06 21:45
categories: [English, Coding]
keywords: soap services, dotnet core, wcf, wsdl
author: Özgür Adem Işıklı
post_img: 05.jpg
post_img_link: https://pixabay.com/en/michelangelo-abstract-boy-child-71282
lang: en
---

## Generating Service Classes

First of all, in a dotnet core project we should define SOAP services classes. If you use Visual Studio you can find some tools about it but If you don't like user interfaces like me, you can have another option to do that.

In first step, you should add `dotnet-svcutil` tool reference in your project and execute `dotnet restore`;

```xml
<ItemGroup>
    <DotNetCliToolReference Include="dotnet-svcutil" Version="1.0.*" />
</ItemGroup>
```

After that, you can fetch services structure using a simple command in your project folder;

```bash
$ dotnet svcutil http://my-service-host.com/wsdl/IMyService -n "*,MyProject.Services.IMyService" -d ServiceFolder/IMyService
```

With `-n` argument we can set a namespace. If you are working multiple service, you should set a namespace. With `-d` argument we can choose a folder to put objects.

## Preparing The Service

```csharp
BasicHttpBinding binding = new BasicHttpBinding();
binding.Security.Transport.ClientCredentialType = HttpClientCredentialType.None;
binding.Security.Mode = BasicHttpSecurityMode.None;

EndpointAddress endpoint = new EndpointAddress(new Uri("http://my-service-host.com/soap/IMyService"));
ChannelFactory<IMyService> factory = new ChannelFactory<IMyService>(binding, endpoint);
IMyService service = factory.CreateChannel();

// then you can call the service
```

If you have some authentication configuration, you should set them in `BasicHttpBinding` section. In this example I use `http` protocol, not `https`. If you are using https protocol, please be careful to select BasicHttp**s**Binding object.

## Usage

```csharp
await service.GetUsersAsync()
```

It is easy right? But in my experiences, I could call methods only async.

## Troubleshooting

```bash
The operation 'xxxx' could not be loaded because it specifies "rpc-style" in "literal" mode, but uses message contract types or the System.ServiceModel.Channels.Message. This combination is disallowed -- specify a different value for style or use parameters other than message contract types or System.ServiceModel.Channels.Message.
```

You may see that error. To solve it, you should open .csproj file and edit following packages minumum version like these;

```xml
<ItemGroup>
    <PackageReference Include="System.ServiceModel.Duplex" Version="4.5.*" />
    <PackageReference Include="System.ServiceModel.Http" Version="4.5.*" />
    <PackageReference Include="System.ServiceModel.NetTcp" Version="4.5.*" />
    <PackageReference Include="System.ServiceModel.Security" Version="4.5.*" />
</ItemGroup>
```
