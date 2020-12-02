Sample app that stores the byte code from a CGLib proxy and loads it back at runtime (avoiding defining the class dynamically). Build the app and run it:

```
./mvnw package && java -jar target/*.jar
```

It works (output):

```
Name: com.example.Tool, org.springframework.cglib.proxy.Enhancer
Intercepted
Name: com.example.Tool, org.springframework.cglib.reflect.FastClass
Name: com.example.ToolEnhanced, org.springframework.cglib.reflect.FastClass
Called
```

where "Intercepted" comes from the proxy, and "Called" comes from the target. The "Name" output comes from the CGLib enhancer.

Now build a native image:

```
./compile.sh
```

then run it

```
./target/config
```

It works, but this is not using the proxy (output):

```
Called
```

Try to build an image that loads the enhanced class from bytecode:

```
./compiles.sh enhanced com.example.EnhancedApplication
```

it blows up (with cryptic error message:)

```
...
[enhanced:10263]        image:   4,000.91 ms,  3.48 GB
[enhanced:10263]        write:   1,161.81 ms,  3.48 GB
[enhanced:10263]      [total]: 148,730.02 ms,  3.48 GB

real    2m30.261s
user    10m2.289s
sys     0m9.935s
FAILURE: an error occurred when compiling the native-image.
```

Attempt to use the same classpath and `-agentlib:native-image-agent=config-output-dir=target/native-image/` and it dumps core:

```
...
#
# A fatal error has been detected by the Java Runtime Environment:
#
#  SIGSEGV (0xb) at pc=0x00007fe02b18b48a, pid=9326, tid=0x00007fe032ff5700
#
# JRE version: OpenJDK Runtime Environment (8.0_272-b10) (build 1.8.0_272-b10)
# Java VM: OpenJDK 64-Bit Server VM GraalVM 20.3.0-dev (25.272-b10-jvmci-20.3-b04-fastdebug mixed mode linux-amd64 compressed oops)
# Problematic frame:
# C  [libnative-image-agent.so+0x5248a]
#
# Core dump written. Default location: /workspaces/config/core or core.9326
#
# An error report file with more information is saved as:
# /workspaces/config/hs_err_pid9326.log
#
# If you would like to submit a bug report, please visit:
#   https://github.com/oracle/graal/issues
# The crash happened outside the Java Virtual Machine in native code.
# See problematic frame for where to report the bug.
#
Current thread is 140600904996608
Dumping core ...
Aborted (core dumped)
```