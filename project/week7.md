
## 7주차 과제
 
 ### activity_main
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_weight="1"
        android:orientation="vertical">

        <TextView
            android:id="@+id/xinputCount"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="0/80 바이트"
            android:layout_gravity="right"
            android:layout_marginTop="10dp"
            android:textColor="#ff0000ff"
            android:textSize="20sp"/>

        <EditText
            android:id="@+id/xinputMessage"
            android:layout_width="300dp"
            android:layout_height="500dp"
            android:maxLength="80"
            android:layout_gravity="center_horizontal"
            android:textSize="36sp"/>
    </LinearLayout>
    <LinearLayout
        android:orientation="horizontal"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:gravity="center"
        android:layout_weight="3">

            <Button
                android:id="@+id/xsendButton"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_gravity="top"
                android:layout_margin="20dp"
                android:paddingLeft="20dp"
                android:paddingRight="20dp"
                android:text="전송"
                android:textSize="18sp"
                />
            <Button
                android:id="@+id/xcloseButton"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_gravity="top"
                android:layout_margin="20dp"
                android:paddingLeft="20dp"
                android:paddingRight="20dp"
                android:onClick="closeButton"
                android:text="닫기"
                android:textSize="18sp"
                />
    </LinearLayout>

```

### MainActivity
```java
package com.example.mission;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.text.Editable;
import android.text.TextWatcher;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;

import java.io.UnsupportedEncodingException;

public class MainActivity extends AppCompatActivity {
    EditText jinputMessage;
    TextView jinputCount;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        jinputMessage = findViewById(R.id.xinputMessage);
        jinputCount = findViewById(R.id.xinputCount);

        Button jsendButton = findViewById(R.id.xsendButton);
        jsendButton.setOnClickListener(new View.OnClickListener() {
            public void onClick(View V) {
                String message = jinputMessage.getText().toString();
                Toast.makeText(getApplicationContext(), "전송할 메시지\n\n" + message, Toast.LENGTH_LONG).show();
            }
        });

        Button jcloseButton = findViewById(R.id.xcloseButton);
        jcloseButton.setOnClickListener(new View.OnClickListener() {
            public void onClick(View V) {
                finish();
            }
        });
        TextWatcher watcher = new TextWatcher() {
            public void onTextChanged(CharSequence str, int start, int before, int count) {
                byte[] bytes = null;
                try {
                    bytes = str.toString().getBytes("KSC5601");
                    int strCount = bytes.length;
                    jinputCount.setText(strCount + " / 80바이트");
                } catch (UnsupportedEncodingException ex) {
                    ex.printStackTrace();
                }
            }

            public void beforeTextChanged(CharSequence s, int start, int count, int after) {

            }

            public void afterTextChanged(Editable strEditable) {
                String str = strEditable.toString();
                try {
                    byte[] strBytes = str.getBytes("KSC5601");
                    if (strBytes.length > 80) {
                        strEditable.delete(strEditable.length() - 2, strEditable.length() - 1);
                    }
                } catch (Exception ex) {
                    ex.printStackTrace();
                }
            }
        };
        jinputMessage.addTextChangedListener(watcher);
    }
    }
