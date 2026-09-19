## EX:NO:05:Develop a program to create a simple calculator using android studio.
## Aim:
To create and design an android application for a simple calculator using android studio.

## EQUIPMENTS REQUIRED:
Android Studio(Latest Version)

## ALGORITHM:
Step 1: Open Android Stdio and then click on File -> New -> New project.

Step 2: Then type the Application name as smsintent and click Next.

Step 3: Then select the Minimum SDK as shown below and click Next.

Step 4: Then select the Empty Activity and click Next. Finally click Finish.

Step 5: Design layout in activity_main.xml.

Step 6:Display details give in MainActivity file.

Step 7: Save and run the application.

## PROGRAM:
Program to create and design an android application simple calculator using Intent.

Developed by: Dharshini

Registeration Number :212225220024
## AndroidMainfest.xml
```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/Theme.AppCompat.Light.DarkActionBar">

        <activity android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

    </application>
</manifest>
```
## MainActivity.java
```
package com.example.expfour;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;

import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    private TextView tvDisplay;
    private double firstValue = Double.NaN;
    private String currentInput = "";
    private String fullExpression = "";
    private String pendingAction = "";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        tvDisplay = findViewById(R.id.tvDisplay);

        setNumericButtonListeners();
        setOperatorButtonListeners();
    }

    private void setNumericButtonListeners() {
        int[] numericButtons = {
                R.id.btn0, R.id.btn1, R.id.btn2, R.id.btn3, R.id.btn4,
                R.id.btn5, R.id.btn6, R.id.btn7, R.id.btn8, R.id.btn9
        };

        View.OnClickListener listener = view -> {
            Button button = (Button) view;
            String val = button.getText().toString();
            
            // If we just finished an expression (e.g. "4+4=8"), clear and start new
            if (fullExpression.contains("=")) {
                clear();
            }

            currentInput += val;
            tvDisplay.setText(fullExpression + currentInput);
        };

        for (int id : numericButtons) {
            findViewById(id).setOnClickListener(listener);
        }
    }

    private void setOperatorButtonListeners() {
        findViewById(R.id.btnAdd).setOnClickListener(v -> prepareCompute("+"));
        findViewById(R.id.btnSub).setOnClickListener(v -> prepareCompute("-"));
        findViewById(R.id.btnMul).setOnClickListener(v -> prepareCompute("*"));
        findViewById(R.id.btnDiv).setOnClickListener(v -> prepareCompute("/"));
        findViewById(R.id.btnEqual).setOnClickListener(v -> finalizeCompute());
        findViewById(R.id.btnC).setOnClickListener(v -> clear());
    }

    private void clear() {
        firstValue = Double.NaN;
        currentInput = "";
        fullExpression = "";
        pendingAction = "";
        tvDisplay.setText("0");
    }

    private void prepareCompute(String action) {
        if (currentInput.isEmpty()) return;

        double val = Double.parseDouble(currentInput);

        if (!Double.isNaN(firstValue)) {
            switch (pendingAction) {
                case "+": firstValue += val; break;
                case "-": firstValue -= val; break;
                case "*": firstValue *= val; break;
                case "/": if (val != 0) firstValue /= val; break;
            }
        } else {
            firstValue = val;
        }

        pendingAction = action;
        fullExpression += currentInput + action;
        currentInput = "";
        tvDisplay.setText(fullExpression);
    }

    private void finalizeCompute() {
        if (currentInput.isEmpty() || Double.isNaN(firstValue)) return;

        double secondValue = Double.parseDouble(currentInput);
        double result = 0;

        switch (pendingAction) {
            case "+": result = firstValue + secondValue; break;
            case "-": result = firstValue - secondValue; break;
            case "*": result = firstValue * secondValue; break;
            case "/": 
                if (secondValue != 0) result = firstValue / secondValue;
                break;
        }

        // Format result to remove .0 if it's an integer
        String resultStr = (result == (long) result) ? String.valueOf((long) result) : String.valueOf(result);
        
        tvDisplay.setText(fullExpression + currentInput + "=" + resultStr);
        
        // Prepare for next operation or clear
        fullExpression = tvDisplay.getText().toString();
        currentInput = "";
        firstValue = Double.NaN;
    }
}
```
## themes.xml
```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <!-- Base application theme. -->
    <style name="Theme.Expfour" parent="Theme.AppCompat.NoActionBar">
        <!-- Primary brand color. -->
        <item name="colorPrimary">@color/bg_charcoal</item>
        <item name="colorPrimaryDark">@color/black</item>
        <item name="colorAccent">@color/btn_amber</item>
        <item name="android:windowBackground">@color/bg_charcoal</item>
    </style>

    <!-- Numeric Button Style -->
    <style name="CalcButton.Numeric" parent="Widget.AppCompat.Button">
        <item name="android:backgroundTint">@color/btn_grey</item>
        <item name="android:textColor">@color/white</item>
        <item name="android:textSize">24sp</item>
        <item name="android:layout_margin">4dp</item>
    </style>

    <!-- Operator Button Style -->
    <style name="CalcButton.Operator" parent="Widget.AppCompat.Button">
        <item name="android:backgroundTint">@color/btn_amber</item>
        <item name="android:textColor">@color/white</item>
        <item name="android:textSize">24sp</item>
        <item name="android:layout_margin">4dp</item>
    </style>

    <!-- Action Button Style (C, =) -->
    <style name="CalcButton.Action" parent="Widget.AppCompat.Button">
        <item name="android:backgroundTint">@color/btn_orange</item>
        <item name="android:textColor">@color/white</item>
        <item name="android:textSize">24sp</item>
        <item name="android:layout_margin">4dp</item>
    </style>
</resources>
```
## string.xml
```
<resources>
    <string name="app_name">Calculator</string>
</resources>
```
## colours.xml
```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <!-- Basic Palette -->
    <color name="black">#FF000000</color>
    <color name="white">#FFFFFFFF</color>

    <!-- Calculator Theme Colors -->
    <color name="bg_charcoal">#121212</color>
    <color name="btn_grey">#333333</color>
    <color name="btn_amber">#FF9800</color>
    <color name="btn_orange">#F4511E</color>
    <color name="display_bg">#000000</color>
</resources>
```
## output
<img width="1915" height="1198" alt="image" src="https://github.com/user-attachments/assets/1ec5a9be-bac7-473f-b1ac-0f9d1c9e0909" />

## RESULT
Thus a Simple Android Application create a simple calculator using Android Studio is developed and executed successfully.
