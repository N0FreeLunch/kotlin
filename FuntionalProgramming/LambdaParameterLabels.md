# 람다 매개변수 레이블

람다 매개변수 레이블은 함수의 파라메터로 함수를 사용한 경우 파라메터인 함수의 시그니처에 이름을 붙여주는 기능을 의미한다.

파라메터로 사용되는 함수 시그니처는 전달되는 함수가 전달 받는 함수 내부에서 사용될 때, 타입 불일치를 방지하기 위해서 반드시 필요한 기능이다. 타입 불일치를 확인하기 위해 시그니처를 정하는 것이고, 시그니처의 변수명을 지정하는 것이 아니기 때문에 레이블은 코드의 의미를 알기 쉽게 하기 위해 주석을 대체하는 구문일 뿐, 로직이나 동작에 영향을 주지는 얺는다.

## 람다 매개변수 레이블

```kotlin
fun setListItemListener(
    listener: (Int, Int, View, View) -> Unit
) {
    listeners = listeners + listener
}
```

위의 예에서 `listener`의 각각의 인자는 타입만 있어서 각각의 함수 파라메터가 어떤 역할을 하는지 알 수 없는 경우가 있다. `Int`가 2개 `View`가 2개인 코드가 예시인 이유도, 동일한 타입이지만 서로 다른 역할을 가질 것인데, 어떤 값을 넣어야 하는지 모호성을 알려주기 위해서이다.

이런 경우를 위해서 코틀린은 파라메터의 함수 시그니처를 타입으로만 표기하는 것 뿐만 아니라 다음과 같이 이름을 붙이는 기능을 제공한다. 이를 매개변수 이름 힌트(Parameter Name Hints) 또는 람다 매개변수 레이블(Lambda Parameter Labels)라고 부른다.

```kotlin
fun setListItemListener(
    listener: (position: Int, id: Int, child: View, parent: View) -> Unit
) {
    listeners = listeners + listener
}
```

위의 코드에서, `position`, `id`, `child`, `parent`는 파라메터가 아니다. 시그니처를 구성하는 각각에 알기 쉽게 이름을 붙여준 것이라고 할 수 있다. 함수 시그니처로 파라메터를 나타낸 곳에 함수를 전달할 때 로직상 활용되는 것은 타입 정보와 함수 그 자체의 로직이며, 시그니처의 타입에 붙인 레이블은 아무런 로직상의 영향이 없다는 것에 주의하자.

## 명명된 매개변수와 개념 혼동 주의

람다 매개변수 레이블의 개념을 명명된 매개변수와 혼동하지 않도록 하자.

### 명명된 인자

함수를 사용할 때, 함수의 파라메터가 요구하는 인자를 전달 해 줘야 한다. 함수를 정의하는 쪽에서는 각각의 파라메터가 무엇을 하기 위해서 전달 받아야 하는 변수인지 변수명을 보고 알 수 있는 반면, 함수를 사용하는 측에서는 파라메터가 정의된 순서대로 값을 나열해 주어야 한다. 문제는 함수를 사용할 때는 몇 번째 파라메터가 무슨 역할을 하는지 알 수 없다는 점이다.

```kotlin
fun repeat(word: String, count: Int): String {
    var result = ""

    for (i in 1..count) {
        result += word
    }

    return result
}

fun main() {
    println(repeat("kotlin", 5))  // "kotlinkotlinkotlinkotlinkotlin"
}
```

많은 경우, `repeat("kotlin", 5)`와 같이 파라메터 이름 없이, 함수명과 전달된 값만으로도 어떤 기능을 하는 함수인지 알 수 있지만, 가끔 파라메터 이름을 모르고서는 정확히 어떤 기능을 하는 것인지 알기 어려운 경우가 있다. 이 때 명명된 매개변수를 사용하면, 각각의 몇 번째 파라메터에 어떤 파라메터를 전달하는지를 알 수 있기 때문에 이해하기 쉬운 코드가 된다.

```kotlin
fun main() {
    println(repeat(word="kotlin", count=5))  // "kotlinkotlinkotlinkotlinkotlin"
}
```



