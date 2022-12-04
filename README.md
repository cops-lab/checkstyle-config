# Checkstyle Config

This project contains a checkstyle config that is shared across all COPS lab projects. This becomes technically possible by embedding the configuration file as a resource in a fake project that is then added as a dependency to the checkstyle plugin.

Four simple steps need to be adapted in a `pom.xml` file to use checkstyle:

- Add a plugin repository
- Add checkstyle to the build config
- Add a dependency to this project
- Override the `configLocation`

These options are illustrated in the following snippet.

```xml
<pom>
    ...

    <!-- (1) add a plugin repository -->
    <pluginRepositories>
        <pluginRepository>
            <id>github-cops</id>
            <url>https://maven.pkg.github.com/cops-lab/packages/</url>
        </pluginRepository>
    </pluginRepositories>

    <build>
        <plugins>
            <!-- (2) add checkstyle to the build config-->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-checkstyle-plugin</artifactId>
                <version>3.2.0</version>
                <!-- (3) add a dependency to the package registry -->
                <dependencies>
                    <dependency>
                        <groupId>dev.c0ps</groupId>
                        <artifactId>checkstyle-config</artifactId>
                        <version>0.0.1-SNAPSHOT</version>
                    </dependency>
                </dependencies>
                <configuration>
                    <!-- (4) override the `configLocation` (and other options) -->
                    <configLocation>cops-lab.checkstyle.xml</configLocation>
                    <sourceDirectories>${project.basedir}</sourceDirectories>
                    <includes>src/**/*.java,**/*.xml,**/*.yml</includes>
                    <excludes>**/target/**</excludes>
                </configuration>
                <executions>
                    <execution>
                        <id>validate</id>
                        <phase>validate</phase>
                        <goals>
                            <goal>check</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
</pom>
```
