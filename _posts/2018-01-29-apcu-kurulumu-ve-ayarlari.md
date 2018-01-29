---
layout: post
title: "APCu Kurulumu ve Ayarları"
description: "PHP5.X sürümlerinde APCu kurulumu, ayarları ve APCu monitörünün kullanımı."
keywords: "APCu,PHP,Cache,Server,sysadmin"
tags: [APCu,PHP,Cache,Server,sysadmin]
---

Digital Ocean üzerinde Mediawiki sistemini çalıştırdığım sunucuda yüksek `cpu load`  değerleri almaya başlamıştım. `APCu` ile cpu load değerlerini 1.20 seviyelerinden 0.20 seviyelerine çektim.

APCu kurulumu PHP sürümünüze ve sunucu yapılandırmanıza göre değişkenlik gösterebilir. PHP 7 ve sonraki sürümler için APCu full backward desteği olmadığı için ek olarak `apcu-bc` eklentisini de kurmanız gerekiyor. PHP 7 üzerinde kurulum yapacaksanız başka kaynakları araştırmanız gerekiyor.

Serverpilot ile yönettiğim sunucumda PHP 5.6 sürümü için kurulum yaptım. Rehber boyunca komutlarda kullanan `phpX.Y` gibi ifadelerde X ve Y değerleri yerine sürüm bilgilerini yazmalısınız.

## PHP 5.4, 5.5 ve 5.6 için APCu Kurulumu

Serverpilot ile yönetilen sunucunuza SSH ile `root` olarak bağlanın. Ardından aşağıdaki komutları sırasıyla çalıştırın. 

    sudo apt-get -y install gcc make autoconf libc-dev pkg-config
    sudo pecl5.Y-sp install apcu-4.0.11

Aşağıdaki sorular geldiğinde enter tuşunuza basıyoruz.

    Enable full APC compatibility [yes] :
    Enable internal debugging in APCu [no] :

Kurulum tamamlandıktan sonra yapılandırma dosyasını oluşturmak ve PHP'yi yeniden başlatmak için sırasıyla aşağıdaki komutları çalıştırıyoruz.

## Kurulumu Doğrulama

APCu'nun doğru bir şeklilde yüklenip yüklenmediğini aşağıdaki komutla kontrol edebilirsiniz.

    phpX.Y-sp -i | grep apcu

Şu şekilde bir çıktı almanız gerekiyor.

    Additional .ini files parsed => /etc/phpX.Y-sp/conf.d/apcu.ini,
    apcu

## `apc.shm_size` Değerini Artırmak
apc.shm_size için yeni bir değer atamadan önce ilk olarak şimdiki değerini öğrenmeniz faydalı olabilir. Bunun için aşağıdaki komutu çalıştırın.

    php -i | grep apc.shm_size

Şu şekilde bir çıktı alacaksınız.

    apc.shm_size => 32M => 32M

32M değeriyle sorun yaşadığınızı varsayalım. Bu değeri değiştirmek için php.ini dosyasına ekleme yapmamız gerekiyor. 

    apc.shm_size = 64M

Serverpilot için /etc/phpX.Y-sp/conf.d dizinindeki apcu.ini dosyasına ekleme yapabiliriz.

    apc.shm_size = 64M

Ardından sunucumuza SSH ile root olarak bağlanarak aşadağı komutu çalıştırıyoruz ve PHP'yi yeniden başlatıyoruz.

    sudo service phpX.Y-fpm-sp restart

## APCu Monitörü Kurulumu
APCu ile ilgili verileri takip etmek için resmi [APCu - APC](https://github.com/krakjoe/apcu/){:target="_blank"} reposunda bulunan [apc.php](https://raw.githubusercontent.com/krakjoe/apcu/master/apc.php) scriptini kullanabilirsiniz. Bu script bize basit ama anlamlı veriler sunacak.

Scripti kullanmak için [apc.php](https://raw.githubusercontent.com/krakjoe/apcu/master/apc.php){:target="_blank"} sayfasındaki içerikle bir `.php` dosyası oluşturun. 

    defaults('ADMIN_USERNAME','apc'); 			// Admin Username
    defaults('ADMIN_PASSWORD','password');  	// Admin Password - CHANGE THIS TO ENABLE!!!

Giriş bilgilerini yukarıdaki kod blokunu bularak değiştirin. Ardından sitenizin ana dizinine atarak web browser üzerinden erişebilirsiniz.

## APCu Bellek Kullanımı
 
Script bize kullanılan ve boş bellek değerlerini gösteriyor. Ayrıca yüzdelik olarak `cache hits` ve `missed hits` değerlerini gösteriyor. Missed hits değeri ne kadar düşükse performans için o kadar iyidir.

![APCu Monitor](/images/apcu-monitor.png)

## APCu Cache Fragmentation

`Cache fragmentation` değeri aşırı data yüklenmesi olduğunda anlaması daha zor bir sistem. Kısaca düşük cache fragmentation performans için daha iyidir.

![APCu Fragmentation](/images/apcu-fragmentation.png)

## Kaynaklar ve İleri Okuma

 1. [How to monitor and tune APCu](https://anavarre.net/how-to-monitor-and-tune-apcu/){:target="_blank"}
 2. [How to Install the PHP APCu Extension](https://serverpilot.io/community/articles/how-to-install-the-php-apcu-extension.html){:target="_blank"}
 