  1. **Easy to learn**
> > If you know Java and JUnit, you are almost ready to go.
  1. **Powered by Groovy**
> > Java's dynamic companion lets you do more in less time. Plus, it's a lot of fun!
  1. **Eliminates waste**
> > No assertion API. No record/replay mocking API. No superfluous annotations. Everything is questioned, and only the essential is kept.
  1. **Detailed information**
> > Spock's runtime collects a wealth of information, and presents it to you when needed.
```
    Condition not satisfied:

    max(a, b) == c
    |   |  |  |  |
    3   1  3  |  2
              false
```
  1. **Designed for use**
> > We always start from a user's perspective, without worrying about implementation details. Everything else follows from that.
  1. **Open-minded**
> > Test-first? Test-last? Unit-level? Integration-level? Test-driven? Behavior-driven? We believe there are many ways to create good software, and try to give you the flexibility to do it your way.
  1. **Beautiful language**
> > Express your thoughts in a beautiful and highly expressive specification language.
```
    def "subscribers receive published events at least once"() {
      when: publisher.send(event)
      then: (1.._) * subscriber.receive(event)
      where: event << ["started", "paused", "stopped"]
    }
```
  1. **Extensible for everyone**
> > @Transaction? @SpringBean? @DeployApp? With Spock's interception-based extension mechanism, you can easily create your own extensions.
  1. **Compatible with JUnit**
> > Run specifications with your IDE, build tool, and continuous integration server. Leverage JUnit's reporting capabilities.
  1. **Learns from the history**
> > Spock combines the best features of proven tools like JUnit, jMock, and RSpec, and innovates on top of them.

Learn [more](SpockBasics.md) about Spock, or [get started](GettingStarted.md) right away.