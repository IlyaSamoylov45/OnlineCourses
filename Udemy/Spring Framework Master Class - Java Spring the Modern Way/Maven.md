Maven
  - The great majority of Maven users are going to call Maven a “build tool”: a tool used to build deployable artifacts from source code. Build engineers and project managers might refer to Maven as something more comprehensive: a project management tool.
    - A build tool such as Ant is focused solely on preprocessing, compilation, packaging, testing, and distribution.
    - A project management tool such as Maven provides a superset of features found in a build tool. In addition to providing build capabilities, Maven can also run reports, generate a web site, and facilitate communication among members of a working team.

What happens when I add a Maven dependency?
  - In general in all pom.xml we found dependency like this -
  ```xml
    <dependency>
      <groupId>org.apache.commons</groupId>
      <artifactId>commons-lang3</artifactId>
      <version>3.4</version>
    </dependency>
  ```
  - Here groupId, artifactId and version are 3 keys by which a jar is uniquely identified. These 3 combination works like a coordinate system for uniquely identifying a point in a space using x, y and z coordinates.
  - Whenever you issue a mvn package command maven tries to add the the jar file indicating by the dependency to you build path. For doing this maven follows these steps :
    1. Maven search in your local repository (default is ~/.m2 for linux). If the dependency/jar is found here then it add the jar file to you build path. After that it uses required class file from the jar for compilation.
    2. If the dependency is not found in ~/.m2 then it looks for your local private repository (If you already have configured any using setting.xml file) and maven central remote repository respectively. If you don't have any local private repository then it directly goes to the maven central remote repository.
    3. Whenever the jar is found in the local/remote repository it is downloaded and saved in ~/.m2.
  - Going forward, when you again issue a mvn package command then it's never search for the dependency to any repository since it already in your ~/.m2.

Build LifeCycle
  - Validate Project.
   - validate the project is correct and all necessary information is available
  - Compile the project.
     - compile the source code of the project
  - Test
    - test the compiled source code using a suitable unit testing framework. These tests should not require the code be packaged or deployed successful it continues.
  - Package
     - take the compiled code and package it in its distributable format, such as a JAR.
  - Verify
    - run any checks on results of integration tests to ensure quality criteria are met
  - Install
    - install the package into the local repository, for use as a dependency in other projects locally
  - Deploy
    - done in the build environment, copies the final package to the remote repository for sharing with other developers and projects.

Usual Command line calls for build lifecycle
  - You should select the phase that matches your outcome. If you want your jar, run package. If you want to run the unit tests, run test.
  - If you are uncertain what you want, the preferred phase to call is
    - mvn verify
    - This command executes each default lifecycle phase in order (validate, compile, package, etc.), before executing verify. You only need to call the last build phase to be executed, in this case, verify. In most cases the effect is the same as package. However, in case there are integration-tests, these will be executed as well. And during the verify phase some additional checks can be done, e.g. if your code written according to the predefined checkstyle rules.
  - In a build environment, use the following call to cleanly build and deploy artifacts into the shared repository.
    - mvn clean deploy

How does maven work?
  - Local maven Repository => Local System.
  - Remote Maven Repository => Central Repositories
    - Stores all versions of all dependencies.
  - mvn install vs maven deploy
    - copies the created jar to local maven repository - a temp folder on machine.

Important maven commands :
  - mvn clean
    -  deletes all the generated files and starts again from scratch.
    - Certain plugins require a clean in order to work properly. For instance (at least in Maven 2), the maven-war-plugin explodes each dependent WAR into an existing directory tree. It requires a clean to get rid of files that have been removed from the dependent WARs.
    - Another problem is that when you rename a class, the old compiled version can hang around in the build tree, and will get included in JAR files, etcetera ... until you run mvn clean.
  - mvn test
  - mvn package
  - mvn install
  - mvn deploy
  - mvn build
    - This will try to launch the configured custom run configurations.
    - It is not a part of lifecycle.
    - This will actually launch the previous dialog where we created a new run configuration. What happens is that M2Eclipse will create a new one, that you can fill exactly like above. You could see it as a short-cut for creating custom "Maven Build" run configurations.
