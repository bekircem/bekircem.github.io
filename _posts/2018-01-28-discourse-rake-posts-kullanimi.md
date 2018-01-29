---
layout: post
title: "Discourse Tüm Mesajlarda Bir String İfade Değiştirmek"
description: "Discourse rake posts özelliğini kullanarak tüm mesajlarda bir string ifadeyi değiştirmek"
keywords: "Discourse, Ruby"
tags: [Discourse]
---

Mybb forum yazılımından Discourse yazılımına geçiş yapmak için Discourse'un import script'ini kullandım. 300.000'in üzerinde mesaj ve 10.000'in üzerinde konuyu Discourse yazılımına aktarmak zorlu bir süreç oldu. Aktarım sonrasında Mybb mesaj editörüyle oluşturulmuş bazı mesaj stillendirme etiketleri sorun çıkardı. Bu etiketlerin tamamını temizlemek için Discourse'un 'rake posts' özelliğini kullandım. 

Öncelikle SSH üzerinden sunucuya bağlanıyoruz ve Docker container'ımıza giriyoruz.

    cd /var/discourse
    ./launcher enter app

Docker container'ımıza giriş yaptıktan sonra ihtiyacımıza göre kullanabileceğimiz iki farklı rake posts komutu var. String ifadeyi başka bir string ifadeyle değiştirmek istiyorsak remap, string ifadeyi silmek istiyorsak delete_word kullanacağız.

Spesifik bir string ifadesini değiştirmek

    rake posts:remap["bul","degistir"]

Örnek:

    rake posts:remap[":slightly_smiling:",":slight_smile:"]
    
Bir string’i silmek için delete_word kullanıyoruz.

	rake posts:delete_word["silinecek-string"]

Değiştirmek ya da silmek istediğimiz string virgül içeriyorsa virgülün komut işlemi görmesini engellemek için “\” kullanmamız gerekiyor.

	rake posts: delete_word[“elma\,\ armut”]

