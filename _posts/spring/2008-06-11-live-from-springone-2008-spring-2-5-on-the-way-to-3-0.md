---
layout: post
title: Live from SpringOne 2008 - Spring 2.5 on the way to 3.0
permalink: /archives/14/index.html
---
As all of you probably know of the nice new features that arrived with
Spring 2.5, I'm going to concentrate on the "gems" I harvested from
[Jürgens](http://blog.springsource.com/main/author/juergenh/) presentation. As for the 3.0 stuff, I'm going to collect the
various bits of information that seem to pop up in various talks and
write about it once I find the time to put all the pieces of the
puzzle together...

One of the main themes in Spring 2.5 has been the progression towards
Java 5 and JEE 5 programming models (e.g. by means of annotation
driven configuration options and support for JEE annotations). The
other apparent trend was the advent of load-time weaving as
alternative to the traditionally proxy-based AOP model in earlier
Spring releases. As Jürgen explained the major features that have been
introduces in this context, he mentioned a couple of not so obvious
facts about Spring 2.5 that I'd like to relay to you:

## Registering custom annotations for component scan 
You can write your own stereotype annotations (yes, completely independent from the
Spring annotations) and make the container automatically pick up your
annotated components by supplying filters to the
`ClassPathBeanDefinitionScanner` responsible for determining valid
candidates. See the [domain-driven design](http://static.springframework.org/spring/docs/2.5.x/api/org/springframework/context/annotation/ClassPathScanningCandidateComponentProvider.html#addIncludeFilter(org.springframework.core.type.filter.TypeFilter)][documentation]] for details. This sounds great for implementing [[http://www.domaindrivendesign.org/)
principles by providing intention revealing annotations.

## OpenJPA save point support
Spring's [OpenJPA](http://static.springframework.org/spring/docs/2.5.x/api/org/springframework/orm/jpa/vendor/OpenJpaVendorAdapter.html][OpenJpaVendorAdapter]] exposes [[http://openjpa.apache.org/)'s
[extended EntityManager implementation](http://openjpa.apache.org/docs/latest/javadoc/org/apache/openjpa/persistence/OpenJPAEntityManager.html). As Spring's JpaTransactionManager
has support for JDBC 3.0 savepoints, this eventually allows you to use
OpenJPA's save point semantics. I'll make sure to give this a try and
blog about how it works.

## LTW mode for `<tx:annotation-driven>`
All of you know
about the annotation driven transaction demarcation, which is usually
done by the application context scanning your classes for
`@Transactional` and wrap them in Proxies. While this usually works
great, there is, as various forum threads suggest, some amount of
confusion regarding the limits of dynamic proxies, mainly when
transaction propagation doesn't work as expected with "class-internal"
method calls. As I just learned, you can alleviate that problem by
enabling load-time weaving via an agent or the custom classloader
implementations that are bundled with Spring and configuring the
transaction support with an additional attribute like shown here
(ommiting the loadtime weaving configuration itself):

{% highlight xml %}
<tx:annotation-driven mode="aspectj"/>
{% endhighlight %}

Neat, isn't it? Methods annotated with `@Transactional` will be
augmented during load-time afterwards, no more proxies are needed.

Well, that's it for now, I'm off to listen to Mark Fisher's talk about
Spring Integration.
