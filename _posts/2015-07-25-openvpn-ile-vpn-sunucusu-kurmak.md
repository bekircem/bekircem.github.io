---
layout: post
title: OpenVPN ile VPN Sunucusu Kurmak
description: Açık kaynak kodlu OpenVPN teknolojisi ile kendi VPN servisinizi oluşturun.
keywords: 'VPN, VPS, Digital Ocean, OpenVPN'
tags:
  - VPN
  - VPS
  - Digital Ocean
  - OpenVPN
redirect_from: /2015/07/25/openvpn-ile-vpn-sunucusu-kurmak/
published: true
---


Dün geceden itibaren internette genel anlamda bir yavaşlık olması ve özellikle Facebook, Twitter, Youtube gibi mecralara erişimde kesintiler yaşanması sebebiyle VPN kullanma ihtiyacı duydum. IPVanish, Hidemyass gibi VPN servislerini daha önceden kullanmıştım. Ancak bu servisleri istediğim zaman pasifleştirip aktifleştiremediğim ve isteğime göre yapılandıramadığım için bu sefer tercih etmedim. Bu rehberde yapılabilecek en basit haliyle kendi sanal sunucunuzu nasıl VPN servisi haline getirebileceğinizi anlatacağım.

Bu rehberde geliştiriciler için cloud hizmeti sunan [Digital Ocean](http://www.digitalocean.com){:target="_blank"} VPS'lerinden 5 dolarlık VPS paketini kullandım. (VPN için 5 dolarlık paket fazlasıyla yeterli.)

Henüz bir Digital Ocean hesabınız yoksa [şuradan](https://m.do.co/c/32c1cc7dec0c){:target="_blank"} kayıt olarak 2 ay bedava VPS kullanabileceğiniz 10 dolarlık kuponu edinebilirsiniz.

## VPS Oluşturma
Digital Ocean'da `Creat Droplet` butonuna tıklayarak yeni bir sanal sunucu oluşturuyoruz. `Droplet Hostname` için herhangi bir isim girebilirsiniz, VPN olarak kullanacağınız için bu çok fark etmiyor. `Select Size` kısmından 5 dolarlık paketi seçiyoruz. `Select Region` kısmında VPS'imizin lokasyonunu belirleyeceğiz. Genelde Amsterdam önerilse de ben Frankfurt lokasyonunu seçmenizi öneririm. `Select Image` kısmında VPS'imizde kullanacağımız işletim sistemini seçeceğiz. Ubuntu 14.04 x64 sürümünü seçin.

Temel düzeyde diğer seçeneklere dokunmanıza gerek yok. İşlemleri yaptıysanız `Create Droplet` butonuna tıklayarak VPS'i oluşturabilirsiniz. Buradan bir sayfaya yönleneceksiniz. 50-60 saniye içerisinde VPS'iniz oluşturulacak ve VPS bilgileriniz kayıt olduğunuz mail adresinize gelecek.

## VPN için VPS Yapılandırma
Mail adresinize gelen IP, user ve pass bilgileriyle VPS'imize bağlanmamız gerekiyor. Bunun için Windows'ta [Putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html){:target="_blank"}, Mac OS'te için iTerm2, Linux'te terminali kullanabilirsiniz.

SSH istemcinize VPS'inizin IP, user ve pass bilgilerini girerek VPS'imize bağlanmanız gerekiyor. İlk girişte sizden şifre standart tanımlanmış şifrenizi yenisiyle değiştirmenizi isteyecek. Bir şifre belirleyip değiştirin.

### OpenVPN Access Server Kurulumu
`OpenVPN Access Server` 3 adımda OpenVPN'i sanal sunucunuza kurmanıza yarayan, aynı ekip tarafından geliştirilen bir çözümdür. Bu rehberde en basit yolu izleyeceğimiz için VPN'imizi OpenVPN Access Server ile kuracağız.

OpenVPN Access Server'ın sanal sunucunuza uygun en güncel sürümünü bulmak için şu [sayfayı](http://swupdate.openvpn.org/as/){:target="_blank"} ziyaret edin. En güncel sürümler en aşağıda yer alıyor. Sisteminize uygun en güncel sürümün bağlantısını kopyalayın. Ben bu rehberi hazırlarken en güncel OpenVPN AS sürümünün adı `openvpn-as-2.0.19-Ubuntu14.amd_64.deb` bağlantısı ise `http://swupdate.openvpn.org/as/openvpn-as-2.0.19-Ubuntu14.amd_64.deb` idi. 

SSH ile sanal sunucunuza bağlanın ve aşağıdaki komutu vererek Ubuntu 14.04, 64 bit sisteme uygun paketi sunucunuza indirin:

{% highlight bash %}
sudo wget http://swupdate.openvpn.org/as/openvpn-as-2.0.19-Ubuntu14.amd_64.deb
{% endhighlight %}
İndirdiğiniz `openvpn-as-2.0.19-Ubuntu14.amd_64.deb` isimli dosya ile OpenVPN AS kurmak için aşağıdaki komutu verin:
{% highlight bash %}
dpkg -i openvpn-as-2.0.19-Ubuntu14.amd_64.deb
{% endhighlight %}
Kurulum tamamlandığında çıktı olarak şu şekilde iki tane adres verilecektir:

    Admin  UI: https://VPSIPadresi:943/admin
    Client UI: https://VPSIPadresi:943/

`Admin UI` adresi yönetim paneline erişmek için kullanacağımız adrestir. `Client UI` ise VPN'i kullanacak kişilerin erişebileceği paneldir.

Şimdi OpenVPN paneli için şifre belirlememiz gerekiyor. Aşağıdaki komutu terminalde verin ve şifrenizi belirleyin:
{% highlight bash %}
sudo passwd openvpn
{% endhighlight %}
Şifreyi belirledikten sonra `https://VPSIPadresi:943/admin` adresine giderek OpenVPN Admin UI'e giriş yapalım. Karşımıza kullanıcı adı ve şifre isteyen bir ekran çıkacak. Kullanıcı adı `openvpn` olarak tanımlanıyor. Şifre kısmına da `sudo passwd openvpn` komutuyla belirlediğiniz şifreyi gireceksiniz. Admin UI üzerinde VPN'inizi yönetebilirsiniz. Şimdilik burada bir işimiz yok, böyle bir panelin olduğunu bilmeniz yeterli. Araştırma yapmadan bu panelde işlem yapmanızı önermem.

## VPN'i Cihazlarda Kullanmak

Şimdi `https://VPSIPadresi:943/` adresine bağlanarak `Client UI` sayfasına giriş yapalım. Burada kullanıcı adı olarak `openvpn` şifre olarak ise yine `sudo passwd openvpn` komutuyla önceden belirlediğiniz şifreyi girip aşağıdaki `login` seçeneğini seçerek panele giriş yapın.

**Not:** Bazı tarayıcılarda `Client UI` giriş ekranı doğru açılmayabiliyor. Eğer yükleme sayfası geçmez ve giriş ekranı karşınıza gelmezse başka bir tarayıcı ile giriş sayfasını görüntülemeyi deneyin.

`Client UI`'e başarıyla giriş yaptıysanız karşınıza Windows, Mac OS X, Linux, iOS ve Android için OpenVPN uygulamalarının linklerinin bulunduğu bir sayfa karşınıza çıkacak. Bilgisayarınız için işletim sisteminize uygun olarak OpenVPN uygulamasını indirin ve uygulamayı kurduktan sonra kullanıcı adı (openvpn) ve şifrenizi girerek giriş yapın. 

Android ve iOS için mağazadan OpenVPN uygulamasını telefonunuza yükleyin. Ardından `Client UI` sayfasında belirtilen `Yourself (user-locked profile)` linkine tıklayarak `client.ovpn` dosyasını indirin. Telefonunuza OpenVPN uygulamasını kurduktan sonra seçeneklerden import menüsüne gelin ve bu dosyayı uygulamaya aktarın. Ardından kullanıcı adı ve şifrenizi girip `connect` butonuna bastığınızda VPN aktifleşmiş olacak. 

**Güncelleme (21.03.2016):** Son dönemlerde yaşanan talihsiz olaylarla birlikte gelen erişim engelleri ve kısıtlamalar VPN'e olan ihtiyacı tekrar arttırdı. Yazıda bahsettiğim Digital Ocean'a -benim de şu an kullandığım- daha uygun bir alternatif buldum. Sadece VPN kurup kullanmak için [IPXCore](http://ipxcore.com/budget-vps/)'un VPS çözümlerini inceyebilirsiniz.

## Kaynaklar ve İleri Okuma
1. [How To Install OpenVPN Access Server on Ubuntu 12.04](https://www.digitalocean.com/community/tutorials/how-to-install-openvpn-access-server-on-ubuntu-12-04){:target="_blank"}
2. [How To Set Up an OpenVPN Server on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-openvpn-server-on-ubuntu-14-04){:target="_blank"}
3. [How To Install and Configure an OpenVPN Access Server on CentOS 6.5](https://www.digitalocean.com/community/tutorials/openvpn-access-server-centos){:target="_blank"}
