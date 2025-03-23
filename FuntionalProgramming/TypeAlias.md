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

위의 코드에서 `listeners`라는 타입 별칭을 만들어 사용하였다.

```kotlin
private var listeners =
    emptyList<(Int, Int, View, View) -> Unit>()
```
