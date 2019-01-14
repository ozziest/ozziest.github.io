---
layout: post
title:  "AngularJS Cache Problemi Ve Çözüm Önerisi"
date:   2015-10-25 21:00
categories: [Turkish, Coding]
tags: angularjs, cache, çözüm önerisi, cache sorunu
meta: angularjs, cache, çözüm önerisi, cache sorunu
author: ozziest
---

AngularJS kullanarak çok güçlü web uygulamaları geliştirmeniz mümkün. Ancak ***html*** ya da ***javascript*** yüklemeleri genellikle tarayıcılar tarafından ön bellekte tutularak, uygulamanızın performansının arttırılması sağlanmaktadır. 

Fakat uygulamalar sabit değildir ve sürekli geliştirilir. Siz, bir dosyanın yapısını değiştirdiğinizde, ilgili dosya eğer kullanıcının tarayıcısda ön bellekte tutuluyorsa, kullanıcı bu son değişikliği alamayacaktır. Bu tür durumları çözmek için ilk yapılan, cache özelliğini ***tamamen*** ortadan kaldırmaktadır.

<pre><code class="language-markup">
&lt;html ng-app="MyApp"&gt;
    &lt;head&gt;
        &lt;script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.4.7/angular.min.js"&gt;&lt;/script&gt;
    &lt;/head&gt;
    &lt;body ng-controller="MyController"&gt;
        &lt;ng-include src="'/template.html?v=' + version"&gt;&lt;/ng-include&gt;
        &lt;script type="text/javascript"&gt;
            angular.module('MyApp', []).controller('MyController', function ($scope) {
                $scope.version = new Date().getTime();
            });
        &lt;/script&gt;
    &lt;/body&gt;
&lt;/html&gt;
</code></pre>

Ancak bu yöntemle, her içerik yeniden yüklenir. Bu da performansı düşüreceğinden, soruna başka bir yolla çözüm bulmamız gerekecektir. 

#### Gulp İle Sürümleme

Eğer Gulp (ya da Grunt) ile script dosyalarınızı birleştirme işi gerçekleştiriyorsanız, scriptlerinizi kolay bir şekilde versiyonlayabilmeniz mümkündür.

Öncelikle, basit bir ***gulp task***'ı örnekleyelim;

<pre><code class="language-js">
gulp.task('example-task', function () {

  return gulp.src([
    'scripts/script_1.js',
    'scripts/script_2.js',
    'scripts/script_3.js'
  ])
    .pipe(concat('scripts.min.js'))
    .pipe(gulp.dest('public_html/dist/js'));

});
</code></pre>

Bu ***gulp task***'ı, javascript dosyalarımızı birleştiriyor ve `scripts.min.js` adıyla `public_html/dist/js` klasörü içerisine kaydediyor. Bu görevi otomatikleştirerek, her script kaydedildikten sonra tetiklemeniz mümkün. Bizim ilgilendiğimiz konus ise; birleştirilen bu scriptlerin sonuna ufak bir versiyon numarası dahil etmek.

Bu işlem için [gulp-insert](https://github.com/rschmukler/gulp-insert) eklentisinden faydalanacağız. 

<pre><code class="language-bash">
$ npm install gulp-insert --save
</code></pre>

<pre><code class="language-js">
var gulp = require('gulp'),
  concat = require('gulp-concat'),
  insert = require('gulp-insert');

gulp.task('default', function () {

  return gulp.src([
    'scripts/script_1.js',
    'scripts/script_2.js'
  ])
    .pipe(concat('scripts.min.js'))
    .pipe(insert.append('function version () {return "' + new Date().getTime() + '";};'))
    .pipe(gulp.dest('public_html/dist/js'));

});
</code></pre>

***Gulp task***ımızı bu şekilde güncellediğimizde, her script derleme işlemi sonrası yenilenen bir versiyon numaramız olacaktır. Bu adımdan sonra tek yapmamız gereken; script yükleme işlemlerinde bu fonksiyonu kullanarak scriptleri versiyon numarasına göre çağırmaktır. 

<pre><code class="language-markup">
&lt;html ng-app="MyApp"&gt;
    &lt;head&gt;
        &lt;script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.4.7/angular.min.js"&gt;&lt;/script&gt;
    &lt;/head&gt;
    &lt;body ng-controller="MyController"&gt;
        &lt;ng-include src="'/template.html?v=' + version"&gt;&lt;/ng-include&gt;
        &lt;script type="text/javascript"&gt;
            angular.module('MyApp', []).controller('MyController', function ($scope) {
                $scope.version = version();
            });
        &lt;/script&gt;
    &lt;/body&gt;
&lt;/html&gt;
</code></pre>

Böylelikle; scriptler üzerinde yapılan her güncelleme sonrası yeni bir versiyon numarası oluşur. Uygulama içerisinde yüklenen ***html*** ya da ***javascript*** dosyaları ilgili versiyon numarasına göre alındığından daha önceden yüklenmemiş versiyonlar tarayıcı tarafından bir daha yüklenmez. Böylelikle yapılan güncellemeler sonrası bir defa yeniden yükleme işlemi gerçekleştirilir.
