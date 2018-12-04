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

## Saat, Dakika, Saniye, Milisaniye ve Offset Sembolleri

| Giriş        | Örnek           | Açıklama     |
| ------------ |:---------------:| ------------:|
| H HH         | 0..23  | 24 saatlik dilimi temsil eder |
| h hh         | 1..12  | a ya da A ile kullanıldığında 12 saatlik dilimi temsil eder. |
| k kk         | 1..24  | 24 saatlik dilimi temsil eder. Farklı olarak değer 1'den başlar. |
| a A         | am pm  | Öğleden öncesi ve öğleden sonrası olarak temsil edilir. [1](https://eksisozluk.com/post-meridiem--202275) [2](https://eksisozluk.com/ante-meridiem--202274) |
| m mm         | 0..59  | Dakikaları temsil eder |
| s ss         | 0..59  | Saniyeleri temsil eder |
| S SS SSS         | 0..999  | Kesirli saniyeleri verir |
| Z ZZ         | +12:00  | Offset yani saat farkını belirtmek için kullanılır [1](https://tr.wikipedia.org/wiki/UTC_offset) |

::: warning UYARI
**2.10.5** sürümünden itibaren: Kesirli saniyelerde, kesirli saniyeye belirten sembollerin uzunluğu 4 karakterden 9 karaktere kadar uzayabilir. Ancak sadece ilk 3'ü dikkate alınacaktır.
:::

::: tip BİLGİ
Açıkca bir saat dilimi belirtmediğiniz sürece, verilen değer geçerli zaman dilimini kullanarak parsing işlemine tabi tutulur.
:::

**Örnek**

```js
moment("2010-10-20 4:30",       "YYYY-MM-DD HH:mm");   // 4:30 yerel saat parse edildi
moment("2010-10-20 4:30 +0000", "YYYY-MM-DD HH:mm Z"); // 4:30 UTC zamanı olarak parse edildi
```

::: tip BİLGİ
Eğer moment'e verilen tarih gerçekte yoksa `moment#isValid` fonksiyonu **false** değeri dönecektir.
:::

**Örnek**

```js
moment("2010 13",           "YYYY MM").isValid();     // false (geçerli bir ay değil)
moment("2010 11 31",        "YYYY MM DD").isValid();  // false (geçerli bir gün değil)
moment("2010 2 29",         "YYYY MM DD").isValid();  // false (artık gün değil)
moment("2010 biraydegil 29", "YYYY MMM DD").isValid(); // false (geçerli bir ay adı değil)
```

::: tip BİLGİ
**2.0.0** sürümünden itibaren dil sembolü *(örnek: tr)* `moment()` ve `moment#utc` fonksiyonlarına üçüncü parametre olarak atanabilir.
:::

```js
moment('2012 July', 'YYYY MMM', 'en');
moment('2012 Ocak',    'YYYY MMM', 'tr');
```

::: warning DİKKAT
Moment parser'ı biraz dikkatsiz davranabilir. Bu yüzden aşağıdakine benzer durumlara karşı dikkatli olmanız gerekmektedir.
:::

**Örnek**

```js
moment('2016 bir tarihtir', 'YYYY-MM-DD').isValid() 
// true dönecektir çünkü 2016 tarihle eşleşiyor.
```

Bunu aşmak için **2.3.0** sürümüyle birlikte gelen katı modda parse etme işlemini uygulayabilirsiniz. Üçüncü parametre olarak boolean türünden bir değer bu iş için yardımcı olacaktır.

**Örnek**

```js
moment('2016 bir tarihtir', 'YYYY-MM-DD', true).isValid() 
// false dönecektir. Çünkü geçerli bir tarih değil
```

## Özel Formatlar

Burası doldurulacak...