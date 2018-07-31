## Zero to building, running, and debugging Apache Spark source on Ubuntu

My first step to understanding a codebase usually involves learning how to build, run, and attach a debugger.
In the case of Apache Spark, I wanted to know how to make codebase changes, build and see them in spark-shell, and attach with the IntelliJ debugger. Steps to do so on Ubuntu are below.

All of this information is just paraphrased from other pages, linked below.

### Setup <sup>1</sup>

1. Make sure Java and Maven are installed on your machine
2. git clone https://github.com/apache/spark.git
3. cd into cloned spark directory
4. ./build/mvn -DskipTests clean package

### IntelliJ setup <sup>2</sup>

1. Download IntelliJ and get the Scala plugin
2. "Go to File -> Import Project, locate the spark source directory, and select “Maven Project”. In the Import wizard, it’s fine to leave settings at their default. However it is usually useful to enable “Import Maven projects automatically”, since changes to the project structure will automatically update the IntelliJ project."


### Building quicker with SBT <sup>2</sup>

```bash
$ build/sbt clean package
$ ./bin/spark-shell
$ export SPARK_PREPEND_CLASSES=true
$ ./bin/spark-shell # Now it's using compiled classes
# ... do some local development ... #
$ build/sbt compile
# ... you can now see your changes in spark-shell #
$ ./bin/spark-shell
```

### Attaching IntelliJ debugger <sup>3, 4</sup>

1. Open IntelliJ. Open the spark project you imported earlier. Open  "Run -> Edit Configurations..." Then click the "+" to add a new run/debug configuration, and select "Remote". Give it a name, e.g. "SparkLocal", and select "Socket" for Transport, "Attach" for Debugger mode, and type in "localhost" for Host and the port you used above for Port, in this case, "5005". Click "OK" to save. 
2. export SPARK_SUBMIT_OPTS=-agentlib:jdwp=transport=dt_socket,server=y,suspend=y,address=5005
3. run ./bin/spark-shell, it will should "Listening for transport dt_socket at address: 5005"
4. In IntelliJ, clikc Debug and select your SparkLocal configuration. spark-shell should now continue executing, and you should be able to set breakpoints in the codebase.
5. If you want to turn off debugging:
    unset SPARK_SUBMIT_OPTS

### References

[1] https://github.com/apache/spark <br>
[2] https://spark.apache.org/developer-tools.html <br>
[3] https://stackoverflow.com/questions/30403685/how-can-i-debug-spark-application-locally <br>
[4] https://community.hortonworks.com/articles/15030/spark-remote-debugging.html <br>
