---
layout: post
title: TDD with Spring's configuration magic
permalink: /archives/8/index.html
---
While preparing a presentation on test-driven development, I had the
chance to play around with the new configuration
annotations. Despite my initial aversion against
"polluting" my domain classes, the experience was rather
pleasant.  The trivial three class example I prepared will hardly be
usable as proof of concept for real-life development with Spring 2.5
configuration annotations. Nevertheless I'm rather sure that those
annotations will save valuable time during prototyping and early
development phases. So without further ado, here's what the code
looks like...

{% highlight java %}
package com.springify.example;
  
import static org.hamcrest.core.Is.is; 
import static org.junit.Assert.assertThat; 
import org.junit.Test; 
import org.junit.runner.RunWith; 
import org.springframework.beans.factory.annotation.Autowired; 
import org.springframework.test.context.ContextConfiguration; 
import org.springframework.test.context.TestExecutionListeners; 
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
import org.springframework.test.context.support.DependencyInjectionTestExecutionListener;
  
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = {"classpath:/com/springify/example/applicationContext.xml"})
@TestExecutionListeners(DependencyInjectionTestExecutionListener.class)
public class UserServiceIntegrationTest {
  
    @Autowired private UserService userService;
 
    @Test public void addUserAndLogin() {
        userService.registerUser("admin", "password");
        assertThat(userService.login("admin", "password"), is(true)); 
    }
 
}
{% endhighlight %}

Please note that I only had to use the `@TestExecutionListeners` annotation because for the
purpose of demonstrating TDD I felt no urge to dive into the
complexities of transaction management. While this is already pretty
concise considering I'm setting up a complete environment here, just
have a look at the bean definition: 

{% highlight xml %}
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans-2.5.xsd
                           http://www.springframework.org/schema/context
                           http://www.springframework.org/schema/context/spring-context-2.5.xsd">
  
  <context:component-scan
      base-package="com.springify"/>
  <context:annotation-config/>
  
</beans>
{% endhighlight %}

Not much in there, actually. But it has to be somewhere, doesn't it?
Maybe...

{% highlight java %}
package com.springify.example;
  
import org.springframework.beans.factory.annotation.Autowired; 
import org.springframework.stereotype.Service;
  
@Service public class DefaultUserService implements UserService {
  
    public boolean login(String userName, String password) { 
        throw new UnsupportedOperationException("TODO"); 
    }
  
    public void registerUser(String userName, String password) { 
        throw new UnsupportedOperationException("TODO"); 
    }
  
    @Autowired public void injectUserRepository(UserRepository userRepository) { 
        // TODO 
    } 
}
{% endhighlight %}

...

{% highlight java %}
package com.springify.example;
  
import org.springframework.stereotype.Repository;
  
@Repository public class DefaultUserRepository implements UserRepository {
  
    public User getUser(String username, String password) throws LoginFailedException { 
        throw new UnsupportedOperationException("TODO"); 
    }
  
    public void addUser(User user, String password) { 
        throw new UnsupportedOperationException("TODO"); 
    } 
}
{% endhighlight %}

Yes, that looks like it could do the trick. Even if the amount of
configuration saved by this approach might not be huge for the
few beans used in this example, it still felt a lot more
natural and ... dare I say... agile, because I was able to
concentrate on driving the design with my tests instead of
wiring changing bean graphs together.

As a side note, it was fun to use the new Spring TestContext with my
favorite acceptance testing framework [Concordion](http://www.concordion.org/). For quick results,
all you actually have to do is copy the code from
`org.concordion.integration.junit3.ConcordionTestCase` into a base
class:

{% highlight java %}
package com.springify.integration.concordion;
  
import org.springframework.test.context.junit4.AbstractJUnit4SpringContextTests;
import org.junit.Test; 
import org.concordion.Concordion; 
import org.concordion.api.ResultSummary; 
import org.concordion.internal.ConcordionBuilder;
  
import java.io.IOException;
  
public abstract class AbstractSpringConcordionTest extends AbstractJUnit4SpringContextTests {
  
    @Test public void concordion() throws IOException { 
        Concordion concordion = new ConcordionBuilder().build(); 
        ResultSummary resultSummary = concordion.process(this);
        System.out.print("Successes: " + resultSummary.getSuccessCount());
        System.out.print(", Failures: " + resultSummary.getFailureCount()); 
        if (resultSummary.hasExceptions()) { 
            System.out.print(", Exceptions: " + resultSummary.getExceptionCount()); 
        }
        System.out.println("\n"); resultSummary.assertIsSatisfied(); 
    }
}
{% endhighlight %}

After that, you can create your dependency-injected
fixtures like this:

{% highlight java %}
package com.springify.example;
  
import com.springify.integration.concordion.AbstractSpringConcordionTest;
import org.junit.runner.RunWith; 
import org.springframework.beans.factory.annotation.Autowired; 
import org.springframework.test.context.ContextConfiguration; 
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
  
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = {"classpath:/com/springify/example/applicationContext.xml"}) 
public class LoginTest extends AbstractSpringConcordionTest {
  
    @Autowired private UserService userService;

    public void addUser(String userName, String password) {
        userService.registerUser(userName, password); 
    }
  
    public String loginWith(String userName, String password) { 
        return (userService.login(userName, password) ? "was successful" : "failed"); 
    }
}
{% endhighlight %}
