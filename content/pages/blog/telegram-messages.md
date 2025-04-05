---
author: Ignacio Lopez
colors: bg-light-fg-dark
date: '2023-07-08'
featuredImage:
  altText: Thumbnail
  styles:
    self:
      borderRadius: medium
  type: ImageBlock
  url: /images/telegram.jpg
isFeatured: false
seo:
  metaDescription: A Guide to Sending Telegram Messages.
  metaTitle: A Guide to Sending Telegram Messages
  socialImage: /images/telegram.jpg
  type: Seo
slug: sendingjsonCopy code -telgram-messages
styles:
  self:
    flexDirection: col
tags:
- integrations
- telegram
title: A Guide to Sending Telegram Messages
type: PostLayout
---

# Sending Telgram Messages via 'webhook'

1.  Start a conversation with @BotFather.
2.  Run the /newbot command.
3.  Choose a name for your bot and save the HTTP token that's provided.
4.  Since your browser can send HTTPS requests, you can quickly test the API. After obtaining your token, try pasting this string into your browser to get started:
5.  Find your chat ID in the list

```
https://api.telegram.org/bot<TOKEN>/getUpdates
```

6. Make a GET request to:

```
 https://api.telegram.org/bot{{ $json.bot_token }}/sendMessage?chat_id={{ $json.chat_id}}&parse_mode=html&text={{ $json.message }}
```

![](./images/nachoficate.png)