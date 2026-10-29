# 15. Develop an Android Application Implementing Runtime Permissions and Security Handling in AndroidManifest.xml

## Project Name: `PermissionDemo`

## Aim

To develop an Android application that demonstrates runtime permission handling by checking, requesting, and managing camera permission securely.

## Technologies Used

- **Programming Language:** Java
- **IDE:** Android Studio
- **UI Design:** XML
- **Permission Handling:** Android Runtime Permissions API
- **Security Configuration:** AndroidManifest.xml
- **Libraries:** AndroidX AppCompat and AndroidX Core

## Project Structure

```text
PermissionDemo/
├── app/
│   └── src/
│       └── main/
│           ├── java/
│           │   └── com/
│           │       └── example/
│           │           └── permissiondemo/
│           │               └── MainActivity.java
│           ├── res/
│           │   └── layout/
│           │       └── activity_main.xml
│           └── AndroidManifest.xml
└── README.md
```

---

## 1. AndroidManifest.xml

**Location:** `app/src/main/AndroidManifest.xml`

Add the camera permission and camera hardware feature declaration above the `<application>` tag. The permission allows the app to request camera access, while `android:required="false"` means the app can be installed on devices without camera hardware.

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android">

    <!-- Camera Permission -->
    <uses-permission
        android:name="android.permission.CAMERA" />

    <!-- Camera hardware is optional -->
    <uses-feature
        android:name="android.hardware.camera"
        android:required="false" />

    <application
        android:allowBackup="true"
        android:label="PermissionDemo"
        android:supportsRtl="true"
        android:theme="@style/Theme.PermissionDemo">

        <activity
            android:name=".MainActivity"
            android:exported="true">

            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>

        </activity>

    </application>

</manifest>
```

**Note:** Ensure the theme name matches the one defined in your Android Studio project. Keep your existing theme if its name differs.

---

## 2. activity_main.xml

**Location:** `app/src/main/res/layout/activity_main.xml`

This XML file creates a simple interface containing a button to request camera permission.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center">

    <Button
        android:id="@+id/btnPermission"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Request Camera Permission" />

</LinearLayout>
```

---

## 3. MainActivity.java

**Location:** `app/src/main/java/com/example/permissiondemo/MainActivity.java`

This Java class checks whether camera permission has already been granted. If permission is not granted, it requests permission from the user and handles the result.

```java
package com.example.permissiondemo;

import android.content.pm.PackageManager;
import android.os.Bundle;
import android.widget.Button;
import android.widget.Toast;

import androidx.appcompat.app.AppCompatActivity;
import androidx.core.app.ActivityCompat;
import androidx.core.content.ContextCompat;

public class MainActivity extends AppCompatActivity {

    Button btnPermission;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        btnPermission = findViewById(R.id.btnPermission);

        btnPermission.setOnClickListener(v -> {
            checkPermission();
        });
    }

    private void checkPermission() {

        // Check if Camera permission is already granted
        if (ContextCompat.checkSelfPermission(
                this,
                android.Manifest.permission.CAMERA
        ) == PackageManager.PERMISSION_GRANTED) {

            Toast.makeText(
                    this,
                    "Permission Already Granted",
                    Toast.LENGTH_SHORT
            ).show();

        } else {

            // Request Camera permission
            ActivityCompat.requestPermissions(
                    this,
                    new String[]{
                            android.Manifest.permission.CAMERA
                    },
                    1
            );
        }
    }

    // Permission Result
    @Override
    public void onRequestPermissionsResult(
            int requestCode,
            String[] permissions,
            int[] grantResults
    ) {
        super.onRequestPermissionsResult(
                requestCode,
                permissions,
                grantResults
        );

        if (requestCode == 1) {

            if (grantResults.length > 0
                    && grantResults[0]
                    == PackageManager.PERMISSION_GRANTED) {

                Toast.makeText(
                        this,
                        "Permission Granted",
                        Toast.LENGTH_SHORT
                ).show();

            } else {

                Toast.makeText(
                        this,
                        "Permission Denied",
                        Toast.LENGTH_SHORT
                ).show();
            }
        }
    }
}
```

---

## 4. How to Run the Application

1. Open Android Studio.
2. Create a new Android project named `PermissionDemo`.
3. Select **Java** as the programming language.
4. Set the package name to `com.example.permissiondemo`.
5. Add the camera permission and optional camera hardware declaration to `AndroidManifest.xml`.
6. Replace the contents of `activity_main.xml` with the XML code provided above.
7. Replace the contents of `MainActivity.java` with the Java code provided above.
8. Ensure that the theme reference matches your project.
9. Run the application on an Android emulator or physical device.
10. Click the **Request Camera Permission** button.
11. Grant or deny camera permission in the system dialog and observe the resulting Toast message.

---

## 5. Expected Output

### Initial Screen

```text
+--------------------------------------+
|                                      |
|                                      |
|    [ Request Camera Permission ]     |
|                                      |
|                                      |
+--------------------------------------+
```

### When the Button Is Clicked

If camera permission has not been granted, Android displays a system permission dialog asking whether the application can access the camera.

### If Permission Is Granted

```text
Permission Granted
```

### If Permission Is Denied

```text
Permission Denied
```

### If Permission Was Already Granted

```text
Permission Already Granted
```

---

## 6. Working of the Application

1. The application starts and displays the permission request button.
2. The user clicks the button.
3. The `checkPermission()` method checks the current camera permission status.
4. If permission is already granted, a Toast message is displayed.
5. If permission has not been granted, Android displays a runtime permission request.
6. The `onRequestPermissionsResult()` method processes the permission result.
7. The application displays a message indicating whether permission was granted or denied.

### Flowchart

```text
        Start Application
                |
                v
        Display Main Screen
                |
                v
        Click Permission Button
                |
                v
        Check Camera Permission
                |
          +-----+-----+
          |           |
       Granted     Not Granted
          |           |
          v           v
    Show Already   Request Runtime
      Granted       Permission
          |           |
          |           v
          |      User Responds
          |           |
          |      +----+----+
          |      |         |
          |   Granted    Denied
          |      |         |
          |      v         v
          |   Show       Show
          |  Granted    Denied
          |  Message    Message
          |      |         |
          +------+---------+
                 |
                 v
                End
```

---

## 7. Key Concepts

- **AndroidManifest.xml:** Declares the permissions and features required by the application.
- **`uses-permission`:** Declares that the app needs camera permission.
- **`uses-feature`:** Specifies whether camera hardware is required for installation.
- **Runtime Permission:** Allows the user to grant or deny access to protected resources while using the application.
- **`checkSelfPermission()`:** Checks whether the requested permission has been granted.
- **`requestPermissions()`:** Requests permission from the user.
- **`onRequestPermissionsResult()`:** Receives the permission decision in this implementation.
- **Toast:** Displays short messages to the user.

---

## 8. Security Considerations

- Request only the permissions needed by the application.
- Explain why camera access is needed before requesting it when appropriate.
- Handle permission denial gracefully.
- Check permission status before accessing the camera.
- Do not assume that declaring a permission in the manifest automatically grants access.
- Use the Android permission APIs appropriate to the target Android version.

**Note:** This practical demonstrates permission handling only. It does not capture photographs or open the camera.

---

## 9. Result

Thus, an Android application implementing runtime permissions and security handling through `AndroidManifest.xml` was developed successfully. The application checks camera permission, requests it when necessary, and displays the appropriate message based on the user's response.
