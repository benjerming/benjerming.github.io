---
title: AndroidStusio集成CMake
---

# Android Studio调用cmake样例

## 样例1

* abi: arm64-v8a
* variant: Debug

```bash
$ /opt/android-sdk/cmake/3.22.1/bin/cmake \
  -H/home/atlas/PDF2Word_libs/MuPDF/app/src/main/mupdf \
  -DCMAKE_SYSTEM_NAME=Android \
  -DCMAKE_EXPORT_COMPILE_COMMANDS=ON \
  -DCMAKE_SYSTEM_VERSION=22 \
  -DANDROID_PLATFORM=android-22 \
  -DANDROID_ABI=arm64-v8a \
  -DCMAKE_ANDROID_ARCH_ABI=arm64-v8a \
  -DANDROID_NDK=/opt/android-sdk/ndk/26.1.10909125 \
  -DCMAKE_ANDROID_NDK=/opt/android-sdk/ndk/26.1.10909125 \
  -DCMAKE_TOOLCHAIN_FILE=/opt/android-sdk/ndk/26.1.10909125/build/cmake/android.toolchain.cmake \
  -DCMAKE_MAKE_PROGRAM=/opt/android-sdk/cmake/3.22.1/bin/ninja \
  -DCMAKE_LIBRARY_OUTPUT_DIRECTORY=/home/atlas/PDF2Word_libs/MuPDF/app/build/intermediates/cxx/Debug/583j5311/obj/arm64-v8a \
  -DCMAKE_RUNTIME_OUTPUT_DIRECTORY=/home/atlas/PDF2Word_libs/MuPDF/app/build/intermediates/cxx/Debug/583j5311/obj/arm64-v8a \
  -DCMAKE_BUILD_TYPE=Debug \
  -B/home/atlas/PDF2Word_libs/MuPDF/app/.cxx/Debug/583j5311/arm64-v8a \
  -GNinja
```
