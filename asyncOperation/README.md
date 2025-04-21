# Async Operations

# Q1: Compare async await in Javascript and Kotlin
It is known that fetchUser is async.

Javascript version,
```javascript
fetchUserAsync()
    .then(scope) { user -> fetchProfile(user.id) }
    .then(scope) { profile -> profile.name }
```
Kotlin version,
```kotlin
suspend fun loadUserName(): String {
    val user = fetchUser()
    val profile = fetchProfile(user.id)
    return profile.name
}
```
Or, with suspend functions:
```kotlin
suspend fun loadUserName(): String {
    val user = fetchUser()
    val profile = fetchProfile(user.id)
    return profile.name
}
```