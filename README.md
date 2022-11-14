# java-native-builder-multiarch
A single `Cloud Native Buildpack` that can build native images for AMD64 and ARM64

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
