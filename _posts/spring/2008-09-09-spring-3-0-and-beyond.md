--- 
layout: post
title: Spring 3.0 and beyond
permalink: /archives/19/index.html
description: Information about the pending Spring 3.0 release as
             gathered on the SpringSource Seminar Day in
             Linz.
---
I know that I promised some more reports about SpringOne, but as so
often, plans haven't survived confrontation with reality. As
compensation, I offer you the latest news about the upcoming Spring
3.0 release, fresh from the
SpringSource Seminar Day in Linz (which I had the opportunity to attend to, as my employer,
[OPITZ CONSULTING](http://www.opitz-consulting.de), now SpringSource Premier Systems Integrator Partner, was one of the
event's sponsors). Please bear in mind that many of the new features
still only exist as prototypes, so things can and probably will change
before the final release. But now without further ado...

## Spring 3.0...

Apart from the switch to Java 5 that I [mentioned earlier](http://www.springify.com/archives/15), the
main new features of Spring 3.0 will revolve around the two areas of
Spring EL (dubbed Unified EL++) and the Spring web modules. While I
have already written [a rather detailed roundup of the former](http://www.springify.com/archives/16), I had a chance to get a
glimpse at the upcoming usage of expression language within Spring
bean definitions.

### Spring EL

Having made a first appearance in Spring Web MVC, Spring EL is not
entirely new. Nevertheless, the promotion of this feature to the
Spring core will enable all modules to incorporate it into their
functionality. Before I give you some examples regarding Spring XML
configuration, let me explain what Spring EL will actually do.

Expressions will be evaluated relative to the context they appear in
and can be used to address what J&uuml;rgen called implicit attributes
of the respective contexts. Inside the definition of a session scoped
bean, you will, for example, be able to reference session and request
attributes. You will also be able to use system properties and fields
of Spring-managed beans. The really interesting thing is, that,
instead of binding early as BeanPostProcessors do, EL expressions will
return the value available at bean instantiation time.

Now for the example:

{% highlight xml %}
<bean id="someBean" class="com.springify.example.SomeClass"
      scope="prototype">
  <property name="someProperty" value="#{anotherBean.anotherProperty}"/>
</bean>
  
<bean id="anotherBean" class="com.springify.example.AnotherClass"/>
{% endhighlight %}

The current value of anotherBean's anotherProperty will be injected at
someBean's creation time, i.e. every time someBean is accessed, as its
scope is set to prototype.

Similar functionality will also be provided by means of annotation
configuration. The examples from the presentation sported the
syntax...

{% highlight java %}
@Component
public class SomeBean {
          
    private String someProperty;
          
    @Value("#{anotherBean.anotherProperty}") 
    public void setSomeProperty(String someProperty) { 
        this.someProperty = someProperty; 
    }
          
}
{% endhighlight %}

..., in this case providing a dynamically bound default value for
someProperty.

### REST support for Spring Web MVC

I've already blogged about the [upcoming REST support](http://www.springify.com/archives/16),
so I will only give you a quick overview of what's new. Apparently,
there's now a consensus about how requests for different
representations will be handled. As mentioned before, both accept
headers and file name extensions will be evaluated. The actual
functionality will be implemented on top of Spring Web MVC well know
view resolution mechanism, complete with new View types for the most
commonly used representations (JSON, XML, Atom).

### Portlet 2.0 support

By now there's more to the web than servlets, and the Spring support
for JSR 168 has always worked like a bliss. With the recently
finalized [Portlet 2.0 specification](http://jcp.org/en/jsr/detail?id=286), some long awaited features will soon make their way
into the popular portlet containers. Spring Portlet MVC will address
the most important of those in the 3.0 release, namely named actions,
resource request and event-based inter-portlet communications, by
means of additional annotations for `@Controller` annotated
portlet controllers. `@ActionMapping` and
`@RenderMapping` will be able to bind to named
actions. `@ResourceMapping` will allow serving server-side
resources while `@EventMapping` will allow you to implement
handling of inter-portlet events.

### Model Validation

With the advent of annotation based configuration, the old Validator
mechanism feeled a little old-fashioned every now and then. For those
of you who wished for something more Java 5 like, there's hope on the
horizon. Annotation-based model validation will be part of the final
Spring 3.0 release. While the exact syntax is not yet finalized, the
examplex given showed `@NotNull` and
`@ShortDate` as field level annotations. Probably one of
the more interesting facts about the new model validation mechanisms
is the deep integration with Spring Web MVC and Spring JS to provide
gracefully degrading client side validation when using the form
namespace to bind to an annotated model class.  Also, the Spring team
seems to be evaluating [JSR 303](http://jcp.org/en/jsr/detail?id=303) and the [Hibernate Validator](http://hibernate.org/subprojects/validator)
project, so we can probably hope for support for standard validation
annotations.

### Conversational scope and stateful controller objects

While not as detailed as the features above, Jürgen also said that the
team was evaluation possibilities for providing something along the
lines of the Spring SWF conversation scope inside the Spring core. It
would most likely be implemented as combination of a new bean scope
and an API to programatically start and end conversations. While
Spring SWF provides a very high level, declarative way of dealing with
conversation semantics, this would enable all Spring modules and
Spring based applications to utilize similar functionality.

### Factory annotations

Spring 3.0 will contain a new stereotype annotation
`@Factory`, that, combined with a method level
`@FactoryMethod` annotation will allow implementing
functionality similar to the current `BeanFactory` but with
multiple bean creation methods.

### Pruning and deprecating

It has now be confirmed that all currently deprecated artifacts will
be removed with the final release of Spring 3.0 as long as there is no
really compelling reason to leave them in the codebase. Most likely,
the traditional MVC controller class hierarchy will be deprecated in
favour of the new `@Controller` style. The same fate awaits
the JUnit 3.8 based integration test class hierarchy and probably the
HibernateTemplate and JPATemplated based classes. Additionally,
support for Commons Attributes and the traditional Toplink API will be
removed from the Spring core and probably factored out into separate
legacy modules.

### Roadmap

While Jürgen was very careful not to actually promise any release
dates, the team is pressing hard to release a first milestone this
month, containing a good share of the REST support as well as some of
new the Spring EL functionality. RC1 is planned for December, probably
just before christmas. Depending on community feedback on this and
subsequently published release candidates, the release of a GA version
should occur around February 2009.

## ... and beyond.

Jürgen made clear that plans don't end with 3.0. There's already
some vision of the upcoming functionality in Spring 3.1 and 3.2,
although, given the timeframe, this should apparently be handled with
care. Spring 3.1, which is scheduled for July 2009 might contain
support for [JAX-RS'](http://jcp.org/en/jsr/detail?id=311)s endpoint model as alternative to Spring Web MVC based RESTful
applications as well as support for the upcoming
[Servlet 3.0 specification](http://jcp.org/en/jsr/detail?id=315). There's also a discussion about supporting [Web Beans](http://jcp.org/en/jsr/detail?id=299) annotations
while that specification is awaiting it's finalization.

About Spring 3.2, due in late 2009, only wild guesses can be made, but
there will likely be some support for Java 7 features (like binding
support for the new DateTime API). Java 5 support will, however, be
guaranteed throughout the lifetime of Spring 3.x.
