---
layout: post
title: "Paddlefest App: Technical Discussion"
date: 2022-04-22 11:44:36 -0400
image:
  path: https://og-image.xyz/og/Paddlefest App/Technical Discussion/blog.jackcrane.rocks/https/menlo/cheerfulorange/{{h}}ffffff/data.png
---

![Promotional banner](/images/paddlefest.png)

## The Idea

Paddlefest is an event on the Ohio River where we put thousands of canoes and kayaks on the river. It is a fundraiser for Outdoor Adventure Crew, and I am a member of the executive board for the event. Many large-scale events like this feature mobile apps allowing attendees to interact with the event and so the event can push urgent updates.

## Features

- Paddlefest attendees are able to access a calendar of all the events that are happening both for the riverfront festivities and the acutal paddle
- Paddlefest is able to send notification blasts to all attendees, helping drive engagement and improve safety and efficiency
- A live photo feed allows participants to share their photos with the event and other eventgoers. All photos are filtered on the backend and are stored on an aws s3 bucket.
- A map is availible that shows the location of each booth, important event, and upcoming activties.
- Volunteers are able to access a hidden page that provides them with more information on their volunteer roles and who to contact for help.

## The tech stack

- React Native (Expo)
- Node.JS (Express) server
- Next.js (React) admin panel
- AWS S3 bucket for storing photos
- Backend is hosted on DigitalOcean Kubernetes

<script data-name="BMC-Widget" data-cfasync="false" src="https://cdnjs.buymeacoffee.com/1.0.0/widget.prod.min.js" data-id="jackcrane" data-description="Support me on Buy me a coffee!" data-message="Feeling generous?" data-color="#FFDD00" data-position="Right" data-x_margin="18" data-y_margin="18"></script>
