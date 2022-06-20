---
layout: post
title: "Apple Music Presence: Technical Discussion"
date: 2022-05-02 11:44:36 -0400
image:
  path: https://og-image.xyz/og/Apple Music Presence/Technical Discussion/blog.jackcrane.rocks/https/menlo/cheerfulorange/{{h}}ffffff/data.png
---

![Promotional banner](/images/amp.png)

## The Idea

I was about sick of my friends who use Spotify having the song, artist, and album cover of the song they are listening to displayed on Discord and us Apple Music fans not having the same feature. After a little research, I discovered AppleScript, a seemingly forgotten-about language used on MacOS to interact with apps, and it allowed me to pull information on the current track from Apple Music. It then uploads the cover art to a S3 bucket (because you can only submit URLs to Discord) and finally sends everything to Discord.

## The tech stack

- Node.js
- DigitalOcean S3 bucket for storing photos
- AppleScript

## Community Involvement

I have no idea how many people use this app, but what I can say is that the GitHub repo has 26 stars at the moment, I have recieved $40 in donations, and the s3 bucket saw 7,000 new album cover images last month.

<script data-name="BMC-Widget" data-cfasync="false" src="https://cdnjs.buymeacoffee.com/1.0.0/widget.prod.min.js" data-id="jackcrane" data-description="Support me on Buy me a coffee!" data-message="Feeling generous?" data-color="#FFDD00" data-position="Right" data-x_margin="18" data-y_margin="18"></script>
