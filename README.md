# Spring Cloud Config Server

A Config Server that runs as a native image.

```
./compile.sh
./target/config-server
```

and

```
curl localhost:8080/app/default/main
```

or

```
curl localhost:8080/main/app-default.properties
```

