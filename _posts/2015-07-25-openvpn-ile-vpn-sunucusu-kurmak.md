---
title: OpenVPN ile VPN Sunucusu Kurmak
description: Açık kaynak kodlu OpenVPN teknolojisi ile kendi VPN servisinizi oluşturun.
date: 2015-07-25 00:00:00 +0300
categories: [Linux, VPN]
tags: [vpn, vps, digitalocean, openvpn]
redirect_from:
  - /2015/07/25/openvpn-ile-vpn-sunucusu-kurmak/
  - /2015/07/openvpn-ile-vpn-sunucusu-kurmak/
---

Dün geceden itibaren internette genel anlamda bir yavaşlık olması ve özellikle Facebook, Twitter, Youtube gibi mecralara erişimde kesintiler yaşanması sebebiyle VPN kullanma ihtiyacı duydum. IPVanish, Hidemyass gibi VPN servislerini daha önceden kullanmıştım. Ancak bu servisleri istediğim zaman pasifleştirip aktifleştiremediğim ve isteğime göre yapılandıramadığım için bu sefer tercih etmedim. Bu rehberde yapılabilecek en basit haliyle kendi sanal sunucunuzu nasıl VPN servisi haline getirebileceğinizi anlatacağım.

Bu rehberde geliştiriciler için cloud hizmeti sunan [Digital Ocean](http://www.digitalocean.com) VPS'lerinden 5 dolarlık paketi kullandım. VPN için 5 dolarlık paket fazlasıyla yeterli.

## VPS Oluşturma

Digital Ocean'da **Create Droplet** butonuna tıklayarak yeni bir sanal sunucu oluşturuyoruz. **Droplet Hostname** için herhangi bir isim girebilirsiniz. **Select Size** kısmından 5 dolarlık paketi seçiyoruz. **Select Region** kısmında VPS lokasyonunu belirleyeceğiz — genelde Amsterdam önerilse de Frankfurt lokasyonunu öneririm. **Select Image** kısmında Ubuntu 14.04 x64 sürümünü seçin.

**Create Droplet** butonuna tıklayarak VPS'i oluşturun. 50-60 saniye içerisinde VPS'iniz hazır olacak ve bilgiler kayıt olduğunuz mail adresinize gelecek.

## VPN için VPS Yapılandırma

Mail adresinize gelen IP, kullanıcı adı ve şifre bilgileriyle VPS'e bağlanmanız gerekiyor. Bunun için Windows'ta [Putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html), macOS'te iTerm2, Linux'te terminali kullanabilirsiniz.

SSH istemcinize VPS bilgilerini girerek bağlanın. İlk girişte standart şifrenizi yenisiyle değiştirmeniz istenecek.

### OpenVPN Access Server Kurulumu

OpenVPN Access Server, OpenVPN'i sanal sunucunuza 3 adımda kurmanızı sağlayan resmi bir çözümdür.

Sisteminize uygun en güncel paketi bulmak için [bu sayfayı](http://swupdate.openvpn.org/as/) ziyaret edin. En güncel sürümler en aşağıda yer alıyor. Uygun sürümün bağlantısını kopyalayın ve aşağıdaki komutla sunucunuza indirin:

```bash
sudo wget http://swupdate.openvpn.org/as/openvpn-as-2.0.19-Ubuntu14.amd_64.deb
```

İndirdiğiniz dosyayı kurmak için:

```bash
dpkg -i openvpn-as-2.0.19-Ubuntu14.amd_64.deb
```

Kurulum tamamlandığında çıktı olarak iki adres verilecektir:

```
Admin  UI: https://VPSIPadresi:943/admin
Client UI: https://VPSIPadresi:943/
```

- **Admin UI** — yönetim paneline erişmek için kullanılır
- **Client UI** — VPN'i kullanacak kişilerin erişeceği paneldir

Şimdi OpenVPN paneli için şifre belirleyin:

```bash
sudo passwd openvpn
```

Şifreyi belirledikten sonra `https://VPSIPadresi:943/admin` adresine gidin. Kullanıcı adı `openvpn`, şifre ise az önce belirlediğiniz şifredir.

## VPN'i Cihazlarda Kullanmak

`https://VPSIPadresi:943/` adresine giderek **Client UI** sayfasına giriş yapın. Kullanıcı adı `openvpn`, şifre ise `sudo passwd openvpn` komutuyla belirlediğiniz şifredir.

> **Not:** Bazı tarayıcılarda Client UI giriş ekranı doğru açılmayabiliyor. Sorun yaşarsanız farklı bir tarayıcı deneyin.

Başarıyla giriş yaptıysanız Windows, macOS, Linux, iOS ve Android için OpenVPN uygulama bağlantılarının bulunduğu bir sayfa karşınıza çıkacak. İşletim sisteminize uygun uygulamayı indirin, kurun ve kullanıcı adı ile şifrenizi girerek bağlanın.

**Mobil cihazlar için:** Mağazadan OpenVPN uygulamasını yükleyin. Ardından Client UI sayfasındaki **Yourself (user-locked profile)** linkine tıklayarak `client.ovpn` dosyasını indirin. Uygulamada import menüsünden bu dosyayı içe aktarın ve kullanıcı adı ile şifrenizi girerek **Connect** butonuna basın.

> **Güncelleme (21.03.2016):** Daha uygun bir alternatif olarak [IPXCore](http://ipxcore.com/budget-vps/)'un VPS çözümlerini inceleyebilirsiniz.

## Kaynaklar

1. [How To Install OpenVPN Access Server on Ubuntu 12.04](https://www.digitalocean.com/community/tutorials/how-to-install-openvpn-access-server-on-ubuntu-12-04)
2. [How To Set Up an OpenVPN Server on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-openvpn-server-on-ubuntu-14-04)
3. [How To Install and Configure an OpenVPN Access Server on CentOS 6.5](https://www.digitalocean.com/community/tutorials/openvpn-access-server-centos)
