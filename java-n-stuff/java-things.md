# This section covers Java versions for each of the supported versions of Minecraft.

| Table of contents |
|:---:|
| [1. Picking a Java version](#picking-a-java-version) |
| [2. JVM Arguments](#jvm-arguments) |

# Picking a Java version

## 1.20.1+
| Mod Loader | Java | Notes |
|:---:|:---:|:---:|
| Fabric | [Adoptium Java 25] | - |
| Forge/NeoForge | [Adoptium Java 25] | - | 
| Forge | [Adoptium Java 21] | Use ONLY if you have a mod incompatible with Java 25 |

## 1.18.2 & 1.19.2
| Mod Loader | Java | Notes |
|:---:|:---:|:---:|
| Fabric | [Adoptium Java 25] | - |
| Forge | [Adoptium Java 21] | - |

## 1.16.5
| Mod Loader | Java | Notes |
|:---:|:---:|:---:|
| Fabric | [Adoptium Java 25] | - |
| Forge | [Adoptium Java 21] | I highly recommend using [these JVM arguments](https://github.com/embeddedt/ModernFix/wiki/1.16---required-arguments-for-Java-17), as well as [NashornCompatLayer](https://github.com/embeddedt/NashornCompatLayer/releases) |
| Fabric/Forge | [Adoptium Java 8] | Use ONLY if you have a mod which doesn't work with Java 21 or 25 |

## 1.12.2
| Mod Loader | Java | Notes |
|:---:|:---:|:---:|
| Cleanroom | [Adoptium Java 25] | - |

## 1.8.9
| Mod Loader | Java | Notes |
|:---:|:---:|:---:|
| Forge | [Adoptium Java 8] | - |
| Ornithe | [Adoptium Java 25] | Requires [Legacy LWJGL3](https://modrinth.com/mod/moehreag-legacy-lwjgl3) |

## 1.7.10
| Mod Loader | Java | Notes |
|:---:|:---:|:---:|
| Forge | [Adoptium Java 25] | Requires [LWJGL3ify](https://modrinth.com/mod/lwjgl3ify) | - |

# JVM Arguments

Depending on the Java version, different JVM arguments are available. Here's what I usually use:

| Java version | JVM arguments |
|:---:|:---:|
| Java 25 | `-XX:+UseCompactObjectHeaders -XX:+UseZGC` |
| Java 21 | `-XX:+UseZGC -XX:+ZGenerational` |
| Java 8 | `-XX:+UseG1GC` |

>[!NOTE]
>If you have an old CPU or less than 16 GB of total system RAM, I wouldn't recommend using ZGC. G1GC is a better option for such systems.

Brief explanation of what these arguments do:

`-XX:+UseG1GC` - Enables the G1 Garbage Collector. G1 is the default since Java 9, so the argument is only necessary on Java 8, as it uses Parallel GC by default, which causes stop-the-world pauses.

`-XX:+UseZGC` - Enables the Z Garbage Collector. It uses more RAM and CPU (lowering throughput) but eliminates GC-related stutters. Only available on Java 17 and above.

`-XX:+ZGenerational` - Makes ZGC generational, significantly improving performance. Only necessary on Java 21, as ZGC is generational by default since Java 23.

`-XX:+UseShenandoahGC` - Enables the Shenandoah Garbage Collector. It is a middle-ground between G1GC, providing good throughput, RAM usage, and significantly lowers GC-related stutters.

`-XX:+UnlockExperimentalVMOptions -XX:ShenandoahGCMode=generational` - Makes Shenandoah generational, significantly improving performance. Only necessary on Java 24, as Java 25 makes it the default.

`-XX:+UseCompactObjectHeaders` - Enables Compact Object Headers. This feature reduces RAM usage and boosts performance a bit, at no cost. Only available on Java 24 and above.

### Additional JVM arguments

`-Djava.locale.providers=JRE` Fixes `NNBSP` characters showing up in DateFormat outputs (e.g. near timestamps) on some versions when using Java 20 or newer.

[Adoptium Java 25]: https://adoptium.net/temurin/releases/?version=25&package=jre
[Adoptium Java 21]: https://adoptium.net/temurin/releases/?version=21&package=jre
[Adoptium Java 8]: https://adoptium.net/temurin/releases/?version=8&package=jre
