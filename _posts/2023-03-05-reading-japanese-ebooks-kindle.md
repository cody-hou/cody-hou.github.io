---
title: "Reading Japanese E-Books on Kindle"
date: 2023-03-05
last_modified_at: 2023-03-14
tags:
  - Guides
header:
  image: /assets/images/headers/2023-03-05-reading-japanese-ebooks-kindle-header.jpg
  teaser: /assets/images/headers/2023-03-05-reading-japanese-ebooks-kindle-header.jpg
gallery:
  - url: /assets/images/posts/2023-03-05-reading-japanese-ebooks-kindle-1.png
    image_path: /assets/images/posts/2023-03-05-reading-japanese-ebooks-kindle-1.png
    alt: "IPAex 明朝"
    title: "IPAex 明朝"
  - url: /assets/images/posts/2023-03-05-reading-japanese-ebooks-kindle-2.png
    image_path: /assets/images/posts/2023-03-05-reading-japanese-ebooks-kindle-2.png
    alt: "Noto Serif Japanese Regular"
    title: "Noto Serif Japanese Regular"
  - url: /assets/images/posts/2023-03-05-reading-japanese-ebooks-kindle-3.png
    image_path: /assets/images/posts/2023-03-05-reading-japanese-ebooks-kindle-3.png
    alt: "Source Hans Serif Japanese Regular"
    title: "Source Hans Serif Japanese Regular"
  - url: /assets/images/posts/2023-03-05-reading-japanese-ebooks-kindle-4.png
    image_path: /assets/images/posts/2023-03-05-reading-japanese-ebooks-kindle-4.png
    alt: "Zen Old Mincho Regular"
    title: "Zen Old Mincho Regular"
---

I have been learning Japanese since winter break of 10th grade. Since graduating high school I haven't dedicated myself to actively doing exercises, and have instead been learning passively through TV shows and YouTube. To improve my stagnating vocabulary and get back into reading, I decided to find a platform to read Japanese e-books.

## Choosing a Format and Store

There are [quite a few e-book platforms available](https://www.tofugu.com/japanese/how-to-buy-japanese-ebooks/), each with their own pros and cons discussed in the linked Tofugu article. The platforms have 立ち読み/試し読み (sample reading) that you can take advantage of to try out their apps and format. Some important points to consider:

* Consider first what type of book you will primarily be reading: novels/light novels, visual books/magazines, or manga.
* BookWalker has a wide selection of manga as they are the store for Kadokawa, a huge Japanese publisher. BookLive seems to have a lot of free manga and a decent selection as well.
* BookLive needs a VPN outside of Japan for their mobile reading apps. 
* I found reading novels on BookWalker and Honto apps frustrating as their novels were either scans of the book or had very limited options for customizing the font size and reading experience.
* E-Ink users should really look at Amazon.co.jp or Rakuten Kobo (strangely, not mentioned in the article), though you cannot use a credit card issued outside of Japan on the Japanese Rakuten Kobo website. Foreign credit cards work perfectly fine on Amazon.co.jp.
* Amazon.co.jp accounts are separate from Amazon.com accounts, so you cannot be logged in to both on the same device.

Ultimately I chose Amazon.co.jp as I wanted to easily read novels or light novels on an e-ink device, which I find much easier on the eyes, without having to use a VPN.

## Considerations for Kindle E-Readers

Despite my reservations with Amazon, the Amazon Kindle series of e-readers are objectively good devices. They don't need to be a big investment either; a used or hand-me-down Kindle is a great budget option as the battery ages surprisingly well. I have a Kindle PaperWhite 2 from 2013 that still holds a charge for at least 6 hours of continuous reading. (The older models tend to be more repairable too.)

Display resolution is an important consideration for Japanese language books because the diagonal strokes in characters tend to become pixelated at lower resolutions, which I personally find very distracting. The older base models, like the Kindle 8th generation from 2016 that I use, have a 167 ppi resolution display, though the reading experience on 167 ppi is actually not too bad [depending on the font and font size used](#customizing-fonts). If you want a "paper-like" experience, aim for any of the newer PaperWhite models, the 10th generation base Kindle, and the Oasis models.

## Setting Up a Amazon.co.jp Account

Creating an [Amazon.co.jp account](https://www.amazon.co.jp/-/en/ap/register?openid.pape.max_auth_age=0&openid.return_to=https%3A%2F%2Fwww.amazon.co.jp%2F%3F_encoding%3DUTF8%26ref_%3Dnav_custrec_newcust&openid.identity=http%3A%2F%2Fspecs.openid.net%2Fauth%2F2.0%2Fidentifier_select&openid.assoc_handle=jpflex&openid.mode=checkid_setup&openid.claimed_id=http%3A%2F%2Fspecs.openid.net%2Fauth%2F2.0%2Fidentifier_select&openid.ns=http%3A%2F%2Fspecs.openid.net%2Fauth%2F2.0&) is fairly simple, as their site also has an English language option. However, by default you will still be region locked from buying books. Go to "Your Content and Devices" and access the "Preferences" tab at the top. Add payment methods to both the "Kindle Payment Setting" and "Amazon Digital Payment Setting" sections; I highly recommend a credit card that does not have foreign transaction fees. Then, under "Country/Region Settings" you'll need to add a Japanese address (any will do; for example, I used Kenroku-en's address and contact phone number). The store will now think you are a Japanese resident and should not restrict you from buying books based on your current location.

## Improving Vocabulary Lookup

Kindle has a Japanese-Japanese and Japanese-English dictionary you can use for looking up vocab, but I find them to be insufficient, especially at detecting conjugated verbs. A sideloadable version of the JMdict and JMnedict dictionaries, [jmdict-kindle](https://github.com/jmdict-kindle/jmdict-kindle), is compatible with most Kindle devices and really improves the word search experience. The Kindle will even remember words that you searched in its Vocabulary Builder (up to 2000 search terms), which you can copy off of the Kindle (search for `vocab.db`) and [convert to Anki cards](https://github.com/Kartoffel0/Kindle2Anki).

## Customizing Fonts

The Kindle comes with one Gothic (sans-serif) and two Mincho (serif) typefaces. I personally find Mincho typefaces easier on the eyes when reading, but wasn't happy with the default selection of fonts. The Kindle supports OpenType (.otf) and TrueType (.ttf, .ttc) fonts, which can be copied into the Kindle's `fonts` folder (a `README` document is also in that folder for reference).

* Open-Source Fonts
    * [IPAex 明朝](https://moji.or.jp/ipafont/)
    * [Noto Serif Japanese Regular](https://fonts.google.com/noto/specimen/Noto+Serif+JP?noto.script=Jpan)
    * [Source Han Serif Japanese Regular](https://github.com/adobe-fonts/source-han-serif/tree/release/OTF/Japanese)
    * [Zen Old Mincho Regular](https://fonts.google.com/specimen/Zen+Old+Mincho)
* Microsoft Fonts (find in C:\Windows\Fonts\)
    * BIZ UDMincho
    * 游明朝
* Apple Fonts (find in /Library/Fonts/)
    * ヒラギノ明朝 ProN W3

{% include gallery layout="half" caption="Screenshots of example text on a 167 ppi display using the open-source fonts listed above. The font used is in the caption of each photo (click to expand). The text is from [本好きの下剋上～司書になるためには手段を選んでいられません～第一部「兵士の娘I」](https://www.amazon.co.jp/gp/product/B00TKIAMYW/ref=ppx_yo_dt_b_d_asin_title_o02?ie=UTF8&psc=1)." %}

I like to rotate different fonts depending on how I'm feeling. I find myself switching between Source Han Serif and IPAex 明朝 the most.

## My Reading List (as of 2023-03-14)

While I haven't read through the full list below, I have seen good reviews about these light novels (though there are lots of reincarnation/fantasy ones here). I find it especially fun to watch the anime adaptations of books as I read to visualize what I'm reading or make sure that I understood a chapter correctly.

* [本好きの下剋上～司書になるためには手段を選んでいられません～第一部「兵士の娘I」](https://www.amazon.co.jp/gp/product/B00TKIAMYW/ref=ppx_yo_dt_b_d_asin_title_o02?ie=UTF8&psc=1): I have finished this book and can confirm that it's a great read; also has a popular anime adaptation.
* [氷菓 「古典部」シリーズ](https://www.amazon.co.jp/%E6%B0%B7%E8%8F%93-%E3%80%8C%E5%8F%A4%E5%85%B8%E9%83%A8%E3%80%8D%E3%82%B7%E3%83%AA%E3%83%BC%E3%82%BA-%E8%A7%92%E5%B7%9D%E6%96%87%E5%BA%AB-%E7%B1%B3%E6%BE%A4-%E7%A9%82%E4%BF%A1-ebook/dp/B009PKN0D0/ref=sr_1_1?crid=IYN6VJ60C2MU&keywords=%E6%B0%B7%E8%8F%93+%E5%B0%8F%E8%AA%AC&qid=1678803042&s=digital-text&sprefix=%2Cdigital-text%2C361&sr=1-1): has a stellar anime adaptation by Kyoto Animation.
* [響け！ ユーフォニアム 北宇治高校吹奏楽部へようこそ](https://www.amazon.co.jp/%E9%9F%BF%E3%81%91%EF%BC%81-%E3%83%A6%E3%83%BC%E3%83%95%E3%82%A9%E3%83%8B%E3%82%A2%E3%83%A0-%E5%8C%97%E5%AE%87%E6%B2%BB%E9%AB%98%E6%A0%A1%E5%90%B9%E5%A5%8F%E6%A5%BD%E9%83%A8%E3%81%B8%E3%82%88%E3%81%86%E3%81%93%E3%81%9D-%E5%AE%9D%E5%B3%B6%E7%A4%BE%E6%96%87%E5%BA%AB-%E6%AD%A6%E7%94%B0%E7%B6%BE%E4%B9%83-ebook/dp/B07SZBJR55/ref=sr_1_1?crid=PCXDOF0HJSFN&keywords=%E9%9F%BF%E3%81%91+%E3%83%A6%E3%83%BC%E3%83%95%E3%82%A9%E3%83%8B%E3%82%A2%E3%83%A0+%E5%B0%8F%E8%AA%AC&qid=1678803212&s=digital-text&sprefix=%E9%9F%BF%E3%81%91%2Cdigital-text%2C203&sr=1-1): has a stellar anime adaptation by Kyoto Animation.
* [転生したらスライムだった件1](https://www.amazon.co.jp/gp/product/B00L33R6K0/ref=ppx_yo_dt_b_d_asin_title_o03?ie=UTF8&psc=1): has a popular anime adaptation.
* [異世界食堂　１](https://www.amazon.co.jp/gp/product/B071JYJ8ZB/ref=ppx_yo_dt_b_d_asin_title_o04?ie=UTF8&psc=1)
* [転生王女と天才令嬢の魔法革命](https://www.amazon.co.jp/gp/product/B083TR8VBH/ref=ppx_yo_dt_b_d_asin_title_o05?ie=UTF8&psc=1)
* [狼と香辛料](https://www.amazon.co.jp/gp/product/B00GOO636G/ref=ppx_yo_dt_b_d_asin_title_o01?ie=UTF8&psc=1)

Happy reading!
