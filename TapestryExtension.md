The Spock Tapestry extension facilitates the creation of higher-level specs for Tapestry 5 based application. Its main functions are the creation and shutdown of a Tapestry registry with the modules required by a spec, and the injection of a spec's fields.

The Tapestry extension does not provide an API. Instead, it relies on Tapestry's own API - in particular the following Tapestry IoC annotations:

  * `@SubModule` indicates which Tapestry module(s) should be created (and subsequently shut down) for a spec
  * `@Inject` indicates that a spec field should be injected with a Tapestry service or symbol

Other annotations such as `@Service`, `@Symbol`, and `@Value` are also supported. For information on their use, see the [Tapestry IoC documentation](http://tapestry.apache.org/tapestry5/tapestry-ioc/). To interact directly with the Tapestry registry, define an injection point of type `ObjectLocator`. However, this should be rarely needed.

The Tapestry extension is activated by putting the spock-tapestry Jar on the test (runtime) class path. Once activated, the extension will create a Tapestry registry for every spec annotated with `@SubModule`, and inject the spec's fields. Because this happens _before_ field initializers and the `setup()` and `setupSpec()` methods are run, all injected values can be safely accessed from those places. Shutdown of the Tapestry registry will occur after the `cleanupSpec()` method has completed (if it exists). Therefore, injected services may also be used within `cleanup()` and `cleanupSpec()`.

Shared fields will be injected once per specification; regular fields once per feature (iteration). However, this does _not_ mean that a fresh service instance will be created for each injection; rather, control over a service's scope is left to Tapestry. Most Tapestry services use the default _singleton_ scope, which will result in the same service instance being injected every time.

Feature methods that require their own service instance(s) should be moved into separate specifications. To avoid code fragmentation and duplication, multiple specifications can be put into the same source file, with their commonalities factored out into a base class. Alternatively, a service definition (in a module created specifically to aid testing) may be marked with `@Scope("perIteration")`, resulting in the creation of a fresh service instance for every feature (iteration).

Sometimes it can be necessary to perform setup work before the Tapestry registry has been created. This is achieved with the following callback:

```
def beforeRegistryCreated() { /* do work here */ }  
```

If `beforeRegistryCreated()` returns a `Registry` instance, that instance will be used to do all injection, and any arguments to `@SubModule` will be ignored. For example, this makes it possible to use [PageTester](http://tapestry.apache.org/tapestry5/apidocs/org/apache/tapestry5/test/PageTester.html) and still have services injected. Note, however, that the `@SubModule` annotation itself is still required in order to activate the Tapestry extension for the spec.

Specs inheriting from other specs are treated as follows: All `beforeRegistryCreated()` callbacks along the inheritance chain are invoked (in parent-child order), all `@SubModule` annotations considered, all of the listed modules created, and all annotated fields injected.

Basic usage example (don't forget to put the spock-tapestry Jar on the test class path!):

```
import org.apache.tapestry5.ioc.annotations.*
import spock.lang.*

@SubModule(UniverseModule)
class UniverseSpec extends Specification {
  @Inject
  UniverseService service

  UniverseService copy = service

  def "service knows the answer to the universe"() {
    expect:
    copy == service        // injection occurred before 'copy' was initialized
    service.answer() == 42 // what else did you expect?!
  }
}
```

For more examples, see the specs in the [spock-tapestry](http://code.google.com/p/spock/source/browse/#svn/trunk/spock-tapestry/src/test/groovy/org/spockframework/tapestry/) module.