### Creating mocks ###

Mocks can be created for interfaces, non-final Java classes, and non-final Groovy classes (experimental). All mocks are lenient. In other words, if a method call does not match any interaction, the default value for the method's return type is returned.

```
def subscriber = Mock(Subscriber) // "dynamic" style (mock name inferred from variable name)

Subscriber subscriber = Mock()    // "static" style" (mock name and type inferred from variable name and type)
```

### Global vs. local interactions ###

Interactions defined outside of a `then` block are called _global_. They are valid from the point of their definition to the end of the feature method.

```
setup:
subscriber.isAlive() >> true
```

Interactions defined inside of a `then` block are called _local_. They are valid only within the preceding `when` block.

```
when: publisher.send(event)
then: 1 * subscriber.receive(event)
```

### Optional vs. required interactions ###

Optional interactions don't have a cardinality, and must have a return value.

```
then: subscriber.isAlive() >> true
```

Required interactions must have a cardinality, and may have a return value.

```
then: 1 * subscriber.isAlive() >> true

then: n * subscriber.receive(event)
```

The most commonly used combinations are global+optional (stubbing as part of setup) and local+required (mocking as part of when-block), but the other two combinations are allowed as well.

### Cardinalities ###

```
n * subscriber.receive(event)      // exactly n times

(n.._) * subscriber.receive(event) // at least n times

(_..n) * subscriber.receive(event) // at most n times 
```

### Target constraints ###

```
subscriber.receive(event) // receive is called on 'subscriber'
```

```
_.receive(event) // receive is called on any mock object
```

### Method constraints ###

```
subscriber./set.*/(_) // any setter is called on subscriber (any regular expression allowed)
```

### Argument constraints ###

```
subscriber.receive()                       // no arguments

subscriber.receive(_)                      // any argument

subscriber.receive(!null)                  // any non-null argument

subscriber.receive(event)                  // any argument equal to event

subscriber.receive(!event)                 // any argument not equal to event

subscriber.receive(_ as Message)           // any argument that is-a Message (null is not allowed)

subscriber.receive( { it.priority >= 5 } ) // custom constraint

// For methods that take multiple arguments, specify one constraint per argument:

mock.foo(_, _, _)                    // any three arguments

mock.foo(_, !null, { it ==~ /a*b/ }) // any first arg, second arg non-null, third arg matching the given regex

// Constraints for varargs can be specified in list style or vararg style. Let's say we are mocking the following method: def foo(String... args)

mock.foo(["one", "two"]) // list style

mock.foo("one", "two")   // vararg style
```

### Return values ###

```
subscriber.isAlive() >> true                     // single return value, repeated indefinitely

subscriber.isAlive() >>> [true, false, true]     // multiple return values (anything that Groovy can iterate over), last one repeated indefinitely

def random = new Random()
subscriber.isAlive() >> { random.nextBoolean() } // custom return value

subscriber.isAlive() >> { throw new TimeoutException() } // custom action
```

### Strict definition of interactions ###

Given that all mocks are lenient, how can I express that no interactions other than the explicitely specified ones should take place?

```
/* expected interactions go here */
0 * _._ // no (more) method call on any mock
```

### Explicit denotation of interactions ###
If an interaction definition in a `then` block depends on other code in the same block, this relationship has to be made explicit as follows:

```
then:
interaction {
  def count = 3
  count * subscriber.receive(_)
}
```

The same is true for an interaction defined in a helper method:

```
then:
interaction {
  subscriberReceivesThreeMessages()
}

...

def subscriberReceivesThreeMessages() {
  3 * subscriber.receive(_)
  // you may define additional interactions here
}
```

## New in 0.4 ##

### Property syntax ###

```
subscriber.alive >> 5 // may use property syntax instead of method syntax for reading values
subscriber./a.*/ >> 5 // regex also supported for properties
```

### Ordered interactions ###

```
when:
publisher.publish.event

then:
1 * subscriber1.receive(event)
1 * subscriber2.receive(event) // order of interaction within same then-block is not defined; hence, subscriber1 might be notified either before or after subscriber2

then:
1 * subscriber3.receive(event) // must come after all interactions in previous then-blocks; hence, subscriber3 must be notified after both subscriber1 and subscriber2 
```

### Shortcut notation for "any interaction" ###

```
0 * _ // _ is shortcut for _._
```

### Sensible default behavior for equals(Object), hashCode(), toString() ###

By default, a mock will only be equal to itself, will return System.identityHashCode() for its hash code, and will return a descriptive message for toString(). Nevertheless, these methods can still be stubbed (or even mocked) if desired.
Furthermore, these methods no longer match the wildcard method name `_`. This is to avoid fragile tests that break once a test is debugged and the debugger calls one of these methods.