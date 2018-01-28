---
layout: post
title: Jekyll ve Github Pages ile Statik Blog Oluşturmak
description: Jekyll ve Github Pages ile statik blog kurulumu.
keywords: "Jekyll, Github Pages"
tags: 
  - Jekyll
  - Github Pages
redirect_from: "/2015/02/09/jekyll-ve-github-pages-ile-statik-blog-olusturmak/"
published: true
---


Bir süredir Wordpress’e alternatif olabilecek statik blog oluşturucuları araştırıyorum. [Jekyll](http://jekyllrb.com/)’yi kullanmayı tercih ettim. Şu an baktığınız sayfa  Jekyll statik blog oluşturucu ile Github Pages üzerinde yayın yapıyor. 

### Jekyll ve Statik Blog Oluşturucular Hakkında Bazı Bilgiler

Wordpress ve diğer içerik yönetim sistemlerinden farklı olarak statik bloglar veritabanına ihtiyaç duymuyorlar. Bir statik blog oluşturucu olan Jekyll, veritabanı ve `PHP` dosyası içermiyor. Bu da statik blogların hızlı ve güvenli olmasını sağlıyor.

> I see the future of Wordpress as a web operating system.” – Matt Mullenweg 

Genel çalışma mantığı olarak statik blog oluşturucular `Markdown` diliyle yazılmış içerikleri yayınlanmadan önce HTML sayfasına çeviriyorlar. Bu yüzden de `NodeJS`, `Ruby` ve `Ruby GEM` gibi farklı gereksinimlere ihtiyaç duyuyorlar. Github Pages sayesinde  bu gereksinimleri atlamak mümkün olsa da (en azından `Jekyll` için) statik blog oluşturucuların yerel makinelere de mutlaka kurulması gerekiyor. Windows üzerinde bazı aşamalarında uyumluluk sorunları yaşadığım ve bir an önce yayına alma konusunda heyecanlı olduğum için gereksinimlerin konfigürasyonunu henüz tam olarak yapmadan Github Pages'i adeta Jekyll'nin `CMS` bileşeni olarak kullanarak blogu yayına aldım. 

### Github Pages ile Jekyll Kurulumu

Jekyll’yi Github Pages ile kullanmak için [Jekyll Now](http://www.github.com/barryclark/jekyll-now) sayfasına gidin ve sayfanın sağ üst köşesindeki `Fork` butonuna tıklayarak temanın deposunu kendi Github hesabınıza kopyalayın. [Şu listedeki](https://github.com/jekyll/jekyll/wiki/Themes) Jekyll temalarından istediğinizi seçerek aynı işlemi yapabilirsiniz. Depoyu fork ettikten sonra sağ kısımdaki `Settings`’e tıklayın ve depo adını `kullaniciadiniz.github.io` olarak değiştirin. Blogunuzu kendi alan adınız ile kullanmak için deponuzdaki `CNAME` dosyasının içerisine alan adınızı eklemeli ve ardından alın adı sağlayıcınızın ayar panelinden [gerekli host ayarlarını](https://help.github.com/articles/setting-up-a-custom-domain-with-pages) yapmalısınız.

İnternet tarayıcısından bloga girdiğinizde "You're up and running" yazısını görüyorsanız Jekyll kullanıma hazır demektir. Bundan sonrası için başlangıç olarak Jekyll’nin dizin yapısını anlamaya çalışın. Depodaki `_config.yml` dosyası blogunuzun ana ayaları ile birlikte tüm temel ayarların bulunduğu dosyadır. Bu dosyadan anasayfa başlığı, açıklama ve etiketlerini belirleyebilir, bazı ayarları açık kapatabilir ve daha birçok şey yapabilirsiniz. `_posts` dizini blog yazılarınızın bulunduğu dizindir. Bu dizine girdiğinizde `Markdown` yani `.md` uzantılı örnek yazılar listelenelir, sistemi daha iyi anlamak için o dosyaları incelemenizde fayda var. Bu dizine, şablona uyarak yeni bir dosya eklediğinizde blog yazınız da blogunuza eklenmiş olur.

Konuyla ilgili kaynaklar:

1. [Build A Blog With Jekyll And GitHub Pages](http://www.smashingmagazine.com/2014/08/01/build-blog-jekyll-github-pages)
2. [Jekyll ve Github Pages kullanarak kendi blogumuzu oluşturmak](http://aristona.github.io/jekyll-ve-github-pages-kullanarak-kendi-blogumuzu-olusturmak/)
3. [Blog altyapısı olarak Jekyll kullanmak](http://blog.arsln.org/blog-altyapisi-olarak-jekyll-kullanmak/)
4. [Run Jekyll on Windows](http://jekyll-windows.juthilo.com/)
