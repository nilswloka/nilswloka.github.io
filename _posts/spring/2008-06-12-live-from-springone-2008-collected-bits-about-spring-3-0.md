---
layout: post
title: Live from SpringOne 2008 - Collected bits about Spring 3.0
permalink: /archives/15/index.html
---
I wouldn't go as far as to call the SpringSource team secretive, but
they sure are building up tension with regards to the Spring 3.0. The
talk I expected to provide a roadmap and feature summary actually did
focus on version 2.5 feature. I wrote about some of the more
interesting points [here](http://www.springify.com/archives/14). I tried, however, to collect the various bits
of information about the 3.0 release, so here you go (just remember
that most of this could still be subject to change and shouldn't be
considered official):

## Timeline 
According to Jürgen, there's the plan to have a first milestone by
August 2008, although I've also heard July mentioned. If everything
goes well, we might see a GA release late in Q4 2008.

## Theme
Actually, Spring 3.0 will probably intensify what Spring 2.5 started:
the embrace of the Java 5 programming model, this time to the extend
that the Spring Core itself will be partially rearchitected and for
the first time depend on Java 5. Internal (and gracefully downgrading)
usage of Java 6 features will be expanded, although where and what was
not announced. Also, there seems to be the trend to generalise certain
parts of non-core modules in order to incorporate them into the core -
for more about this see below.

## Features
Well, there was not an aweful lot of information availabe about new
features planned for 3.0 (with the exception of RESTful Spring, I'll
blog about that later tonight when I've ordered my notes), but here
you go:

- Full scale REST support by means of additions to the Spring MVC
  API - already pretty detailed, and apparently going to be
  included in the first milestone release
- Support Unified EL (as seen in Spring Web Flow) - very likely part
  of 3.0, but no details given
- Annotation support for declaring factory methods - as above
- Support for Portlet 2.0 ([JSR 286](http://jcp.org/en/jsr/detail?id=286)), including resource requests
  (ResourceServingPortlet) - as above
- "Preparations" for Servlet 3.0 specification - sounded a lot like
  architectural preparations not visible to the "consumer"
- Something to fill the gap between Spring Web Flow and Spring MVC -
  that sounded very vague
- Inclusion (probably generalisation) of the repeat, retry and resume
  semantics provided by [Spring Batch](http://static.springframework.org/spring-batch/index.html) - was only hinted at, no
  details given
- Inclusion of the OXM support provided by Spring WS - sounded pretty
  definitive, but no details given
- Some kind of site definition language for the web stack - no idea
  whether this is more than a rumour
- Model-based validation for use both in server and client - as above

There's probably more planned, but I guess we'll have to wait for the
first milestone to get a detailed roadmap.

## Important changes
The one apparent and rather fundamental change is that Spring 3.0 will
no longer run on Java 1.4. Apparently most of the core is refactored
to use generics, enumerations and variable argument lists. Apart from
that, some pruning is going to happen:

- Deprecated artifacts will be removed
- Some artifacts will probably marked as deprecated as of Spring 3.0,
  including the traditional Spring MVC controller hierarchy and
  the old JUnit 3.8 based integration test class hierarchy.
- Commons Attributes support will probably be removed because it
  doesn't make too much sense in a Java 5 environment
- Support for plain Toplink will probably removed from the core

According to Jürgen, the programming model itself will be 100%
backwards compatible with Spring 2.5. The framework's extension points
will, however, be pruned to accomodate for the fact that some of them
won't make sense in a Java 5 environment. Jürgen mentioned 95%
backwards compatibility with regards to extension points, so that
shouldn't be much of an issue.

That's it for now, expect another posting about developing RESTful
Spring applications with Spring 3.0 later tonight.
