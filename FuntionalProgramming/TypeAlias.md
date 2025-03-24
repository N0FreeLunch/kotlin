# 타입 별칭

때때로 타입 정의가 길어지는 경우가 있다. 긴 타입 정의를 반복해서 사용하는 것이 아니라, 타입 별칭을 만들어서 타입 별칭을 반복적으로 사용하여 코드가 장황해지는 것을 방지할 수 있다.

```kotlin
class View(val name: String) {
    override fun toString() = "View(name=$name)"
}

private var listeners =
    emptyList<(Int, Int, View, View) -> Unit>()

fun setListItemListener(
    listener: (
        position: Int, id: Int,
        View, parent: View
    ) -> Unit
) {
    listeners = listeners + listener
}

fun removeListItemListener(
    listener: (Int, Int, View, View) -> Unit
) {
    listeners = listeners - listener
}

fun notifyListeners(position: Int, id: Int, itemView: View, parentView: View) {
    for (listener in listeners) {
        listener(position, id, itemView, parentView)
    }
}

fun main() {
    val itemView = View("ItemView")
    val parentView = View("ParentView")

    val listener: (Int, Int, View, View) -> Unit = { pos, id, item, parent ->
        println("Listener called with pos=$pos, id=$id, item=$item, parent=$parent")
    }

    setListItemListener(listener)
    println("After adding listener:")
    notifyListeners(1, 100, itemView, parentView)
    
    removeListItemListener(listener)
    println("After removing listener:")
    notifyListeners(2, 200, itemView, parentView) // No output expected
}
```

위의 코드에서 `ListItemListener`라는 타입 별칭을 만들어 반복되는 타입을 간결하게 만들었다.

```kotlin
class View(val name: String) {
    override fun toString() = "View(name=$name)"
}

private typealias ListItemListener = (Int, Int, View, View) -> Unit

private var listeners =
    emptyList<ListItemListener>()

fun setListItemListener(
    listener: (
        position: Int, id: Int,
        View, parent: View
    ) -> Unit
) {
    listeners = listeners + listener
}

fun removeListItemListener(
    listener: ListItemListener
) {
    listeners = listeners - listener    
}

fun notifyListeners(position: Int, id: Int, itemView: View, parentView: View) {
    for (listener in listeners) {
        listener(position, id, itemView, parentView)
    }
}

fun main() {
    val itemView = View("ItemView")
    val parentView = View("ParentView")

    val listener: ListItemListener = { pos, id, item, parent ->
        println("Listener called with pos=$pos, id=$id, item=$item, parent=$parent")
    }

    setListItemListener(listener)
    println("After adding listener:")
    notifyListeners(1, 100, itemView, parentView)
    
    removeListItemListener(listener)
    println("After removing listener:")
    notifyListeners(2, 200, itemView, parentView) // No output expected
}
```

```kotlin
private typealias ListItemListener = (Int, Int, View, View) -> Unit
```

타입 별칭을 보면, private 접근 제한자를 사용한다. 타입 별칭에 사용할 수 있는 접근 제한자는 public, internal, private가 있고, private는 선언된 파일 안에서만 사용할 수 있고, internal은 같은 모듈에서만 접근할 수 있고, public은 어디에서든 이 타입에 접근할 수 있다는 차이점을 갖는다. 주의할 점은 protected 접근　제한자는 타입 별칭에 사용할 수 없다는 점이다.

## 타입 별칭의 특징

장황한 타입 정의를 별칭을 달아 반복적으로 사용하기 쉽게하는 것으로, 컴파일이 되면 다시 장황한 타입으로 변경된다. 타입 별칭은 변수 처럼 복사와 참조가 되는 것이 아니라, 장황한 코드를 간단히 대체하기 위함이다. 타입별칭을 장황한 타입 정의로 언제든지 대체해도 괜찮다.

```kotlin
data class User(val name: String)

typealias Users = List<User>

fun updateUsers(users: Users) {
    users.forEach(::println)
}

fun main() {
    val users: Users = listOf(User("Alice"), User("Bob"))

    updateUsers(users)
}
```

데이터 클래스 `data class User(val name: String)`를 사용하면, `toString()`의 값으로 `User(param=argValue)`으로 클래스(파라메터=인자로 전달한 값)의 꼴로 문자열이 출력된다.

`Users` 타입을 타입 별칭으로 정의하여 여러 타입이 자리하는 곳에 별칭을 사용하였다.

`::println`는 함수 참조를 의미하며, 코틀린에 내장된 함수를 직접 사용한다는 의미를 가진다. 함수 참조가 아닌 람다식을 사용해서 `users.forEach { user -> println(user) }`으로 사용해도 된다.

## 타입별칭으로 라이브러리 타입 충돌 방지

다음 예제의 두 코드는 동일한 코드이며, 타입 별칭만 부여하였다.

```kotlin
import thirdparty.Name

class Foo {
    val name1: Name
    val name2: my.Name
}
```

`import` 할 때는 .의 가장 마지막 이름이 타입이 된다. `import thirdparty.Name`의 타입은 `Name`이 된다.

타입 부분에 .을 사용한 코드가 있다면 패키지에서 타입을 가져오는 것을 의미한다. `my` 패키지에서 `Name`이란 타입을 가져온 것이다.

```kotlin
import my.Name

typealias ThirdPartyName = thirdparty.name

class Foo {
    val name1: ThirdPartyName
    val name2: Name
}
```

`my.Name`의 타입은 `Name`이 되고, `thirdparty.name`의 타입도 `name`이 되지만, 타입 별칭을 부여하여 `ThirdPartyName`가 되어 타입의 이름이 겹치지 않도록 하였다.
