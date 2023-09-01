# Build Logic for Android Project

## 🔧 How To Use

```kt
// Root project settings.gradle.kts
pluginManagement {
  repositories {
    maven(url = "https://maven.pkg.github.com/indramahkota/build-logic-public/") {
      name = "GitHubPackages"
      credentials {
        username = providers.gradleProperty("github.username").orNull
          ?: System.getenv("GITHUB_USERNAME")
        password = providers.gradleProperty("github.token").orNull
          ?: System.getenv("GITHUB_TOKEN")
      }
    }
    google()
    mavenCentral()
    gradlePluginPortal()
  }
}

dependencyResolutionManagement {
  repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
  repositories {
    maven(url = "https://maven.pkg.github.com/indramahkota/version-catalog-public/") {
      credentials {
        username = providers.gradleProperty("github.username").orNull
          ?: System.getenv("GITHUB_USERNAME")
        password = providers.gradleProperty("github.token").orNull
          ?: System.getenv("GITHUB_TOKEN")
      }
    }
    google()
    mavenCentral()
  }

  versionCatalogs {
    create("indra") {
      from("com.indramahkota.gradle.version:catalog-indramahkota:0.4.0")
    }
  }
}
```

```kt
// Root project build.gradle.kts
plugins {
  alias(indra.plugins.convention.android.app) apply false
  alias(indra.plugins.convention.android.lib) apply false
  alias(indra.plugins.convention.compose.app) apply false
  alias(indra.plugins.convention.compose.lib) apply false
  alias(indra.plugins.convention.publishing) apply false

  alias(indra.plugins.convention.android.config)
  alias(indra.plugins.convention.compose.config)
  alias(indra.plugins.convention.publish.config)
  alias(indra.plugins.convention.detekt)
}

// Initial configuration for subprojects
indramahkota {
  jvmTarget.set(JavaVersion.VERSION_11)

  // Report directory:
  // rootDir/reports/detekt-reports/
  detekt {
    checkOnlyDiffWithBranch("main") {
      fileExtensions = setOf(".kt", ".kts")
    }
  }

  android {
    minSdk.set(23)
    targetSdk.set(34)
    compileSdk.set(34)
  }

  // Report directory:
  // rootDir/reports/compose-reports/
  // rootDir/reports/compose-metrics/
  compose {
    compilerVersion.set("1.5.3")
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
          email = "indramahkota1@gmail.com",
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
  alias(indra.plugins.convention.compose.app)
  alias(libs.plugins.secret.gradle.plugin)
}

//or

plugins {
  // Automatically apply android plugin
  alias(indra.plugins.convention.android.lib)
  alias(indra.plugins.convention.publishing)
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