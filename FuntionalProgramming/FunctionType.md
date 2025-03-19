# 함수를 타입으로 취급

함수 타입은 하나의 메소드만 제공하는데, invoke라는 메소드를 제공한다.

```kotlin
fun fetchText(
    onSuccess: (String) -> Unit,
    onFailure: (Throwable) -> Boolean
) {
    // isSuccess 부분을 true로 하면 성공 콜백 실행, false로 하면 실패 콜백 실행
    val isSuccess = false
    if (isSuccess) {
        onSuccess("some text")
    } else {
        val handled: Boolean = onFailure(Error("Some error"))
        println("Error handled: $handled")
    }
}

fun main() {
    fetchText(
        onSuccess = { text -> println("Success: $text") },
        onFailure = { error ->
            println("Failure: ${error.message}")
            true // 오류를 처리했다고 가정
        }
    )
}
```

위의 예제 코드에서 onSuccess, onFailure는 함수를 담고 있는 변수이다. 함수는 하나의 메소드 invoke를 갖는데, 변수가 함수를 담고 있을 때는 `onSuccess.invoke()`, `onFailure.invoke()`와 같은 코드를 써 주면 해당 변수가 함수인지 쉽게 파악하고 읽을 수 있다.

```kotlin
fun fetchText(
    onSuccess: (String) -> Unit,
    onFailure: (Throwable) -> Boolean
) {
    // isSuccess 부분을 true로 하면 성공 콜백 실행, false로 하면 실패 콜백 실행
    val isSuccess = true
    if (isSuccess) {
        onSuccess.invoke("some text")
    } else {
        val handled: Boolean = onFailure.invoke(Error("Some error"))
        println("Error handled: $handled")
    }
}

fun main() {
    fetchText(
        onSuccess = { text -> println("Success: $text") },
        onFailure = { error ->
            println("Failure: ${error.message}")
            true // 오류를 처리했다고 가정
        }
    )
}
```

### 람다 함수의 특징

```kotlin
{ error
    -> println("Failure: ${error.message}")
    true
}
```

위 람다함수의 시그니처는 `(Throwable) -> Boolean`이다. 따라서 반환 값으로 true를 반환하지만, 사이드 이펙트로 `println("Failure: ${error.message}")`를 실행하고 있다. 람다 함수의 `->` 다음에 오는 값이 무조건 반환값이 되는 것이 아니라, `->` 다음에 마지막에 위치한 값이 반환 값이 된다. 개행은 단순히 식의 끝을 나타내며 다음과 같이 세미콜론으로 끊어진 역할을 한다.

```kotlin
{ error -> println("Failure: ${error.message}"); true }
```

### 함수 파라메터가 nullable인 경우

```kotlin
fun fetchText(
    onSuccess: ((String) -> Unit)? = null,
    onFailure: ((Throwable) -> Boolean)? = null
) {
    // isSuccess 부분을 true로 하면 성공 콜백 실행, false로 하면 실패 콜백 실행
    val isSuccess = false
    if (isSuccess) {
        onSuccess?.invoke("some text")
    } else {
        val handled: Boolean? = onFailure?.invoke(Error("Some error"))
        println("Error handled: $handled")
    }
}

fun main() {
    fetchText(
        onSuccess = { text -> println("Success: $text") }
    )
}
```

함수의 파라메터에 인자를 할당하지 않으면 null을 세팅하는 함수를 만들었다. `((String) -> Unit)`, `((Throwable) -> Boolean)`와 같이 함수의 시그니처가 함수의 타입을 의미한다. 따라서 시그니처를 괄호로 감싸서 하나의 타입으로 만들고 해당 타입에 ?를 붙이므로써 nullable 타입을 만들었다.

`onSuccess`, `onFailure` 변수는 nullable 타입의 변수이기 때문에 이 변수를 사용한 코드의 메소드를 호출할 때는 null일 때는 메소드 체인을 하지 않고 null을 반환하도록 `?.`의 코드를 추가해 주어야 한다. 그리고 변수 `handled`의 타입도 null이 들어갈 수 있는 Boolean? 타입으로 바꿔 주었다.
