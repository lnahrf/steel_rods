---
title: "Are hackers watching your screen right now?"
seoTitle: "Are hackers watching your screen right now?"
seoDescription: "Learn Van Eck Phreaking and data protection against electromagnetic hacks on HackFM"
datePublished: Wed Apr 10 2024 19:07:27 GMT+0000 (Coordinated Universal Time)
cuid: cluu6mwjv000208lea4f19tiy
slug: are-hackers-watching-your-screen-right-now
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1712427998555/82e8a9f6-edee-44a4-be06-26b9891e0e5e.png
tags: technology, hacking, cybersecurity-1

---

It's a possibility.

I want to share with you one of the most sophisticated hacks I have ever come across. If you truly love technology, this is something you need to be aware of.

This extraordinary exploit exists in every single electronic device you own, and there is no reliable way to protect yourself from it completely.

"Van Eck Phreaking" is the concept of spying and gathering data by eavesdropping on electromagnetic fields produced by electronic devices.

In theory, it is possible to pick up every bit of data passing through every one of your electronic devices, without the need to physically access or handle the device.

Sounds scary? Yes, it is (but there is nothing to be afraid of).

This is not a science fiction novel or some loosely based theory, but rather a real hack that has been used successfully in the past.

Back in 1985, Wim van Eck published a technical analysis of the security risks involved in using computer monitors (hence the name, Van Eck Phreaking). Since then, (or maybe even before that) this method has been used with varying levels of success in private and military operations around the planet.

### How does it work?

Electronic devices are emitting electromagnetic radiation by nature. There is no possible way to avoid electromagnetic leakage while using an electronic device.

For example, a USB keyboard is connected to your computer's USB port. Every time you type a character - the electronic signal travels through the wire and into your computer's USB port, where it is then analyzed using multiple layers of logic and turned into the character you see on your screen.

While the signal is traveling through the wire, an electromagnetic field is created for a brief moment around the conductive wire. The movement of electrons generates a "force" field that expands outside the wire (depending on the wire and its insulation, the field can extend over long distances).

The electromagnetic field can be picked up by attackers, which will then attempt to reverse-engineer the signal and reconstruct the data passed through the wire. In theory, the same approach applies to every electronic device. every connection, whether wired, wireless, internal or external.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1712320934022/fabdcff4-a3d3-4ed6-ad0b-4ad61b40eaa5.png align="center")

"Van Eck Phreaking" takes advantage of the fact that many people aren't aware of this process. Some public examples of incredible spy operations went unnoticed for years using such methods.

### Insulation and mitigation

While there is no way to avoid electromagnetic leakage completely. Some concepts and standards aim to reduce the risk of eavesdropping significantly - such as the TEMPEST standards (learn more in the Quicklinks below).

My recommendation to reduce the risks of being attacked by such methods is to always pay the extra dollar when purchasing cables. This does not eliminate the risk, but proper insulation reduces electromagnetic leakage significantly, and may even make it impossible for an attacker to eavesdrop on your devices unless they are in the same room as you.

### What is realistically possible?

Using the right hardware and tools such as GNURadio (GR-Tempest) and TempestSDR, it is possible to get a live feed of an HDMI cable from up to 50~ meters away.

For my example, I used the cheapest SDR receiver, running on my Windows and Linux machines with the TempestSDR tool. I was able to capture the other machine's screen with minimal configuration on two different occasions.

![Using my windows machine and an SDR to capture my linux machine's screen ](https://cdn.hashnode.com/res/hashnode/image/upload/v1712425300465/f1bfc86f-d6f7-45d3-8b0c-8a9b1309eff4.png align="center")

![Using my windows machine and an SDR to capture the HDMI cable going to a second monitor](https://cdn.hashnode.com/res/hashnode/image/upload/v1712425671012/bbb56703-cafa-415f-b050-684c44dbb271.png align="center")

### Quicklinks

[Van Eck Phreaking on Wikipedia](https://en.wikipedia.org/wiki/Van_Eck_phreaking)

[GNURadio](https://www.gnuradio.org/)

[GR-Tempest](https://github.com/git-artes/gr-tempest)

[Tempest Standards](https://www.astrodynetdi.com/blog/tempest-emi-filters#:~:text=The%20TEMPEST%20standards%20mandate%20elements,the%20equipment%20and%20building%20pipes.)

[TempestSDR](https://github.com/martinmarinov/TempestSDR)

### Who am I?

My name is Lev, a self-proclaimed hacker and radio enthusiast, writing for my personal tech hub at [HackFM](https://hackfm.com/). I have been tinkering with computers for over a decade, and although I may not always be successful in putting them back together, I have gained some knowledge in the field. I currently work as a senior data engineer and I am always available for consulting or discussing anything tech-related. Want to write for [HackFM](https://hackfm.com/) or collaborate on a gritty technical article? Please let me know!