# Goal

The goal is simply to create the most basic Micronaut and Quarkus
"Hello World" application and compare the number of classes
loaded by both after making one request and stopping the application.

I'll use:
  - `java -Xlog:class+load=info:classloaded.txt -jar target/filename.jar`
  - `java -Xlog:class+preorder=debug:preorder-debug.txt -jar target/demo-0.1.jar`

`preorder` is interesting because it let us better understand how
the framework works, as the doc says:
>Enables tracing of all loaded classes in the order in which they’re referenced.

https://docs.oracle.com/javase/9/tools/java.htm#JSWOR624

## What's the point?

It's just out of curiosity.

It's important to note that having more classes loaded initially
is not necessarily a bad thing, because maybe it's just doing more
work at startup time to give you better latency for example.

Another important thing to remember is that some classes take longer
to load than others. We can realize that in this comparison, because
even though Micronaut loads more classes, it still starts faster than
Quarkus.

`code-with-quarkus 1.0.0-SNAPSHOT on JVM (powered by Quarkus 1.11.3.Final) started in 1.730s`

`io.micronaut.runtime.Micronaut - Startup completed in 1488ms`

## Results

  -Xlog             |Micronaut | Quarkus
--------------------|----------|--------
class+load=info     |5284      | 3939
class+preorder=debug|4844      | 3498

### Micronaut 2.3.2

Generate the project
  - https://micronaut.io/launch/

Create jar
  - mvn package

Run
  - java -Xlog:class+load=info:classloaded.txt -jar target/demo-0.1.jar

Testing
  - curl localhost:8080/hello

### Quarkus 1.11.3.Final

Generate the project
  - https://code.quarkus.io/

Create jar
  - mvn package

Run
  - java -Xlog:class+load=info:classloaded.txt -jar target/code-with-quarkus-1.0.0-SNAPSHOT-runner.jar

Testing
  - curl localhost:8080/hello

