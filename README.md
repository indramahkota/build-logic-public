# Build Logic For Android Exploration

## 🔧 How To Use

```kt
// Root project settings.gradle.kts
pluginManagement {
    repositories {
        maven {
            url = uri("https://maven.pkg.github.com/indramahkota/build-logic-public/")
            credentials {
                username = "YOUR GITHUB USERNAME"
                password = "YOUR GITHUB TOKEN"
            }
        }
}
```

```kt
// Root project build.gradle.kts
plugins {
    id("com.indramahkota.detekt")
    id("com.indramahkota.android.config")
    id("com.indramahkota.compose.config")
    id("com.indramahkota.publish.config")
}

// Initial configuration for subprojects
indramahkota {
    // Default JavaVersion.VERSION_1_8
    jvmTarget.set(JavaVersion.VERSION_11)
    withCompilerArgs {
        compilerArgs = setOf(
            "-Xlint:unchecked", "-Xlint:deprecation"
        )
    }

    // Report directory: $reportsDir/detekt-reports/
    detekt {
        // Related with :detektDiff task
        checkOnlyDiffWithBranch("main") {
            fileExtensions = setOf(".kt", ".kts")
        }
    }

    android {
        minSdk.set(23)
        targetSdk.set(33)
    }

    // Report directory:
    // - $reportsDir/compose-reports/
    // - $reportsDir/compose-metrics/
    compose {
        // https://developer.android.com/jetpack/androidx/releases/compose
        // compiler and runtime is mandatory property
        compilerVersion.set("1.4.3")
        // Must be same with supported version
        // Current using bom version 2023.01.00
        runtimeVersion.set("1.3.3")
        enableComposeCompilerMetrics.set(true)
        enableComposeCompilerReports.set(true)
    }

    // Set maven pom for all sub projects
    publishing {
        pom {
            setGitHubProject {
                owner = "indramahkota"
                repository = "android-exploration"
            }

            licenses {
                mit()
            }

            developers {
                developer(
                    id = "indramahkota",
                    name = "Indra Mahkota",
                    email = "indramahkota1@gmail.com"
                )
            }
        }
    }
}

```

```kt
// In submodules project build.gradle.kts
plugins {
    // Automatically apply android plugin
    id("com.indramahkota.compose.app")
    id("com.indramahkota.hilt")
}

//or

plugins {
    // Automatically apply android plugin
    id("com.indramahkota.compose.lib")
    id("com.indramahkota.publishing")
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