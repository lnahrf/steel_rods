---
title: "AI, but at what cost? The energy-inefficient AI era is already here"
datePublished: Thu Mar 28 2024 19:50:50 GMT+0000 (Coordinated Universal Time)
cuid: clubngmdw000h08l322w3b312
slug: ai-but-at-what-cost-the-energy-inefficient-ai-era-is-already-here
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/64-xGOdUEnU/upload/d0400ad62add92516aa9ff2f208ccad9.jpeg
tags: ai, artificial-intelligence, web-development, machine-learning

---

Artificial intelligence is exciting, the fresh technological breakthrough that took over our lives in less than a year, hasn't stopped since.

Personally, I have some issues with AI tools and how they are being used at the moment. With that, I cannot deny that they are a mighty instrument when given to the right person to do the right job - but as we all know, with great power comes great responsibility.

![](https://c.tenor.com/RzzxRsZg-eoAAAAd/tenor.gif align="center")

Amidst the excitement and semi-fulfilled promises - did we ever stop to think about the cost of running 1,2,3 billion parameter models for hours on end, scaled across multiple instances, for millions of users, all at once?

I was always an advocate of the "minimal web" concept (I will write an article about the concept in the future, there is a lot to say). We live in an era where on one side, everyone seems extremely concerned about the health of the planet (and for good reason). Yet on the other hand, no one is stopping to think how much the internet in its current state is costing us in terms of raw energy. Is it efficient? Is it green? far from it.

### Traffic and energy

A while ago, I wrote a tiny TUI tool using Perl ([webmeter](https://github.com/lnahrf/webmeter)), that monitors my internet traffic and aggregates the byte-length of packets that are sent and received while the tool is running.

Just by opening Reddit's front page, my tool logged a whopping 5.85 megabytes of data transferred between my interface and Reddit's servers.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1711652371744/70b4c3c5-3eb9-46e8-af06-261e3567373d.png align="center")

Reddit as a website is a combination of text, low-to-mid-quality images, and the occasional video. It seems extremely unreasonable that just by opening Reddit's front page I transferred so much data across the planet (auto-playing videos were off during testing, this result excludes video content).

Reddit's front page, a page that could have concluded with 10 kilobytes of content, javascript, and CSS (perhaps a few more kilobytes for additional images), needed 300 times the bandwidth just to load up.

In comparison (considering the moon landing did not take place in a Hollywood basement), the software written for the Apollo 11 mission was only 72 kilobytes in size.

Just for the kick of it, imagine billions of people, opening Reddit's front page, all together, at every moment, all the time. Try and think: what is the direct cost of transferring the data? what is the secondary cost? How much energy are we wasting as internet-consuming beings?

This concept has been dwelling in my mind for the past year or so, which was the main reason I hacked together my Perl tool in the first place. One "minimal web" thought led to another, and I realized that we are so consumed by the hype (or hate) that we are ignoring the cost of our artificial intelligence endeavors.

### How can we calculate the energy cost of AI as a service (or stable diffusion for example)?

Accurately? We can't. Not without knowing how these services operate: which CPUs, GPUs (or dedicated chips) they are using. How many instances are they running, what is their load, what is the exact power consumption of the critical hardware, etc.

But we can come to a pretty realistic (although not as accurate) conclusion if we put our minds to it. I chose [Fooocus](https://github.com/lllyasviel/Fooocus) for this example, which is the most straightforward (and I believe popular) stable diffusion GUI out there. Let's start simple:

Using Fooocus, it took my old-timer GPU (a Quadro P2000) about 10 minutes to generate an image. According to the performance table on Fooocus's repository, we know that using an Nvidia RTX 4XXX GPU would give us the fastest result. My Quadro is somewhat comparable to an Nvidia GTX 1060, therefore we know that using an Nvidia GTX 1060 GPU, we can generate an image in approximately 10 minutes.

After some searching online, I concluded that the most commonly used (enterprise) GPU for AI/ML is the Nvidia A100 (up to 400W under load).

![](https://c.tenor.com/5vo_w_jDfwgAAAAC/tenor.gif align="center")

The Nvidia A100 is a massively more powerful GPU than the Nvidia GTX 1060, it is also built specifically for AI and ML. Therefore let's assume, that generating an image using an Nvidia A100 will take 5 seconds (if anyone reading this has accurate data about the A100 and its performance, please share).

Using the formula E = P \* T, an Nvidia A100 GPU power consumption under load for 5 seconds costs around 0.5 watt-hours.

Adding on to this example: Midjourney (or other AI image generation services) generates 4 images per prompt. Let's assume the service is using an array of Nvidia A100s to generate the images, which means that the service is probably wasting around 2 watt-hours of energy for each prompt, for each user.

As of November 2023, Midjourney has 2.5 million DAUs.

```plaintext
2Wh x 2,500,000 Users x 24 Hours = 120,000,000Wh (per day, considering every DAU is executing a prompt per hour, in reality, it is much more than that).
120,000,000Wh = 120,000MWh
```

120,000 Mega watt hours could potentially power over 100,000 households for a whole month.

Modestly, services such as Midjourney, Dall-E, or even self-hosted solutions such as Fooocus, waste at the very least 50 million watt-hours per day, each (considering their GPUs are state-of-the-art and each DAU uses the service for a few hours every day).

***This amount of energy could potentially power over 100,000 average-sized households for a month, per day.***

Keep in mind we only talked about image generation services, let's not forget about all other services, such as ChatGPT, Gemini, Bard, and all of their versions and flavors. The list, and waste, goes on.

### What should we do?

How should we, as developers, come together to address this emerging issue? How can we take measures to educate others to use energy responsibly? I would love to hear your thoughts about this topic and its possible solutions in the comments below.