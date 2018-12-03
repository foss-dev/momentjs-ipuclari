---
title: Moment.js Giriş
---

# İçerik

[[toc]]

# Nerede Kullanılır

Moment tarayıcı ve Node.js üzerinde çalışmak için tasarlanmıştır.

Tüm kodlar ve birim testleri bu iki geliştirme ortamında da sorunsuzca çalışacaktır.

Şu anda [CI sistemi](https://medium.com/@selcukusta/continuous-integration-ci-%C3%BCzerine-laflamalar-9b7f7d2dad07) şu tarayıcıları desteklemektedir: 

Windows XP üzerinde Chrome, Windows 7'de IE 8, 9 ve 10 sürümleri, Windows 10'da IE 11 sürümü, Linux için en son Firefox sürümü ve OSX 10.8 ve 10.11 üzerinde en son Safari sürümü.

Eğer moment'i şimdi denemek isterseniz sizin için CodePen üzerinde oluşturulan [Moment PlayGround](https://codepen.io/aligoren/pen/rQPbza?editors=0011)'a gidebilirsiniz. Orada yer alan konsol ekranına aşağıdaki kodları yazabilirsiniz.

## Node.js

```bash
npm install moment
```

```js
var moment = require('moment')

moment().format()
```

ya da destekliyorsa

```js
import moment from 'moment'

moment().format()
```

## Tarayıcı

```html
<script src="moment.js"></script>
<script>
    moment().format();
</script>
```

Moment.js şu anda [cdnjs.com](https://cdnjs.com/libraries/moment.js) ve [jsDelivr](https://www.jsdelivr.com/package/npm/moment) üzerinde yer almaktadır.

## Bower

```bash
bower install --save moment
```

::: warning UYARI
Bower şu anda aktif olarak geliştirilmiyor. Diğer paket yöneticilerini kullanmanız öneriliyor.
:::

## NuGet

Moment.js ayrıca NuGet depolarında da bulunmaktadır.

```bash
Install-Package Moment.js
```