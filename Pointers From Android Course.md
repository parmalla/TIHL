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
   app:srcCompat="@drawable/empty_dice"
   ```
 ---
