# Vaadin JPA CRUD

This is an example application that shows how your can build beautiful RIA UIs for your Bluemix backed execution environment (Liberty server and DB2 database).

Before you start, make sure you need to have Java SDK 1.7 (or higher) and Maven installed. Also you should [install cloudfoundry command line tools](https://www.ng.bluemix.net/docs/#cli/index.html#cli) and configure them for Bluemix.

To build this project, just execute following commands:

```
git clone https://hub.jazz.net/git/vaadin/vaadin-jpa-app
cd vaadin-jpa-app
mvn install
```

or execute the same goal via your IDE.

In Bluemix you need to prepare an execution environment, that provides a Java EE 6 server for Vaadin UI and a database mapped to "jdbc/vaadindb". The easiest way to accomplish this, is to is to use the Vaadin boilerplate. Just follow the "}>" logo in [Bluemix](http://bluemix.net/) catalog. Manually, you can just create an SQLDB service with a name "vaadindb". Naturally you can also use different name, but then you'll need to modify persistence.xml in src/main/resources/META-INF accordingly.

Once you have the execution environment ready, just issue following command. If you haven't used cloudfoundry command line tools before, a full tutorial personalized for your account and application can be found from "VIEW QUICK START" in BlueMix dashboard.
```
cf push <app-name> -p target/vaadin-jpa-application.war
```
... and you have your first Vaadin app deployed to Bluemix!

### Local development

If you want to develop/debug the application locally, you'll just need to introduce the data source in your local WAS Liberty Profile development server and deploy it there e.g. via your favorite IDE. Virtually any DB works, so if you are e.g. using Mac as you development environment, and can't start DB2, you can still debug the application locally. E.g. an in memory Derby server works just fine, simple instructions below.

* Download and place a derby jar file to usr/shared/resources/derby/derby.jar into your Liberty server directory.
* Place following configuration snippet into your development server.xml (most likely usr/servers/defaultServer/server.xml in your Liberty server directory):
```
  <!--  JDBC Driver configuration -->
  <jdbcDriver id="DerbyEmbedded" libraryRef="DerbyLib" />
  <library id="DerbyLib" filesetRef="DerbyFileset" />
  <fileset id="DerbyFileset" dir="${shared.resource.dir}/derby" includes="derby.jar" />
  <!-- Configure an in-memory db for the vaadin app configuration -->
  <dataSource id="jdbc/vaadindb" jndiName="jdbc/vaadindb" jdbcDriverRef="DerbyEmbedded" transactional="true">
    <properties databaseName="memory:jpasampledatabase" createDatabase="create" />
  </dataSource>
```


### Troubleshooting

**The application don't build properly** 

If you have [Maven](https://maven.apache.org/download.cgi) and Java 7 or later installed, the most common problem is that you are using Mac and your JAVA_HOME environment variable still points to 1.6 version of Java. An easy way to fix this is executing: 
```export JAVA_HOME=`/usr/libexec/java_home -v 1.7` ```
and/or adding that to your .bash_profile file.

Also note, that if you haven't used Maven before, the build may take several minutes on first run as Maven downloads several dependencies used by the application itself and the build.
 
**The deployment fails**

Deploying and setting up the database may take a while with a slow network connection, so be patient. In some cases there might happen an error due to e.g. network communication. Canceling the deployment with CTRL-C and trying again usually fixes the issue.
