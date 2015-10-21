#Environment requirement
* JDK 1.8
* Eclipse 4.5
* Wildfly 9.0 Final
* [AspectJ 1.8.7](https://eclipse.org/aspectj/)

#Run with Eclipse
Modify VM arguments  

* replace this line  
  `-Djboss.modules.system.pkgs=org.jboss.byteman,org.jboss.logmanager,com.manageengine`
* append this line
  `-javaagent:${ASPECTJ_HOME}/lib/aspectjweaver.jar -Djava.util.logging.manager=org.jboss.logmanager.LogManager -Xbootclasspath/p:${WILDFLY_HOME}/modules/system/layers/base/org/jboss/logmanager/main/jboss-logmanager-2.0.0.Final.jar`

>Hint: This configuration require fill absolute path, in my case:  
*${ASPECTJ_HOME} is /Users/LD-Mac/aspectj1.8*  
*${WILDFLY_HOME} is /Users/LD-Mac/wildfly-9.0.0.Final*

![open launch configuration](https://slack-files.com/files-pub/T09SX28QK-F0B81KMPV-6c4e91be37/_____________2015-09-24_17.26.52.png)

---
# Run with CLI
Modify ***${WILDFLY_HOME}***/bin/standalone.conf

* find `if [ "x$JBOSS_MODULES_SYSTEM_PKGS" = "x" ]; then`  
  replace below code after that  
  `JBOSS_MODULES_SYSTEM_PKGS="org.jboss.byteman,org.jboss.logmanager,com.manageengine"`
* find `if [ "x$JAVA_OPTS" = "x" ]; then`  
  add below three line code after that  
  `JAVA_OPTS="$JAVA_OPTS -Xbootclasspath/p:$JBOSS_HOME/modules/system/layers/base/org/jboss/logmanager/main/jboss-logmanager-2.0.0.Final.jar"`  
  `JAVA_OPTS="$JAVA_OPTS -Djava.util.logging.manager=org.jboss.logmanager.LogManager"`  
  `JAVA_OPTS="$JAVA_OPTS -javaagent:$ASPECTJ_HOME/lib/aspectjweaver.jar"`

>Hint: You must config your aspectj home directory to $ASPECTJ_HOME.

---
# Test launch
1. Launch your Wildfly with upon configuration.
1. Send a http **POST** to http://localhost:8080/cms-scheduler/cron/execute with these parameters.
   * workflowTypeName=Helloflow
   * workflowTypeVersion=0.2
   * cronPattern=*/20 * * * * *
   >Hint: Recommend use [Postman](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop)
