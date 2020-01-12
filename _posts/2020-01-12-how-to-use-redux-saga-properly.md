---
title: How to use Redux properly [EN]
---

Let me introduce what [Redux](https://redux.js.org/) is. Ideally you always declare the variables in the same place they are going to be used. But sometimes it's convenient to declare a variable at the "application level". For example, let's say that we have a menu that can be opened from the application header, but those components are located at different places in the application hierarchy, so the header can't access the menu directly. This is a good reason to declare a global "menu state variable".

That's what Redux is. It's a system that guarantees the effective communication between different components of the application, and it consists of a collection of "state variables", also called `store`.

Now that we now what Redux is, let's talk about two important rules.

## Rule #1: Use Redux moderately

As I commented earlier, the source code is better understood if we declare "local variables". Also, different components may be using the same "global variables" for different purposes and that may lead to instability problems. That's the reason why declaring "global variables" is not a good idea, no matter if you are a front-end or a back-end developer.

So having one or two "global variables" is good. Having a lot of them is not good.

## Rule #2: There's no rule number two!

And that's all, folks!
