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

타입 별칭을 보면, private 접근 제한자를 사용한다. 타입 별칭에 사용할 수 있는 접근 제한자는 public, internal, private가 있고, private는 선언된 파일 안에서만 사용할 수 있고, internal은 같은 모듈에서만 접근할 수 있고, public은 어디에서든 이 타입에 접근할 수 있다는 차이점을 갖는다. 주의할 점은 protected 접근제한자는 사용할 수 없다는 점이다.

## 타입별칭으로 라이브러리 타입 충돌 방지
