# Async Operations

# Q1: Compare async await in Javascript and Kotlin
It is known that fetchUser is async.

Javascript version,
```javascript
fetchUserAsync()
    .then(scope) { user -> fetchProfile(user.id) }
    .then(scope) { profile -> profile.name }
```
Kotlin non-blocking version,
```kotlin
suspend fun loadUserName(): String {
    return try {
        val user = fetchUser() // suspends here if needed
        val profile = fetchProfile(user.id) // suspends here if needed
        profile.name
    } catch (e: Exception) {
        println("Error occurred: ${e.message}")
        "Unknown User"
    }
}

```
or blocking version: 
```kotlin
fun main() = runBlocking {
    val name = loadUserName() // suspending call, but runBlocking blocks the main thread
    println("Loaded name: $name")
}
```
