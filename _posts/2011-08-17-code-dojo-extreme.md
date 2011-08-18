---
layout: post
flattr: 1
title: Code Dojo Extreme
description: During our latest Code Dojo, we learned that self-administered stress can actually
             be a lot of fun while giving Robert Chatley's and Matt Wynne's Extreme Startup
             game a try.
keywords: programming,code dojo,training,extreme startup,game,exercise,serious games
---
<div style="float:right; margin-left: 15px; margin-botton: 15px;"><img src="/images/extreme-startup-1.jpg" alt="Extreme Startup - Flipchart" title="Extreme Startup - Flipchart" /></div>
Among the things we brought with us from [XP 2011](http://xp2011.org/), [Robert Chatley's](http://chatley.com/) and [Matt Wynne's](http://blog.mattwynne.net/) [Extreme Startup](https://github.com/rchatley/extreme_startup) game sounded like the most fun. While I unfortunately missed the session back in Madrid, [Christian](http://twitter.com/#!/holzig), a colleague of mine, was extremely enthusiastic about it. So tonight, we gave it a try at our company internal Code Dojo. What can I say? I never knew that stress could be so much fun.

### What's that thing about?

If you haven't heard about Extreme Startup yet, you can think of it as a programming competition. Participants register their own web applications at a Ruby-based game server, which will in turn spam the participants' application with requests containing all kind of questions. Every correct answer scores some points while wrong answers, timeouts or other failures are punished with a small amout of negative points. Scores are kept track of on a leaderboard served by the game server.

### How does it feel?

[Norbert](http://www.herr-norbert.de/) and I were planning to implement our server in Clojure. I actually felt quite confident because turnaround time seemed like a major success factor and [interactive development](http://www.bestinclass.dk/index.clj/2009/12/dynamic-interactive-webdevelopment.html) with Clojure excels in this regard. So I made some resolutions up front: I would of course write unit tests and use version control, maybe even do some TDD, which in Clojure I don't feel exactly comfortable with yet. The warm-up round went smoothly and we were looking forward to the real thing. Then the requests started rolling in. *Rushing* in! When the dust finally settled, we had lost a few hundred points and lots of sweat.

<div style="float:left; margin-right: 15px; margin-botton: 15px;"><img src="/images/extreme-startup-2.jpg" alt="Extreme Startup - Leaderboard" title="Extreme Startup - Leaderboard" /></div>

While things improved from there, it was not before the third round that what could only be described as frantic hacking turned into something I dare to call software development. It can be attributed to the Clojure REPL that we were still able to take and defend the lead until the game server died unexpectedly in round four. It goes without saying that I didn't write a single unit test nor committed a line of code to the repository before the end of the game (you can have a look at the result [here](https://gist.github.com/1152743)).

So how did it feel like? Why, terrible, energizing, stressful and very funny at the same time.

### Why should I try it?

While not exactly a realistic exercise, Extreme Startup will tell you a lot about what you don't yet feel completely comfortable with. Be it keyboard shortcuts, refactorings, language idioms or test-driven development, trying to write code with the other teams on your tail sure does a good job of exposing your weak points. Besides, you'll have a lot of fun, so I can only recommend giving it a try.

### A little advice

If you consider hosting an Extreme Startup session yourself, this checklist might help:

* Make sure that all participants have a [skeleton project](https://github.com/bodil/extreme_startup_servers) before joining or your warmup round might take longer than expected.
* Tell the participants to check whether their servers can be reached from the network. We experienced all kind of weird firewall issues during our session.
* Participants need to be able to write an application that responds to HTTP GET requests. You should also consider telling them that they should be prepared to parse strings.
* Play through all rounds before hosting the session. Be prepared to deal with bugs in the game server.

Finally, I'd like to thank Matt and Robert for the awesome idea and for providing the game server!
