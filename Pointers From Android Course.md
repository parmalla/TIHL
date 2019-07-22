#### Pointers From Android Course
---
Vector Drawables are usable from API 19. Use support libraries to make them usable in lower android versions.
- Enable the use of support library for vector drawables in build.gradle file (app level):
   ```kotlin
   vectorDrawables.useSupportLibrary = true
   ```
- You’ll also need to add the namespace to the root of the layout:
   ```XML
   xmlns:app="http://schemas.android.com/apk/res-auto"
   ```
- Use app:srcCompat in the image tag in the layout file:
   ```XML
   app:srcCompat="@drawable/image_tag"
   ```
 ---
Always refactor when renaming a view or resource so that all references are changed in the code.
```
Right-click > Refactor > Rename
```
---
Reformat the code for better reading experience in Android Studio
   ```
   Reformat Code: Ctrl+Alt+L
   ```
---
In your click handler, add this code to hide the keyboard after input is complete
```Kotlin
val imm = getSystemService(Context.INPUT_METHOD_SERVICE) as InputMethodManager
imm.hideSoftInputFromWindow(view.windowToken, 0)
```
---
[Data Binding Library](https://developer.android.com/topic/libraries/data-binding) helps us to reduce the overhead caused by *findViewById()*.
>The Data Binding Library is a support library that allows you to bind UI components in your layouts to data sources in your app using a declarative format rather than programmatically.
---
Steps to use data binding to replace calls to findViewById(): 
1. Enable data binding in the android section of the build.gradle file:
   ```kotlin
   dataBinding { enabled = true }
   ```
2. Use <layout> as the root view in your XML layout.
3. Define a binding variable: 
   ```kotlin
   private lateinit var binding: ActivityMainBinding
   ``` 
4. Create a binding object in MainActivity, replacing setContentView:
   ```kotlin
   binding = DataBindingUtil.setContentView(this, R.layout.activity_main)
   ``` 
5. Replace calls to findViewById() with references to the view in the binding object.  

Steps for binding views to data:
1. Create a data class for your data.
2. Add a <data> block inside the <layout> tag.
3. Define a <variable> with a name, and a type that is the data class.
   ```XML
   <data>
    <variable
       name="myName"
       type="com.example.android.aboutme.MyName" />
   </data>
   ```
4. In MainActivity, create a variable with an instance of the data class. For example: 
   ```kotlin
   private val myName: MyName = MyName("Jon Doe")
   ``` 
5. In the binding object, set the variable to the variable you just created: 
   ```kotlin
   binding.myName = myName
   ``` 
6. In the XML, set the content of the view to the variable that you defined in the <data> block. Use dot notation to access the data inside the data class.
   ```XML
   android:text="@{myName.name}"
   ``` 
---