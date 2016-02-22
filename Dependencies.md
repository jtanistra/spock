# Mandatory dependencies #
  * Java 5 or higher
  * Groovy 1.6 or higher (latest 1.6.x release recommended)
  * JUnit 4.4 or higher

**Note**: There is also a Spock version for Groovy 1.7 (currently built against beta-2).

# Optional dependencies #
Dropping one of these on the classpath will enable additional features. No further configuration is required.
  * CGLIB 2.1\_3: enables mocking of classes (in addition to interfaces)
  * Objenesis 1.1: enables mocking of classes without default constructor (together with CGLIB)