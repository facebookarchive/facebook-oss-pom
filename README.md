# Facebook OSS POM

## Rationale

A base pom codifies policy for projects. It allows a new Maven project
to quickly get off the ground and focus on the project itself and not
how to build it. The Facebook OSS POM focuses on building libraries
and components that are distributed as jar files through the Maven
central repository.

It is possible to use the base pom in a new project without any additional changes and modifications.

## Preliminaries

The Facebook OSS POM enforces a minimum version of Maven due to various bugs found in earlier versions.

By default, the Facebook OSS POM enforces JDK 1.7. To use another version, add

```xml
<properties>
  <project.build.targetJdk>1.6</project.build.targetJdk>
  ...
</properties>
```

### Local setup required!

To fully leverage the deployment options from the Facebook OSS POM, a number of servers need to be configured in the local `~/.m2/settings.xml` file. If these servers are missing, either artifact or site deployment will fail.

As described on https://docs.sonatype.org/display/Repository/Sonatype+OSS+Maven+Repository+Usage+Guide, the `sonatype-nexus-staging` and `sonatype-nexus-snapshots` repositories should be configured:

```xml
<servers>
  ...
  <server>
    <id>sonatype-nexus-snapshots</id>
    <username>user</username>
    <password>password</password>
  </server>
  <server>
    <id>sonatype-nexus-staging</id>
    <username>user</username>
    <password>password</password>
  </server>
  ...
</servers>
```

To support releasing and tagging repositories that are hosted on Github and to allow site deployment to Github, entries for Github and the project site must exist:

```xml
<servers>
  ...
  <server>
    <id>github</id>
    <username>github-id</username>
    <password>github-password</password>
  </server>
  <server>
    <id>github-project-site</id>
    <username>git</username>
  </server>
  ...
</servers>
```

The hard-coded username `git` and no password for the github-project-site are a limitation of the deployment tool used for the github site. They must exist in the local settings file.

## Usage

Add the Facebook OSS POM as the parent to a project:

```xml
<parent>
  <groupId>com.facebook</groupId>
  <artifactId>facebook-oss-pom</artifactId>
  <version> ... current pom release version ...</version>
</parent>
```

## Project pom structure

The following elements should be present in a pom using the Facebook OSS POM as parent:

* `groupId`, `artifactId`, `version`, `packaging`, `name`, `description` and `inceptionYear`

  Define the new project. These elements should always be present. If any of those fields are missing from the project,
the values from the Facebook OSS POM are picked up instead.

* `scm`

  Defines the SCM location and URL for the project. This is required to use the `release:prepare` and `release:perform` targets
  to deploy artifacts to Maven Central.

* `organization`, `developers`, `distributionManagement`

  Empty elements override the values inherited from the Facebook OSS. 

This is a sample skeleton pom using the Facebook OSS POM:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

  <modelVersion>4.0.0</modelVersion>

  <parent>
    <groupId>com.facebook</groupId>
    <artifactId>facebook-oss-pom</artifactId>
    <version> ... current version ...</version>
  </parent>

  <groupId> ... group id of the new project ... </groupId>
  <artifactId> ... artifact id of the new project ... </artifactId>
  <version> ... version of the new project ... </version>
  <packaging> ... jar|pom ... </packaging>
  <description> ... description of the new project ...  </description>
  <name>${project.artifactId}</name>
  <inceptionYear>2013</inceptionYear>
  
  <scm>
    <connection> ... scm read only connection ... </connection>
    <developerConnection>... scm read write connection ... </developerConnection>
    <url> ... project url ... </url>
  </scm>

  <distributionManagement/>
  <developers/>
  <organization/>
  ...
</project>
```

## Project POM conventions

In large maven projects, especially with multi-module builds, the pom files can become quite large. In many places, properties defined in the `<properties>` section of the pom are used. 

To avoid confusion with properties, the following conventions are used in the Facebook OSS POM:

* Properties defined in the POM that influence the build configuration are prefixed with `fb`.
* Properties that factor out plugin versions (because the plugin is used in multiple places in the POM and the versions should be uniform) start with `dep.plugin` and end with `version`.
* Properties that factor out dependency versions (to enforce uniform dependency versions across multiple, related dependencies) start with `dep` and end with `version`.

Examples:

```xml
<properties>
  <!-- Sets the minimum maven version to build (influences build) -->
  <fb.maven.version>3.0.4</fb.maven.version>

  <!-- The surefire plugin version -->
  <dep.plugin.surefire.version>2.13</dep.plugin.surefire.version>

  <!-- The version for all guice related dependencies -->
  <dep.guice.version>3.0</dep.guice.version>
<properties>
```

## Deploy to Maven Central (oss.sonatype.org)

The Facebook OSS POM is intended for open source projects that should be deployed to Maven Central. 

It inherits from the oss-parent POM and supports the OSS deployment process as outlined at https://docs.sonatype.org/display/Repository/Sonatype+OSS+Maven+Repository+Usage+Guide.


## Project Build and Checkers

The Facebook OSS POM hooks various checkers into the build lifecycle and executes them on each build. 

Generally speaking, running a set of checks at each build is a good way to catch problems early and any problem reported by a checker should be treated as something that needs to be fixed before releasing.

Checkers are organized in two groups, basic and extended.

### Basic checkers
* Maven Enforcer                  (http://maven.apache.org/enforcer/maven-enforcer-plugin/)
* Maven Dependencies              (http://maven.apache.org/plugins/maven-dependency-plugin/)
* Maven Dependency version check  (https://github.com/ning/maven-dependency-versions-check-plugin)
* Maven Duplicate finder          (https://github.com/ning/maven-duplicate-finder-plugin)

### Extended checkers
* Findbugs                        (http://mojo.codehaus.org/findbugs-maven-plugin/)
* PMD                             (http://maven.apache.org/plugins/maven-pmd-plugin/)
* License check                   (http://code.google.com/p/maven-license-plugin/)
* Code coverage                   (http://www.eclemma.org/jacoco/trunk/doc/maven.html)


All checkers are enabled by default and all checkers *will fail the build* if a problem is encountered. 

Each checker has a switch to turn it on or off and also whether a problem will be a warning or fatal to the build.

```xml
...
<properties>
  <!-- Do not run the duplicate finder --->
  <fb.check.skip-duplicate-finder>true</fb.check.skip-duplicate-finder>
</properties>
...
```

The following switches exist:

<table>
  <tr>
    <th>Group</th>
    <th>Check</th>
    <th>Skip check (Setting to <tt>true</tt> skips the check)</th>
    <th>Fail build (Setting to <tt>false</tt> only reports a warning)</th>
  </tr>
  <tr>
    <td>Basic</td>
    <td>Maven Enforcer</td>
    <td><tt>fb.check.skip-enforcer</tt></td>
    <td><tt>fb.check.fail-enforcer</tt></td>
  </tr>
  <tr>
    <td>Basic</td>
    <td>Maven Dependencies</td>
    <td><tt>fb.check.skip-dependency</tt></td>
    <td><tt>fb.check.fail-dependency</tt></td>
  </tr>
  <tr>
    <td>Basic</td>
    <td>Maven Dependency version check</td>
    <td><tt>fb.check.skip-dependency-version-check</tt></td>
    <td><tt>fb.check.fail-dependency-version-check</tt></td>
  </tr>
  <tr>
    <td>Basic</td>
    <td>Maven Duplicate finder</td>
    <td><tt>fb.check.skip-duplicate-finder</tt></td>
    <td><tt>fb.check.fail-duplicate-finder</tt></td>
  </tr>
  <tr>
    <td>Extended</td>
    <td>Findbugs</td>
    <td><tt>fb.check.skip-findbugs</tt></td>
    <td><tt>fb.check.fail-findbugs</tt></td>
  </tr>
  <tr>
    <td>Extended</td>
    <td>PMD</td>
    <td><tt>fb.check.skip-pmd</tt></td>
    <td><tt>fb.check.fail-pmd</tt></td>
  </tr>
  <tr>
    <td>Extended</td>
    <td>License check</td>
    <td><tt>fb.check.skip-license</tt></td>
    <td><tt>fb.check.fail-license</tt></td>
  </tr>
  <tr>
    <td>Extended</td>
    <td>Code coverage</td>
    <td><tt>fb.check.skip-jacoco</tt></td>
    <td><tt>fb.check.fail-jacoco</tt></td>
  </tr>
</table>

Checks can be turned on and off in groups:

<table>
  <tr>
    <th>Group</th>
    <th>Skip checks</th>
    <th>Fail build</th>
  </tr>
  <tr>
    <td>All Checks</td>
    <td><tt>fb.check.skip-all</tt></td>
    <td><tt>fb.check.fail-all</tt></td>
  </tr>
  <tr>
    <td>All Basic checks</td>
    <td><tt>fb.check.skip-basic</tt></td>
    <td><tt>fb.check.fail-basic</tt></td>
  </tr>
  <tr>
    <td>All Extended Checks</td>
    <td><tt>fb.check.skip-extended</tt></td>
    <td><tt>fb.check.fail-extended</tt></td>
  </tr>
</table>

A more specific setting (checker specific, then group, then all) overrides a more general setting:

```xml
...
<properties>
  <fb.check.skip-all>true</fb.check.skip-all>
  <fb.check.skip-duplicate-finder>false</fb.check.skip-duplicate-finder>
</properties>
...
```
will skip *all* checks except the duplicate finder.


### Checker notes

#### License checker

To ensure that a project has an uniform license header in all source files, the Maven license plugin can be used to check and format license headers.

The plugin expects the license header file as `src/license/LICENSE-HEADER.txt` in the root folder of a project. 

For a multi-module project, this file should exist only once, in the root pom of the project. In all other sub-modules, add

```xml
<properties>
  <fb.main.basedir>${project.parent.basedir}</fb.main.basedir>
</properties>
```

to each pom. This is a limitation of the Maven multi-module build process (see http://stackoverflow.com/questions/1012402/maven2-property-that-indicates-the-parent-directory for details).

#### Enforcer checker

The Enforcer plugin outlaws a number of dependencies that project might use for various reasons:

<table>
  <tr>
    <th>Outlawed dependency</th>
    <th>Rationale</th>
    <th>What to use</th>
  </tr>
  <tr>
    <td><tt>commons-logging:commons-logging-api</tt></td>
    <td>Ill-fated attempt to split commons-logging implementation and commons-logging API.</td>
    <td><tt>commons-logging:commons-logging</tt></td>
  </tr>
  <tr>
    <td><tt>cglib:cglib</tt></td>
    <td>Has all its dependencies packed inside, therefore leads to duplicate classes.</td>
    <td><tt>cglib:cglib-nodep</tt></td>
  </tr>
  <tr>
    <td><tt>junit:junit</tt></td>
    <td>Has all its dependencies packed inside, therefore leads to duplicate classes.</td>
    <td><tt>junit:junit-dep</tt></td>
  </tr>
  <tr>
    <td><tt>com.google.collections:google-collections</tt></td>
    <td>Superseded by Guava, duplicate classes with Guava.</td>
    <td><tt>com.google.guava:guava</tt></td>
  </tr>
  <tr>
    <td><tt>com.google.code.findbugs:jsr305</tt></td>
    <td>Subset of the full findbugs annotations, contains only the JSR-305 annotations</td>
    <td><tt>com.google.code.findbugs:annotations</tt></td>
  </tr>
</table>


## Well known dependencies

The Facebook OSS POM provides a number of dependencies to projects. These dependencies are considered "well known and stable". When a project wants to use any of these dependencies, it can declare them in the project `<dependencies>` section without a version and automatically pick up a version from the Facebook OSS POM.

The following dependencies are defined:

<table>
  <tr><th>Dependency name</th><th>Group/Artifact Ids</th><th>property</th></tr>
  <tr>
   <td>Google Guice</td>
   <td><tt>com.google.inject:guice</tt><p/><tt>com.google.inject.extensions:guice-servlet</tt><p/><tt>com.google.inject.extensions:guice-assistedinject</tt><p/><tt>com.google.inject.extensions:guice-multibindings</tt><p/><tt>com.google.inject.extensions:guice-throwingproviders</tt></td>
   <td><tt>dep.guice.version</tt></td>
  </tr>
  <tr>
   <td>Google Guava</td>
   <td><tt>com.google.guava:guava</tt></td>
   <td><tt>dep.guava.version</tt></td>
  </tr>
  <tr>
    <td>Joda Time</td>
    <td><tt>joda-time:joda-time</tt></td>
   <td><tt>dep.joda.version</tt></td>
  </tr>
  <tr>
    <td>Apache Commons Lang 3</td>
    <td><tt>org.apache.commons:commons-lang3</tt></td>
    <td><tt>dep.commons-lang3.version</tt></td>
  </tr>
  <tr>
    <td>Apache Commons Lang</td>
    <td><tt>commons-lang:commons-lang</tt></td>
    <td><tt>dep.commons-lang.version</tt></td>
  </tr>
  <tr>
    <td>Apache Commons Configuratio</td>
    <td><tt>commons-configuration:commons-configuration</tt></td>
    <td><tt>dep.commons-configuration.version</tt></td>
  </tr>
  <tr>
    <td>Apache Commons Codec</td>
    <td><tt>commons-codec:commons-codec</tt></td>
    <td><tt>dep.commons-codec.version</tt></td>
  </tr>
  <tr>
    <td>Apache Commons Collections</td>
    <td><tt>commons-collections:commons-collections</tt></td>
    <td><tt>dep.commons-collections.version</tt></td>
  </tr>
  <tr>
    <td>Apache Commons IO</td>
    <td><tt>commons-io:commons-io</tt></td>
    <td><tt>dep.commons-io.version</tt></td>
  </tr>
  <tr>
    <td>Apache Commons Beanutils</td>
    <td><tt>commons-beanutils:commons-beanutils</tt></td>
    <td><tt>dep.commons-beanutils.version</tt></td>
  </tr>
  <tr>
    <td>Java Inject API</td>
    <td><tt>javax.inject:javax.inject</tt></td>
    <td><tt>dep.javax-inject.version</tt></td>
  </tr>
  <tr>
    <td>Java Servlet API</td>
    <td><tt>javax.servlet:javax.servlet-api</tt></td>
    <td><tt>dep.javax-servlet.version</tt></td>
  </tr>
  <tr>
    <td>slf4j (Simple Logging Facade for Java)</td>
    <td><tt>org.slf4j:slf4j-api</tt><p/><tt>org.slf4j:slf4j-jcl</tt><p/><tt>org.slf4j:slf4j-jdk14</tt><p/><tt>org.slf4j:slf4j-log4j12</tt><p/><tt>org.slf4j:slf4j-nop</tt><p/><tt>org.slf4j:slf4j-simple</tt><p/><tt>org.slf4j:slf4j-ext</tt><p/><tt>org.slf4j:jcl-over-slf4j</tt><p/><tt>org.slf4j:jul-to-slf4j</tt><p/><tt>org.slf4j:log4j-over-slf4j</tt></td>
    <td><tt>dep.slf4j.version</tt></td>
  </tr>
  <tr>
    <td>Logback</td>
    <td><tt>ch.qos.logback:logback-core</tt><p/><tt>ch.qos.logback:logback-classic</tt></td>
    <td><tt>dep.logback.version</tt></td>
  </tr>
  <tr>
    <td>log4j</td>
    <td><tt>log4j:log4j</tt></td>
    <td><tt>dep.log4j.version</tt></td>
  </tr>
  <tr>
    <td>Findbugs Annotations</td>
    <td><tt>com.google.code.findbugs:annotations</tt></td>
    <td><tt>dep.findbugs-annotations.version</tt></td>
  </tr>
  <tr>
    <td>JUnit testing</td>
    <td><tt>junit:junit-dep</tt></td>
    <td><tt>dep.junit.version</tt></td>
  </tr>
  <tr>
    <td>TestNG testing</td>
    <td><tt>org.testng:testng</tt></td>
    <td><tt>dep.testng.version</tt></td>
  </tr>
   <tr>
    <td>Easymock Mocking framework</td>
    <td><tt>org.easymock:easymock</tt></td>
    <td><tt>dep.easymock.version</tt></td>
  </tr>
  <tr>
    <td>Hamcrest matchers</td>
    <td><tt>org.hamcrest:hamcrest-core</tt><p/><tt>org.hamcrest:hamcrest-library</tt></td>
    <td><tt>dep.hamcrest.version</tt></td>
  </tr>
  <tr>
    <td>Objenesis</td>
    <td><tt>org.objenesis:objenesis</tt></td>
    <td><tt>dep.objenesis.version</tt></td>
  </tr>
</table>

### Lock down a well known dependency

It is possible to "lock down" the version of a dependency so that any Facebook OSS POM updates will not affect the version used by the project. 
Locking a version is done by adding a property to the `<properties>` section of the project POM:

```xml
  <properties>
    <!-- change guice version in the Facebook OSS POM. -->
    <dep.guice.version>2.0</dep.guice.version>
    ...
  </properties>
  ...
  <dependencies>
    <dependency>
      <!-- Use the version from the Facebook OSS POM -->
      <groupId>com.google.inject</groupId>
      <artifactId>guice</artifactId>
    </dependency>
    <dependency>
      <!-- Use the version from the Facebook OSS POM -->
      <groupId>com.google.inject.extensions</groupId>
      <artifactId>guice-multibinder</artifactId>
    </dependency>
    ...
  </dependency>
```

to the project POM. This will lock the version of a dependency to the version desired. This can also be used to force a version update to a dependency.

```xml
  <properties>
    <!-- change log4j version in the Facebook OSS POM. -->
    <dep.log4j.version>1.2.18-SNAPSHOT</dep.log4j.version>
    ...
  </properties>
  ...
  <dependencies>
    <dependency>
      <!-- Use the version from the Facebook OSS POM -->
      <groupId>log4j</groupId>
      <artifactId>log4j</artifactId>
    </dependency>
    ...
  </dependency>
```


## Site deployment to Github

The Facebook OSS POM supports deployment of the maven generated site to Github. See http://khuxtable.github.com/wagon-gitsite/ for more information.

For a first time deployment, it is necessary to generate the `gh-pages` branch as described on Github at https://help.github.com/articles/creating-project-pages-manually.

For site deployment, the github user and github project site must be configured in the local `~/.m2/settings.xml` file!

```xml
<servers>
  ...
  <server>
    <id>github</id>
    <username>github-id</username>
    <password>github-password</password>
  </server>
  <server>
    <id>github-project-site</id>
    <username>git</username>
  </server>
  ...
</servers>
```

The hard-coded username `git` and no password for the github-project-site are a limitation of the deployment tool used for the github site.

## Other properties

These are default properties that affect some aspects of the build. All of them can be overriden in the `<properties>` section of the project pom.

### fb.build.jvmsize

Sets the default heap size for the compiler, javadoc generation and other plugins. Default is 1024M.

### fb.release.push-changes

When a project creates a release using the maven-release-plugin and `mvn release:prepare`, this switch controls whether the generated tags, modified POM files etc. are pushed automatically to the upstream repository or not. Default is `false` (do not push the changes).

### fb.maven.version

The minimum version of Maven to build a project.

### fb.main.basedir

The 'root' directory of a project. For a simple project, this is identical to `project.basedir`. In a multi-module build, it should point at the root project.

For a multi-module project, all other sub-modules must have this explicitly set to the root directory and therefore the following code

```xml
<properties>
  <fb.main.basedir>${project.parent.basedir}</fb.main.basedir>
</properties>
```
must be added to each pom. This is a limitation of the Maven multi-module build process (see http://stackoverflow.com/questions/1012402/maven2-property-that-indicates-the-parent-directory for details).

### fb.test.fork-mode

Defines the fork mode for running tests. Default is 'once' which is forking one JVM for all tests. Valid values are the same as for the maven-surefire-plugin (once, always, never).

