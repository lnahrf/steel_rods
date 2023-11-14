---
title: "Javascript Proxy Magic: How I built a 2kB state manager with zero dependencies (and how it got me 2 different job offers)"
datePublished: Tue Nov 14 2023 23:20:25 GMT+0000 (Coordinated Universal Time)
cuid: cloyyi4uq000209joda6u74fb
slug: javascript-proxy-magic-how-i-built-a-2kb-state-manager
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1700003060508/20b07335-65f1-40ba-892d-458b541d5ecb.jpeg
tags: javascript, web-development, redux, jobs

---

What is a state manager exactly? A state manager is a smart module that is capable of retaining session data (of an application or web application) and reacting to changes in the data.

Are you a web developer? Ever used libraries such as Redux, Mobx or Zustand? Congratulations! You have used a state manager.

I remember my first days trying to set up (the old) Redux for React. I get PTSD just thinking about all the unnecessary complications - dispatchers, reducers, middleware! I just wanted to declare some variables, *please make it stop*.

![](https://media.tenor.com/Fj8YV_9ut8UAAAAC/makeitstop-i-just-want-it-to-stop.gif align="center")

It was an over-engineered, bloated library that EVERYBODY used! And for some crazy, unknown reason, it was the industry standard back then.

### Some backstory

One night back in 2021 when I could not fall asleep, I aimlessly opened GitHub and noticed my old college course teacher (which I followed on GH) uploaded an assignment for his current students. The assignment required the students to create a Pokedex website using a public Pokémon API. The goal was to implement it in Javascript (without frameworks or libraries because his current students were web-dev beginners who were still learning the basics of Javascript and development).

As a joke, and mainly because I couldn’t sleep, I started working on my Pokémon website. Eventually, I was able to create something viable, without using any external libraries.

### But along the way, I struggled...

You see, I was so used to having a state manager, that the requirement of building even a simple, two-page application without using external frameworks or libraries got me thinking - *Why do state managers have to be so complicated? It's just variables and events.*

Long story short, I found myself hacking together a super-simple state manager module at 2 AM, just to manage the state of my Pokémon web app. I deployed my site to GitHub pages and forgot all about it.

A few months went by, but for some reason, I kept thinking about my state management solution now and then... You see, it had something other libraries didn't have - *IT WAS BRAIN DEAD SIMPLE.*

*"Hey!" I thought to myself, "I should rewrite it as an NPM package".*

That same night, it was exactly what I did - I wrote it as an independent NPM package. At the end of it all, it weighed 2kB (compared to Redux's 150kB), had zero dependencies and was so dead simple to use, that you could have set it up in just 3 lines of code.

### I called it VSSM

Stands for ***Very Small State Manager***.

You can check out the source code on [GitHub](https://github.com/lnahrf/Vssm). Also, take a look at the [documentation site](https://lnahrf.github.io/Vssm-docs/) that is built with React and VSSM.

The following day, I published my NPM package, and once again forgot all about it.

Later that same year, I interviewed for two different companies for a full-stack developer role. I aced the interviews of the first company, which was a very established tech company. As a part of the interview process, they asked me to tell them about whether I code in my free time or if there are any open source projects I contributed to and so on.

The only cool thing I did back then was VSSM, so I told them about it. They were impressed with the idea that I built a "Redux alternative" all on my own.

On the other hand, I failed miserably in the second company's interview. I had blackouts, I was nervous and could not answer simple questions such as

> "Does React re-render the entire application upon state changes, or does it only update the affected components and their children when using Redux?"

"It re-renders the whole app every time the state is updated", I said.

![](https://media.tenor.com/ZFc20z8DItkAAAAd/facepalm-really.gif align="center")

I was very nervous, haha, obviously I knew the correct answer was "It only renders the registered components and possibly their affected children".

To this day I have no idea why company No. 2 decided to give me a second chance. They invited me over for another interview (YES!).

During my second interview, they asked me to tell them about whether I code in my free time, open source contributions, you know the drill. The interviewer looked very pleased when I told him about my little side project, it seemed he liked me just for the fact that I coded a state manager from scratch.

I guess that was the case because I also failed the second interview (ran out of time during the coding challenge) but still got an offer. Company No. 1 intended to send me an offer but I had already signed my offer with Company No. 2.

My bottom line is - the fact I built VSSM helped me get both offers.

![](https://media.tenor.com/BuoCYXAkk0AAAAAC/big-lebowski.gif align="center")

### How did I do it?

Did you know that Javascript has all the functionality needed to monitor variable changes already built in?

It's called a Proxy (and it's magic).

A Javascript proxy is an additional layer of logic between the code and the assignment of variables.

If you were to wrap an object in a proxy, you could decide to log its values to the console every time it is updated, without doing anything but assigning a new value to the object.

```javascript
const target = {
  v: "hello"
}

const proxyTarget = new Proxy(target, {
  set: (target, property, value) => {
  
  	console.log(`${property} is now ${value}`); 
    
    target[property] = value;
    return target[property];
  }
});

proxyTarget.v = "world!" // v is now world!
```

VSSM is built on proxies, it creates a layer between variable assignment and the rest of the code. Using a Proxy, you can set setters, getters and implement any sort of logic whenever the value of the target is manipulated or requested.

VSSM is a bit more than just a proxy, it's an assortment of intelligent proxies that know whether the value you assigned to the variable is its new value or a callback method.

For example, using VSSM you can set a state, listen to changes and emit an event with just a few lines of code.

```javascript
import { createVSSM, createState } from 'vssm';
import { getVSSM } from 'vssm';

// Create the initial state
createVSSM({
  user: createState('user', {
    address: ''
  })
});

// Get the user proxy reference
const { user } = getVSSM();

// Listen to events on user.address
user.address = () => {
    console.log(`Address updated! the new address is ${user.address}`);
};

// Emit the mutation event
user.address = 'P.Sherman 42 Wallaby Way, Sydney'
```

As you can see, I made sure my state manager was as simple as they could be. My goal was to get rid of having to wrap your head around reducers, middlewares and extremely complicated configurations just to assign some variables.

Now, everything works by assigning variables! want to set up a listener? Assign a callback function to the variable. Want to edit the value and emit an event? Just assign a new value.

To this day I don't understand why popular state managers have to be so complex, maybe I never will.

I encourage you to go ahead and read all about Javascript Proxies on [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy).

### What is the conclusion of all of this?

Passion for what you do is key, I think.

I built VSSM just to push my limits and publish a reasonable NPM package. It managed to impress interviewers and peers and got me into different positions ever since.

Nobody will ever use VSSM, it will not be popular. I was aware of this fact when I published it to NPM. Yet I still chose to do it to the best of my abilities, because I had a passion for doing something that I conceive is better than the industry standard. I knew I could make something that had to be better, even if it meant it was just better for me.

Even though VSSM is lying dead in the NPM graveyard, it brought me a lot of value and is continuing to do so because of this article.

The best way to get a development job is to build amazing things, even if you think it all has been done before - build it better. Even if you think no one will ever use it, what is the point? - Build it now, value will come later.

Do not underestimate your abilities, if you believe they are lacking, know that you will improve. Go out there and build projects that bring value, one small step at a time.

Good luck on your engineering journey.