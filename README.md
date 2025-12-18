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
                <id>betonquest-repo</id>
                <url>https://repo.betonquest.org/betonquest/</url>
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

To update the versions of all dependencies that are on a release version, use the following commands:
```shell
./mvnw --projects :parent,:versions clean -DgenerateBackupPoms=false versions:update-properties
```
To update the versions of all dependencies that are on a snapshot version, use the following commands:
```shell
./mvnw --projects :parent,:versions -DgenerateBackupPoms=false versions:unlock-snapshots versions:lock-snapshots
```

## Bump version

To bump the version of the project, use the following commands to interactively set the new version:
```shell
read -e -i "$(./mvnw --raw-streams --non-recursive help:evaluate -Dexpression=revision --quiet -DforceStdout)" -p 'new version: ' -r NEW_VERSION
```
Then set the new version:
```shell
if [ -n "$$NEW_VERSION" ]; then ./mvnw versions:set-property -DgenerateBackupPoms=false -Dproperty=revision -DnewVersion=$NEW_VERSION; fi
```
