---
type: PostLayout
title: Hide Your Server's IP While Exposing Your Service
date: '2022-10-10'
excerpt: How to run cloudflare vpn with docker?
featuredImage:
  type: ImageBlock
  url: /images/Screenshot 2025-03-30 210853.png
  altText: Thumbnail
  elementId: ''
  styles:
    self:
      padding:
        - pt-0
        - pl-0
        - pb-0
        - pr-0
bottomSections:
  - type: DividerSection
    title: Divider
    elementId: ''
    colors: bg-light-fg-dark
    styles:
      self:
        padding:
          - pt-3
          - pl-3
          - pb-3
          - pr-3
  - type: RecentPostsSection
    title:
      type: TitleBlock
      text: Recent posts
      color: text-dark
      styles:
        self:
          textAlign: center
    recentCount: 3
    showThumbnail: true
    showExcerpt: true
    showDate: true
    showAuthor: true
    actions: []
    elementId: ''
    variant: three-col-grid
    colors: bg-light-fg-dark
    styles:
      self:
        justifyContent: center
slug: cloudflare-vpn-with-docker
isFeatured: false
isDraft: false
seo:
  type: Seo
  metaTitle: how to use cloudflare vpn with docker ?
  metaDescription: how to use cloudflare vpn with docker ?
  addTitleSuffix: false
  metaTags: []
  socialImage: /images/Screenshot 2025-03-30 210853.png
colors: bg-light-fg-dark
styles:
  self:
    flexDirection: col
---
![](/images/Screenshot%202025-03-30%20210853.png)

In this post we are going to explain how to expose our service through a Cloudflare VPN **for free**, no matter where we are deploying.

## Why this is important?

When exposing our service directly to the internet, we lack the ability to effectively manage a DDoS attack. Instead, we can use Cloudflare as a reverse proxy and delegate that responsibility to them. Another key reason is that we should avoid exposing our IP address at all costs. This enhances our privacy and helps prevent issues like censorship. For instance, if an ISP or agency wanted to censor us, they would need to block a Cloudflare nameserver, which would disrupt countless other websites as well.

## Top 5 IT Companies Using Cloudflare Protection

*   IBM

*   GitHub

*   GitLab

*   Atlassian

*   Zendesk

## How to use Cloudflare zero trust?

### Create a "Tunnel" under zero trust > networks > tunnels.

### ![](/images/image_2025-04-06_211944711.png)Run a cloudflare vpn on your server.

I am using a docker container but you can use [cloudflared cli](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/do-more-with-tunnels/local-management/create-local-tunnel/) as well.

```
```

[View docker-compose.yml file on GitHub](https://github.com/byli-dev/cloudflare_tunnel_demo)

As you may notice, we don’t need to expose our Docker service port to the host because both services are on the same network.

We should use the service name we are exposing as dns.

![](/images/image_2025-04-08_000810504.png)

And that's it !

[Visit Demo](zero-trust.byli.dev)![](/images/image_2025-04-08_001014400.png)

