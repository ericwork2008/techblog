# Android Window Basic

{% embed url="https://medium.com/@MrAndroid/android-window-basic-concepts-a11d6fcaaf3f" %}

## Can an Activity have multiple window?

**ChatGPT answered as**:

An Android activity typically has a **single window**, but there are ways to create additional windows within the same activity. Here’s how it works:

#### **1. Default Case: One Window per Activity**

* Each `Activity` is associated with a single `Window`, which is usually managed by the system.
* This `Window` fills the screen by default, displaying the activity’s UI.

#### **2. Multiple Windows in an Activity**

Though an activity has a single primary window, you can create additional windows in different ways:

**(a) Dialogs (`Dialog`, `DialogFragment`)**

* You can show a floating window using `Dialog` or `DialogFragment`, which appears on top of the main activity window.
*   Example:

    ```java
    AlertDialog.Builder builder = new AlertDialog.Builder(this);
    builder.setTitle("Dialog Title")
           .setMessage("This is a dialog window.")
           .setPositiveButton("OK", null)
           .show();
    ```

**(b) Popup Windows (`PopupWindow`)**

* `PopupWindow` creates a separate window on top of the activity's main window.
*   Example:

    ```java
    PopupWindow popup = new PopupWindow(view, width, height, true);
    popup.showAtLocation(parentView, Gravity.CENTER, 0, 0);
    ```

**(c) Multi-Window Mode (Android 7.0+)**

* Android supports **split-screen** and **freeform window mode** where multiple activities (even from the same app) can be displayed simultaneously.
*   Example:

    ```java
    ActivityOptions options = ActivityOptions.makeBasic();
    startActivity(intent, options.toBundle());
    ```

**(d) Overlays (`TYPE_APPLICATION_OVERLAY`)**

* Floating windows (like chat heads in Messenger) use `WindowManager` with `TYPE_APPLICATION_OVERLAY` permission.

#### **Summary**

* Normally, an activity has **one window**.
* You can create **additional windows** using dialogs, popups, overlays, or multi-window mode.

## Add view in an region of the window

In **AOSP Launcher3's OverlayPlugin**, you can control the size and position of the overlay by setting its layout parameters properly when adding the overlay view to the window manager. Instead of taking the full screen, you can define a **specific region** of the launcher where the overlay should appear.

#### **Key Steps to Overlay a Partial Window in OverlayPlugin**

1. **Modify the `OverlayPlugin` Implementation**
   * In your `OverlayService` (which implements `OverlayPlugin`), ensure that you use `WindowManager.LayoutParams` to set the overlay window's size and position.
2. **Use `WindowManager.LayoutParams` with Partial Screen Size**
   * Instead of `MATCH_PARENT`, specify exact dimensions using `WRAP_CONTENT` or custom width/height.

***

#### **Example Code to Show a Partial Overlay**

Modify your `OverlayService.java` inside **Launcher3**:

**1️⃣ Add a Partial Overlay Instead of Fullscreen**

```java
@Override
public void onCreate() {
    super.onCreate();
    
    // Create an overlay view
    overlayView = new FrameLayout(this);
    overlayView.setBackgroundColor(Color.parseColor("#AA000000")); // Semi-transparent black

    // Define layout parameters for a **partial** window overlay
    WindowManager.LayoutParams params = new WindowManager.LayoutParams(
            600, // Width in pixels (e.g., 600px)
            400, // Height in pixels (e.g., 400px)
            WindowManager.LayoutParams.TYPE_APPLICATION_OVERLAY, // Overlay type
            WindowManager.LayoutParams.FLAG_NOT_FOCUSABLE | 
            WindowManager.LayoutParams.FLAG_LAYOUT_IN_SCREEN,
            PixelFormat.TRANSLUCENT
    );

    // Set overlay position (e.g., bottom-right corner)
    params.gravity = Gravity.BOTTOM | Gravity.END;
    params.x = 50; // Offset from the right
    params.y = 50; // Offset from the bottom

    // Add the overlay to the window manager
    windowManager = (WindowManager) getSystemService(WINDOW_SERVICE);
    windowManager.addView(overlayView, params);
}
```

**2️⃣ Ensure the Overlay Only Affects a Part of Launcher**

* The `params.gravity` allows positioning inside the launcher.
* `params.x` and `params.y` set specific coordinates for the overlay.
* The overlay is **not full-screen** because we explicitly define `width` and `height`.

***

#### **Additional Customization**

* **Dynamically Resize the Overlay**\
  You can allow resizing by modifying `params.width` and `params.height` dynamically.
* **Handle Clicks Inside Overlay**\
  If you need user interaction, remove `FLAG_NOT_FOCUSABLE`.
* **Smooth Animations for Overlay Appearance**\
  You can add fade-in/fade-out animations using `View.animate()`.

***

#### **Final Thoughts**

This approach ensures that the **OverlayPlugin** only takes up part of the screen instead of covering the entire Launcher3 window. Would you like further refinements, such as gesture support for dragging the overlay? 🚀



#### **Adding Drag Support to the Overlay in Launcher3's OverlayPlugin**

To enable **dragging** of the overlay window inside Launcher3, we need to:

1. **Detect touch gestures** (ACTION\_DOWN, ACTION\_MOVE) inside the overlay view.
2. **Update the overlay position** dynamically using `WindowManager.LayoutParams`.
3. Ensure smooth dragging while maintaining **bounds within the screen**.

***

#### **📌 Full Implementation of Overlay with Dragging**

Modify your `OverlayService.java` to include touch handling for dragging.

```java
package com.example.launcheroverlay;

import android.app.Service;
import android.content.Intent;
import android.graphics.PixelFormat;
import android.os.IBinder;
import android.view.Gravity;
import android.view.MotionEvent;
import android.view.View;
import android.view.WindowManager;
import android.widget.FrameLayout;

public class OverlayService extends Service {

    private WindowManager windowManager;
    private FrameLayout overlayView;
    private WindowManager.LayoutParams params;

    // Last known touch position
    private float touchStartX, touchStartY;
    private float lastX, lastY;

    @Override
    public void onCreate() {
        super.onCreate();

        // Create an overlay view
        overlayView = new FrameLayout(this);
        overlayView.setBackgroundColor(0xAA000000); // Semi-transparent background

        // Define layout parameters (initial size and position)
        params = new WindowManager.LayoutParams(
                600,  // Width in pixels
                400,  // Height in pixels
                WindowManager.LayoutParams.TYPE_APPLICATION_OVERLAY, // Overlay type
                WindowManager.LayoutParams.FLAG_NOT_FOCUSABLE | 
                WindowManager.LayoutParams.FLAG_LAYOUT_IN_SCREEN,
                PixelFormat.TRANSLUCENT
        );

        // Initial position (bottom-right corner)
        params.gravity = Gravity.TOP | Gravity.START;
        params.x = 200; // Starting X position
        params.y = 300; // Starting Y position

        // Get window manager and add view
        windowManager = (WindowManager) getSystemService(WINDOW_SERVICE);
        windowManager.addView(overlayView, params);

        // Enable drag gesture
        enableDragging();
    }

    private void enableDragging() {
        overlayView.setOnTouchListener(new View.OnTouchListener() {
            @Override
            public boolean onTouch(View v, MotionEvent event) {
                switch (event.getAction()) {
                    case MotionEvent.ACTION_DOWN:
                        // Capture initial touch position
                        touchStartX = event.getRawX();
                        touchStartY = event.getRawY();
                        lastX = params.x;
                        lastY = params.y;
                        return true;

                    case MotionEvent.ACTION_MOVE:
                        // Calculate new position
                        float dx = event.getRawX() - touchStartX;
                        float dy = event.getRawY() - touchStartY;

                        params.x = (int) (lastX + dx);
                        params.y = (int) (lastY + dy);

                        // Update overlay position
                        windowManager.updateViewLayout(overlayView, params);
                        return true;
                }
                return false;
            }
        });
    }

    @Override
    public IBinder onBind(Intent intent) {
        return null;
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        if (overlayView != null) {
            windowManager.removeView(overlayView);
        }
    }
}
```

***

#### **💡 Explanation of Dragging Mechanism**

1. **Detect touch start (`ACTION_DOWN`)**
   * Save the initial X/Y position of the touch.
   * Store the last position of the overlay window.
2. **Detect movement (`ACTION_MOVE`)**
   * Calculate the difference (`dx`, `dy`) between the new touch position and the original touch position.
   * Apply this difference to update `params.x` and `params.y`.
   * Call `windowManager.updateViewLayout()` to move the overlay in real time.
3. **Ensure smooth dragging**
   * Using `event.getRawX()` and `event.getRawY()` ensures accurate positioning across different screen densities.

***

#### **🔥 Bonus Features**

*   **Restrict Movement to Screen Boundaries**

    ```java
    int screenWidth = windowManager.getDefaultDisplay().getWidth();
    int screenHeight = windowManager.getDefaultDisplay().getHeight();

    params.x = Math.max(0, Math.min(screenWidth - overlayView.getWidth(), params.x));
    params.y = Math.max(0, Math.min(screenHeight - overlayView.getHeight(), params.y));
    ```
*   **Add Click Interaction to the Overlay**

    ```java
    overlayView.setOnClickListener(v -> Toast.makeText(getApplicationContext(), "Overlay Clicked!", Toast.LENGTH_SHORT).show());
    ```
*   **Animate Overlay While Moving**

    ```java
    overlayView.animate().translationX(params.x).translationY(params.y).setDuration(100).start();
    ```

***

#### **🎯 Final Result**

✅ The overlay appears in **a partial window** of Launcher3.\
✅ You can **drag it anywhere** on the screen.\
✅ You can add **additional features** like gestures or resizing.

Would you like to extend it further with gestures like **pinch-to-resize**? 🚀😊**Adding Drag Support to the Overlay in Launcher3's OverlayPlugin**

To enable **dragging** of the overlay window inside Launcher3, we need to:

1. **Detect touch gestures** (ACTION\_DOWN, ACTION\_MOVE) inside the overlay view.
2. **Update the overlay position** dynamically using `WindowManager.LayoutParams`.
3. Ensure smooth dragging while maintaining **bounds within the screen**.

***

#### **📌 Full Implementation of Overlay with Dragging**

Modify your `OverlayService.java` to include touch handling for dragging.

```java
package com.example.launcheroverlay;

import android.app.Service;
import android.content.Intent;
import android.graphics.PixelFormat;
import android.os.IBinder;
import android.view.Gravity;
import android.view.MotionEvent;
import android.view.View;
import android.view.WindowManager;
import android.widget.FrameLayout;

public class OverlayService extends Service {

    private WindowManager windowManager;
    private FrameLayout overlayView;
    private WindowManager.LayoutParams params;

    // Last known touch position
    private float touchStartX, touchStartY;
    private float lastX, lastY;

    @Override
    public void onCreate() {
        super.onCreate();

        // Create an overlay view
        overlayView = new FrameLayout(this);
        overlayView.setBackgroundColor(0xAA000000); // Semi-transparent background

        // Define layout parameters (initial size and position)
        params = new WindowManager.LayoutParams(
                600,  // Width in pixels
                400,  // Height in pixels
                WindowManager.LayoutParams.TYPE_APPLICATION_OVERLAY, // Overlay type
                WindowManager.LayoutParams.FLAG_NOT_FOCUSABLE | 
                WindowManager.LayoutParams.FLAG_LAYOUT_IN_SCREEN,
                PixelFormat.TRANSLUCENT
        );

        // Initial position (bottom-right corner)
        params.gravity = Gravity.TOP | Gravity.START;
        params.x = 200; // Starting X position
        params.y = 300; // Starting Y position

        // Get window manager and add view
        windowManager = (WindowManager) getSystemService(WINDOW_SERVICE);
        windowManager.addView(overlayView, params);

        // Enable drag gesture
        enableDragging();
    }

    private void enableDragging() {
        overlayView.setOnTouchListener(new View.OnTouchListener() {
            @Override
            public boolean onTouch(View v, MotionEvent event) {
                switch (event.getAction()) {
                    case MotionEvent.ACTION_DOWN:
                        // Capture initial touch position
                        touchStartX = event.getRawX();
                        touchStartY = event.getRawY();
                        lastX = params.x;
                        lastY = params.y;
                        return true;

                    case MotionEvent.ACTION_MOVE:
                        // Calculate new position
                        float dx = event.getRawX() - touchStartX;
                        float dy = event.getRawY() - touchStartY;

                        params.x = (int) (lastX + dx);
                        params.y = (int) (lastY + dy);

                        // Update overlay position
                        windowManager.updateViewLayout(overlayView, params);
                        return true;
                }
                return false;
            }
        });
    }

    @Override
    public IBinder onBind(Intent intent) {
        return null;
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        if (overlayView != null) {
            windowManager.removeView(overlayView);
        }
    }
}
```

***

#### **💡 Explanation of Dragging Mechanism**

1. **Detect touch start (`ACTION_DOWN`)**
   * Save the initial X/Y position of the touch.
   * Store the last position of the overlay window.
2. **Detect movement (`ACTION_MOVE`)**
   * Calculate the difference (`dx`, `dy`) between the new touch position and the original touch position.
   * Apply this difference to update `params.x` and `params.y`.
   * Call `windowManager.updateViewLayout()` to move the overlay in real time.
3. **Ensure smooth dragging**
   * Using `event.getRawX()` and `event.getRawY()` ensures accurate positioning across different screen densities.

***

#### **🔥 Bonus Features**

*   **Restrict Movement to Screen Boundaries**

    ```java
    int screenWidth = windowManager.getDefaultDisplay().getWidth();
    int screenHeight = windowManager.getDefaultDisplay().getHeight();

    params.x = Math.max(0, Math.min(screenWidth - overlayView.getWidth(), params.x));
    params.y = Math.max(0, Math.min(screenHeight - overlayView.getHeight(), params.y));
    ```
*   **Add Click Interaction to the Overlay**

    ```java
    overlayView.setOnClickListener(v -> Toast.makeText(getApplicationContext(), "Overlay Clicked!", Toast.LENGTH_SHORT).show());
    ```
*   **Animate Overlay While Moving**

    ```java
    overlayView.animate().translationX(params.x).translationY(params.y).setDuration(100).start();
    ```

***

#### **🎯 Final Result**

✅ The overlay appears in **a partial window** of Launcher3.\
✅ You can **drag it anywhere** on the screen.\
✅ You can add **additional features** like gestures or resizing.

Would you like to extend it further with gestures like **pinch-to-resize**? 🚀😊
