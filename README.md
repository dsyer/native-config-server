# Spring Cloud Config Server

A Config Server that runs as a native image.  If you have GraalVM and `native-image` (Java 11, GraalVM 20.3.0):

```
./compile.sh
./target/config-server
```

Or with a buildpack:

```
mvn spring-boot:build-image
docker run -p 8080:8080 config-server:0.0.1-SNAPSHOT
```


Then you can test:

```
curl localhost:8080/app/default/main
```

or

```
curl localhost:8080/main/app-default.properties
```

