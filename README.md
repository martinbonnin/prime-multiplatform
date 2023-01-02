## Purpose

The purpose of the repository is to compare build time and performances of Kotlin on various platforms:
 - Kotlin/JVM
 - Kotlin/Native
 - Kotlin/JVM compiled to native using GraalVM native image compiler
 - Kotlin/Wasm

It is based on simple program generating pairs of twin primes from https://discuss.kotlinlang.org/t/why-is-kotlin-native-much-slower-than-jvm/10226/10.

## Data points

| Platform            | Build time | Execution time | Artifact size | peak RSS |
|---------------------|------------|----------------|---------------|----------|
| Kotlin/JVM          |   13s      | 58s            | 1.9 MB + JDK  | 606 MB   |
| GraalVM CE native   |   15.3s    | 2m25           | 12.4 MB       | 61 MB    |
| Kotlin/Native       |   38s      | 3m03s          | 0.57 MB       | 6 MB     |
| Kotlin/Wasm preview |   11s      | 2m12           | 0.18 MB + Node| 109 MB   |

## Environment
- MacBook Pro M1 14" 2021
- GraalVM CE 22.3.0 (build 17.0.5+8-jvmci-22.3-b08)

## Getting started

[![Compile and run](https://github.com/sdeleuze/prime-multiplatform/actions/workflows/main.yml/badge.svg)](https://github.com/sdeleuze/prime-multiplatform/actions/workflows/main.yml)

Notice I was not able to use a single Gradle multiplatform build due to a conflict between Kotlin/Native and Native Build Tools plugins. 

### Kotlin/JVM

Compile
```
./mvnw clean package
```
Run
```
java -jar target/prime-multiplatform-1.0-SNAPSHOT-jar-with-dependencies.jar
```

### Kotlin/JVM with GraalVM native image 

Compile
```
./mvnw clean native:compile
```
Run
```
target/prime-multiplatform 
```

### Kotlin/Native

Compile
```
./gradlew clean linkReleaseExecutableNative
```
Run
```
java -jar target/prime-multiplatform-1.0-SNAPSHOT-jar-with-dependencies.jar
```

### Kotlin/Wasm

Compile
```
./gradlew clean wasmMainClasses
```
Run
```
./gradlew wasmNodeProductionRun
```
