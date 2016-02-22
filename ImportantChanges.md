<b>Note: This information has moved to <a href='http://docs.spockframework.org/en/latest/migration_guide.html'>http://docs.spockframework.org/en/latest/migration_guide.html</a></b>

## Version 0.5 (released 2010-12-11) ##

  * Assignments in expect- and then-blocks are no longer allowed and will result in a compile error (with a helpful error message). Here is an example that works in 0.4, but no longer works in 0.5:

```
expect:
def person = repo.findPerson("fred")
person.isHappy()

and:
person = repo.findPerson("barney") // no longer allowed
person.isHappy()
```

To make this work in 0.5, either move the assignment to some other place (maybe it makes more sense to have it in a when-block anyway), or introduce another variable like so:

```
...

and:
def barney = repo.findPerson("barney")
barney.isHappy()
```

## Version 0.4 (released 2010-05-25) ##

  * Grails plugin base classes have been renamed from XYZSpecification to XYZSpec (e.g. UnitSpecification -> UnitSpec). The old classes have been deprecated and will be removed in Spock 0.5.

## Version 0.3 (released 2009-11-19) ##

  * `@Speck` has been deprecated. The new way to turn a class into a specification is to extend class `spock.lang.Specification` (or a subclass thereof).
  * `setupSpeck()`/`cleanupSpeck()` have been deprecated and renamed to `setupSpec()`/`cleanupSpec()`.
  * `spock.lang.Predef` has been deprecated. You should no longer directly use this class. Instead, extend from `spock.lang.Specification`, which provides all the same methods.
  * The Maven plugin's `find-specks` goal has been deprecated and renamed to `find-specs`.
  * The Ant selector `SpeckClassFileSelector` has been renamed to `SpecClassFileSelector`.

All deprecated elements will be removed in Spock 0.4.