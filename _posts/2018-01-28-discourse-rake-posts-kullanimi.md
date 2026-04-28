---
title: "Discourse Tüm Mesajlarda Bir String İfade Değiştirmek"
description: "Discourse rake posts özelliğini kullanarak tüm mesajlarda bir string ifadeyi değiştirmek"
date: 2018-01-28 00:00:00 +0300
categories: [Discourse]
tags: [discourse, ruby]
---

Mybb forum yazılımından Discourse yazılımına geçiş yapmak için Discourse'un import script'ini kullandım. 300.000'in üzerinde mesaj ve 10.000'in üzerinde konuyu Discourse yazılımına aktarmak zorlu bir süreç oldu. Aktarım sonrasında Mybb mesaj editörüyle oluşturulmuş bazı mesaj stillendirme etiketleri sorun çıkardı. Bu etiketlerin tamamını temizlemek için Discourse'un `rake posts` özelliğini kullandım.

## Hazırlık

Öncelikle SSH üzerinden sunucuya bağlanıp Docker container'a giriyoruz:

```bash
cd /var/discourse
./launcher enter app
```

## Rake Posts Komutları

Container'a girdikten sonra ihtiyaca göre iki farklı komut kullanılabilir:

- **`remap`** — bir string ifadeyi başka bir string ifadeyle değiştirmek için
- **`delete_word`** — bir string ifadeyi silmek için

### String İfadeyi Değiştirmek

```bash
rake posts:remap["bul","degistir"]
```

Örnek:

```bash
rake posts:remap[":slightly_smiling:",":slight_smile:"]
```

### String İfadeyi Silmek

```bash
rake posts:delete_word["silinecek-string"]
```

## Özel Karakter İçeren String'ler

Değiştirmek ya da silmek istediğiniz string virgül içeriyorsa, virgülün komut işlemi görmesini engellemek için `\` karakteri kullanın:

```bash
rake posts:delete_word["elma\,\ armut"]
```
