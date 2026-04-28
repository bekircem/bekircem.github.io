---
title: Statik Siteler için İletişim Formu
description: "Statik siteler için veritabanı, PHP ve Javascript gerektirmeyen iletişim formu uygulaması: Formspree."
date: 2015-07-10 00:00:00 +0300
categories: [Jekyll]
tags: [jekyll, formspree]
redirect_from:
  - /2015/07/10/statik-siteler-icin-iletisim-formu/
  - /2015/07/statik-siteler-icin-iletisim-formu/
---

Jekyll gibi statik site oluşturucularla oluşturulan web sitelerinde statik sitenin mantığı dolayısıyla veritabanı kullanmıyoruz. [Formspree](http://formspree.io/), sunucunuza herhangi bir ek gereksinim yaratmadan iletişim formu oluşturmanızı sağlayan bir çözüm. Formspree sayesinde fonksiyonel HTML iletişim formlarınızı PHP veya Javascript kullanmak zorunda kalmadan oluşturabiliyorsunuz.

## Form Oluşturma

[Formspree.io](http://formspree.io) adresine gittiğinizde son derece basit bir yapıda örnek iletişim formu dökümanını göreceksiniz.

```html
<form action="//formspree.io/your@email.com" method="POST">
  <input type="text" name="name">
  <input type="email" name="_replyto">
  <input type="submit" value="Send">
</form>
```

Yukarıdaki örnek kodda göründüğü gibi Formspree'nin iletişim formunda çalışması için `action` niteliği ile mail adresinizi tanımlamanız gerekiyor.

## Ek Özellikler

Yukarıdaki iletişim formu isim, mail bilgisi ve gönderme butonundan oluşan son derece basit bir iletişim formu. Bu formu ek özellikleri de kullanarak geliştirebiliriz.

```html
<form id="contactform" action="//formspree.io/mail@adresin.com" method="POST">
  <input type="text" name="name" placeholder="İsminiz">
  <input type="email" name="_replyto" placeholder="Mail adresiniz">
  <textarea name="message" placeholder="Mesajınız"></textarea>
  <input type="submit" value="Gönder">
</form>
```

Burada formumuzu CSS ile özelleştirmek için `contactform` id değerini atadık ve kullanıcıların mesaj yazabileceği bir `textarea` ekledik. `input` bloklarındaki `name` niteliğine `name`, `_replyto` gibi [Formspree tarafından tanımlanan](https://github.com/asm-products/formspree#advanced-features) değerleri ekledik.

## Form Yönlendirmesi

Formspree tarafından tanımlanan `_next` değeri sayesinde iletişim formuyla mesaj gönderildikten sonra kullanıcıyı başka bir sayfaya yönlendirebiliyoruz.

```html
<input type="hidden" name="_next" value="//site.com/tesekkurler.html" />
```

Yukarıdaki örnekte `name` niteliğine Formspree'nin tanıdığı `_next` değerini girdik, `value` değerinde ise iletişim formundan sonra yönlenecek teşekkür sayfasını tanımladık.

## Formu Güvenli Yapmak

Formspree ile oluşturduğumuz iletişim formunu "[honeypot](https://tr.wikipedia.org/wiki/Honeypot)" olarak bilinen spam önleme yöntemiyle güvenli bir hale getirebiliriz. Honeypot tekniği oldukça basittir; iletişim formunun içerisine botların görebileceği ama kullanıcıların göremeyeceği bir alan oluştururuz. Spambot bu alanı doldurmaya çalışacak ama kullanıcılar bu alanı hiç görmediği için dolduramayacak. Bu şekilde gönderilen iletiler spam olarak kabul edilip elenecek.

Bu tekniği formunuza şu kodu ekleyerek kullanabilirsiniz:

```html
<input type="text" name="_gotcha" style="display:none" />
```

`style="display:none"` niteliği alanı kullanıcılara görünmez yaparken, `name="_gotcha"` değeri Formspree'nin bu alanı tanıyarak doldurulmuş gönderileri engellemesini sağlar.

`action` niteliğine tanımladığımız `//formspree.io/mail@adresin.com` değeri spambotlar tarafından HTML dosyasından alınabilir. Mail adresinizin botlara karşı korunması için Javascript ile adresinizi parçalara bölebilirsiniz:

```html
<form id="contactform" method="POST">
  <input type="text" name="name" placeholder="İsminiz">
  <input type="email" name="_replyto" placeholder="Mail adresiniz">
  <textarea name="message" placeholder="Mesajınız"></textarea>
  <input type="text" name="_gotcha" style="display:none" />
  <input type="submit" value="Gönder">
</form>
<script>
  var contactform = document.getElementById('contactform');
  contactform.setAttribute('action', '//formspree.io/' + 'mail' + '@' + 'adresin' + '.' + 'com');
</script>
```

Burada `action` niteliğini form elementinden kaldırdık ve formun altına eklediğimiz ufak bir JavaScript koduyla mail adresimizi tanımlamış olduk.

## Formu Onaylama

Formu oluşturduktan sonraki son adımda formumuzun bulunduğu sayfayı internet tarayıcısından ziyaret ederek örnek bir ileti gönderiyoruz ve ardından gelen pencerede formumuza tanımladığımız mail adresini onaylıyoruz.

![Formspree Onay](/images/confirm.png)

Mail adresinize gelen onay linkine tıkladıktan sonra onaylama işlemi tamamlanacak.

![Formspree Onaylandı](/images/confirmed.png)

> **Not:** Bu onaylama işlemini formu yeni bir sayfaya eklediğinizde tekrar yapmanız gerekir.
