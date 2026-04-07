mkdir neuromantico && cd neuromantico

# settings.gradle
cat > settings.gradle << 'EOF'
rootProject.name = "Neuromantico"
include(":app")
EOF

# build.gradle (root)
cat > build.gradle << 'EOF'
buildscript {
    ext {
        compose_version = '1.5.0'
    }
}

plugins {
    id 'com.android.application' version '8.2.0' apply false
    id 'org.jetbrains.kotlin.android' version '1.9.0' apply false
}
EOF

# estructura
mkdir -p app/src/main/java/com/neuromantico
mkdir -p app/src/main/res/values

# app/build.gradle
cat > app/build.gradle << 'EOF'
plugins {
    id 'com.android.application'
    id 'org.jetbrains.kotlin.android'
}

android {
    namespace 'com.neuromantico'
    compileSdk 34

    defaultConfig {
        applicationId "com.neuromantico"
        minSdk 24
        targetSdk 34
        versionCode 1
        versionName "1.0"
    }

    buildFeatures {
        compose true
    }

    composeOptions {
        kotlinCompilerExtensionVersion '1.5.0'
    }

    kotlinOptions {
        jvmTarget = '17'
    }
}

dependencies {
    implementation 'androidx.core:core-ktx:1.12.0'
    implementation 'androidx.activity:activity-compose:1.8.0'
    implementation 'androidx.compose.ui:ui:1.5.0'
    implementation 'androidx.compose.material3:material3:1.1.0'
}
EOF

# AndroidManifest
cat > app/src/main/AndroidManifest.xml << 'EOF'
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.neuromantico">

    <application
        android:label="Neuromántico"
        android:theme="@android:style/Theme.Material.Light">

        <activity
            android:name=".MainActivity"
            android:exported="true">

            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>

        </activity>
    </application>

</manifest>
EOF

# MainActivity
cat > app/src/main/java/com/neuromantico/MainActivity.kt << 'EOF'
package com.neuromantico

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.foundation.layout.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        setContent {
            App()
        }
    }
}

@Composable
fun App() {
    var estado by remember { mutableStateOf("Sin registro") }

    Column(
        modifier = Modifier.fillMaxSize(),
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {

        Text("🧠 Neuromántico")

        Spacer(modifier = Modifier.height(20.dp))

        Button(onClick = {
            estado = "Registro: ${System.currentTimeMillis()}"
        }) {
            Text("Registrar")
        }

        Spacer(modifier = Modifier.height(20.dp))

        Text(estado)
    }
}
EOF

echo "Proyecto Neuromántico creado."
