# State Sharing
We use question asking to explore the topic.
## Questions and Answers


### Q1: What's the basic knowledge about state setting in Android app development?
Official document: https://developer.android.com/develop/ui/compose/state
1. 🔥 Default to rememberSaveable, and elevate to ViewModel when state needs to be shared or orchestrated.
2. 
```
Compose                       | React Equivalent        | Behavior on Refresh/Reload
remember { mutableStateOf() } | useState                | Gone on full reload
rememberSaveable { ... }      | localStorage + useState | Persists on reload
ViewModel + StateFlow         | Redux / Context         | Persists as long as global
```

### Q2: How to code when a component uses multiple ViewModel configurations?
🧩 Scenario:

You're building a CouponScreen that handles:

    CouponFormViewModel

    ValidationViewModel

    TimerViewModel
- Define each ViewModel
```kotlin
class CouponFormViewModel : ViewModel() {
    var couponCode by mutableStateOf("")
    fun updateCode(newCode: String) {
        couponCode = newCode
    }
}

class ValidationViewModel : ViewModel() {
    var isValid by mutableStateOf(false)
    fun validate(code: String) {
        isValid = code.length >= 5
    }
}

class TimerViewModel : ViewModel() {
    var remainingSeconds by mutableStateOf(60)
    fun tick() {
        remainingSeconds -= 1
    }
}
```
- Group into a UI-level container
```kotlin
data class CouponScreenViewModels(
    val couponFormViewModel: CouponFormViewModel,
    val validationViewModel: ValidationViewModel,
    val timerViewModel: TimerViewModel,
)
```
- In your screen-level @Composable
```kotlin
@Composable
fun CouponScreen() {
    val couponFormVM = viewModel<CouponFormViewModel>()
    val validationVM = viewModel<ValidationViewModel>()
    val timerVM = viewModel<TimerViewModel>()

    val viewModels = remember {
        CouponScreenViewModels(
            couponFormVM,
            validationVM,
            timerVM,
        )
    }

    CouponContent(viewModels)
}
```
- In your CouponContent
```kotlin
@Composable
fun CouponContent(viewModels: CouponScreenViewModels) {
    val couponCode by viewModels.couponFormViewModel.couponCode
    val isValid by viewModels.validationViewModel.isValid
    val secondsLeft by viewModels.timerViewModel.remainingSeconds

    Column {
        TextField(
            value = couponCode,
            onValueChange = {
                viewModels.couponFormViewModel.updateCode(it)
                viewModels.validationViewModel.validate(it)
            },
            label = { Text("Enter Coupon") }
        )

        Text("Valid: $isValid")
        Text("Time Left: $secondsLeft sec")
    }
}
```  
