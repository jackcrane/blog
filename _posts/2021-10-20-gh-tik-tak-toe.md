---
layout: post
title:  "Github readme Tik Tak Toe: Technical Discussion"
date:   2021-10-20 16:32:00 -0400
image:
  path: https://og-image.xyz/og/GH Tik Tak Toe/Technical Discussion/blog.jackcrane.rocks/https/menlo/cheerfulorange/{{h}}ffffff/data.png
---

# Github readme tik-tak-toe

Powered by Cloudflare Workers, this is meant to be a tik-tak-toe game visitors to my GitHub Profile can play. Due to the limitations of GH being frontend, it works by requesting an image for that particular cell in a 3x3 table wrapped in a link that is also affiliated with the cell on the backend. This allows the game to be played from static sites around the internet. Unfortunately, GitHub's caching service breaks the images, even though I have been working on fixing caching issues on the backend.

I know what I am trying to do with dynamic images because of the existance of badges on GitHub readmes that often have to change quickly. I have reached out to GH support and am waiting to hear back.

Current player: 

<img src="https://gh-tik-tak-toe.jackcrane.workers.dev/current-player?escape-cache">

<table>
  <tr>
    <td>
      <a href="https://gh-tik-tak-toe.jackcrane.workers.dev/move/0">
        <img src="https://gh-tik-tak-toe.jackcrane.workers.dev/image/0?escape-cache">
      </a>
    </td>
    <td>
      <a href="https://gh-tik-tak-toe.jackcrane.workers.dev/move/1">
        <img src="https://gh-tik-tak-toe.jackcrane.workers.dev/image/1?escape-cache">
      </a>
    </td>
    <td>
      <a href="https://gh-tik-tak-toe.jackcrane.workers.dev/move/2">
        <img src="https://gh-tik-tak-toe.jackcrane.workers.dev/image/2?escape-cache">
      </a>
    </td>
  </tr>
  <tr>
    <td>
      <a href="https://gh-tik-tak-toe.jackcrane.workers.dev/move/3">
        <img src="https://gh-tik-tak-toe.jackcrane.workers.dev/image/3?escape-cache">
      </a>
    </td>
    <td>
      <a href="https://gh-tik-tak-toe.jackcrane.workers.dev/move/4">
        <img src="https://gh-tik-tak-toe.jackcrane.workers.dev/image/4?escape-cache">
      </a>
    </td>
    <td>
      <a href="https://gh-tik-tak-toe.jackcrane.workers.dev/move/5">
        <img src="https://gh-tik-tak-toe.jackcrane.workers.dev/image/5?escape-cache">
      </a>
    </td>
  </tr>
  <tr>
    <td>
      <a href="https://gh-tik-tak-toe.jackcrane.workers.dev/move/6">
        <img src="https://gh-tik-tak-toe.jackcrane.workers.dev/image/6?escape-cache">
      </a>
    </td>
    <td>
      <a href="https://gh-tik-tak-toe.jackcrane.workers.dev/move/7">
        <img src="https://gh-tik-tak-toe.jackcrane.workers.dev/image/7?escape-cache">
      </a>
    </td>
    <td>
      <a href="https://gh-tik-tak-toe.jackcrane.workers.dev/move/8">
        <img src="https://gh-tik-tak-toe.jackcrane.workers.dev/image/8?escape-cache">
      </a>
    </td>
  </tr>
</table>