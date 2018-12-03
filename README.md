---
title: Moment.js Nedir?
---

# İçerik

[[toc]]

# Moment.js Nedir?

Moment.js tarih ve saatleri parçalamada, doğrulamada ve değiştirmede yardımcı bir JavaScript kütüphanesidir. Basit kurulumu ve kolay kullanımı nedeniyle geliştiriciler tarafından tercih edilmektedir.

## CDN Linkleri ile Kurulum

Kurulum CDN kullanarak tarayıcı üzerinden direkt gerçekleştirilebilir, moment.js sunucularından indirilebilir ya da farklı paket yöneticileri kullanılarak projeye dahil edilebilir.

### Moment.js Sunucu Linkleri

Bu linkleri production ortamında kullanmak yerine indirerek projenizde yerel olarak barındırabilirsiniz.

[moment.js 16.4k gz](https://momentjs.com/downloads/moment.min.js)

Aşağıda yer alan sürümde ayrıca yerelleştirme seçenekleri de yer almaktadır. Ay ve gün isimleri Türkçe olarak bu sürümde kullanılabilir.

[moment-with-locales.min.js 66.4k gz](https://momentjs.com/downloads/moment-with-locales.min.js)

### Moment.js CDN Linkleri

**jsDelivr**

Bu link ile jsDelivr üzerinden sisteminizde kullanım sağlayabilirsiniz.

[moment.min.js](https://cdn.jsdelivr.net/npm/moment@2.22.2/moment.min.js)

**cdnjs**

Bu link ile cdnjs üzerinden sisteminizde kullanım sağlayabilirsiniz.

[moment.min.js](https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.22.2/moment.min.js)

[moment-with-locales.min.js](https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.22.2/moment-with-locales.min.js)

Bu linkleri bildiğiniz üzere sitenizin gerekli yerlerine eklemelisiniz. Örnek;

```html
<script src="moment.min.js"></script>
```

ya da

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.22.2/moment.min.js"></script>
```

## Paket Yöneticileri ile Kurulum

Aşağıdaki paket yöneticileri ile kurulumlar da desteklenmektedir.

```bash
npm install moment --save   # npm

yarn add moment             # Yarn

Install-Package Moment.js   # NuGet

spm install moment --save   # spm

meteor add momentjs:moment  # meteor

bower install moment --save # bower (artık geliştirilmiyor)
```

Bu işlemlerden sonra kodlarımızı yazdığımız dosyada şu komutu verebiliriz;

```js{3,5}
//ornek.js

var simdi = moment().format()

console.log(simdi)
```