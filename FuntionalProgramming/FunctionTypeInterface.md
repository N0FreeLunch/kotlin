# 함수 타입은 인터페이스

```kotlin
class OnClick : (Int) -> Unit {
    override fun invoke(viewId: Int) {
        println("clicked viewId: $viewId")
    }
}

fun setListener(l: (Int) -> Unit) {
    l(3)
}

fun main() {
    val onClick = OnClick()
    setListener(onClick)
}
```

`Function1`은 코틀린에 내장된 함수 타입을 나타내는 인터페이스이다. 이 함수 타입을 나타내는 인터페이스의 제네릭으로 `<Int, Unit>`으로 파라메터의 타입과, 반환 타입(가장 마지막에 나열된 타입) 을 지정하는 것과 동일하다. 이를 시그니처의 형태로 함수 타입을 나타내도록 한 것이 함수 타입이다.

```kotlin
class OnClick : Function1<Int, Unit> {
    override fun invoke(viewId: Int) {
        println("clicked viewId: $viewId")
    }
}

fun setListener(l: (Int) -> Unit) {
    l(3)
}

fun main() {
    val onClick = OnClick()
    setListener(onClick)
}
```

위와 같이 `Function1<Int, Unit>` Int를 받아서 Unit을 반환하는 `Function1` 인터페이스를 만든것도 함수 타입 `(Int) -> Unit`를 한 것과 동일하게 동작한다는 것을 알 수 있다.
