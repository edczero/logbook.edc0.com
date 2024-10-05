---
title: DaviantArt RSS Feed
author: edc0
categories: [Notes, RSS]
tags: [deviantart, rss]
render_with_liquid: false
---


Most DeviantArt features are available through now long lost RSS feed. For how to create RSS in DeviantArt, you can read the official instructions for example in &quot;How do I use RSS Feeds?&quot; or &quot;Access deviations with RSS feeds&quot;. 
<!--more-->

But even so, I managed to run into problem of creating RSS Feeds. I found two solutions for how to

- We use an example from DeviantArt with changes.

```
https://backend.deviantart.com/rss.xml?type=deviation%26q=[name]+sort%3Atime+meta%3Aall
```

For example 

```
https://backend.deviantart.com/rss.xml?type=deviation%26q=zeon-in-a-tree+sort%3Atime+meta%3Aall
```

This method does not work with all profiles and for some profiles this method does not work.

- This is a universal way to create RSS Feeds.
https://backend.deviantart.com/rss.xml?q=gallery:[name]

For example `https://backend.deviantart.com/rss.xml?q=gallery:zeon-in-a-tree`

Also in this repository I found how to access a specific gallery.

For example `https://backend.deviantart.com/rss.xml?q=gallery:[deviant name]/[gallery]`

Search term

Digital Silhouette Illustration

```
Wallpapers gallery

https://backend.deviantart.com/rss.xml?q=gallery:rbatinic/gallery

Featured
https://backend.deviantart.com/rss.xml?q=gallery:rbatinic/13180918


https://backend.deviantart.com/rss.xml?q=gallery:bo0xvn/6423159


https://backend.deviantart.com/rss.xml?q=gallery:oviotti/57741731

https://backend.deviantart.com/rss.xml?q=gallery:bisbiswas/63594491/


https://backend.deviantart.com/rss.xml?q=gallery:anatofinnstark/68362759/
```

FanArt Illustration


From old ifttt accounts

```
https://backend.deviantart.com/rss.xml?q=gallery:hellobaby/289539/


https://backend.deviantart.com/rss.xml?q=gallery%3Anicolas-gouny-art%2F51685%26type=deviation


http://backend.deviantart.com/rss.xml?q=gallery%3APicolo-kun%2F47824199%26type=deviation

https://backend.deviantart.com/rss.xml?q=gallery%3Akmyechan%26type=deviation
```



Getting photo url from feed-entry-url using oembed api:

Imge url will be `<url>; tag/key`

```
https://www.deviantart.com/developers/oembed

http://backend.deviantart.com/oembed?format=xml%26url=$body


json: https://backend.deviantart.com/oembed?url=https://www.deviantart.com/nicolas-gouny-art/art/Birds-908192053



xml: https://backend.deviantart.com/oembed?format=xml%26url=https://www.deviantart.com/nicolas-gouny-art/art/Birds-908192053
```