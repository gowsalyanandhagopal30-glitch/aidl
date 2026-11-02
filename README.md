# Ex.No:2 Develop an android application to implement the AIDL server and client app. The server app hosts a Bound Service and contains the logic to return random colours to its client.The client app calls the service and changes the button's colour within the Main activity.



## AIM:

To Develop an android application to implement the AIDL server and client app. The server app hosts a Bound Service and contains the logic to return random colours to its client.
The client app calls the service and changes the button's colour within the Main activity using AIDL interface in Android Studio.

## EQUIPMENTS REQUIRED:

Android Studio(Min.required Griaffe )

## ALGORITHM:

Step 1: Open Android Stdio and then click on File -> New -> New project.

Step 2: Then type the Application name as CSAIDL and click Next. 

Step 3: Then select the Minimum SDK as shown below and click Next.

Step 4: Then select the Empty Activity and click Next. Finally click Finish.

Step 5: Design layout in activity_main.xml.

Step 6: Display message give in MainActivity file(client/server).

Step 7: Save and run the application.

## PROGRAM:
```
/*
Program to print the client/server services using AIDL”.
Developed by:N.Gowsalya
Registeration Number :212225230085
*/
```
## Server App

## IColorService.aidl
```

package com.example.aidlserver;

interface IColorService {
    int getRandomColor();
}

```
## ColorService.java
```

package com.example.aidlserver;

import android.app.Service;
import android.content.Intent;
import android.os.IBinder;
import android.os.RemoteException;
import android.graphics.Color;
import java.util.Random;

public class ColorService extends Service {

    private final IColorService.Stub binder = new IColorService.Stub() {
        @Override
        public int getRandomColor() throws RemoteException {
            Random random = new Random();
            int red = random.nextInt(256);
            int green = random.nextInt(256);
            int blue = random.nextInt(256);
            return Color.rgb(red, green, blue);
        }
    };

    @Override
    public IBinder onBind(Intent intent) {
        return binder;
    }
}

```
## AndroidManifest.xml
```

<service
    android:name=".ColorService"
    android:exported="true"
    android:enabled="true">

    <intent-filter>
        <action android:name="com.example.aidlserver.IColorService" />
    </intent-filter>

</service>

```

## Client App

## IColorService.aidl
```

package com.example.aidlserver;

interface IColorService {
    int getRandomColor();
}

```
## activity_main.xml
```

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical">

    <Button
        android:id="@+id/btnChangeColor"
        android:layout_width="200dp"
        android:layout_height="200dp"
        android:text="Tap me!"
        android:textSize="24sp" />

</LinearLayout>

```
## MainActivity.java
```

package com.example.aidlclient;

import android.content.ComponentName;
import android.content.Context;
import android.content.Intent;
import android.content.ServiceConnection;
import android.os.Bundle;
import android.os.IBinder;
import android.os.RemoteException;
import android.widget.Button;
import android.widget.Toast;

import androidx.appcompat.app.AppCompatActivity;

import com.example.aidlserver.IColorService;

public class MainActivity extends AppCompatActivity {

    private IColorService colorService;
    private boolean isBound = false;
    private Button btnChangeColor;

    private ServiceConnection connection = new ServiceConnection() {

        @Override
        public void onServiceConnected(ComponentName name, IBinder service) {
            colorService = IColorService.Stub.asInterface(service);
            isBound = true;
            Toast.makeText(MainActivity.this, "Connected",
                    Toast.LENGTH_SHORT).show();
        }

        @Override
        public void onServiceDisconnected(ComponentName name) {
            isBound = false;
            colorService = null;
        }
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        btnChangeColor = findViewById(R.id.btnChangeColor);

        btnChangeColor.setOnClickListener(v -> {
            if (isBound && colorService != null) {
                try {
                    int randomColor = colorService.getRandomColor();
                    btnChangeColor.setBackgroundColor(randomColor);
                } catch (RemoteException e) {
                    Toast.makeText(this, "Error",
                            Toast.LENGTH_SHORT).show();
                }
            } else {
                Toast.makeText(this, "Not connected",
                        Toast.LENGTH_SHORT).show();
            }
        });
    }

    @Override
    protected void onStart() {
        super.onStart();

        Intent intent = new Intent(
                "com.example.aidlserver.IColorService");

        intent.setPackage("com.example.aidlserver");

        bindService(intent, connection, Context.BIND_AUTO_CREATE);
    }

    @Override
    protected void onStop() {
        super.onStop();

        if (isBound) {
            unbindService(connection);
            isBound = false;
        }
    }
}

```
## Client AndroidManifest.xml
```
/*
<queries>
    <package android:name="com.example.aidlserver" />
</queries>
*/
```

## OUTPUT

<img width="1920" height="1020" alt="Screenshot 2026-11-01 225212" src="https://github.com/user-attachments/assets/5ad9a2ba-9fd6-4d4b-9ee4-8db0c764b068" />

<img width="1920" height="1020" alt="Screenshot 2026-11-01 225221" src="https://github.com/user-attachments/assets/61d29d7f-0df7-4310-926b-fb61176f9a85" />

<img width="1920" height="1020" alt="Screenshot 2026-11-01 225231" src="https://github.com/user-attachments/assets/95e8b94f-a791-4ec6-9ad9-dfedea8b7b95" />



## RESULT
Thus a Simple Android Application to create a AIDL interface and communicate the process between client and server using AIDL interface in Android Studio is developed and executed successfully.
