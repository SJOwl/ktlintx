# Ktlint module for android projects
## Usage
1. Include this repo as module into your app
2. Main gradle.kts file
``` kotlin
buildscript {
    repositories {
        google()
        jcenter()
        mavenCentral()
    }
    dependencies {
        classpath("com.android.tools.build:gradle:3.4.1")
        classpath("com.pinterest:ktlint:${Versions.ktlint}")
        classpath("org.jetbrains.kotlin:kotlin-gradle-plugin:${Versions.kotlin}")
    }
}
```

Any other module
``` kotlin
val ktlint by configurations.creating
val outputDir = "${project.buildDir}/reports/ktlint/"
val inputFiles = project.fileTree(mapOf("dir" to "src", "include" to "**/*.kt"))

dependencies {
    ktlint("com.github.shyiko:ktlint:${Versions.ktlint}")

    ktlint(project(":ktlintx"))
}
val ktlintFormat by tasks.creating(JavaExec::class) {
    inputs.files(inputFiles)
    outputs.dir(outputDir)
    description = "Fix Kotlin code style deviations."
    classpath = ktlint
    main = "com.github.shyiko.ktlint.Main"
    args = listOf("-F", "src/**/*.kt")
}
// task: format all changed files before build
tasks {
    preBuild {
        dependsOn(ktlintFormat)
    }
}
```
