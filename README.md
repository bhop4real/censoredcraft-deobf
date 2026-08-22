# Censoredcraft

Self-contained snapshot of the recovered source for [Censored]craft,
a Minecraft 1.12.2 Forge mod by Trolmastercard

The original release JAR was obfuscated with Zelix KlassMaster (ZKM). This
archive is the output of a deobfuscation and recovery effort: the class tree
was decompiled, semantically renamed where possible, and wired up as a Gradle
project. The Gradle wrapper is included, so no local Gradle install is needed.

> **Adult content notice.** This is an 18+ mod. The original game ships an
> age-gate dialog at launch (`AgeWarningLauncher`), and the mod's own metadata
> describes it as containing pornographic content. This repository is for
> source study and preservation only.

<img width="258" height="246" alt="deobf-1" src="https://github.com/user-attachments/assets/9b454d1f-0be6-493b-abbb-622e9e41ab53" />
<img width="356" height="359" alt="deobf-2" src="https://github.com/user-attachments/assets/a332f2ed-a6b6-4507-9781-0e54730f08b7" />

## Layout

```text
gradlew / gradlew.bat      Gradle wrapper scripts (Gradle 9.7.0)
gradle/wrapper/            Wrapper JAR and distribution config
build.gradle               Build wiring for the recovered project
settings.gradle            Gradle project name
src/main/java/             Recovered, semantically renamed Java sources (296 classes)
src/main/java-runtime-fix/ Manually repaired classes compiled into the JAR
                           (Main.java, ModConstants.java)
src/main/resources/        Original mod assets: animations, geo models, textures,
                           sounds, structures, recipes, loot tables, translations,
                           mcmod.info
censoredcraft-runtime.jar  Renamed runtime bytecode packaged into the final JAR
external/                  SRG-mapped Forge jars used as compile-time references
libs/                      log4j-api jar used as a compile-time reference
deobf-1.jpg, deobf-2.png   The aforementioned memes
dist/                      Build output (created by Gradle)
```

## Building

The archive is self-contained: all binary dependencies are included.

```bash
./gradlew clean jar
```

The runnable mod JAR is written to:

```text
dist/censoredcraft-1.1.0-deobfuscated.jar
```

Notes:

- The build compiles with **Java 8** and packages the 
  valid renamed runtime bytecode in SRG form, which Forge
  1.12.2 expects outside a deobfuscated development environment.
- If your JDK 8 lives elsewhere, adjust `options.forkOptions.executable`
  in `build.gradle` accordingly.

## Deobfuscation Status

The recovered tree under `src/main/java` is **reference material**, not a
compilable project:

- ZKM erased member names, so most methods and fields still carry names like
  `a`, `b`, or `l`.
- The obfuscator produced **duplicate erased method signatures** that make the
  decompiled tree impossible to compile as-is.
- Classes were renamed to meaningful names (`CompanionFollowAi`,
  `CustomModelManager`, `CumParticleManager`, ...) during recovery, and the
  two boot-critical classes (`Main`, `ModConstants`) were manually repaired in
  `src/main/java-runtime-fix` and are compiled into the final JAR.
- Minecraft member references remain in **SRG form** (e.g.
  `Vec3d.field_186680_a`), matching what Forge 1.12.2 loads at runtime.
