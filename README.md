<p align="center">
<h1 align='center'>Convert Speech to Text</h1>
<h3 align='center'>
   Visit my youtube channel <a href="https://www.youtube.com/@LearnWithYeamin">Learn With Yeamin</a>
</h3>
</p>

## Step 1: Here is `activity_main.xml` code.
```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <ImageView
        android:id="@+id/iv_mic"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/ic_mic"
        android:layout_centerInParent="true"/>

    <Spinner
        android:id="@+id/spinner_languages"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/iv_mic"
        android:layout_marginTop="16dp"
        android:layout_marginStart="16dp"
        android:layout_marginEnd="16dp"/>

    <TextView
        android:id="@+id/tv_speech_to_text"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/spinner_languages"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="16dp"
        android:text="Speech to Text"
        android:textSize="20sp"/>
</RelativeLayout>
```
## Step 2: Here is `MainActivity.java` code.
```java
import android.os.Bundle;
import android.view.View;
import android.widget.AdapterView;
import android.widget.ArrayAdapter;
import android.widget.ImageView;
import android.widget.Spinner;
import android.widget.TextView;
import android.widget.Toast;
import android.content.Intent;
import android.speech.RecognizerIntent;
import android.app.Activity;
import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;
import java.util.ArrayList;
import java.util.Locale;
import java.util.Objects;

public class MainActivity extends AppCompatActivity {
    private ImageView iv_mic;
    private TextView tv_Speech_to_text;
    private Spinner spinnerLanguages;
    private static final int REQUEST_CODE_SPEECH_INPUT = 1;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        iv_mic = findViewById(R.id.iv_mic);
        tv_Speech_to_text = findViewById(R.id.tv_speech_to_text);
        spinnerLanguages = findViewById(R.id.spinner_languages);

        // Define array of languages
        String[] languages = {"English", "Spanish", "French", "Chinese", "German", "Italian", "Japanese"};

        // Create ArrayAdapter using the array and default layout
        ArrayAdapter<String> adapter = new ArrayAdapter<>(this, android.R.layout.simple_spinner_item, languages);

        // Specify the layout to use when the list of choices appears
        adapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);

        // Apply the adapter to the spinner
        spinnerLanguages.setAdapter(adapter);

        iv_mic.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                startSpeechToText();
            }
        });

        spinnerLanguages.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
            @Override
            public void onItemSelected(AdapterView<?> parentView, View selectedItemView, int position, long id) {
                String selectedLanguage = (String) parentView.getItemAtPosition(position);
                // You can use the selected language here
            }

            @Override
            public void onNothingSelected(AdapterView<?> parentView) {
                // Do nothing
            }
        });
    }

    private void startSpeechToText() {
        Intent intent = new Intent(RecognizerIntent.ACTION_RECOGNIZE_SPEECH);
        intent.putExtra(RecognizerIntent.EXTRA_LANGUAGE_MODEL, RecognizerIntent.LANGUAGE_MODEL_FREE_FORM);
        
        // Get the selected language from the spinner
        String selectedLanguage = spinnerLanguages.getSelectedItem().toString();
        Locale selectedLocale = getLocaleFromLanguage(selectedLanguage);
        if (selectedLocale != null) {
            intent.putExtra(RecognizerIntent.EXTRA_LANGUAGE, selectedLocale);
        } else {
            // Default to English if no matching locale found
            intent.putExtra(RecognizerIntent.EXTRA_LANGUAGE, Locale.ENGLISH);
        }
        
        intent.putExtra(RecognizerIntent.EXTRA_PROMPT, "Speak to text");

        try {
            startActivityForResult(intent, REQUEST_CODE_SPEECH_INPUT);
        } catch (Exception e) {
            Toast.makeText(MainActivity.this, " " + e.getMessage(), Toast.LENGTH_SHORT).show();
        }
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == REQUEST_CODE_SPEECH_INPUT) {
            if (resultCode == RESULT_OK && data != null) {
                ArrayList<String> result = data.getStringArrayListExtra(RecognizerIntent.EXTRA_RESULTS);
                if (result != null && !result.isEmpty()) {
                    tv_Speech_to_text.setText(result.get(0));
                } else {
                    tv_Speech_to_text.setText("No speech text recognized");
                }
            }
        }
    }

    // Helper method to get Locale from language string
    private Locale getLocaleFromLanguage(String language) {
        if (language.equals("English")) {
            return Locale.ENGLISH;
        } else if (language.equals("Spanish")) {
            return new Locale("es", "ES");
        } else if (language.equals("French")) {
            return Locale.FRENCH;
        } else if (language.equals("Chinese")) {
            return Locale.CHINESE;
        } else if (language.equals("German")) {
            return Locale.GERMAN;
        } else if (language.equals("Italian")) {
            return Locale.ITALIAN;
        } else if (language.equals("Japanese")) {
            return Locale.JAPANESE;
        } else {
            return null;
        }
    }
}
```
## Authors
**MD YEAMIN** - *Android Software Developer* - <a href="https://github.com/i-rin-eam">MD YEAMIN</a> - *Learn with Ease*
<h1 align="center">Thank You ❤️</h1>
