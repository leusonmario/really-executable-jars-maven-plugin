To use it, add a plugin to your pom like

``` xml

<!-- You need to build an exectuable uberjar, I like Shade for that -->
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-shade-plugin</artifactId>
    <version>2.0</version>
    <executions>
        <execution>
            <phase>package</phase>
            <goals>
                <goal>shade</goal>
            </goals>
            <configuration>
                <transformers>
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer"/>
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">

                        <!-- note that the main class is set *here* -->

                        <mainClass>com.example.Main</mainClass>
                    </transformer>
                </transformers>
                <createDependencyReducedPom>false</createDependencyReducedPom>
                <filters>
                    <filter>
                        <artifact>*:*</artifact>
                        <excludes>
                            <exclude>META-INF/*.SF</exclude>
                            <exclude>META-INF/*.DSA</exclude>
                            <exclude>META-INF/*.RSA</exclude>
                        </excludes>
                    </filter>
                </filters>
            </configuration>
        </execution>
    </executions>
</plugin>

<!-- now make the jar chmod +x style executable -->
<plugin>
  <groupId>org.skife.maven</groupId>
  <artifactId>really-executable-jar-maven-plugin</artifactId>
  <version>1.4.1</version>
  <configuration>
    <!-- value of flags will be interpolated into the java invocation -->
    <!-- as "java $flags -jar ..." -->
    <flags>-Xmx1G</flags>

    <!-- (optional) name for binary executable, if not set will just -->
    <!-- make the regular jar artifact executable -->
    <programFile>nifty-executable</programFile>

    <!-- (optional) support other packaging formats than jar -->
    <!-- <allowOtherTypes>true</allowOtherTypes> -->
    
    <!-- (optional) name for a file that will define what script gets -->
    <!-- embedded into the executable jar.  This can be used to -->
    <!-- override the default startup script which is -->
    <!-- `#!/bin/sh -->
    <!--            -->
    <!-- exec java " + flags + " -jar "$0" "$@" -->
    <!-- <scriptFile>src/packaging/someScript.extension</scriptFile> -->
  </configuration>

  <executions>
    <execution>
      <phase>package</phase>
      <goals>
        <goal>really-executable-jar</goal>
      </goals>
    </execution>
  </executions>
</plugin>
```

Changes:

1.4.0 - require Java 7, change code to use JDK7 APIs
      - Support Windows
      - Don't suppress errors

1.3.0 - add helpmojo
      - allow attachment of executable instead of unconditional replacement
      - make extension configurable
      - allow script replacement in the resulting executable

1.2.0 - never released

1.1.0 - If programFile is set, do not make the base artifact (the
.jar) executable, just the programFile one.

1.0.0 - Stable

----

[![Build Status](https://travis-ci.org/brianm/really-executable-jars-maven-plugin.svg?branch=master)](https://travis-ci.org/brianm/really-executable-jars-maven-plugin)
