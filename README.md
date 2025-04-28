# Přehled syntaxe Java pro Android

Vítejte u komplexního přehledu syntaxe pro vývoj Android aplikací v jazyce Java. Tento dokument pokrývá základní syntaxi Java, Android-specifické třídy, strukturu projektu, UI komponenty, konfiguraci manifestu, Gradle skripty a další.

---

## Obsah

1. [Struktura projektu](#struktura-projektu)  
2. [Základy Java](#základy-java)  
3. [AndroidManifest.xml](#androidmanifestxml)  
4. [Aktivity a životní cyklus](#aktivity-a-životní-cyklus)  
5. [Intenty](#intenty)  
6. [UI komponenty](#ui-komponenty)  
7. [Rozvržení](#rozvržení)  
8. [Zdroje](#zdroje)  
9. [Obslužné posluchače událostí](#obslužné-posluchače-událostí)  
10. [Vlákna a asynchronní úlohy](#vlákna-a-asynchronní-úlohy)  
11. [Ukládání dat](#ukládání-dat)  
12. [Síťová komunikace](#síťová-komunikace)  
13. [Oprávnění](#oprávnění)  
14. [Gradle Build Skript](#gradle-build-skript)  
15. [Logování](#logování)  
16. [Nejlepší postupy](#nejlepší-postupy)  

---

## Struktura projektu

```text
MyApp/
├── app/
│   ├── build.gradle
│   └── src/
│       ├── main/
│       │   ├── java/com/example/myapp/
│       │   │   └── MainActivity.java
│       │   ├── res/
│       │   │   ├── layout/activity_main.xml
│       │   │   ├── values/strings.xml
│       │   │   └── drawable/
│       │   └── AndroidManifest.xml
│       └── test/
└── build.gradle
```

---

## Základy Java

### Třída a metody

```java
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    public void greetUser(String name) {
        String message = "Ahoj, " + name + "!";
        Log.d("MainActivity", message);
    }
}
```

### Proměnné, datové typy a řízení toku

```java
int pocet = 10;
String text = "Android";
boolean jeAktivni = true;

if (pocet > 0) {
    for (int i = 0; i < pocet; i++) {
        Log.i("Smycka", "Iterace " + i);
    }
} else {
    Log.w("Smycka", "Žádné iterace");
}
```

---

## AndroidManifest.xml

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.myapp">

    <uses-permission android:name="android.permission.INTERNET" />
    <application
        android:allowBackup="true"
        android:label="@string/app_name"
        android:theme="@style/Theme.MyApp">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

---

## Aktivity a životní cyklus

```java
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onStart() {
        super.onStart();
        // Volá se, když se aktivita stává viditelnou
    }

    @Override
    protected void onResume() {
        super.onResume();
        // Volá se, když aktivita začne reagovat na uživatele
    }

    @Override
    protected void onPause() {
        super.onPause();
        // Volá se, když jiná aktivita získává fokus
    }

    @Override
    protected void onStop() {
        super.onStop();
        // Volá se, když aktivita není již viditelná
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        // Volá se před zničením aktivity
    }
}
```

---

## Intenty

### Explicitní intent

```java
Intent intent = new Intent(this, SecondActivity.class);
intent.putExtra("KLIC", "hodnota");
startActivity(intent);
```

### Implicitní intent

```java
Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse("https://www.example.com"));
startActivity(intent);
```

---

## UI komponenty

```java
Button tlacitko = findViewById(R.id.my_button);
TextView textView = findViewById(R.id.my_textview);
EditText editText = findViewById(R.id.my_edittext);
ImageView imageView = findViewById(R.id.my_imageview);
```

---

## Rozvržení

### XML rozvržení

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <TextView
        android:id="@+id/my_textview"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Ahoj světe!" />

    <Button
        android:id="@+id/my_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Klikni na mě" />
</LinearLayout>
```

---

## Zdroje

### Řetězce (res/values/strings.xml)

```xml
<resources>
    <string name="app_name">Moje Aplikace</string>
    <string name="welcome_message">Vítejte v MojeAplikace!</string>
</resources>
```

### Barvy (res/values/colors.xml)

```xml
<resources>
    <color name="primaryColor">#6200EE</color>
    <color name="primaryVariant">#3700B3</color>
</resources>
```

---

## Obslužné posluchače událostí

```java
tlacitko.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        Toast.makeText(MainActivity.this, "Tlačítko stisknuto!", Toast.LENGTH_SHORT).show();
    }
});
```

---

## Vlákna a asynchronní úlohy

```java
new Thread(new Runnable() {
    @Override
    public void run() {
        // práce na pozadí
    }
}).start();
```

---

## Ukládání dat

### SharedPreferences

```java
SharedPreferences prefs = getSharedPreferences("MojePrefs", MODE_PRIVATE);
SharedPreferences.Editor editor = prefs.edit();
editor.putString("klic", "hodnota");
editor.apply();
```

### SQLite

```java
public class DBHelper extends SQLiteOpenHelper {
    public DBHelper(Context context) {
        super(context, "MojeDB", null, 1);
    }

    @Override
    public void onCreate(SQLiteDatabase db) {
        db.execSQL("CREATE TABLE uzivatele(id INTEGER PRIMARY KEY, jmeno TEXT)");
    }
}
```

---

## Síťová komunikace (OkHttp příklad)

```java
OkHttpClient client = new OkHttpClient();
Request request = new Request.Builder()
    .url("https://api.example.com/data")
    .build();

client.newCall(request).enqueue(new Callback() {
    @Override
    public void onResponse(Call call, Response response) throws IOException {
        String body = response.body().string();
        runOnUiThread(() -> textView.setText(body));
    }

    @Override
    public void onFailure(Call call, IOException e) {
        Log.e("Síť", "Chyba", e);
    }
});
```

---

## Oprávnění (Android M a novější)

```java
if (ContextCompat.checkSelfPermission(this, Manifest.permission.CAMERA)
        != PackageManager.PERMISSION_GRANTED) {
    ActivityCompat.requestPermissions(this,
        new String[]{Manifest.permission.CAMERA}, REQUEST_CODE);
}
```

---

## Gradle Build Skript (app/build.gradle)

```groovy
plugins {
    id 'com.android.application'
    id 'java'
}

android {
    compileSdkVersion 31

    defaultConfig {
        applicationId "com.example.myapp"
        minSdkVersion 21
        targetSdkVersion 31
        versionCode 1
        versionName "1.0"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.3.1'
    implementation 'com.google.android.material:material:1.4.0'
    implementation 'com.squareup.okhttp3:okhttp:4.9.1'
}
```

---

## Logování

```java
Log.v("TAG", "Verbose zpráva");
Log.d("TAG", "Debug zpráva");
Log.i("TAG", "Info zpráva");
Log.w("TAG", "Varování");
Log.e("TAG", "Chyba");
```

---

## Nejlepší postupy

- Řiďte se [Android developer guidelines](https://developer.android.com/guide)  
- Udržujte UI operace na hlavním vlákně  
- Používejte ViewModel a LiveData pro MVVM  
- Vyhněte se únikům paměti odregistrováním posluchačů  
- Používejte ProGuard nebo R8 pro zmenšení kódu  
- Pište unit testy a UI testy  
