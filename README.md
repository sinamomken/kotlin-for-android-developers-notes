Important notes of 'Kotlin for Android Developers' book by Antonio Leiva.

# 1 Introduction
This book can help you understand the various ways how Kotlin can take you one step ahead and make your code much better.

## 1.1 What is Kotlin?
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

# 3 Creating a new project
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

# 4 Classes and functions
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
    * Default value for parameters (preventing need of function overloadings):
      ```kotlin
      fun niceToast(message: String, length: Int = Toast.LENGTH_SHORT) {
        Toast.makeText(this, message, length).show()
      }
      ```
    * Named arguments in function calls:
      ```kotlin
      niceToast(message = "Hello", length = Toast.LENGTH_SHORT)
      ```
    * String templates: just write the $ symbol.
      ```kotlin
      "[$className] $message"
      "Your name is ${user.name}"
      ```

# 5 Writing your first class
  * Defining variables and casting with `val` and `as`:
    ```kotlin
    val forecastList = findViewById(R.id.forecast_list) as RecyclerView
    ```
  * Instantiation: "`Class(this)`" in **kotlin** instead of "`new Class(this)`" in **java**.
  * Default visibility for classes, functions or properties is `public`.
  * Creating constant lists (immutable lists) by using function `listOf()` which infers type of arguments.

# 6 Variables and properties
  * **Everything is an object.** There is ~no primitive type~.
  * No automatic conversion b/w numertic types.
    ```kotlin
    val i: Int = 7
    val d: Double = i.toDouble()

    val c: Char = 'c'
    val i: Int = c.toInt()

    ```
  * Bitwise operations:
    ```kotlin
    val bitwiseOr = FLAG1 or FLAG2 // FLAG1 | FLAG2; in java
    val bitwiseAnd = FLAG1 and FLAG2 // FLAG1 & FLAG2; in java
    ```
  * Compiler can infer the type from the literals:
    ```kotlin
    val i = 12 // An Int
    val iHex = 0x0f // Still an Int
    val l = 3L // A Long
    val d = 3.5 // A Double
    val f - 3.5F // A Float
    ```
  * A `String` can be accessed as an array and also iterated:
    ```kotlin
    val s = "Example"
    val c = s[2] // Char 'a'

    for (c in s){
      print(c)
    }
    ```
  * Variables: **mutable** (`var`) or **immutable** (`val`)
    * `val` in kotlin is similar to `final` in java
    * **immutability**: If you need a new version, a new object needs to be created. Makes software much more robust and predictable.
    * Java: most objects are mutable -> Any part of code changing object may affect rest of the app.
    * **Immutable objects** are **thread-safe** by definition.
    * **Just use `val` as much as possible.**
