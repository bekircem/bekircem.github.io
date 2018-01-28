---
layout: post
title: Ubuntu Android SDK Kurulumu
description: Ubuntu (Debian) üzerinde Android SDK kurulumu ve PATH ayarları.
keywords: "Android, Linux, Ubuntu"
tags: 
  - Android
  - Linux
  - Ubuntu
redirect_from: "/2015/02/14/ubuntu-android-sdk-kurulumu/"
published: true
---


Bir süredir HTML, CSS ve Javascript ile tüm mobil platformlarda uygulama yapma işlevini gören [Apache Cordova](http://cordova.apache.org/)'yı test ediyorum. Bu testlerim sırasında Android platformunu eklerken Android SDK'sına ihtiyaç duydum. Önceden PATH ayarlarını yapmadığım için hem Windows'ta hem de Linux'te `cordova platform add android` komutunun çıktısında hata aldım. Bu yüzden de temiz bir Android SDK kurulumu yapmam gerekti. 

Ubuntu'da (veya Debian tabanlı herhangi bir Linux işletim sisteminde) Android SDK kurulumunu yapmak için şu adımları izliyoruz:

1. İşletim sisteminizin 32-bit mi 64-bit mi olduğunu kontrol edin.
2. Sisteminiz 32-bit ise `sudo apt-get install libgl1-mesa-dev` komutunu, 64-bit ise `sudo apt-get install ia32-libs` komutunu terminalde çalıştırın. 
3. `sudo apt-get install openjdk-7-jdk` komutuyla JDK kurulumunu yapın. 
4. [Stand-alone SDK tools](http://developer.android.com/sdk/installing/index.html?pkg=tools) sayfasından Android SDK'sını indirin. 
5. İndirdiğiniz .tgz uzantılı arşiv dosyasını açarak "android-sdk-linux" adındaki klasörü arşivden çıkarın. 
6. Terminalde `nautilus ~` kodunu çalıştırın ya da elle home dizininize gidin.
7. Açılan home dizini penceresine "android-sdk-linux" klasörünü taşıyın.
8. Terminalde `cd ~/android-sdk-linux/tools` komutunu ve ardından `./android` komutunu çalıştırın. Eğer işlemleri doğru yaptıysanız Android SDK Manager'i göreceksiniz.
9. Terminalde `sudo gedit ~/.bashrc` komutunu çalıştırın ve açılan sayfanın en üstüne aşağıdaki kodları ekleyin.

{% highlight bash %}#AndroidDev PATH
export PATH=${PATH}:~/android-sdk-linux/tools
export PATH=${PATH}:~/android-sdk-linux/platform-tools{% endhighlight %}


10. Sayfayı kaydedip kapatın.
11. Terminalde `exec bash` komutunu çalıştırın ve ardından `android` komutunu 	verin.
12. "android-sdk-linux" dosyasına yazma izni verin.

Eğer Apache Cordova için SDK kurulumu yaptıysanız bu işlemlerden sonra `cordova platform add android` komutunun çıktısında Android versiyonuyla alakalı bir hata alabilirsiniz. Bunun için terminalde `android` komutunu verin ve açılan "Android SDK Manager" penceresinden istediğiniz versiyonları seçerek kurun.
