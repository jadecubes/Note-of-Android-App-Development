# Gradle
✅ Android (Gradle)

In Android, you don’t need to run an explicit install command like pnpm install.

Instead:

```
./gradlew build
```
or
```
./gradlew assembleDebug
```

- Gradle will automatically download any missing dependencies

- It uses your build.gradle.kts or build.gradle to figure out what to fetch

- It caches everything in ~/.gradle/caches, similar to node_modules, but global

🔄 When do dependencies get resolved?
Task                   | Behavior
./gradlew build        | Resolves dependencies and builds the app
./gradlew dependencies | Lists and verifies all dependencies
Android Studio sync    | Automatically runs dependency resolution behind the scenes when you edit build.gradle

🔧 How to enable dependency locking in Gradle
1. Add this to your root gradle.properties:
```
dependencyLocking.enabled=true
```
2. In your build.gradle.kts or build.gradle:
```
dependencyLocking {
    lockAllConfigurations()
}
```
3. Generate the lock file:
```
./gradlew dependencies --write-locks
```

4. This creates files like:
```
gradle/dependency-locks/runtimeClasspath.lockfile
```

🚦 Enforcing locked dependencies
To ensure builds always use locked versions:
```
./gradlew build --update-locks your.group:dependency
```
Or configure Gradle to fail the build if locks are out-of-sync:
```
dependencyLocking.lockMode=strict
```

🔍 Version is defined in libs.versions.toml (Version Catalog)
