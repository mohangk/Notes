---
draft: true
title: Gradle
categories:
  - Java
---
#buildscript
- these are the dependencies for gradle to run 

using gradle with kotlin - https://kotlinlang.org/docs/reference/using-gradle.html
shadow plugin - http://imperceptiblethoughts.com/shadow/#benefits_of_shadow
application plugin - https://docs.gradle.org/current/userguide/application_plugin.html

# application plugin
- automatically includes the distribution plugin
- you can build and run locally by doing “gradle run”

#fat-jar 
- http://www.baeldung.com/gradle-fat-jar

#configurations
- compile
- testCompile
- implementation
- api
https://stackoverflow.com/questions/44493378/whats-the-difference-between-implementation-and-compile-in-gradle'

Dependencies appearing in the api configurations will be transitively exposed to consumers of the library, and as such will appear on the compile classpath of consumers.
Dependencies found in the implementation configuration will, on the other hand, not be exposed to consumers, and therefore not leak into the consumers' compile classpath. This comes with several benefits:
 •	






