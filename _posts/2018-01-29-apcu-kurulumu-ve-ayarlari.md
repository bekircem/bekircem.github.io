---
title: "APCu Kurulumu ve Ayarları"
description: "PHP5.X sürümlerinde APCu kurulumu, ayarları ve APCu monitörünün kullanımı."
date: 2018-01-29 00:00:00 +0300
categories: [PHP, Server]
tags: [apcu, php, cache, server, sysadmin]
---

Digital Ocean üzerinde Mediawiki sistemini çalıştırdığım sunucuda yüksek `cpu load` değerleri almaya başlamıştım. `APCu` ile cpu load değerlerini 1.20 seviyelerinden 0.20 seviyelerine çektim.

APCu kurulumu PHP sürümünüze ve sunucu yapılandırmanıza göre değişkenlik gösterebilir. PHP 7 ve sonraki sürümler için APCu full backward desteği olmadığından ek olarak `apcu-bc` eklentisini de kurmanız gerekiyor. PHP 7 üzerinde kurulum yapacaksanız başka kaynakları araştırmanız gerekiyor.

Serverpilot ile yönettiğim sunucumda PHP 5.6 sürümü için kurulum yaptım. Rehber boyunca komutlarda kullanılan `phpX.Y` ifadelerinde `X` ve `Y` değerleri yerine kendi sürüm bilgilerinizi yazmalısınız.

## PHP 5.4, 5.5 ve 5.6 için APCu Kurulumu

Serverpilot ile yönetilen sunucunuza SSH ile `root` olarak bağlanın. Ardından aşağıdaki komutları sırasıyla çalıştırın:

```bash
sudo apt-get -y install gcc make autoconf libc-dev pkg-config
sudo pecl5.Y-sp install apcu-4.0.11
```

Aşağıdaki sorular geldiğinde `Enter` tuşuna basın:

```
Enable full APC compatibility [yes] :
Enable internal debugging in APCu [no] :
```

Kurulum tamamlandıktan sonra yapılandırma dosyasını oluşturun ve PHP'yi yeniden başlatın.

## Kurulumu Doğrulama

APCu'nun doğru yüklenip yüklenmediğini kontrol edin:

```bash
phpX.Y-sp -i | grep apcu
```

Beklenen çıktı:

```
Additional .ini files parsed => /etc/phpX.Y-sp/conf.d/apcu.ini,
apcu
```

## `apc.shm_size` Değerini Artırmak

Mevcut değeri öğrenmek için:

```bash
php -i | grep apc.shm_size
```

Örnek çıktı:

```
apc.shm_size => 32M => 32M
```

Bu değeri artırmak için Serverpilot'ta `/etc/phpX.Y-sp/conf.d/apcu.ini` dosyasına şu satırı ekleyin:

```ini
apc.shm_size = 64M
```

Ardından PHP'yi yeniden başlatın:

```bash
sudo service phpX.Y-fpm-sp restart
```

## APCu Monitörü Kurulumu

APCu verilerini takip etmek için resmi [APCu reposundaki](https://github.com/krakjoe/apcu/) [apc.php](https://raw.githubusercontent.com/krakjoe/apcu/master/apc.php) scriptini kullanabilirsiniz.

Script içindeki giriş bilgilerini değiştirin:

```php
defaults('ADMIN_USERNAME','apc');       // Admin Username
defaults('ADMIN_PASSWORD','password');  // Admin Password - CHANGE THIS TO ENABLE!!!
```

Dosyayı sitenizin ana dizinine atarak web tarayıcısından erişebilirsiniz.

## APCu Bellek Kullanımı

Script kullanılan ve boş bellek değerlerini gösteriyor. Ayrıca yüzdelik olarak `cache hits` ve `missed hits` değerlerini gösteriyor. Missed hits değeri ne kadar düşükse performans için o kadar iyidir.

![APCu Monitor](/images/apcu-monitor.png)

## APCu Cache Fragmentation

Düşük cache fragmentation değeri performans için daha iyidir.

![APCu Fragmentation](/images/apcu-fragmentation.png)

## Kaynaklar

1. [How to monitor and tune APCu](https://anavarre.net/how-to-monitor-and-tune-apcu/)
2. [How to Install the PHP APCu Extension](https://serverpilot.io/community/articles/how-to-install-the-php-apcu-extension.html)
