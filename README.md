Important notes of 'Kotlin for Android Developers' book by Antonio Leiva.

# 1: Introduction
This book can help you understand the various ways how Kotlin can take you one step ahead and make your code much better.

## 1.1: What is Kotlin?
And the support for this language
comes from the company who develops the IDE, so we Android developers are first-class citizens.

* **More expressive**: Write more with less code.

  No need to getters, setters and toString(), equals(), hashCode().

* **Safer**: Null safe. Possible null situations in compile time. Explicitly specify an object can be null. Save a lot of time debugging null pointer exceptions.

  Java: Big amount of code is defensive.
  ```kotlin
  var artist: Artist? = null
  artist?.print()
  val name = artist?.name ?: "empty"
  ```
* **Functional**: Uses many concepts from functional programming: lambda exp, dealing with collections.

  lambda exp:
  ```kotlin
  view.setOnClickListener { toast("Hello world!") }
  ```
* **Extension functions**: Extend any class without having access to the source code.
  ```kotlin
  fun Fragment.toast(message: CharSequence, duration: Int = Toast.LENGTH_SHORT){
    Toast.makeText(getActivity(), message, duration).show()
  }

  fragment.toast("Hi")
  ```
* **Interoperable** with java libs and code.

# 3: Creating a new project
Automated convert from Java to Kotlin:

`Code -> Convert Java File to Kotlin File.`

Kotlin Android Extensions' magic: No need to findViewById()
```kotlin
<TextView
android:id="@+id/message"
android:text="@string/hello_world"
android:layout_width="wrap_content"
android:layout_height="wrap_content"/>

override fun onCreate(savedInstanceState: Bundle?) {
  super.onCreate(savedInstanceState)
  setContentView(R.layout.activity_main)
  message.text = "Hello Kotlin!"
}
```
We can use `message.text` instead of `message.setText` for free.

# 4: Classes and functions

  * Class and its constructor:
    ```kotlin
    class Person(name: String, surname: String) {
      init {
        ...
      }
    }
    ```
  * Class inheritance:
    * A class always extends from <code>**Any**</code> (similar to Java `Object`).
    * Classes are closed by default (final).
    * We can only extend a class if declared `open` or `abstract`:
      ```kotlin
      open class Animal(name: String)
      class Person(firstName: String, lastName: String) : Animal(firstName)
      ```
  * Functions:
    ```kotlin
    fun onCreate(savedInstanceState: Bundle?){
    }
    ```
    * Always return a value. default = `Unit` similar to `void` in Java.
    * Single expression functions:
    ```kotlin
    fun add(x: Int, y: Int) : Int = x + y
    ```
