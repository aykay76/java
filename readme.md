https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html

```sh
mvn archetype:generate -DgroupId=com.aykay76 -DartifactId=test-app -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.5 -DinteractiveMode=false
mvn archetype:generate -DgroupId=com.aykay76 -DartifactId=test-lib -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.5 -DinteractiveMode=false
cd test-lib
mvn deploy
cd ../test-app
mvn compile ear:ear
mvn package
cd target
unzip test-app-1.0-SNAPSHOT.ear
java -cp target/com.aykay76.lib-test-lib-1.0-SNAPSHOT.jar:target/test-app-1.0-SNAPSHOT.jar com.aykay76.app.App

```

When I downloaded it compiled to class file version 61.0. Need to ensure a compatible Java runtime is installed according to this list: https://docs.oracle.com/javase/specs/jvms/se21/html/jvms-4.html

For me it was 17, which can be found here: https://www.oracle.com/java/technologies/javase/jdk17-archive-downloads.html



Create `~/.m2/settings.xml` for GitHub authentication:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.1.0 http://maven.apache.org/xsd/settings-1.1.0.xsd" xmlns="http://maven.apache.org/SETTINGS/1.1.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <servers>
        <server>
            <id>github-maven</id>
            <username>dummy</username>
            <password>${env.GITHUB_TOKEN}</password>
        </server>
    </servers>
</settings>

```



To create package, add the following to `pom.xml` for the shared library after build:

```xml
    <distributionManagement>
        <repository>
            <id>github-maven</id>
            <name>GitHub Packages</name>
            <url>https://maven.pkg.github.com/aykay76/devlib-java-config</url>
        </repository>
    </distributionManagement>
```

Then use `mvn deploy` to deploy the package via workflow.



To use packages from an internal repository, add the following to `pom.xml` after dependencies:

```xml
  <repositories>
    <repository>
      <id>github-maven</id>
      <url>https://maven.pkg.github.com/aykay76/*</url>  
    </repository>
  </repositories>
```

