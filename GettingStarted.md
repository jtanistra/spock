## Spock Web Console ##

The easiest way to get to know Spock is http://webconsole.spockframework.org, which instantly lets you view, edit, run and even publish Spock specifications for others to see. Zero install required!

## Spock Example Project ##

If you prefer to run Spock on your machine, just clone the [spock-example Git repository](https://github.com/spockframework/spock-example) and type `./gradlew test` (Windows: `gradlew test`). Seconds later, you will be running your first Spock specifications. The only prerequisite is that you have JDK 5 or higher installed; everything else will be downloaded automatically. Also included are build scripts for Ant and Maven, and instructions how to generate an Eclipse or IDEA project. For more information, see the README file.

Need help? Ask in our [forum](http://groups.google.com/group/spockframework)!


---


The remainder of this page explains in detail how to manually setup Spock in various environments.

## Command line ##

  * Download and setup Java 5 or higher (JRE will do for our purposes)
  * Download and setup Groovy 2.x binary release ([download page](http://groovy.codehaus.org/Download))

Detailed instructions for these two steps can be found in the [Groovy tutorial](http://groovy.codehaus.org/Tutorial+1+-+Getting+started).

  * Download junit-4.12.jar ([download page](https://github.com/junit-team/junit/releases))
  * Download [spock-core-0.7-groovy-2.0.jar](http://repo1.maven.org/maven2/org/spockframework/spock-core/0.7-groovy-2.0/spock-core-0.7-groovy-2.0.jar)
  * Create a file named HelloSpock.groovy with the following content: [HelloSpock.groovy](HelloSpock.md)
  * Run HelloSpock with `groovy -cp "junit-4.12.jar:spock-core-0.7-groovy-2.0.jar" HelloSpock` (make sure the classpath is correct; on Windows, separate classpath entries with `;` instead of `:`)

## Ant/Gradle/Maven ##

See the [spock-example build](https://github.com/spockframework/spock-example).

## Eclipse ##

Verified against Eclipse 3.6, 3.7

  * Install the [Groovy Eclipse Plugin](http://groovy.codehaus.org/Eclipse+Plugin)
> > Important: Make sure to use the latest release or snapshot version of the Groovy Eclipse plugin.
  * Enable the following option: Preferences->Groovy->Use monospace font for JUnit. This is important for Spock's condition output to be aligned correctly. If this doesn't work immediately, uncheck the option, press 'Apply', and check the option again.
  * For Eclipse 3.5 and below, make sure you have a source attachment set for the spock-core Jar. (Otherwise you might run into the following Eclipse JDT bug: https://bugs.eclipse.org/bugs/show_bug.cgi?id=175582) For Maven projects, the easiest way to achieve this is `mvn eclipse:eclipse -DdownloadSources`.
  * Create a new Java project named HelloSpock
  * Download [spock-core-0.7-groovy-2.0.jar](http://repo1.maven.org/maven2/org/spockframework/spock-core/0.7-groovy-2.0/spock-core-0.7-groovy-2.0.jar)
  * Add JUnit 4 (4.7 or higher) and spock-core-0.7-groovy-2.0.jar to the project's class path
  * In the src folder, create a Groovy class named HelloSpock with the following content: [HelloSpock.groovy](HelloSpock.md)
  * In Project Explorer, right-click HelloSpock and select Run As -> JUnit Test

Here is a screenshot of Spock in action:

![http://spock.googlecode.com/svn/wiki/HelloSpockScreenshotEclipse.png](http://spock.googlecode.com/svn/wiki/HelloSpockScreenshotEclipse.png)

## IntelliJ IDEA ##

Verified against IDEA 10, 11

  * Download and extract Groovy 2.x binary release ([download page](http://groovy.codehaus.org/Download))
  * Download junit-4.12.jar ([download page](https://github.com/junit-team/junit/releases))
  * Download [spock-core-0.7-groovy-2.0.jar](http://repo1.maven.org/maven2/org/spockframework/spock-core/0.7-groovy-2.0/spock-core-0.7-groovy-2.0.jar)
  * Create a new IDEA project named HelloSpock and add a Java module.
  * In "Module settings", add a "Single-Entry Module Library" for groovy-all-2.x.jar, junit-4.12.jar, and spock-core-0.7-groovy-2.0.jar
  * In the src folder, create a Groovy class named HelloSpock with the following content: [HelloSpock.groovy](HelloSpock.md)
  * In the "Project" window, right-click HelloSpock and select "Run"

![http://spock.googlecode.com/svn/wiki/HelloSpockScreenshotIdea.png](http://spock.googlecode.com/svn/wiki/HelloSpockScreenshotIdea.png)

## Netbeans ##

As of Netbeans 6.7.1/6.8M1, the NetBeans Groovy plugin does not support running JUnit 4 tests, and therefore Spock specifications. For general information about the NetBeans Groovy plugin, go to http://groovy.codehaus.org/NetBeans+Plugin.