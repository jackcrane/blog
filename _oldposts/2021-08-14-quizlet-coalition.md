---
layout: post
title:  "Quizlet Coalition: Postmortem"
date:   2021-08-14 11:44:36 -0400
image:
  path: https://og-image.xyz/og/Quizlet Coalition/postmortem/blog.jackcrane.rocks/https/menlo/cheerfulorange/{{h}}ffffff/data.png
nav_exclude: true
---

![Quizlet Coalition Screenshot](https://jackcrane.rocks/images/qc.png)

## The Idea

I was peeved with the difficulty of finding high quality quizlets for my classes, especially when dealing with teachers with common names (i.e. *Smith*). Quizlet's own search engine was typically pretty decent, but I was always looking for some more advanced searching tools that were better implemented than Quizlet's classes feature. I decided to venture into building my own app.

## The Features (or planned features)

- [x] Attractive 2-side homepage
- [x] Signup page, complete with a web scraping system to fetch additional information about the study set from Quizlet's site
- [x] A search engine with advanced filters for teachers, classes, schools, and assignments
- [ ] A publically accessable API
- [ ] Advertisement implementation

## The tech stack

Frontend:

- HTML
- JavaScript
- CSS

Backend:

- PHP
- Python (BeautifulSoup)
- MySQL
- DigitalOcean Droplet

## What happened

After working hard on Quizlet Coalition (QC) for a few weeks, I set off to begin marketing. After a few lazy reddit posts, I had acquired 3 returning users and about 30 hits on the website. Reasonably satisfied with the performance of my reddit posts, I continued to some more careful and targeted marketing which yieled a gigantic zero on all metrics. After several more weeks of working on the tech, I had nothing to show for it. With the cloud VM costing me money with aboslutely no return, I made the decision to shut down the project.

It was an interesting project to work on. It was my first real exploration into building a web crawler, and I did not yet know about any of the super cool frameworks that make web scraping easier (like puppeteer), but I learned about parsing DOM on the backend and extracting further information from headers and other clues passed to the browser.

Although I cannot take full credit for the design of QC, I am proud of the work done on it. It was my first exploration into the CSS flexbox system, which was daunting at the time but after a project of real-world use, I discovered how powerful it is and how much it can simplify the process of building a responsive, modern website.

I learned so much more than just flexbox and some design skills, and unfortunately my "learning opportunities" resulted in so much technical debt that, even if I was able to get this project off the ground, it would not have been a success as its instability would have destroyed the ux before any users could really experience the app. For starters, I did not use any kind of version control (GitHub is now a standard on all my projects), instead I opted to live edit in a simple DigitalOcean droplet over SSH, meaning anything I changed would change in production instantly, with absolutely no infrastructure in place for testing or QA. I also overcomplicated the project by integrating python AND PHP on the backend, passing information back and forth over a command shell with no security provisions in place.

Additionally, when I made this I did not konw any frameworks (like React). While on small projects this is of trivial importance, if I ever wanted this project to scale I would have to rewrite the entire site to switch to a framework like React driven by an API rather than server-side PHP rendered HTML.

When I ultimately shut down this project several years ago, I knew none of this. I attributed the technical failure to my choice of tech stack, and the commercial failure to my lack of marketing effort and competency. I decided to stop using PHP because of this project. Although there is aboslutely nothing wrong with PHP, I switched to node.js (express and recently Next.js), and have simply never looked back.

## GitHub

[jackcrane/quizlet_coalition](https://github.com/jackcrane/quizlet_coalition)

<script data-name="BMC-Widget" data-cfasync="false" src="https://cdnjs.buymeacoffee.com/1.0.0/widget.prod.min.js" data-id="jackcrane" data-description="Support me on Buy me a coffee!" data-message="Feeling generous?" data-color="#FFDD00" data-position="Right" data-x_margin="18" data-y_margin="18"></script>