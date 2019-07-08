Test to reproduce AspectJ weaving issue with ORDS's custom class loader.

Problem description:

Oracle Rest Data Service uses custom class loader to make sure that classes in jar files located in subdirectories like WEB-INF in  a case of war files prepared for the deployment into application servers are presented in a class path.
This prevents the AspectJ from  weaving classes.

Description of the test case

The test case here is the stripped version of the ORDS which includes only class loading part. 
During execution this code loads and executes an example class located at src/oracle/dbtools/jarcl/example/

The class which should be loaded and executed is defined in jarl.properties file
cat jarcl.properties 
main.class=oracle.dbtools.jarcl.example.Main 

Aspect measuring method's execution time is in the ASPECT directory.

How to run.

It was tested with Java 8.

Unzip the ordsclissue.zip into some directory, cd into this directory and then:
- to reproduce the issue run
  $JAVA_HOME/bin/java -javaagent:lib/aspectjweaver.jar  -jar ordswithaspect.war , where there is methodtiming.jar  with aspect in WEB-INF/lib in ordswithaspect.war
- to use workaround
  $JAVA_HOME/bin/java -Xbootclasspath/p:lib/aspectjweaver.jar:lib/methodtiming.jar -javaagent:lib/aspectjweaver.jar  -jar ordsnoaspect.war , where there is no methodtiming.jar in WEB-INF/lib


