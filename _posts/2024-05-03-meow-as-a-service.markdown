---
title: "MaaS: Meows as a Service"
layout: post
date: 2024-05-03 22:44
image: /assets/images/markdown.jpg
headerImage: false
tag:
- interactive narratives
- software development
- memes
- startups
star: true
category: blog
projects: false
author: dario
description: Dial-a-meow Services
---

## Awhile ago I had an idea...

Coffeeshops always have a board with a bunch of business cards pinned onto it. They are always an interesting way to see the local businesses in your community. Usually they are pretty mundane but sometimes you find some unusual or interesting ones. Currently I don't have any business cards for myself to post around town but for some reason I really had the desire to have something to post. Maybe not a business of my own, but for someone else who really needed to start pulling their own weight around the house.

So without ado, introducing my latest bootstrapped startup cofounder:

![Business card for cats](/assets/images/cat/businesscard.png)

![Business card for cats](/assets/images/cat/businesscard2.png)

500 cards for $20 at [https://hotcards.com/](https://hotcards.com/) was a great deal for basically an unlimited source of meme material that I can spread to any coffeeshop I go to. On the back are QR codes to Coco's [Linkedin](https://www.linkedin.com/in/coco-cat-423a132a9/) and [website](https://meowmeowmeowmeowmeow.com/). As it turns out [meow.com](https://meow.com/), [meowmeow.com](https://meowmeow.com/), and [meowmeowmeow.com](https://meowmeowmeow.com/) were all taken so I had to settle for what I could get.

![Business card for cats](/assets/images/cat/businesscard3.jpg)

The phone number (724) COCO-CAT also needed a memeable voicemail system for our Dial-A-Meow service. I've used [TossableDigits.com](https://www.tossabledigits.com/) in the past but [Twilio](https://www.twilio.com/en-us) is the real deal for setting up programmable voice for phone numbers. Was able to quickly set up and deploy a Flask server with the [Serverless Framework](https://www.serverless.com/) and have it respond to calls at that number. Source code at: [Meow-as-a-Service](https://github.com/mcoirad/meow_as_a_service). It was easy enough I am even considering creating a video game you play just by calling a number.

So far all this has been worth the effort. I was even thinking about how to automatically set up 'free 15 minute business consultations' with Zoom calls. I don't know if I will get to that but it would be funny.





