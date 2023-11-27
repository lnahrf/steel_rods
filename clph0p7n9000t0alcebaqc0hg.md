---
title: "Why I keep choosing Javascript over Typescript, and why it is not ideal as a statically typed Javascript "alternative""
datePublished: Mon Nov 27 2023 14:41:45 GMT+0000 (Coordinated Universal Time)
cuid: clph0p7n9000t0alcebaqc0hg
slug: why-i-keep-choosing-javascript-over-typescript
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1701096048938/c133251f-a218-4feb-9ac1-ed29c588a2e6.jpeg
tags: javascript, web-development, typescript

---

[Typescript v5.3.2](https://www.npmjs.com/package/typescript) was published a few days ago - It has 42,000,000 (million) weekly downloads on NPM! It is truly amazing, and it says something good about the open-source web development community.

Now, don't get me wrong - Typescript had a vision, and that vision was spot on! It was created to bring static typing to Javascript - a language that will benefit from being statically typed.

Yes, I also think coding large applications without type safety is a bad idea. Earlier this year I spent a whole day working on a bug caused by an environment variable used as an integer that was dynamically typed as a string! I get it!

Typescript, however, is not the answer to the question that is "statically typed Javascript".

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701095950193/ae6977fe-6751-4f01-8d5b-71b2dfa02eec.jpeg align="center")

The first language I ever learned was statically typed. I began coding around 2010 and studied Java for almost 3 years. Learning to code in Java as a first language was a blessing - it is not as difficult and cumbersome as C or C++, yet it is not as "easygoing" and fluid as Python (or JavaScript).

While you are grasping the basic concepts of development for the first time, it is really easy to develop bad habits, or *"shoot yourself in the foot"* as they say - spending days debugging bugs that could have been easily solved by compilation-time type-checks.

### Typescript is not a language

![](https://media.tenor.com/80plcYSqPQsAAAAC/no-its-not-nope.gif align="center")

Many will say Typescript is a programming *language*, but is it? I know, I know - it's just semantics - but Typescript is not a language in its own right. Typescript is a superset of Javascript. A "superset" of a programming language is an extension that introduces new features, in Typescript's case - it is just a Javascript transpiler wrapped with additional features.

### Wtf does *"transpile"* mean?

It's a combination of "translate" and "compile" - Typescript "transpiles down" to Javascript.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701124357609/826d96fb-f6dc-4026-9847-80bc32acb91e.jpeg align="center")

Many developers I met throughout my career told me that Typescript is an amazing replacement or alternative to Javascript. At some point, I began to wonder whether they knew that Typescript's compilation target was, in fact, Javascript. If the output of your Typescript compilation is a .js source file - you are still using plain ol' JS!

Javascript *is* the programming language that operates behind the scenes when you run a .ts file. Essentially, everything that Typescript does is check your pre-configured types on "compilation time" (transpilation time?)

Typescript can even be configured to compile *with* type errors! I have seen this happen in so many commercial projects, not sure why. The only reason it can be configured to ignore errors is because our Typescript "types" have no meaning in Javascript (after compilation) - they don't do anything.

### A false sense of security

Let's say we defined the result of an HTTP call to be of type integer. We deployed our application to production and everything worked fine. One day, our backend developer decided to return the string representation of our integer instead of the integer itself. They deployed their code to production and forgot all about it.

*5 hours of build time later -* the application crashes, production is down, it's a Friday night and we have no idea what is going on.

Typescript gives us a false sense of security, because we, as Typescript developers, assume that the value of the result is an integer. However, in production, our type checks have no meaning. When we approach this critical bug and try to debug it, we will assume the type we receive from our backend is an integer, because we previously configured our code to accept an integer.

### Unit tests won't save production this time

![](https://media.tenor.com/MYZgsN2TDJAAAAAC/this-is.gif align="center")

Even if you write and rewrite unit tests every time you update your application (which is unlikely). Even if you did your due diligence and made sure your code was tested from all directions - your mocked response value is still configured as an integer. Your tests will pass, your application won't work, and you will have no idea why.

Wondering if I was the only one who thought about Typescript this way - I wanted to hear the opinion of a very good friend of mine, who worked at Intel for a couple of years as a full-stack developer and is now making way more money than me.

I asked him for his opinion about this controversial topic - I presented him with our production bug scenario, and he added the following:

> "Not only will unit tests fail to catch this bug, but even end-to-end tests won't give you a clear error of the origin (of this bug). It will most likely display a random stack trace where the code failed (if it failed at all), and you will still have to debug it thoroughly and trace it back to the original HTTP call."

### The *'Any*' conundrum

While Typescript can be alright to work with when not abused, the majority of projects I encountered over the years struggled with the 'any', or generic typing conundrum.

Sometimes, you just want to change one line of code instead of refactoring 16 modules to alter a type. Enters 'any' - a magical Typescript syntax that reverts any code block to plain old Javascript if not disabled entirely.

The problem is that if we are going to use 'any' in our project, what is the point of using Typescript at all? This has got me thinking that perhaps Typescript is not *the problem* - maybe the developers who use 'any' are the problem *with Typescript*.

### Why transpile when you can just run?

Javascript is a scripting language, that runs on a piece of software called a runtime (V8 engine). The engine can execute Javascript code without having to go through the process of compiling it down to machine code ahead of time. Javascript executes in real-time (runtime) using a JIT (just-in-time) compiler.

That is the major - and probably only upside to using a scripting language over a compiled language (scripting languages are significantly slower when it comes to performance, but they offer the convenience of rapid development).

When we use Typescript, we are essentially using a version of Javascript that has to be compiled down to Javascript. Not only that, but it also gives you a false sense of security and something to do on the weekend after your backend developer changed one specific type in a commit of 200+ changes.

### The language Typescript was supposed to be

There is a hidden bright side to all of this. There is still hope for a strong, statically typed language that is just as versatile as Javascript! it's not Typescript, it's ***Dart!*** Dart is a programming language designed by **Lars Bak and Kasper Lund** , and developed by Google.

Published in late 2013, Dart is primarily used with Google's UI toolkit - Flutter. And is a Frankenstein-like hybrid of Java (OOP) and Javascript, but even better.

It is statistically typed (with complete null safety) and can be compiled or interpreted (offers both AOT and JIT compilers). It can be compiled down to native machine code, and it offers asynchronous development using async/await syntax.

If you haven't tried Dart yet, I highly recommend it as a general-purpose language worth learning! (Flutter is a bonus).

Here is an example of the syntax similarity between the languages.

```javascript
// Javascript async/await
async function fetchData() {
    await sleep(2);
    console.log('Data fetched!');
}
```

```dart
// Dart async/await
Future<void> fetchData() async {
    await Future.delayed(Duration(seconds: 2));
    print('Data fetched!');
}
```

Dart is ***currently*** perceived as less versatile than Javascript - only because it is primarily used in Flutter (which is primarily used for mobile development). But there is no reason Dart cannot be widely used, for the front end and back end, just like Javascript, Typescript and Node.js.

In reality, Dart is more versatile than JavaScript and is incredibly easy to learn if you have experience with OOP, Java, JavaScript or Typescript.

### But what about Typescript?

![](https://media.tenor.com/QrLAg4sFWHAAAAAC/its-ok-evan-morrow.gif align="center")

Don't get me wrong, Typescript has its place in the web development world, but it should be treated as what it is - a wrapper for JavaScript, with significant downsides. It is not an optimal solution, and the issue of quirky, dynamically typed JavaScript remains unresolved.

I encourage you to challenge the norms, not give in to trends and hype, and find out for yourself whether a particular technology lives up to its name, or not. Also, remember that opinions about software will come and go, they are just opinions - and they don't matter much. At the end of the day, you should use the tools you enjoy using, to complete the task.

Happy hacking!