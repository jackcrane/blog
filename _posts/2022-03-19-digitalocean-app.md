---
layout: post
title: "7 Sigma Robotics: Technical Discussion"
date: 2022-03-19 11:44:36 -0400
image:
  path: https://og-image.xyz/og/DigitalOcean App/Technical Discussion/blog.jackcrane.rocks/https/menlo/cheerfulorange/{{h}}ffffff/data.png
---

![Promotional banner](/images/main-do-ph.png)

## The Idea

I was frustrated with the lack of a high quality mobile app to use to control my DigitalOcean products. The official DigitalOcean web dashboard is not mobile friendly, and the mobile apps out there are either extremely difficult to use, not feature rich, or expensive.

## The Features so far

💵 Current billing
🏦 Historical invoice viewing
💧 Droplet information view
📈 Droplet CPU performance information
🏗 Creating a droplet
📱 Viewing information for Apps

This is far from a full complement to DigitalOcean's products, so here is my current roadmap:

⚙️ Change 'account' link to 'settings' include more settings for the app
📊 Database listing and editing. This will require some thinking and work. Design suggestions are appreciated!
🚏 DNS Service support, adding, editing, and deleting entries
📶 Networking setup with Load Balancers and firewalls
💿 Add Spaces support, including uploading, downloading, and sharing files
⛴ Add Kubernetes support with link to the Kube dashboard (?) if possible
🤖 Make availible on Android
💦 Implement disk, memory, and networking monitoring for Droplets
📲 Adding a button that allows the creation of apps
❗️ Improving netcode error handling

## The tech stack

- React Native
- DigitalOcean API
- Cloudflare Workers

## Feedback

Please reach out and let me know what you think! email jack@jackcrane.rocks

## Get it on Testflight

[https://testflight.apple.com/join/TWoGwe16](https://testflight.apple.com/join/TWoGwe16)

<script data-name="BMC-Widget" data-cfasync="false" src="https://cdnjs.buymeacoffee.com/1.0.0/widget.prod.min.js" data-id="jackcrane" data-description="Support me on Buy me a coffee!" data-message="Feeling generous?" data-color="#FFDD00" data-position="Right" data-x_margin="18" data-y_margin="18"></script>
