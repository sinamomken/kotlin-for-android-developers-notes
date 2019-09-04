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
      var artist: Artist? = null
      artist?.print()
      val name = artist?.name ?: "empty"

* **Functional**: Uses many concepts from functional programming: lambda exp, dealing with collections.

    lambda exp:
      view.setOnClickListener { toast("Hello world!") }


* **Extension functions**: Extend any class without having access to the source code.

      fun Fragment.toast(message: CharSequence, duration: Int = Toast.LENGTH_SHORT){
        Toast.makeText(getActivity(), message, duration).show()
      }

      fragment.toast("Hi")

* **Interoperable** with java libs and code.

# 3: Creating a new project
