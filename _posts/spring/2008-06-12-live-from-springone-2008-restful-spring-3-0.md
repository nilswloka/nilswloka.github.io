---
layout: post
title: Live from SpringOne 2008 - RESTful Spring 3.0
permalink: /archives/16/index.html
---
Everybody's talking about REST and RESTful web applications, so it
doesn't come as a surprise that there would be some kind of support
for this architecture style in Spring sooner or later.  Today at the
SpringOne in Antwerp, [Arjen](http://blog.springframework.com/arjen/) gave a
presentation on that topic and actually revealed what the [announced](http://www.springify.com/archives/15)
"full-scale" REST support will look like. So here's the
details for those of you who haven't had the opportunity to be here in
Antwerp:

## REST support will be based on Spring MVC instead of Spring WS
Those of you who have already had a look at the Spring MVC 2.5
`@Controller` annotation and its companions, `@RequestMapping`,
`@RequestParam` and `@ModelAttribute` will have noticed that it's possible
to map a request based on its HTTP method:

{% highlight java %}
@RequestMapping(method = RequestMethod.POST)
{% endhighlight %}

This already "smelled" REST-like, so I guess it was just
consequential to use Spring MVC as base for the upcoming REST support
in Spring 3.0. I had the opportunity to talk about it with Arjen and
he explained that they were actually experimenting with various
concepts for doing this in Spring WS, but in the end, Spring MVC felt
like the more natural choice.  But what will it look like then? Extend
a welcome to...

## `UriTemplate` and `@PathParam`

Well, actually `@PathParam` might not
be the final name, I've also seen `@UriParam` somewhere, but the usage
will be the same. I'll just give you an example:

{% highlight java %}
@RequestMapping(value = "/gadgets/{id}", method = RequestMethod.GET)
public View getGadget(@PathParam String id) { 
    // Fetch a gadget by id and 
    // return its representation as view 
}
{% endhighlight %}

It can't possibly get easier that that. Best of all, you can reuse
your investment in Spring MVC because the programming model doesn't
change. The final release will include support for binding to more
than one URI wildcard as well as identification of the requested
representation based on HTTP Accept headers (for machine clients) or
file extensions (for browsers).
Additional feature will include: 

- sing the Spring WS OXM support for creating XML views for requested resources
- Additional views including AtomView, RssView and probably JsonView, too
- Automatic generation and evaluation of HTTP ETag Header entries for caching purposes implemented as servlet
  filter
- Interpretation of hidden form fields as HTTP method (as HTML only
  allows GET and POST in forms) by means of a servlet filter

You might wonder whether there will be any support for building REST
clients as well. Indeed there is, and it's - not much of a surprise
here - ...

## `RestTemplate`

You can expect the usual usage patterns here and
Arjen showed some actual code, so it's pretty clear what the API
(which by the way seems to be implemented on top of [Commons HttpClient](http://hc.apache.org/httpclient-3.x/)) is
going to look like. I'll just give a few examples based on Arjen's
slides here:

{% highlight java %}
// Retrieving a represenation with 
// getForObject (with parameter substitution) 
RestTemplate template = new RestTemplate(); 
Gadget gadget = template.getForObject("http://www.springify.com/gadgets/{id}", Gadget.class, 1);

// Creating a resource with postForLocation 
// (with parameter substitution, map variant) 
Map<String, String> params = new HashMap<String, String>(); 
params.put("id", 42); 
URI uri = template.postForLocation("http://www.springify.com/gadgets/{id}/features", 
                                   new Feature("Glows in the dark."), params);

// Deleting a resource with delete template.
template.delete("http://www.springify.com/gadgets/{id}", someId);

// ... and the obligatory callback pattern, albeit without 
// further details yet. Method parameter will be provided 
// in the form of Enum types.  
template.execute("http://www.springify.com/gadgets/42/feature/1", 
                 POST, aRequestCallback, aResponseCallback);
{% endhighlight %}C

Looks pretty straightforward to me. So now that we know the
"what", what about the "when". Well, there's some
good news here. The first two features, namely RestTemplate for
building clients and @PathParam for mapping to UriTemplate wildcards
will be available in Spring 3.0 M1, which is scheduled for July or
August. The remaining features will follow while Spring progresses
towards 3.0 GA in late 2008.

## What's left

Well, there's the issue of authentication and
authorization, which is apparently taken care of by the Spring
Security team. Arjen mentioned support for configuring access
restrictions based on both HTTP methods and URLs, but I don't have any
details here.

That's it for tonight as it's getting late and I need to catch the
train back home early tomorrow. I hope to post a summary of the
SpringSource Application Platform sessions early next week, so keep an
eye on the [feed](http://feeds.feedburner.com/Springify).
