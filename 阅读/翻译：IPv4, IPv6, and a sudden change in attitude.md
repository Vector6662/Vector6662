# IPv4, IPv6, and a sudden change in attitude

[Avery Pennarun](https://twitter.com/apenwarr) on July 8, 2020



A few years ago I wrote [*The World in Which IPv6 was a Good Design*](https://apenwarr.ca/log/20170810). I’m still pretty proud of that article, but I thought I should update it a bit.

No, I’m not switching sides. IPv6 is just as far away from universal adoption, or being a “good design” for our world, as it was three years ago. But since then I co-founded a company that turned out to be accidentally based on the principles I outlined in that article. Or rather, from turning those principles upside-down.

In that article, I explored the overall history of networking and the considerations that led to IPv6. I’m not going to cover that ground again. Instead, I want to talk about attitude.

### Internets, Interoperability, and Postel’s Law

Did you ever wonder why “Internet” is capitalized?

> 你是否注意到过为什么"Internet"是大写的？

When I first joined the Internet in the 1990s, I found some now-long-lost introductory tutorial. It talked about the difference between an internet (lowercase i) and the Internet (capital I). An internet is “any network that connects smaller networks together.” The Internet is… well… it turns out that you don’t need more than one internet. If you have two internets, it is nearly unavoidable that someone will soon figure out how to connect them together. All you need is one person to build that one link, and your two internets become one. By induction then, the Internet is the end result when you make it easy enough for a single motivated individual to join one internet to another, however badly.

> 我是在1990年代加入互联网的，那时我注意到一些今天早已失传已久的入门教程，里面讲了“**i**nternet”和“**I**nternet”之间的区别。internet指的是任何可以连接更小的网络的网络，但是有了**I**nternet，你就不需要多余一个的**i**nternet。如果你有两个**i**nternet，不可避免的，需要考虑如何让将它们俩连接起来。归纳起来，如果你想让一个人能够足够容易从一个internet中加入到另一个，不论效果好坏，Internet就是最终的解决方案。

Internets are fundamentally sloppy. No matter how many committees you might form, ultimately connections are made by individuals plugging things together. Those things might follow the specs, or not. They might follow those specs well, or badly. They might violate the specs because everybody else is also violating the specs and that’s the only way to make anything work. The connections themselves might be fast or slow, or flakey, or only functional for a few minutes each day, or subject to [amateur radio regulations](https://en.wikipedia.org/wiki/AMPRNet), or worse. The endpoints might be high-powered servers, [vending machines](https://www.ibm.com/blogs/industries/little-known-story-first-iot-device/), toasters, or satellites, running any imaginable operating system. Only one thing’s for sure: they all have bugs.

Which brings us to Postel’s Law, which I always bring up when I write about networks. When I do, invariably there’s a slew of responses trying to debate whether Postel’s Law is “right,” or “a good idea,” as if it were just an idea and not a force of nature.

Postel’s Law says simply this: be conservative in what you send, and liberal in what you accept. Try your best to correctly handle the bugs produced by the other end. The most successful network node is one that plans for every “impossible” corruption there might be in the input and does something sensible when it happens. (Sometimes, yes, “something sensible” is to throw an error.)

[Side note: Postel’s Law doesn’t apply in every situation. You probably don’t want your compiler to auto-fix your syntax errors, unless your compiler is javascript or HTML, which, kidding aside, actually were designed to do this sort of auto-correction for Postel’s Law reasons. But the law does apply in virtually every complex situation where you need to communicate effectively, including human conversations. The way I like to say it is, “It takes two to miscommunicate.” A great listener, *or* a skilled speaker, can resolve a lot of conflicts.]

Postel’s Law is the principle the Internet is based on. Not because Jon Postel was such a great salesperson and talked everyone into it, but because that is the only winning evolutionary strategy when internets are competing. Nature doesn’t care what you think about Postel’s Law, because the only Internet that happens will be the one that follows Postel’s Law. Every other internet will, without exception, eventually be joined to The Internet by some goofball who does it wrong, but just well enough that it adds value, so that eventually nobody will be willing to break the connection. And then to maintain that connection will require further application of Postel’s Law.