---
title: "AI, but at what cost? The energy-inefficient AI era is already here"
datePublished: Thu Mar 28 2024 19:50:50 GMT+0000 (Coordinated Universal Time)
cuid: clubngmdw000h08l322w3b312
slug: ai-but-at-what-cost-the-energy-inefficient-ai-era-is-already-here
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1711655369848/d6c6b41b-c5f3-4244-a57a-07d4a7e0c28b.png
tags: ai, artificial-intelligence, web-development, machine-learning

---

Artificial intelligence is exciting, the fresh technological breakthrough that took over our lives in less than a year hasn't stopped since.

While personally, I have some issues with AI tools and how they are being used currently, I cannot deny that they are a mighty instrument when given to the right person to do the right job - but as we all know, with great power comes great responsibility.

![](https://c.tenor.com/PUe-3C77JOEAAAAC/tenor.gif align="center")

Amidst the excitement and semi-fulfilled promises - did we ever stop to think about the cost of running 1,2,3 billion parameter models for hours on end, scaled across multiple instances, for millions of users, all at once?

I was always an advocate of the "minimal web" concept (I will write an article about the concept in the future, there is a lot to say). We live in an era where on one side, everyone seems extremely concerned about the health of the planet (and for good reason). But on the other hand, no one is stopping to think how much the internet in its current state is costing us in terms of energy. Is it efficient? Is it green? far from it.

### Traffic and energy

A while ago, I wrote a tiny TUI tool using Perl ([webmeter](https://github.com/lnahrf/webmeter)), that monitors my web traffic and aggregates the byte-length of packets that are sent and received while the tool is running.

Just by opening Reddit's front page, my tool logged a whopping 5.85 megabytes of data transferred between my interface and Reddit's servers.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1711652371744/70b4c3c5-3eb9-46e8-af06-261e3567373d.png align="center")

Reddit as a website is a combination of text, low-to-mid-quality images, and the occasional video. It seems extremely unreasonable that just by opening Reddit's front page I transferred so much data across the planet (auto-playing videos were off during testing, this result excludes any kind of video content).

Reddit's front page, a page that could have concluded with 10 kilobytes of content, javascript, and CSS (perhaps a few more kb for images), needed 300 times the bandwidth just to load up.

In comparison (considering the moon landing did not take place in a Hollywood basement) the software written for the Apollo 11 mission was only 72 kilobytes in size.

Just for the kick of it, imagine billions of people, opening Reddit's front page, all together, at every moment, all the time. Try to think: what is the direct cost of transferring all the data? what is the secondary cost? Just how much energy are we wasting as internet-consuming beings?

This concept has been dwelling in my mind for the past year or so, which was the main reason I hacked together my Perl tool in the first place. One "minimal web" thought led to another, and I realized that we are so consumed in the hype (or hate) that we are ignoring the cost of our Artificial Intelligence endeavors.

### How can we possibly calculate the energy cost of AI as a service (or stable diffusion for example)?

Accurately? We can't. Not without knowing all the data: which CPUs, GPUs (or dedicated chips) every service uses. How many instances are they running, what is their load, what is the exact power consumption of the critical hardware, etc.

But we can come to a pretty realistic (although not as accurate) conclusion if we put our minds to it. I chose [Fooocus](https://github.com/lllyasviel/Fooocus) for this example, which is the most straightforward (and I believe popular) stable diffusion GUI out there. Let's start simple:

Using Fooocus, it took my old-timer GPU (a Quadro P2000) about 10 minutes to generate 1 image. According to the performance table on Fooocus's repository, we know that using an Nvidia RTX 4XXX GPU would give us the fastest result. My Quadro is somewhat comparable to an Nvidia GTX 1060, therefore we know that using an Nvidia GTX 1060 GPU, we can generate an image in approximately 10 minutes.

After some searching online, I concluded that the most commonly used enterprise GPU for AI purposes is the Nvidia A100 (up to 400W power consumption).

![](https://c.tenor.com/5vo_w_jDfwgAAAAC/tenor.gif align="center")

Nvidia A100 is massively more powerful (and built specifically for AI purposes) than an Nvidia GTX 1060. Therefore let's assume, that generating 1 image using an Nvidia A100 at a high load will take me 5 seconds (if anyone reading this has accurate data about the A100 and its performance, please share).

Using the formula E = P \* T, an Nvidia A100 GPU power consumption on a high load for 5 seconds costs around 0.5 watt-hours.

Adding on to this example: Midjourney (or other AI image generation as a service) generates 4 images for every prompt. Let's assume the service is using an array of Nvidia A100s to generate the images, which means that the service is probably wasting around 2 watt-hours of energy for each prompt, for each user.

As of November 2023, Midjourney has 2.5 million DAUs.

```plaintext
2Wh x 2,500,000 Users x 24 Hours = 120,000,000Wh (per day).
```

Modestly, services such as Midjourney, Dall-E, or even self-hosted solutions such as Fooocus, waste at least 50 million watt-hours per day, each (considering their GPUs are the most efficient and each DAU uses the service for a few hours every day).

***This amount of energy could potentially power over 2,000 average-sized households per day.***

Keep in mind we are only talking about image generation as a service, let's not forget about all other AI services, such as ChatGPT, Gemini, Bard, and all of their versions. The list, and waste, goes on.

### What should we do?

How should we, as developers, come together to address this emerging issue? How can we take measures to educate others to use energy responsibly? I would love to hear your thoughts about this topic and its possible solutions in the comments below.