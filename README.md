# Boost for Android

Port of Boost C++ libraries for android platform. Only latest version of boost (1.55.0) is supported.

# Quick Start

## Requirements

* android standalone toolchain from NDK available in PATH variable

  can be easily done with the following command:

  `build/tools/make-standalone-toolchain.sh --platform=android-10 --toolchain=arm-linux-androideabi-4.8 --install-dir=/tmp/toolchain`

  Currently supported arm-linux-androideabi-4.8 toolchain from NDK r9b.

## Usage

Just run straightforward script:

`$ ./build.sh`

to download, patch and build boost. You will find headers and built libs in *build* directory.

Now you should add libraries to your current project. The easiest way is to copy include and lib folders to /jni/boost and modify Android.mk:

    LOCAL_CFLAGS += -I$(LOCAL_PATH)/boost/include
    LOCAL_LDLIBS += -L$(LOCAL_PATH)/boost/lib/ -lboost_system ...
    
However it's better to declare boost as NDK prebuilt libraries, see $NDK/docs/PREBUILTS.html for further information.

Note that you should build your project and boost with one toolchain version.

If your project contains multiple shared libraries you **must** link against a shared library STL implementation (`runtime-link=shared` option for bjam). 
Otherwise, some global variables won't be defined uniquely, which can result in all kind of weird behaviour at runtime, like crashes, exceptions not 
being caught properly, and more surprises.