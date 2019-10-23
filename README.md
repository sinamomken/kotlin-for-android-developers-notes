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
    val f = 3.5F // A Float
    val actionBar = supportActionBar // An ActionBar
    ```
    But for using more generic types (polymorphism), a type must be specified.
    ```kotlin
    val a: Any = 23
    val c: Context = someActivity
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
    * **Just use `val` as much as possible** (except when class constructor is not available).
  * Properties: equivalent to fields in Java + getters + setters.
    * Modifying default getters and setters:
      ```kotlin
      class Person {
        var name: String = ""
          get() = field.toUpperCase()
          set(value){
            field = "Name: $value"
          }
      }
      ```
    * Kotlin can use the property syntax when dealing with code written in Java where a getter is defined in Java.

# 7 Anko and Extension functions
  * Anko: Generation of UI layouts by code instead of XML.
    * Using XML should be easier.
    * Anko uses **extension functions** to add new features to Android framework. Example:
      ```kotlin
      val forecastList: RecyclerView = find(R.id.forecast_list)
      ```
    * Features: instantiation of intents, navigation b/w activities, creating fragments, db access, alerts creation, etc.
  * Extension function: adding new behavior to class without access to its source
    * In java, this is usually implemented in utility classes with static methods.
    * Example:
      ```kotlin
      fun Context.toast(message: CharSequence, duration: Int = Toast.LENGTH_SHORT){
        Toast.makeText(this, message, duration).show()
      }
      ```
    * Anko includes its own `toast()` and `longToast()` extension functions.
    * We also have extension properties:
      ```kotlin
        val ViewGroup.childViews: List<View>
          get() = (0 until childCount).map { getChildAt(it) }
      ```
    * Extension functions don't modify the original class but are add as static import.
    * Common practice: create files including a set of related functions.

# 8 Retrieving data from API
  * ***OpenWeatherMap*** API can be used to retrieve weather data.
  * Kotlin can use **Retrofit** (written in Java) for server requests.
  * Performing a request: basics
    ```kotlin
    class Request(val url: String) {
      fun run() {
        val forecastJsonStr = URL(url).readText()
        Log.d(javaClass.simpleName, forecastJsonStr)
      }
    }
    ```
    * Implementation is easy using `readText()`, an extension function from Kotlin standard library.
    * In Java we needed: `HttpURLConnection`, `BufferedReader` & many other checkings.
  * Performing the request: executing out of the main thread
    * HTTP requests are not allowed in the main thread. A common solution in Android = `AsyncTask`.
    * `AsyncTask`s may be dangerous: When reaches `postExecute()`, activity may be destroyed.
    * Anko: `doAsync(){}` function for executing in another thread with `uiThread{}`option for returning to the main thread:
      ```kotlin
      doAsync() {
        Request(url).run()
        uiThread { longToast("Request performed") }
      }
      ```
    * `uiThread` is safe: it checks finishing or existence of the caller `Activity`.
    * `doAsync` returns a java `Future`. For getting a future with a result use `doAsyncResult`.

# 9 Data classes
A powerful kind of classes instead of POJO classes in Java.
```kotlin
data class Forecast(val date: Date, val temperature: Float, val details: String)
```
  * Data class extra functions: ‍‍‍`equals()`, `hashCode()`, `copy()`, etc.
  * Use of `copy` when using immutability:
    ```kotlin
    val f1 = Forecast(Date(), 27.5f, "Shiny day")
    val f2 = f1.copy(temperature = 30f)
    ```
    * Java classes (e.g. `Date` class above) are not immutable by default. To force immutability you can wrap them (e.g. `ImmutableDate` wrapping `Date`).
    * **Declaration destructuring** = Mapping each property inside an object into a variable:
    ```kotlin
    val f1 = Forecast(Date(), 27.5f, "Shiny day")
    val (date, temperature, details) = f1
    ```
    equals to:
    ```kotlin
    val date = f1.component1()
    val temperature = f1.component2()
    val details = f1.component3()
    ```
    Another example: Iterating over `Map`'s keys and values:
    ```kotlin
    for ((key, value) in map){
      Log.d("map", "key:$key, value:$value")
    }
    ```

# 10 Parsing data
  * Unlike Java, each `*.kt` file can contain more than 1 Kotlin class.
  * **Gson**: to parse json to our classes. Properties must have the same name as the ones in the json, or specify a serialised name.
  * **`companion object`** s of kotlin instead of `static` properties, constants and functions of Java:
    ```kotlin
    class ForecastRequest(val zipCode: String) {
      companion object {
        private val APP_ID = "15646a06818f61f7b8d7823ca833e1ce"
        private val URL = "http://api.openweathermap.org/data/2.5/" +
                "forecast/daily?mode=json&units=metric&cnt=7"
        private val COMPLETE_URL = "$URL&APPID=$APP_ID&q="
      }

      fun execute(): ForecastResult{
        val forecastJsonStr = URL(COMPLETE_URL + zipCode).readText()
        return Gson.fromJson(forecastJsonStr, ForecastResult::class.java)
      }
    }
    ```
  * Every function in Kotlin returns a value. If nothing specified, returns an object of `Unit` class.
  * Definition of a `Command` interface:
    ```kotlin
    public interface Command<out T>{
      fun execute(): T
    }
    ```
  * In this architecture, 1st we retrieve data from API and fill `data class`es in **data** package using Gson, and then convert needed info into **domain** `data class`es.
  * We can convert a list easily without directly using loops by using functional operations over lists:
    ```kotlin
    return list.mapIndexed {i, forecast -> ...}
    ```
  * `with` function in standard Kotlin library:
    * Gets an object and an extension function as parameters, runs the function on the object.
    * Really helpful when doing several operations overt the same object.

# 11 Operator overloading
  * Implement a function with a reserved name mapped to the symbol.
    ```kotlin
    a++ -> a.unaryPlus()
    a..b -> a.rangeTo(b)
    a[i] -> a.get(i)
    a[i]=b -> a.set(i,b)
    a==b -> a?.equals(b)?:b===null
    a(i) -> a.invoke(i)
    ```
  * Improves code readability and simplicity.
  * The reserved function must be annotated with `operator` modifier.
  * Types of operators: unary, binary, array-like, equals, function invocation
  * equal operator (==) must be implemented exactly like this"
    ```kotlin
    operator fun equals(other: Any?): Boolean
    ```
  * Kotlin lists have the array-like operations.
  * We could extend **existing classes** using ***extension functions*** to provide **<u>new operators</u>** on them.

# 12 Making the forecast list clickable
  * ***Picasso***: An image loader library, like *glide*.
  * Defining a setOnClickListener interface with `invoke()` operator can simplify call of the listener:
    ```kotlin
    interface OnItemClickListener{
      operator fun invoke(forecast: Forecast)
    }

    itemclick.invoke(forecast)
    itemclick(forecast)
    ```
  * Implementing an anonymous class in Kotlin: Create an `object` (not `Object`) implementing the desired interface
    ```kotlin
    view.setOnClickListener(
      object: OnItemClickListener{
          override fun invoke(item: Item){
              toast(item.text)
          }
      })
    ```
    * It's not nice. We should instead make use of functional programming with lambdas.

# 13 Lambdas
Lambda expression = Simple way to define an anonymous function (a function w/o name).
  * In Kotlin a function behaves as a type, so it can be:
    * passed as an argument
    * returned by a function
    * saved into a var or property
  * Any function that receives an <u>interface with a single function</u> can be substituted by a <u>lambda</u>.
    * Like `setOnClickListener()` is defined as:
      ```kotlin
      fun setOnClickListener(listener: (View) -> Unit) // a higher order fun accepting another fun
      ```
    * Unlike Java, a *SAM (Single Abstract Method) Interface* `val` ~can't be passed~ instead of a *lambda* function `val`.
  * Without lambda expression:
    ```kotlin
    view.setOnClickListener(object: OnClickListener{
        override fun onClick(v: View){
          toast("Click")
        }
      })
    ```
    With lambda expression:
    ```kotlin
    view.setOnClickListener({ view -> toast("Click")})
    // or
    view.setOnClickListener({toast("Click")}) //input values are of no use
    // or
    view.setOnClickListener(){toast("Click")} //if the last argument is a (lambda) function
    // or
    view.setOnClickListener{ toast("Click") } //if function is the only parameter
    ```
    Unlike Java, in Kotlin we must use `{}` for defining lambda expressions.
  * In lambdas with only one argument, we can ignore the argument and use **`it`**.
    ```kotlin
    val adapter = ForecastListAdapter(result) { toast(it.date) }
    ```
  * Unusable knowledge: A simple Implementation of `with` in Kotlin:
    ```kotlin
    inline fun <T> with(t: T, body: T.() -> Unit) { t.body() }
    // Gets an object of type T and a function that will be used as an extension function.
    ```
  * Benefit of an inline function: Reduces memory allocations and runtime overhead in some situations.
    * Useful example:
    ```kotlin
    inline fun supportsLollipop(code: () -> Unit){
      if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP){
        code()
      }
    }

    supportsLollipop{
      window.setStatusBarColor(color.BLACK)
    }
    ```

# 14 Visibility Modifiers
  * Modifiers:
    * private
      * `private` class: Not accessible outside the file where it was defined.
    * protected
    * internal: visible inside the whole module if it's a package member. We can use internal classes from any other class in the same module.
      * module = Android Studio modules = can be compiled, run, tested and debugged independently.
    * public
      * Default modifier in Kotlin
  * Constructors: public by default
  * Note: No need to specify the return type of a function if can be computed by the compiler.
    ```kotlin
    operator fun get(position: Int) = dailyForecast[position]
    // instead of
    operator fun get(position: Int): Forecast = dailyForecast[position]
    ```

# 15 Kotlin Android Extensions
  * View binder: Automatically creates a set of properties giving access to all views in a XML. So we don't need to explicitly find all the views in the layout before use.
    * Names come from ids of the views. So XML `id`s should not have `_` and must be in CamelCase format.
    * Types also taken from the XML.
  * Benefit: Doesn't add any extra libs to our code. Instead consists of a **plugin** generating the code.
    * Plugin substitutes properties call into a function requesting the view.

How to add it to our project:
```groovy
apply plugin: 'kotlin-android-extensions'
```

How to use it in our code:
  * In Activities or Fragments (regular):
    * `import kotlinx.android.synthetic` + name of the XML. ex:
      `import kotlinx.android.synthetic.main.activity_main`
    * We can access teh views after `setContentView`.
  * In Views: we may need to access XML views in many other parts of the code.
    * `import kotlinx.android.synthetic.main.view_item.view.*`
    * Access by `itemView.textView.text = "Hello"`.
    * It won't cache the views.
      * Every bind will do `findViewById` under the hood for all subviews.
      * If your layout is complex, this may make it slower, specially the `RecyclerView` performance.

# 16 Application Singleton and Delegated Properties
  * Old (not recommended) way of creating a singleton, like in Java:
    ```kotlin
    class App :‌ Application(){
      companion object{
        private var instance: Application? = null
        fun instance() = instance!!
      }

      override fun onCraete(){
        super.onCreate()
        instance = this
      }
    }
    ```
  * Reimplementing App singleton in Kotlin (using concepts below):
    ```kotlin
    class App : Application(){
      companion object{
        lateinit var instance: App
          private set
      }

      override fun onCreate(){
        super.onCreate()
        instance = this
      }
    }
    ```
  * Interesting behaviors in a property: **Lazy values** and **Observable properties**.
  * **Property Delegate**: Instead of having to declare the same code over and over again, delegate the code a property needs to another class (property delegate).
    * Ex: Delegating `get` or `set` of a property to another class:
      ```kotlin
      class Delegate<T> {
        operator fun getValue(thisRef: Any?, property: KProperty<*>): T {
          return ...
        }
        operator fun setValue(thisRef: Any?, property: KProperty<*>, value: T) {
          ...
        }
      }

      class Example{
        var p: String by Delegate()
      }
      ```
      `T`: type of the property delegating its behavior
      `thisRef`: reference to class of the property
      `property`: metadata of the property
      `by`: reserved word to specify the delegation.
  * Standard delegates in Kotlin Standard Library:
    * **lazy**: takes a lambda executed *first* time `getValue` is called.
      * ex:
        ```kotlin
        class App : Application() {
          val database: SQLiteOpenHelper by lazy {
            MyDatabaseHelper(applicationContext)
          }

          override fun onCreate() {
            super.onCreate()
            val db = database.writableDatabase
          }
        }
        ```
      * Subsequent calls will return the same value.
      * Interesting when the property is not always required.
        * saves memory
      * Needed when some other parts must be ready before this one.
        * in ex above we are sure `applicationContext` exists during initialization.
      * `lazy` is thread safe by default.
    * **observable**: takes a lambda executed after *every* time `set` function is called.
      * Helps us detect changes on any property we need to observe.
      * In the observable we receive 'delegated property', 'old value' and 'new value'.
      * ex:
        ```kotlin
        class ViewModel(val db: MyDatabase) {
          var myProperty by Delegates.observable("") {
            _, _, new -> db.saveChanges(this, new)
          }
        }
        ```
      * Note: you can use `_` for unused lambda arguments.
    * **vetoable**: a special kind of observable deciding whether the value must be saved?
      * ex:
        ```kotlin
        var positiveNumber = Delegates.vetoable(0){
          _, _, new -> new >= 0
        }
        ```
      * Can be used to check some conditions before saving a value.
    * **lateinit**: not a delegate, but a property modifier.
      * A non abstract property must have a value before finish of the constructor, which may not be available in Activities, etc. **Solutions**:
        1. Define property as *nullable* and set it to null. **Drawback**: we need to check nullity every time. If property won't be null when used -> unnecessary code. (Java & Kotlin)
        2. **`lateinit`**: Property should have non-nullable value but its assignment will be delayed. If the value is requested before being assigned -> throws an exception.
      * ex:
        ```kotlin
        class App : Application() {
          companion object {
            lateinit var instance: App
          }

          override fun onCreate() {
            super.onCreate()
            instance = this
          }
        }
        ```
      * **NOTE**: `lateinit` can only be used for `var`.
    * **by map**: delegates value of a property to be filled from a map (taken in constructor); name of the property = key of the map.
      * ex:
        ```kotlin
        class Configuration(map: Map<String, Any?>) {
          val width: Int by map
          val height: Int by mapped
          val dp: Int by mapped
          val deviceName: String by map
        }

        val conf = Configuration(mapOf(
            "width" to 1080,
            "height" to 720,
            "dp" to 240,
            "deviceName" to "mydevice"
          ))
        ```
      * Easily creates an object from a dynamic map.

# 17 Creating an SQLiteOpenHelper
  * Old way to get objects from DB: Write SQL sentences, then map objects into `ContentValues` or take them from `Cursors`.
  * New way: Use ORMs for Android like **GreenDAO** or **Room** (google jetpack).

## Dependency injection using Dagger:
Instead of *instantiating collabators inside the class*, we provide them **via constructor**.
  * More **testable**, easy to extend and maintain.
    * Substitute collaborators with other objects (same interface) or make use of **mocks** in tests.

**Dagger**: Most popular dependency injector for Android.
  * Simpler alternative = use of default values in constructors.
    ```kotlin
    class ForecastDbHelper(ctx: Context = App.instance){
      ...
    }

    val dbHelper1 = ForecastDbHelper() // It will use App.instance
    val dbHelper2 = ForecastDbHelper(mockedContext) // For tests, for example
    ```

# 18 Collections and functional operations
Functional programming advantage: We just say **what we want to do**, *not explaining how to do it*.
  * Ex. filtering a list:
    * Non-functional: creat a list, iterate over original list and add selected items to the new list.
    * Functional: use a filter function with a specified filter.
  * Say a lot more using less code.
  
Kotlin native **collection**-related interfaces:
  * **Iterable**: the parent interface.
  * **MutableIterable**
  * **Collection**: including functions for *size*, *is empty*, *contains* & etc.
  * **MutableCollection**: plus *add*, *remove*, *clear*, etc.
  * **List**: ordered. Get an item by `get(position)` function.
  * **MutableList**
  * **Set**: unordered, no duplicate elements allowed.
  * **MutableSet**
  * **Map**: a collection of key-value pairs.
  * **MutableMap**

**Functional operations** available over different collections:
  * **any**: `true` if any member matches condition.
  * **all**: `true` if all members match condition.
  * **count**: number of elements matching condition.
  * **fold**: 