This is the development site for Metl.  To run the latest and greatest use the following instructions.  The latest  release can be downloaded here:

<a href="https://sourceforge.net/projects/metl/files/latest/download" rel="nofollow"><img alt="Download Metl" src="https://a.fsdn.com/con/app/sf-download-button"></a>

Please use the forum for general discussion and questions.  It is location here: https://sourceforge.net/p/metl/discussion/general/

Metl is a simple, web-based integration platform that allows for several different styles of data integration including messaging, file based Extract/Transform/Load (ETL), and remote procedure invocation via Web Services. 

Metl was built to solve fairly simple day to day integration tasks without the need for custom coding, heavy infrastructure or high costs. It can be deployed in the cloud or in an internal data center, and was built to allow developers to extend it to fit their needs by writing their own components that can be included and leveraged by the existing Metl infrastructure.

<p align="center">
  <img src='https://raw.githubusercontent.com/wiki/JumpMind/metl/images/screenshots/design/design.png' />
</p>

## Build Me, Run Me

This project requires the Java JDK to build and run.  The build currently generates a war file 
which can be deployed to an application server like [Apache Tomcat](http://tomcat.apache.org).  The war file can also be run as a standalone application, in which case it uses an embedded [Jetty](http://www.eclipse.org/jetty) web server.

### Build
~~~~~
cd metl-assemble
./gradlew assemble
~~~~~

### Run
~~~~~
cd ../metl-war/build/libs
java -jar metl.war
~~~~~

### Deploy

To Tomcat:
~~~~~
cp ../metl-war/build/libs/metl.war /opt/apache-tomcat-8.0.14/webapps/.
/opt/apache-tomcat-8.0.14/bin/catalina restart
~~~~~

Or install as a standalone service on Linux or Windows:
~~~~~
java -jar metl.war install
~~~~~


### Develop
To develop in [Eclipse](http://eclipse.org) run the following and import the generated projects:
~~~~~
cd metl-assemble
./gradlew develop
~~~~~

To run in Eclipse, use the "Show Your Metl" launch shortcut.

### Publish a release

The GPL modules (`metl-core`, `metl-server`, `metl-ui`, `resource-core` and every `comp-*` in this repository) are
published as `org.jumpmind.metl:*` to the Jumpmind Maven repository at `https://downloads.jumpmind.com/files/maven/releases/`,
and the installer to the `metl/releases` category. The **Release** workflow of `jumpmindinc/metl-com` publishes them; it
builds this repository from the branch with the same name as the metl-com branch.

1. Ask a Jumpmind admin to open a deployment window on Admin › Releases › Deployment windows for the release
   API token. Outside the window every deploy is refused with HTTP 403 "Maven deploys are closed", and 10
   refusals within an hour revoke the token, so start the release only once the window is open.
2. Run the metl-com **Release** workflow on the `release/*` branch with the version, the build number and the
   target Saturn. It fails on a failing test, signs every GPL module, stages and releases them on Saturn (all
   modules or none), uploads the installer, attests the released jars, and only then tags the commit.
3. Check a jar's provenance:
   ~~~~~
   gh attestation verify comp-file-<version>.<buildNumber>.jar -R jumpmindinc/metl-com
   ~~~~~

Released versions are immutable: `metl-core`, `metl-server` and `metl-ui` publish as plain `<version>`, so
each version of them can be released once. A build without `-PbuildNumber` refuses to publish.
