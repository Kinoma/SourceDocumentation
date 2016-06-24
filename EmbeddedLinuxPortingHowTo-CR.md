# Porting KinomaJS to an Embedded Linux Target


KinomaJS is built in a similar way for all Linux platforms, though each has its own SDK, toolchain, features, and peculiarities. Here we look at how to port KinomaJS to a particular embedded Linux target.

## Overview

For your specific target, you need a toolchain that runs on the host build platform, and a sysroot with the headers and libraries necessary to build and run on the target. 

After installing the toolchain and sysroot, you pick a name for the target platform, add instructions to the build system to build for your target, and configure the build for your new platform.

In addition, it is recommended that you add the target to the Kinoma Code IDE to enable Kinoma Code to identify the target and connect to it. To work with Kinoma Code, you need to port the `EmbedShell` application to the device to enable it as a debug target for Kinoma Code. Doing this will let you develop and debug KinomaJS code for the new platform with the Kinoma Code IDE.

## Setup

Start with a fresh clone of the KinomaJS `git` tree.

```
git clone https://github.com/Kinoma/kinomajs.git
```

Set the `F_HOME` and `XS6` environment variables to your new directory:

```
export F_HOME=/path/to/kinomajs
export XS6=/path/to/kinomajs/xs6
```

You need a toolchain for your target platform that runs on the build host. For embedded Linux devices, the typical build host is a desktop Linux installation like Ubuntu. This can be set up as a virtual machine using [VirtualBox](https://www.virtualbox.org/wiki/Downloads).

The documentation for your target device should specify a toolchain and SDK to use. Many ARM-based platforms use the [Linaro](http://www.linaro.org) toolchains and sysroots.

A single build host can build for multiple platforms. To organize the toolchains and sysroots, it is recommended that you store them in a directory like `/opt/tools/` (as in this example).

Create a subdirectory in `/opt/tools/` to store the build tools for the new platform. (We will call the new target platform `nuplat` here.) Unpack the toolchain and sysroot into this subdirectory.

```
mkdir -p /opt/tools/nuplat
cd /opt/tools/nuplat
tar -jxvf nuplat-toolchain.tbz
tar -jxvf nuplat-sysroot.tbz
```

The toolchain should contain a version of `gcc` for your target device. Find the particular `gcc` for your target. The command

```
find . -name "*-gcc" -print
```
returns
   
```
./gcc-linaro-4.9-2014.11-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-gcc
```
   
<!--From CR re passive voice below: If this is something the user/reader must do, please change to active voice ("Set the...")-->

The directory before `bin/` will be set to the platform's `platform_GNUEABI` environment variable.

```
export NUPLAT_GNUEABI=/opt/tools/nuplat/gcc-linaro-4.9-2014.11-x86_64_aarch64-linux-gnu
```

Next, look for the libraries and header files in the sysroot for your device. The command

```
find . -name "include" -print
```
   
returns a number of directories, including:
   
```
./sysroot-linaro-eglibc-gcc4.9-2014.11-aarch64-linux-gnu/usr/include
```
   
   
<!--From CR re passive voice below: Ditto my preceding comment.-->

The directory before `usr/include` will be set as the platform's `platform_SYSROOT` environment variable.

```
export NUPLAT_SYSROOT=/opt/tools/nuplat/sysroot-linaro-eglibc-gcc4.9-2014.11-aarch64-linux-gnu
```

## Modifying XS6 Build Tools

KinomaJS applications are built using a tool called `kprconfig6`. This tool is configured and built on the host build platform. As described below, you need to make these two new files for the build tools, based on existing build files:

```
$XS6/cmake/modules/linux/nuplat.cmake
$XS6/tools/cmake/linux/nuplat.js
```

1. In `$XS6/cmake/modules/linux/`, copy one of the existing `_platform_.cmake` files to `nuplat.cmake`, and modify the `nuplat.cmake` file to set these build variables and find your compiler:

   - Change `CMAKE_SYSTEM_PROCESSOR` to match your target processor.

      ```
   set(CMAKE_SYSTEM_PROCESSOR "aarch64")
   ```

   - Change `TOOL_PREFIX` to match the prefix for your gcc.

      ```
   set(TOOL_PREFIX aarch64-linux-gnu-)
   ```

   - Change `_platform__SYSROOT` to match your `sysroot`.

      ```
   set(NUPLAT_SYSROOT $ENV{NUPLAT_SYSROOT})
   ```

   Also change the `find_path` command to look in your `_platform__GNUEABI` directory to find the compiler.

   ```
   find_path(TOOLCHAIN_BIN ${TOOL_PREFIX}gcc $ENV{PATH} $ENV{NUPLAT_GNUEABI}/bin)
   ```

3. In `$XS6/tools/cmake/linux`, copy one of the existing `_platform_.js` files to `nuplat.js`.

4. Build the XS6 tools.

   ```
   cd ${XS6}
   mkdir tmp
   cd tmp
   cmake ${XS6}
   cmake --build . --config Release
   ```

5. Set your `PATH` environment so it can find the XS6 tools.

   ```
   export PATH=${PATH}:${XS6}/bin/linux/$(uname -m)/Release
   ```

## Adding Platform Files

Next you will choose a platform on which to base the `nuplat` build. 

KinomaJS has been ported to a number of ARM, ARM64, x86, and MIPS embedded Linux platforms. Choose a platform to use as a template (whichever platform is most similar to the new target). Here we demonstrate porting to an ARM-based Linux platform that does not have ALSA audio support and that uses `/dev/fb` for display (RGB565). The `linux/aspen` build configuration most closely matches this, so we will use that as a starting point.

Since we have called the new platform `nuplat`, and since it is a subplatform of Linux, `linux/nuplat` will be the platform specifier for the build.

Using `linux/aspen` as a template, make copies of the build directories. 

```
cd $F_HOME/build/linux
cp -r aspen nuplat
cp kpl/KplScreenLinuxK4.c build/linux/kpl/KplScreenLinux-nuplat.c
```

## Modifying Build Files for the New Platform

The directory `$F_HOME/build/linux/nuplat` now contains the build instructions for the platform layer of KinomaJS. Several build files here need to be modified to update some paths and variables in them.

<!--From CR: I'm not sure what to make of the following file name; are all the changes below made in that file? And if so, why do you say "several files" above?-->

`FskPlatform.mk`

1. Change the `include` paths by replacing

   ```
   <input name="$(FSK_SYSROOT_LIB)/include"/>
   <input name="$(FSK_SYSROOT_LIB)/usr/include"/>
   ```

   with

   ```
   <input name="$(NUPLAT_SYSROOT)/usr/include"/>
   ```

   **Note:** Your platform may need additional `include` directories for libraries and the like.

2. Since this target platform does not have ALSA, we will stub out the audio for now. We may also need to modify a copy of the screen handling code. Replace this:

   ```
   <source name="KplAudioLinuxALSA.c"/>
   <source name="KplScreenLinuxK4.c"/>
   ```

   with:

   ```
   <source name="KplAudioLinux.c"/>
   <source name="KplScreenLinux-nuplat.c"/>
   ```

3. The new platform does not support `.wmmx` instructions, so remove the `.wmmx` optimizations by deleting the following:

   ```
   <source name="FskBilerp565SE-arm.wmmx"/>
   <source name="FskBilerpAndBlend565SE-arm.wmmx"/>
   <source name="FskBlit-arm.wmmx"/>
   <source name="FskRectBlitTo16-arm.wmmx"/>
   <source name="FskRotate-arm.wmmx"/>
   <source name="FskTransferAlphaBlit-arm.wmmx"/>
   <source name="yuv420torgb-arm.wmmx"/>

   <c option="-mcpu=iwmmxt2"/>
   <c option="-DSUPPORT_WMMX=1"/>
   ```

4. Remove these Kinoma Create-specific defines:

   ```
   <c option="-DMARVELL_SOC_PXA168=1"/>
   <c option="-DKIDFLIX_ROTATE=0"/>
   <c option="-DMINITV=1"/>
   ```

5. Remove this feature define:

   ```
   <c option="-DTARGET_RT_ALSA=1"/>
   ```

6. Next we will change the library path, to remove ALSA (`-lasound`). After the first attempt at building the app, I found that I needed to change the math library as well. Replace this:

   ```
   <library name="-Wl,-rpath,$(FSK_SYSROOT_LIB)/usr/lib,-z,muldefs"/>
   <library name="-L$(FSK_SYSROOT_LIB)/usr/lib"/>
   <library name="-lasound"/>
   ```

   with this:

   ```
   <library name="-Wl,-rpath,$(NUPLAT_SYSROOT)/usr/lib,-z,muldefs"/>
   <library name="-L$(NUPLAT_SYSROOT)/usr/lib"/>
   <library name="-lm"/>
   ```


## Adding the Platform Instructions to the Application

The last step is to modify the application's `manifest.json` file to add the `linux/nuplat` platform.

In `kinoma/kpr/applications/balls/manifest.json`, look for the `"aspen"` section and copy the whole section, replacing `aspen` with `nuplat`. For example, replace this:

```
"aspen": {
	"build": {
		"DEVICE": true,
	},
	"extensions": {
		"KplFrameBuffer": "$(KPL_HOME)/framebuffer/KplFrameBuffer.mk",
	},
},
```

with this:

```
"nuplat": {
	"build": {
		"DEVICE": true,
	},
	"extensions":{ 
		"KplFrameBuffer": "$(KPL_HOME)/framebuffer/KplFrameBuffer.mk",
	},
},
```

## Building KinomaJS with the New Application
To build KinomaJS with the new application, use a build command like the one shown below. Here the application is the one located in the directory `$F_HOME/kinoma/kpr/applications/balls/`; it is a very simple application that displays four bouncing balls on the screen.

```
cd $F_HOME
kprconfig6 -x -m -p linux/nuplat $F_HOME/kinoma/kpr/applications/balls/manifest.json
```

The target package will be located in `$F_HOME/bin/linux/nuplat/Release/Balls.tgz`. Transfer the `.tgz` file to your target device, decompress it, and run the app.

```
cd /tmp
tar -zxvf Balls.tgz
cd Balls
./Balls
```

Four bouncing balls should appear on the screen. If the application builds and runs on the device, then the build system has been proven to work, you have the proper libraries defined, and most of the base KinomaJS subsystems have compiled properly.


## Debugging

If you are having problems with your build, examine the build lines by using `VERBOSE=1` before executing the `kprconfig6` build command.

```
VERBOSE=1 kprconfig6 -x -m -p linux/nuplat kinoma/kpr/applications/balls/manifest.json
```
   
After changing environment variables or build files, you may want to do a clean build before building again.

```
kprconfig6 -x -m -p linux/nuplat kinoma/kpr/applications/balls/manifest.json -clean
kprconfig6 -x -m -p linux/nuplat kinoma/kpr/applications/balls/manifest.json
```
   
While tracking down a build problem, I discovered that there were some `inotify` parameters that were not defined on the new platform, and I noted that as an item to resolve in the future. To continue with the port, I commented out the `inotify` definition in the platform makefile `kinoma/kpr/cmake/linux/nuplat/FskPlatform.mk` by changing this:
 
```
<c option="-DUSE_INOTIFY=1"/>
```

to this:

```
<!-- c option="-DUSE_INOTIFY=1"/ -->
```

I had some problems on the final link; some system libraries could not be found. The error indicated that it was looking in `/usr/lib`, not `$NUPLAT_SYSROOT/usr/lib`. This platform was built with Yocto and requires the `--sysroot` `c` option. So, in `kinoma/kpr/cmake/linux/nuplat/FskPlatform.mk` I added the following line to resolve the link issue:

```
<c option="--sysroot=${NUPLAT_SYSROOT}"/>
```

The core software build is defined in `$F_HOME/build/linux/nuplat/FskPlatform.mk`.

Many platform-specific porting files are located in `$F_HOME/build/linux/kpl`. Particular files of interest are:

- `KplScreenLinux...` for display

- `KplInput...` for user input
 
- `KplAudioLinux...` for audio output

See the [Instrumentation](https://github.com/Kinoma/kinomajs/blob/master/doc/instrumentation.md) document for insight on what various subsystems are doing, and the [`manifest.json`](https://github.com/Kinoma/kinomajs/blob/master/doc/`manifest.json.md`) document for information about the `manifest.json` file.

## Connecting to the Ecosystem

The easiest way to build KinomaJS applications is with the Kinoma Code IDE, which connects to your target device over the network to do deployment, development, and debugging. 

To enable your device to be discovered and connect to Kinoma Code, you need to take some additional steps: on the IDE host, the Kinoma Code application needs some additional configuration to recognize the new platform; and on the target device, the application `EmbedShell` needs specific code for your new device.

### Modifying Kinoma Code

Kinoma Code needs some configuration information for the new device. This information is contained in the `$XS6/xsedit/features/devices/configs` directory.

1. Copy an existing configuration.

   ```
   cd $XS6/xsedit/features/devices/configs
   cp -r beaglebone nuplat
   ```

2. Modify the configuration.

   - Change the name of the `_platform_.js` file.

      ```
   cd nuplat
   mv beaglebone.js nuplat.js
   ```

   - In the new `nuplat.js` file, change instances of `beaglebone` and `BeagleBone` to `nuplat`.

   - In `nuplat/helper/application.xml`, change instances of `beaglebone` to `nuplat`.

   - If you wish, change the `icon.png` file in `nuplat`.

3. Rebuild the Kinoma Code application.

   ```
   cd $F_HOME
   kprconfig6 -x -m $XS6/xsedit/manifest.json
   ```


### Modifying EmbedShell

EmbedShell is an application that runs on the target device to enable Kinoma Code to discover and deploy applications to it. To modify EmbedShell:

1. Copy an existing configuration.

   ```
   cd $F_HOME/kinoma/kpr/projects/embed/devices/
   cp -r beaglebone nuplat
   cd nuplat
   ```

2. Change information about the device as follows:

   - In `dd.xml`, change the following lines to better match information about your device.

      ```
   <friendlyName>[friendlyName]</friendlyName>
   <manufacturer>CircuitCo</manufacturer>
   <manufacturerURL>http://beagleboard.org</manufacturerURL>
   <modelDescription>BeagleBone Black</modelDescription>
   <modelNumber>1</modelNumber>
   <modelName>BeagleBone Black</modelName>
   <modelURL>http://beagleboard.org/black/</modelURL>
   ```

   - In `description.json`, change the ID from `beaglebone` to `nuplat` and give the device a good title.

      ```
   "id": "com.marvell.kinoma.launcher.nuplat",
   "title": "New Platform",
   ```

   - Change the `icon.png` and `picture.png` files to match your device.

   - In `$F_HOME/kinoma/kpr/projects/embed/manifest.json`, find the `"beaglebone"` section and copy the whole section, replacing `beaglebone` with `nuplat`.

If your device does not support MRAA for pins access, you may also need to remove or change the extensions for `FskPin...MRAA` to `FskPin...Linux`. For initial porting, it may be most expedient to simply remove them.

### Enabling Extensions

Many KinomaJS extensions build for any platform, but certain extensions require different build commands, depending on the platform. For EmbedShell, we need to modify the `pins` and `zeroconf` extensions.

1. In `$F_HOME/kinoma/kpr/extensions/pins/kprPins.mk`, find `linux/beaglebone` and add `,linux/nuplat` after it.

2. In `$F_HOME/kinoma/kpr/extensions/zeroconf/kprZeroconf.mk`, find `linux/beaglebone` and add `,linux/nuplat` after it.

### Building and Running EmbedShell

EmbedShell is built in a similar way as the other KinomaJS standalone applications.

```
cd $F_HOME
kprconfig6 -x -m -p linux/nuplat $F_HOME/kinoma/kpr/projects/embed/manifest.json
```

The target package will be located in `$F_HOME/bin/linux/nuplat/Release/EmbedShell.tgz`. Transfer the `.tgz` file to your target device and decompress it. 

If your device does not include Avahi (an MDNS daemon), start the built `mdnsd` before launching `EmbedShell`.

```
cd /tmp
tar -zxvf EmbedShell.tgz
./EmbedShell/lib/mdnsd
./EmbedShell
```

If your device has a display, it will show information about the IP address, version number of KinomaJS, and access point. Your modified Kinoma Code application should be able to identify the device in Net Scanner and connect to it.

### Enabling a Kinoma Code App
 
In Kinoma Code, an app's `project.json` file needs to have the following code added so that Kinoma Code recognizes the ability to run that app on the device. In the `project.json` file, add the following before the final closing bracket.

```
"nuplat": {
	"main": "main"
},
```