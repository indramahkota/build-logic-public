# Build Logic For Android Exploration

## 🔧 How To Use

```kt
// Root project settings.gradle.kts
pluginManagement {
    repositories {
        maven {
            url = uri("https://maven.pkg.github.com/indramahkota/build-logic-public/")
            credentials {
                username = "indramahkota"
                password = "" // Artifact available on public repository
            }
        }
}
```

```kt
// Root project build.gradle.kts
@Suppress("StringLiteralDuplication")
plugins {
    id("com.indramahkota.build.logic.convention.detekt") version "0.0.1"

    id("com.indramahkota.build.logic.convention.android-config") version "0.0.1"
    id("com.indramahkota.build.logic.convention.android-lib") version "0.0.1" apply false
    id("com.indramahkota.build.logic.convention.android-app") version "0.0.1" apply false

    id("com.indramahkota.build.logic.convention.compose-config") version "0.0.1"
    id("com.indramahkota.build.logic.convention.compose-lib") version "0.0.1" apply false
    id("com.indramahkota.build.logic.convention.compose-app") version "0.0.1" apply false
}

val androidApplicationId by extra { "com.indramahkota.app.exploration" }
val androidVersionCode by extra { 1 }
val androidVersionName by extra { "0.0.0" }

// Initial configuration for subprojects
indramahkota {
    // Default $root/config/
    configsDir.set(file("config/"))

    // Default $root/build/reports/
    reportsDir.set(file("build/reports/"))

    // Default JavaVersion.VERSION_1_8
    jvmTarget.set(JavaVersion.VERSION_11)

    android {
        minSdk.set(23)
        targetSdk.set(32)
    }

    compose {
        compilerVersion.set("1.3.0")
        enableComposeCompilerMetrics.set(true)
        enableComposeCompilerReports.set(true)
    }
}

```

```kt
// In submodules project build.gradle.kts
plugins {
    id("com.indramahkota.build.logic.convention.android-app")
    id("com.indramahkota.build.logic.convention.compose-app")
}

//or

plugins {
    id("com.indramahkota.build.logic.convention.android-lib")
    id("com.indramahkota.build.logic.convention.compose-lib")
}

```

## License

```markdown
Copyright (c) 2022 Indra Mahkota

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```