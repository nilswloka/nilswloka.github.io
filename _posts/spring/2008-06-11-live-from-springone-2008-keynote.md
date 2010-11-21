---
layout: post
title: Live from SpringOne 2008 - Keynote
permalink: /archives/13/index.html
---
After having won the battle against Antwerp rush hour traffic, I
arrived just in time for this year's SpringOne keynote. Considering
the momentum the Spring Framework has gained since my last attendance
in 2006, I shouldn't have been surprised by the sheer number of people
who were already swarming the Metropolis cinema complex. As [Stephan](http://www.bejug.org/confluenceBeJUG/display/~stephan)
just told us, some 400 people from 25 countries have registered. But
back to what's happening on screen right now: Stephan just finished
his introductory talk, which he mainly spent showing all the shiney
features of [Parleys](http://parleys.com/display/PARLEYS/Home), with the announcement that due to trademark
issue, Javapolis will officially be renamed to [Javoxx](http://www.javoxx.com/display/JV08/Home) and handed
off the microphone to [Rod](http://blog.springsource.com/main/author/rodj/).  Rod's just promised that the keynote he's 
about to hold is going to address technical people and presented
statistics which showed that recently
Spring overtook EJB as requirement on the US job market. Also, 
[Gartner](http://www.gartner.com/) and [Forrester](http://www.forrester.com/) seem to be convinced
that Spring is going to be the key framework for developing enterprise
Java applications and middleware services.  A point that Rod doesn't
get tired to emphasise is that Spring is about choice. One important
step into that direction was the comprehensive support for
annotation-based configuration in Spring 2.5. Even while the syntax is
new, using @Autowired and @Qualifier is equivalent to the autowiring
by type and by name that has always been part of Spring's
configuration options. What's new is the possibility to define own
qualifier annotations by annotating them with @Qualifier, as shown on
the example below.

{% highlight java %}
@Qualifier 
@Component 
@interface Apac { 
    // Implementation omitted 
}
  
@Apac 
public class SomeLocalizedService implements SomeService { 
    // Implementation omitted 
}
  
  
public class SomeConsumer {
  
    @Apac SomeService someService; 
  
    // Implementation omitted 
}
{% endhighlight %}

The framework will associate the custom annotations and use them to
chose the proper instance instance for injection. This leads to using
Spring annotations as "meta-annotations" (a term just coined by Rod)
to build own sets of domain-specific annotations that turn classes
they are applied to to Spring-managed components. Combined with the
classpath scanning functionality included in Spring 2.5, you could -
if you really wanted to - avoid writing any XML configuration at all.

As the keynote progresses, Rod's stressing that one of the most
important freebies you get when using Spring is the ability to secure
your application by means of [[http://static.springframework.org/spring-security/site/index.html][Spring Security]] (formerly Acegi Security). 
As we all happen to know by now, every time you used Acegi, 
[a fairy died](http://netzooid.com/blog/2007/12/03/every-time-you-use-acegi). The configuration was just horrible (someone compared
it to strangling a dragon with bare hands, though I seem to have lost
the link; speak up if its you) as it basically boiled down to writing
bean definition for all the Acegi low level components. Spring
Security comes with a custom namespace that (almost) solves this
problem. Personally I still find it daunting, but I also think that
the domain it applies to is important enough to use it nevertheless.

One thing Rod seems to be surprised of - and I'm with him there - is
that almost no one in the audience uses the Spring integration testing
framework provided by means of Spring TestContext. For those of you
not attending SpringOne, I'm gladly relaying his invitation to give it
a try and donate the money you save by doing so (to him, but that's
just an implementation detail you're free to change). I gave an
[example of how to use it for test-driven development](http://www.springify.com/archives/8) in a recent post.

Spring Integration is another new addition to the Spring Framework
family. Considering that Spring has always been a strong integration
platform, being able to use it as quasi service bus with a domain
specific configuration language implementing [Gregor Hohpe's](http://www.eaipatterns.com/ramblings.html)
EAI patterns seems pretty convenient. I'm going to blog about this in
detail in the near future, so I will not dig into the details here.

If you have used Spring MVC in Spring 2.0, you should also have a look
at the new features introduced with the 2.5 release. I have written
about it before, but I think the new "convention over configuration"
mentality introduced here - especially with the advent of 
[@Controller](http://static.springframework.org/spring/docs/2.5.x/reference/mvc.html#mvc-annotation) - is worth noting again. Things have become a lot easier and, combined
with the new [Spring Web Flow](http://www.springframework.org/webflow) release, development web applications with Java might
even become fun (well, in the broader sense). Some of the highlights
of both frameworks, namely the aforementioned @Controller annotation,
SWF's Ajax features, Spring JavaScript (which is, according to 
[Keith](http://blog.springsource.com/main/author/keithd/), who just took over the microphone, is "a toolkit for progressively
enhancing web pages with AJAX" built on [DOJO](http://dojotoolkit.org/) and Spring Faces (which
basically makes JSF as component framework available within Spring Web
Flow driven applications and could be considered a JSF integration
layer) found their way into the keynote.

With Rod back at the microphone, the topic now is deployment models
and runtimes for Spring based application. He stresses that developing
with the Spring programming models in fact decouples your code from
the runtime environment, as Spring as container works in all
deployment models available today (JEE, Servlet, OSGi, etc.). While
portability on this level doesn't seem like an issue, there's the
ongoing trend of deploying to to lightweight servlet containers like
Tomcat and Jetty instead of commercial JEE containers. Whether or not
OSGi will be the next generation enterprise runtime is yet to be
determined, nevertheless the announcement of SpringSource Application
Platform ([I blogged about it here](http://www.springify.com/archives/11)) last month seems to be an indicator for it's
relevance. [Rob](http://blog.springsource.com/main/author/robh/), Application Platform lead, just entered the stage with an awesome S2AP
t-shirt (100% bloat-free); if I am able to get one of those, I'll post
a picture here.

Rod's talking about the
[SpringSource Enterprise subscription](http://springsource.com/products/enterprise) now, which, besides official support
(including indemnification), grants access to the SpringSource Tool
Suite, an Eclipse-based integrated development environment for
Spring-based applications. As of the latest developer release, the
Tool Suite comes with support for the PAR deployment style introduced
with S2AP. Even while I'm not that much of an Eclipse person, I will
try to give you an overview of what's being presented here. I also
have to mention the incredible pace at which 
[Christian](http://blog.springsource.com/main/author/cdupuis/),
Spring IDE and STS lead, keeps up with the rapidly expanding and
changing Spring stack. One interesting feature seems to be what
Christian is calling "runtime error analysis", which allows you to
automatically look up stack traces in a knowledge base to find likely
solutions to your programming problems. Actually STS incorporates a
lot of what I'd call community features like forum access, recent
news, etc. If you hate media disruptions as I do, this might be a good
thing for you. Christian stresses that, due to the comprehensive
context sensitive hints and the comprehensive knowledge about Spring
best practices built into it, the Tool Suite can be used as a
consulting and teaching platform for Spring based projects.  The
subscription also lets you use the SpringSource Application Management
Suite, which is a runtime monitoring platform for Spring applications
with support for all major JEE containers as well as Tomcat. I leave
it up to you to look up the details of this and the other SpringSource
enterprise products (fingers are already hurting) and write about
SpringSource's goals and motivations as presented by Rod at this
moment.

Interesting enough Peter Cooper-Ellis, former WebLogic Unit Excutive,
has just been hired by SpringSource for Engineering and Product
Management. He's right now talking about his background with
proprietary software and how the the whole industry is moving towards
deployment scenarios like cloud computing. One of his roles at
SpringSource seems to be to help the Spring framework meet the demands
those trends will make on the next generation's application
frameworks.

Well, I think that's about it for now. I'll keep you posted about any
ground-breaking news, so make sure to subscribe to the feed.
