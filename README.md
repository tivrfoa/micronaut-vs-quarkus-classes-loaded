# Goal

The goal is simply to create the most basic Micronaut and Quarkus
"Hello World" application and compare the number of classes
loaded by both after making one request and stopping the application.

I'll use: `java -Xlog:class+load=info:classloaded.txt -jar target/filename.jar`

## Results

Micronaut | Quarkus
----------|--------
5284      | 3939

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

