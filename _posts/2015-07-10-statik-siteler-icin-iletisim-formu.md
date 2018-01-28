---
layout: post
title: Statik Siteler için İletişim Formu
description: "Statik siteler için veritabanı, PHP ve Javascript gerektirmeyen iletişim formu uygulaması: Formspree."
keywords: "Jekyll, Formspree"
tags: [Jekyll]
redirect_from: "/2015/07/10/statik-siteler-icin-iletisim-formu/"
published: true
---

Jekyll gibi statik site oluşturucularla oluşturulan web sitelerinde statik sitenin mantığı dolayısıyla veritabanı kullanmıyoruz. [Formspree](http://formspree.io/){:target="_blank"}, sunucunuza herhangi bir ek gereksinim yaratmandan iletişim formu oluşturmanızı sağlayan bir fikir. Formspree sayesinde fonksiyonel HTML iletişim formlarınızı PHP veya Javascript kullanmak zorunda kalmadan oluşturabiliyorsunuz.

### Form Oluşturma

[Formspree.io](http://formspree.io){:target="_blank"} adresine gittiğinizde son derece basit bir yapıda örnek iletişim formu dökümünü göreceksiniz.

{% highlight html %}   
<form action="//formspree.io/your@email.com" method="POST">
   <input type="text" name="name">
   <input type="email" name="_replyto">
   <input type="submit" value="Send">
 </form>
{% endhighlight %}

Yukarıdaki örnek kodda göründüğü gibi Formspree'nin iletişim formunda çalışması için `action` niteliği ile mail adresinizi tanımlamanız gerekiyor. 

### Ek Özellikler
Yukarıdaki iletişim formu isim, mail bilgisi ve gönderme butonundan oluşan son derece basit bir iletişim formu. Bu formu ek özellikleri de kullanarak geliştirebiliriz.
{% highlight html %} 
<form id="contactform" action="//formspree.io/mail@adresin.com" method="POST">
   <input type="text" name="name" placeholder="İsminiz">
   <input type="email" name="_replyto" placeholder="Mail adresiniz">
   <textarea name="message" placeholder="Mesajınız"></textarea>
   <input type="submit" value="Gönder">
</form>
{% endhighlight %}

Burada formumuzu CSS ile özelleştirmek için `contactform` id değerini atadık ve kullanıcıların mesaj yazabileceği bir `textarea` ekledik. `input` bloklarındaki name niteliğine `name`, `_replyto` gibi [Formspree tarafından tanımlanan](https://github.com/asm-products/formspree#advanced-features) değerleri ekledik. 

### Form Yönlendirmesi
Formspree tarafından tanımlanan `_next` değeri sayesinde iletişim formuyla mesaj gönderildikten sonra kullanıcıyı başka bir sayfaya yönlendirebiliyoruz.

{% highlight html %}
<input type="hidden" name="_next" value="//site.com/tesekkurler.html" />
{% endhighlight %}

Yukarıdaki örnekte görüldüğü gibi name niteliğine Formspree tarafından tanımlanan ve tanınan `_next`değerini girdik, `value` değerinde ise iletişim formundan sonra yönlenecek teşekkür sayfasını tanımladık.

### Formu Güvenli Yapmak
 Formspree ile oluşturduğumuz iletişim formunu "[honeypot](https://tr.wikipedia.org/wiki/Honeypot)" olarak bilinen spam önleme yöntemiyle güvenli bir hale getirebiliriz. Honeypot tekniği oldukça basittir; iletişim formunun içerisine botların görebileceği ama kullanıcıların göremeyeceği bir alan oluştururuz. Spambot bu alanı doldurmaya çalışacak ama kullanıcılar bu alanı hiç görmediği için dolduramayacak. Bu alanın doldurularak gönderildiği gönderiler spam olarak kabul edilecek ve elenecek. 

Bu tekniği formunuza şu kodu ekleyerek kullanabilirsiniz:
{% highlight html %}
<input type="text" name="_gotcha" style="display:none" />
{% endhighlight %}

Buradaki `style` niteliği alanı formu kullanıcılara görünmez yapması bakımından,  `name` niteliğine atanan `_gotcha` değeri ise Formspree'nin bu alanı tanıması ve bu alanın doldurularak gönderildiği iletileri engellemesi açısından önemlidir.

İletişim formunda `action` niteliğine tanımladığımız `//formspree.io/mail@adresin.com` değeri spambotlar tarafından HTML dosyasından alınabilir. Spambotların mail adresinizi seçebilmesi olasılığını düşürmek için Javascript kullanarak mail adresinizi tanımladığınız ifadeyi parçalara bölebilirsiniz. 
{% highlight html %}
<form id="contactform" method="POST">
   <input type="text" name="name" placeholder="İsminiz">
   <input type="email" name="_replyto" placeholder="Mail adresiniz">
   <textarea name="message" placeholder="Mesajınız"></textarea>
   <input type="text" name="_gotcha" style="display:none" />
   <input type="submit" value="Gönder">
</form>
<script>
   var contactform =  document.getElementById('contactform');
   contactform.setAttribute('action', '//formspree.io/' + 'mail' + '@' + 'adresin' + '.' + 'com');
</script>
{% endhighlight %}

Burada `action` niteliğini form elementinden kaldırdık ve formun altına eklediğim ufak bir JavaScript koduyla mail adresimizi belirtmiş olduk.

### Formu Onaylama
Formu oluşturduktan sonraki son adımda formumuzun bulunduğu sayfayı internet tarayıcısından ziyaret ederek formumuzdan örnek bir ileti göndereceğiz ve ardından gelen pencerede formumuza tanımladığımız mail adresini onaylayacağız. 

![Formspree Onay](/images/confirm.png)

Mail adresinize gelen onay linkine tıkladıktan sonra onaylama işlemi tamamlanacak. 

![Formspree Onaylandı](/images/confirmed.png)

**Not:** Bu onaylama işlemini formu yeni bir sayfaya eklediğinizde tekrar yapmanız gerekir. 

