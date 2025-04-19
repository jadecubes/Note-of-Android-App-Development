# State Sharing
We use question asking to explore the topic.
## Questions and Answers


### What's the basic knowledge about state setting in Android app development?
Official document: https://developer.android.com/develop/ui/compose/state
1. 🔥 Default to rememberSaveable, and elevate to ViewModel when state needs to be shared or orchestrated.
2. 
```
Compose                       | React Equivalent        | Behavior on Refresh/Reload
remember { mutableStateOf() } | useState                | Gone on full reload
rememberSaveable { ... }      | localStorage + useState | Persists on reload
ViewModel + StateFlow         | Redux / Context         | Persists as long as global
```