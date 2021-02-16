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

**UPDATE** I had to add the results from jmap -histo <PID> because it's awesome!
I really liked this command. =)

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

## Micronaut 2.3.2

Generate the project
  - https://micronaut.io/launch/

Create jar
  - mvn package

Run
  - java -Xlog:class+load=info:classloaded.txt -jar target/demo-0.1.jar

Testing
  - curl localhost:8080/hello

jmap -histo <PID>

```
 num     #instances         #bytes  class name (module)
-------------------------------------------------------
   1:         38813        6564624  [B (java.base@15.0.1)
   2:          4685        2061512  [I (java.base@15.0.1)
   3:         30118         722832  java.lang.String (java.base@15.0.1)
   4:          5662         671408  java.lang.Class (java.base@15.0.1)
   5:          4717         644352  [Ljava.lang.Object; (java.base@15.0.1)
   6:         13382         428224  java.util.concurrent.ConcurrentHashMap$Node (java.base@15.0.1)
   7:             8         262272  [Ljava.util.concurrent.ForkJoinTask; (java.base@15.0.1)
...
2133:             1             16  sun.util.resources.LocaleData$LocaleDataStrategy (java.base@15.0.1)
2134:             1             16  sun.util.resources.cldr.provider.CLDRLocaleDataMetaInfo (jdk.localedata@15.0.1)
Total        183896       14951560
```

## Quarkus 1.11.3.Final

Generate the project
  - https://code.quarkus.io/

Create jar
  - mvn package

Run
  - java -Xlog:class+load=info:classloaded.txt -jar target/code-with-quarkus-1.0.0-SNAPSHOT-runner.jar

Testing
  - curl localhost:8080/hello

jmap -histo <PID>

```
 num     #instances         #bytes  class name (module)
-------------------------------------------------------
   1:        132124       25021408  [B (java.base@15.0.1)
   2:          6960        6063336  [I (java.base@15.0.1)
   3:         89916        2157984  java.lang.String (java.base@15.0.1)
   4:          8854        1379064  [Ljava.lang.Object; (java.base@15.0.1)
   5:         24887        1194576  java.util.HashMap (java.base@15.0.1)
   6:          4185         502016  java.lang.Class (java.base@15.0.1)
   7:         12229         391328  java.util.concurrent.ConcurrentHashMap$Node (java.base@15.0.1)
...
2068:             1             16  sun.util.resources.cldr.provider.CLDRLocaleDataMetaInfo (jdk.localedata@15.0.1)
2069:             1             16  sun.util.resources.provider.LocaleDataProvider (jdk.localedata@15.0.1)
Total        436458       43537472
```

