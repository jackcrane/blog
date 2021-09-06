---
layout: post
title:  "Building an Open Graph Image Generator"
date:   2021-08-21 11:44:36 -0400
image:
  path: https://og-image.xyz/og/OG Image Generator/Building your own/blog.jackcrane.rocks/https/menlo/cheerfulorange/{{h}}ffffff/data.png
---

## Introduction

If you don't know what Open Graph is, it is a powerful set of meta tags that can be included in a website that allows rich data to be displayed on social media platforms and search engines. [Learn more about Open Graph](https://ogp.me/).

This project is inspired by Vercel's [similar project](https://og-image.vercel.app/).

## Steps

- [ ] Install prerequisites
- [ ] Create our Express app
- [ ] Set up our image generation

## The tech stack

- Node.js
  - Express

## Installing Prerequisites

First, we need to create an NPM project:

```
npm init
```

We need Express and Canvas:

```bash
npm i express canvas
```

## Getting Started writing code

In `index.js`, we need to import the prerequisites we just installed:

```javascript
const express = require('express');
const { createCanvas } = require('canvas');

const app = express();
```

And set up our express server:

```javascript
app.get('/og/:title/:subtitle', (req, res) => {
  const buffer = generateImage({
    title: req.params.title,
    subtitle: req.params.subtitle
  });

  res.writeHead(200, {
    'Content-Type': 'image/png',
    'Content-Length': buffer.length
  });

  res.end(buffer);
})

app.listen(80, () => {
  console.log('App listening on localhost:80');
})
```

Let's generate the images

```javascript
const generateImage = (data) => {
  // Create and configure the canvas:
  const width = 1200;
  const height = 627;
  const canvas = createCanvas(width, height);
  const context = canvas.getContext('2d');

  // Set a black background:
  context.fillStyle = 'black';
  context.fillRect(0, 0, width, height);

  // Set a white border:
  context.strokeStyle = 'white';
  context.lineWidth = 5;
  context.beginPath();
  context.rect(10, 10, width - 20, height - 20)
  context.stroke();

  // Draw the title:
  context.font = `bold 70pt arial`;
  context.fillStyle = 'white';
  context.fillText(data.title, 50, 170);

  // Draw the subtitle:
  context.font = `bold 40pt arial`;
  context.fillStyle = 'white';
  context.fillText(data.subtitle, 50, 170);

  // Render and return our image:
  const buffer = canvas.toBuffer('image/png');

  return buffer;
}
```

## Lets give it a shot!

In your browser, visit [localhost/og/title/subtitle](localhost/og/title/subtitle) and see if you got what you are looking for!

## Full Code
```javascript
const express = require('express');
const { createCanvas } = require('canvas');

const app = express();

const generateImage = (data) => {
  // Create and configure the canvas:
  const width = 1200;
  const height = 627;
  const canvas = createCanvas(width, height);
  const context = canvas.getContext('2d');

  // Set a black background:
  context.fillStyle = 'black';
  context.fillRect(0, 0, width, height);

  // Set a white border:
  context.strokeStyle = 'white';
  context.lineWidth = 5;
  context.beginPath();
  context.rect(10, 10, width - 20, height - 20)
  context.stroke();

  // Draw the title:
  context.font = `bold 70pt arial`;
  context.fillStyle = 'white';
  context.fillText(data.title, 50, 170);

  // Draw the subtitle:
  context.font = `bold 40pt arial`;
  context.fillStyle = 'white';
  context.fillText(data.subtitle, 50, 170);

  // Render and return our image:
  const buffer = canvas.toBuffer('image/png');

  return buffer;
}

app.get('/og/:title/:subtitle', (req, res) => {
  const buffer = generateImage({
    title: req.params.title,
    subtitle: req.params.subtitle
  });

  res.writeHead(200, {
    'Content-Type': 'image/png',
    'Content-Length': buffer.length
  });

  res.end(buffer);
})

app.listen(80, () => {
  console.log('App listening on localhost:80');
})
```

<script data-name="BMC-Widget" data-cfasync="false" src="https://cdnjs.buymeacoffee.com/1.0.0/widget.prod.min.js" data-id="jackcrane" data-description="Support me on Buy me a coffee!" data-message="Feeling generous?" data-color="#FFDD00" data-position="Right" data-x_margin="18" data-y_margin="18"></script>