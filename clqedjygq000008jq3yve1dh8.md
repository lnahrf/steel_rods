---
title: "No Listicles! The chrome add-on that removes list-type articles from your DEV.TO feed"
datePublished: Wed Dec 20 2023 22:57:59 GMT+0000 (Coordinated Universal Time)
cuid: clqedjygq000008jq3yve1dh8
slug: the-chrome-add-on-that-removes-list-type-articles-from-your-devto-feed
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703112712186/bada7817-2b99-44d3-a915-8952fc3f04ef.png
tags: javascript, web-development, opensource, community

---

You probably knew this was coming sooner or later. [DEV.TO](http://DEV.TO) has been filled with listicles lately and something had to be done.

But before we continue, a short disclaimer.

> This add-on and article were created as a somewhat satirical view of the situation. While the tool is completely functional, I do not intend to hurt other authors' feelings, or bash anyone for posting "listicles".

### What is a "Listicle"?

A listicle (list article) is a low-quality, usually low-effort article that is a list. Listicles are often generated using some sort of AI, and while some might bring *some* value to readers, the vast majority of them are posted just for the sake of posting and not for the sake of teaching or passing on knowledge.

![](https://c.tenor.com/KfPxesRgzxAAAAAd/tenor.gif align="center")

Listicles are quite easy to make - ask any GPT for a hot topic in the development community and tell it to list X resources about said topic.

Voila~ You have a listicle that is ready to steal the show.

Personally (and I know many of you will agree with me), I find listicles quite useless and somewhat annoying. Their exaggerated titles, filled with emojis and buzzwords, aim to capture as much attention as possible while providing little to no value. Many authors have been sounding their opinions about listicles lately, and it feels like a good portion of the community is somewhat ticked off by the situation.

### The (temporary) solution

While I trust the DEV team to eventually do something about the low-quality content that is flooding the community. I wanted to stop combing through multiple listicles to find an article worth reading. The best solution I could think of was to create a quick Chrome add-on that identifies listicles and removes them from my feed using some kind of scoring system.

This was my first time creating a Chrome add-on, and it was an... interesting development experience (although somewhat unrefined, to say the least). I wrote the add-on in a day, and it works without using any sort of AI (I bet that is refreshing to hear).

![](https://raw.githubusercontent.com/lnahrf/no_listicles/master/popup/no_listicles_icon.png align="center")

I thought it would be fitting for the add-on icon to be a blocked party emoji icon. Not that I have anything against emojis or parties (both are fun) but it seems that listicle authors are using emojis as the primary hook that grabs your attention on the feed.

I did not publish the add-on to the Chrome webstore, because honestly I don't want to deal with that. But it is available on [GitHub](https://github.com/lnahrf/no_listicles) and you can add it to your local Chrome instance by enabling "developer mode" and loading the directory as an add-on. If you intend to use it and/or like the idea, a star on GitHub would be appreciated.

Let me know in the comments if you would like me to go through the Chrome webstore submission process and publish this.

### Completely arbitrary scoring system

The simplest method I could think of to determine which post was a listicle - was to run the article title through a set of tests and calculate a score based on the title structure. If an article's title's final score is over a pre-defined threshold, it must be a listicle.

### **It is not perfect, but it works**

To my surprise, it works quite well. The scoring system can identify over 90% of listicles with about 5-10% false positives.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703110630836/161aa6f1-b1ab-4d6a-a690-4adac7981cc5.png align="center")

From time to time an article that is not a listicle might get caught in the mix and removed from the feed.

The scoring system is partially built upon a blacklist of words associated with listicle-type articles. The goal with the blacklist was to try and keep things levelled, and not overdo it. You are more than welcome to improve the blacklist as you see fit, either in the repository or in your local fork/clone.

*An explanation of locating the blacklist in the source files can be found in the README file.*

### Quality content

At the end of the day, we all just want to read quality content, whether our definition of "quality" is an academic research paper or a list-type article. I am not pro-censorship, and I do not think listicles should be banned. They are just not to my taste when it comes to technical articles. That being said, if you want to clean up your feed off listicles like me, I may have found a temporary solution for you.

Happy reading!