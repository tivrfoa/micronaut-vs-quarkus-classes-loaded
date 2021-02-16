# Goal

The goal is simply create the most basic Micronaut and Quarkus
"hello world" application and compare the number of classes
loaded by both after one request.

I'll use: `java -Xlog:class+load=info:classloaded.txt -jar target/filename.jar`

## Results

Micronaut | Quarkus
----------|--------
???       | ???

### Micronaut

Generate a project
  - https://micronaut.io/launch/

Create jar
  - mvn package

Run
  - java -Xlog:class+load=info:classloaded.txt -jar target/

Testing
  - curl localhost:8080/hello

### Quarkus

