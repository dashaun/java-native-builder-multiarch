# java-native-builder-multiarch
A single `Cloud Native Buildpack` that can build native images for AMD64 and ARM64

## Quick Start

```bash
# Make a new directory and switch to it
mkdir java-native-builder-multiarch-test
cd java-native-builder-multiarch-test

# Create a Spring Boot 3 application
curl https://start.spring.io/starter.tgz -d dependencies=web,actuator -d javaVersion=17 -d bootVersion=3.0.0-SNAPSHOT -d type=maven-project | tar -xzf -

# Remove the last line of the pom.xml file (on Mac OS X)
sed -i '' -e '$ d' pom.xml
# Add the new profile to the end of the pom.xml, replacing the line we just deleted
echo "
  <profiles>
    <profile>
      <id>dashaun</id>
      <build>
        <plugins>
          <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
              <image>
                <builder>dashaun/java-native-builder-multiarch:7.37.0</builder>
              </image>
            </configuration>
          </plugin>
        </plugins>
      </build>
    </profile>
  </profiles>
</project>" >> pom.xml 

# build the application using the multiarch builder on AMD64 or ARM64
./mvnw -Pnative,dashaun spring-boot:build-image

# run the image
docker run -p 8080:8080 demo:0.0.1-SNAPSHOT &

# test with httpie
http :8080/actuator/health

# test with curl
curl http://localhost:8080/actuator/health
```

## Just a (working) proof of concept

```bash
docker manifest create dashaun/java-native-builder-multiarch:7.37.0 --amend dashaun/java-native-builder-arm64:7.37.0 --amend paketobuildpacks/builder:tiny
docker manifest push dashaun/java-native-builder-multiarch:7.37.0
docker manifest create dashaun/java-native-builder-multiarch:latest --amend dashaun/java-native-builder-arm64:7.37.0 --amend paketobuildpacks/builder:tiny
docker manifest push dashaun/java-native-builder-multiarch:latest
```

## See Also

This article has the instructions for use, validating and replicating the work.

[The related blog post](https://dashaun.com/posts/multiarch-builder-poc/)
