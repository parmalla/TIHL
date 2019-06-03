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
   
