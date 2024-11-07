# MavenSetup

A basic setup for all maven projects containing building and tools for java.

## Howto setup

1. Add the following to your pom.xml:
    ````xml
    <project>
        <parent>
            <groupId>org.betonquest</groupId>
            <artifactId>parent</artifactId>
            <version>X.Y.Z</version>
        </parent>

        <repositories>
            <repository>
                <id>reload-repo</id>
                <url>https://nexus.reloadkube.managedservices.resilient-teched.com/repository/reload/</url>
            </repository>
        </repositories> 
    </project>
    ````

## Maven Wrapper Note

As this setup enforces a minimum maven version,
it is not guaranteed that the maven version you have installed on your system is compatible with the project.
For that, a maven wrapper is recommended to be used.

It is strongly recommended to use the wrapper in any automated build process, as it ensures that the correct maven version is used.

## Update versions

update-dependencies
```shell
./mvnw --projects :parent,:versions clean -DgenerateBackupPoms=false versions:update-properties
```
update-snapshot-dependencies
```shell
./mvnw --projects :parent,:versions -DgenerateBackupPoms=false versions:unlock-snapshots versions:lock-snapshots
```
bump-version
```shell
$(eval SHELL := /bin/bash)
@if [ -z "$(NEW_VERSION)" ]; then read -e -i "$$(./mvnw help:evaluate -Dexpression=revision --quiet -DforceStdout)" -p 'new version: ' -r NEW_VERSION; fi; \
  if [ -z "$$NEW_VERSION" ]; then echo 'ERROR: NEW_VERSION is not set.'; exit 2; fi; \
  ./mvnw versions:set-property -DgenerateBackupPoms=false -Dproperty=revision -DnewVersion=$$NEW_VERSION
```
