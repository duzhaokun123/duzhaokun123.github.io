---
title: 没有意义的标题
date: 2026-09-09 15:36:25
excerpt: 没有意义的摘要
tags: []
---

<script>
  document.title = '没有意义的网页标题 (不是文章标题)'
</script>
<script src="https://cdn.jsdelivr.net/npm/@helia/verified-fetch@8/dist/index.min.js"></script>
<script async type="module" src="https://cdn.jsdelivr.net/npm/bilibili-card/dist/components/index.js"></script>
<script async src="https://platform.x.com/widgets.js" charset="utf-8"></script>

## 没有意义的章节标题

没有意义的正文

meaningless worlds in English

<span lang="ja">
無意味な言葉も日本語で
</span>

<img class="ipfs-img" src="ipfs://bafybeicuqfyptcehuccbxnbelgmd3dkbehrvpw7jmq6qb4asyzfinewfii" alt="meaningless content from ipfs">

<bilibili-card 
  vid="BV1GJ411x7h7"
  type="video"
  title="没有意义的视频"
  author="Rick Astley"
  cover="https://i0.hdslb.com/bfs/archive/259173841f44b9270050544300a717861f6de10f.jpg" duration="03:33" views="1.1亿"
  danmakus="14.9万"
  info-types="views danmakus"
  theme="system">
</bilibili-card>

<blockquote class="twitter-tweet" data-lang="zh-cn" data-theme="dark">
  <p lang="zh" dir="ltr">没有意义 不如发色图</p>
  &mdash; o0kam1 (@duzhaokun123)
  <a href="https://x.com/duzhaokun123/status/2097623080176890341?ref_src=twsrc%5Etfw">2026-09-09</a>
</blockquote>

<script>
  Array.from(document.getElementsByClassName('ipfs-img')).forEach((img) => {
    HeliaVerifiedFetch.verifiedFetch(img.src, { redirect: 'manual'})
      .then((resp) => resp.blob())
      .then((blob) => {
        img.src = URL.createObjectURL(blob)
    })
  })
</script>