---
title: Parsing İşlemleri
---

# İçerik

[[toc]]

# Parsing İşlemleri

Moment.js native `Date.prototype` nesnesinde değişiklikler yapmak yerine `Date` nesnesi için bir wrapper oluşturur. Bu wrapper nesnesini çağırmak için kabul edilen giriş türlerini kullanarak basitçe `moment()` demeniz yeterlidir.

`Moment` prototype'ı `moment.fn` ile çalışmaktadır. Eğer kendi fonksiyonlarınızı yazacaksanız bu tam da onları koymanız gereken yer. Örneğin;

```js
moment.fn.mesajYaz = function() {
    console.log('Burada moment çalıştı')
    return this
}

moment().mesajYaz().format()
```

Tabii ki kendi fonksiyonlarınızı oluşturmanız yukarıdakinden farklı bir gereksinim sonucu ortaya çıkabilir.

## Mevcut Zaman

```js
moment();
// 2.14.0 sürümünden itibaren ileriye yönelik olarak aşağıdaki kullanımlar da desteklenmektedir.
moment([]);
moment({});
```

mevcut zamana dair bilgi almak için herhangi bir parametre olmadan `moment()` demeniz yeterlidir.

```js
var mevcutZaman = moment();
```

::: tip BİLGİ
Bu aşamada dönen sonuç `Object` türünden olacağı için saf bir kullanıma uygun değildir.
:::

## String

Moment nesnesi string türünden bir parametre alarak oluşturulduğu zaman önce girilen değerin [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) formatlarından biriyle eşleşip eşleşmediğine bakar.

Ardından [RFC 2822 Date Time](https://tools.ietf.org/html/rfc2822#section-3.3) formatı için kontrol eder. Eğer bilinen bir geçerli bir format yok ise `new Date(string)` kullanılır.

::: warning UYARI
Tarayıcıların stringleri ayrıştırma işlemleri [tutarsızdır](http://dygraphs.com/date-formats.html). Bunun nedeni hangi formatın desteklenmesi gerekeceğine dair herhangi bir spesifikasyonun bulunmayışıdır. Bazı tarayıcılarda çalışan format, bazılarında çalışmayabilir.

ISO 8601 formatı dışındaki değerler için ayrıca `format()` fonksiyonunu da kullanabilirsiniz.
:::

## Desteklenen ISO 8601 Stringleri

ISO 8601 türünden bir string için tarih bölümü gerekmektedir.

```
2013-02-08  # Takvim tarihi
2013-W06-5  # Haftalık bölüm
2013-039    # Sıralı bir tarih bölümü

20130208    # Kısa tam tarih
2013W065    # Kısa tarih hafta ve haftanın günü
2013W06     # Kısa tarih sadece hafta
2013050     # Kısa sıralı tarih
```

Ayrıca zaman içeren bölümler de kullanılabilir. Bu bölümler boşluk ya da büyük **T** harfi ile ayrılır.

```
2013-02-08T09            # T ile ayrılmış bir zaman bölümü
2013-02-08 09            # Boşluk ile ayrılmış bir zaman bölümü
2013-02-08 09:30         # Saat ve dakikadan oluşan zaman bölümü
2013-02-08 09:30:26      # Saat, dakika ve saniyeden oluşan zaman bölümü
2013-02-08 09:30:26.123  # Saat, dakika, saniye ve milisaniyeden oluşan zaman bölümü
2013-02-08 24:00:00.000  # Saat 24 ve dakika, saniye, milisaniye ise 0 yani ertesi gün 
                            gece yarısı anlamına gelmektedir.

20130208T080910,123      # Virgülle milisaniyeye kadar ayrılmış bir kısa tarih ve zaman.
20130208T080910.123      # Nokta ile milisaniyeye kadar ayrılmış bir kısa tarih ve zaman
20130208T080910          # Saniyeye kadar giden kısa tarih ve zaman
20130208T0809            # Dakikalara kadar gider kısa tarih ve zaman
20130208T08              # Kısa tarih ve zaman sadece saat içermektedir
```

## Tarih Bilgisini Doğrulama

Eğer gelen tarih bilgisinin geçerli olmadığını düşünüyorsanız bunu `isValid()` isimli fonksiyonla aşağıdaki gibi doğrulayabilirsiniz.

```js
moment("tarihten bağımsız string").isValid() // false

moment("19.19.20").isValid() // false
```

## String + Format

```js
moment(String, String)
moment(String, String, String)
moment(String, String, Boolean)
moment(String, String, String, Boolean)
```

Eğer girilen string'in formatını biliyorsanız, zamanı parse etmek için onu kullanabilirsiniz. Örnek:

```js
moment("12-25-1995", "MM-DD-YYYY")
```

Bu aşamada parser alfanumerik olmayan karakterleri yok sayar, yani aşağıdaki iki kullanım da aynı sonucu üretir.

```js
moment("12-25-1995", "MM-DD-YYYY")
moment("12/25/1995", "MM-DD-YYYY")
```

Kullanılan parser sembolleri (tokens) tıpkı `moment#format` fonksiyonunda olduğu gibi kullanılabilir.

## Yıl, Ay ve Gün Sembolleri

| Giriş        | Örnek           | Açıklama     |
| ------------ |:---------------:| ------------:|
| YYYY         | 2018  | 4 ya da 2 karakter yıl |
| YY           | 18    | 2 karakter yıl         |
| Y           | 2018    | Herhangi bir sayının rakamı ve sembolü ile yıl |
| Q          | 1..4    | Yılın hangi çeyreği olduğuna dair bilgiyi sayısal olarak verir |
| M MM           | 1..12    | Ayın sayısal değerini verir |
| MMM MMMM           | Ara.. Aralık    | Ay adı. Ay adı `moment.local()` ile belirlenmiştir. Varsayılan **en** |
| D DD           | 1..31    | Ayın günü |
| Do | 1nd..31nd    | Sıralı olarak ayın günü |
| DDD DDDD | 1.365    | Ayın günü |
| X | 1410715640.579 | Unix timestamp |
| x | 1410715640579 | Unix timestamp ms türünden. Buradaki x küçük harflidir. |

## Hafta, Haftanın Günü ve Yılın Haftası İçin Semboller

Aşağıda yer alan küçük harflerle belirtili semboller yerel hafta günlerini kullanır. Büyük harfle belirtilmiş olan semboller ise [ISO standartlarına göre](https://en.wikipedia.org/wiki/ISO_week_date) haftanın gününü belirtir.

| Giriş        | Örnek           | Açıklama     |
| ------------ |:---------------:| ------------:|
| gggg         | 2018  | 4 karakter yerel tarihli haftanın yılı |
| gg           | 18    | 2 karakter yerel tarihli haftanın yılı |
| w ww           | 1..53    | Yılın hafta sayısı |
| e | 0..6    | Haftanın kaçıncı günü |
| ddd dddd | Pts...Pazar | Haftanın gün adı |
| GGGG         | 2018  | 4 karakter ISO Standartlarına göre haftanın yılı |
| GG           | 18    | 2 karakter ISO Standartlarına göre haftanın yılı |
| W WW           | 1..53    | ISO Standartlarına göre Yılın hafta sayısı |
| E | 1..7    | ISO Standartlarına göre haftanın kaçıncı günü |