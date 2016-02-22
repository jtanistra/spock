
```
import javax.annotation.Resource
import spock.lang.*
// Spring imports omitted for brevity

@ContextConfiguration(locations = "appcontext.xml")
class SpringExtensionExample extends Specification {
  @Autowired
  ItemDao dao

  @Resource
  EmailService email

  @Autowired
  ApplicationContext context
}
```

The Spring extension, compatible with Spring 2.5 and Spring 3, brings Spring's `TestContext` framework to Spock. Instead of providing its own API, the extension relies solely on the `TestContext` API. To activate the extension, annotate your specification with `@ContextConfiguration`. Additionally, at least the following Jars have to be present on the runtime class path:
  * `spring-beans`
  * `spring-test`
  * `spock-spring`

In many cases, you will want to access Spring beans from your specification. To do so, provide the `locations` of your Spring bean definition files, and use Spring's annotation-based injection mechanism to inject the fields of your specification. For example, `@Autowired` will inject beans by type, and `@Resource` will inject beans by name. If required, you can also inject the `ApplicationContext` itself. However, you should prefer injecting beans whenever possible.

The `TestContext` framework provides many features, most of which are supported by the Spring extension. From the annotations listed in the [Spring documentation](http://static.springsource.org/spring/docs/2.5.6/reference/testing.html#testcontext-annotations), all but the following are supported:
  * `@ExpectedException` (use Spock's `thrown()` method or `@FailsWith` annotation instead)
  * `@Timed` (use Spock's `@Timeout` annotation instead)
  * `@Repeat` (Spock will likely add a similar feature in the future)

**Note:** Due to the way Spring's `TestContext` framework is designed, `@Shared` fields cannot currently be injected. This also means that `setupSpec()` and `cleanupSpec()` cannot get access to Spring beans. See [TransactionalExample](http://code.google.com/p/spock/source/browse/trunk/spock-spring/src/test/groovy/org/spockframework/spring/TransactionalExample.groovy) for how to deal with this limitation.

To learn more about Spring's `TestContext` framework, see the [official documentation](http://static.springsource.org/spring/docs/2.5.6/reference/testing.html#testcontext-framework). Also take a look at the code examples in `spock-spring`, in particular [DirtiesContextExample](http://code.google.com/p/spock/source/browse/trunk/spock-spring/src/test/groovy/org/spockframework/spring/DirtiesContextExample.groovy) and [TransactionalExample](http://code.google.com/p/spock/source/browse/trunk/spock-spring/src/test/groovy/org/spockframework/spring/TransactionalExample.groovy).