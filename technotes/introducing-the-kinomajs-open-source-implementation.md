The [KinomaJS source code](https://github.com/Kinoma/kinomajs) is large – more than 3,700 files. The project has a long history as a proprietary technology, with portions dating back over a decade. Designed for portability, it has included support for numerous versions of many different operating systems. There’s a lot there. There’s some history to navigate through. This tech note is a starting point for your exploration of the KinomaJS code. It won’t answer every question, but should give you a feel for what is there, why it is there, and how the pieces fit together.

***
##KPL
The Kinoma Porting Layer (KPL) is the bottom of the KinomaJS stack. It is a very light portability layer modeled, as much as practical, on POSIX. Because KinomaJS runs on a wide variety of operating systems, our goal is to isolate all direct calls to the host operating in KPL. Some RTOS hosts don’t support the full ANSI C library, so we cannot even safely assume that functions like printf are available. To avoid surprises with the size of types, we use a portable type system modeled on that used by QuickTime.

The documentation for KPL is located at [kinomajs/core/kpl/doc/KPL.pdf](https://github.com/Kinoma/kinomajs/tree/master/core/kpl/doc).

KinomaJS uses KPL for Linux builds. However, we have not fully integrated KPL into KinomaJS, so it is not yet used for iOS / Mac OS X, Android, or Windows. For those platforms, the code is conditionally compiled in, primarily in the Fsk portions of the tree. There are a handful of places where this gets messy, particularly [kinomajs/core/ui/FskWindow.c](https://github.com/Kinoma/kinomajs/blob/master/core/ui/FskWindow.c). We intend to fully migrate to KPL, which will clean up parts of the code and also provide an opportunity to prune out support for obsolete operating systems.

***
##XS Virtual Machine
The XS virtual machine is the implementation of JavaScript for KinomaJS, which is designed and optimized for use on embedded devices. The design choices for embedded are very different, where both RAM and static storage memory are much more limited and CPU performance is considerably less. While XS also happens to run very well for mobile applications, it really shines on embedded devices.

For security and portability reasons, the JavaScript engines in a web browser will not allow the programmer to run native code. To get around this, modern browser-based JavaScript engines have very sophisticated tools for converting scripts to native code on the fly. When building an embedded device, the developers of the device can safely integrate native functions into the JavaScript engine. XS supports this through its “XS in C” interface, documented at [kinomajs/xs/doc/XS_in_C.pdf](https://github.com/Kinoma/kinomajs/tree/master/xs/doc). By assuming that the primary developers of the device can use native code when necessary, the performance demands on the JavaScript engine are reduced, which allows for a simpler and more streamlined engine.

XS is based on the JavaScript (ECMAScript) 5th Edition specification. Like every JavaScript engine, it does not completely conform to the specification. For practical purposes, it has proven to be compatible with most 5th Edition scripts.

XS is an interpreted runtime. Scripts are compiled to byte code, which is then executed by the interpreter. The interpreter is written in pure ANSI C. It has no dependencies on the rest of the KinomaJS repository, so it can be used as a standalone JavaScript engine.

The interpreter has an accelerated run loop, which is used for approximately 80% of opcodes executing typical KinomaJS code. When the accelerated run loop cannot handle an opcode, it falls back to the full run loop. The accelerated run loop has optional ARM native optimizations and uses [threaded code dispatching](http://gcc.gnu.org/onlinedocs/gcc-3.2/gcc/Labels-as-Values.html) to speed opcode processing.

Because scripts on an embedded device don’t change, it is practical to pre-compile them to byte code. Pre-compiled byte code is more compact than script code and loads considerably faster. The xsc command line tool (at [kinomajs/xs/sources/xsc](https://github.com/Kinoma/kinomajs/tree/master/xs/sources/xsc)) compiles JavaScript to KinomaJS byte code.

XS has a network debugging protocol, which uses a variation of HTTP with XML payloads. Because it is network based, remote debugging is supported, which is essential for embedded development. The [Kinoma Studio IDE](../../../../develop/studio/) contains a client for the XS debugger protocol.

Internally, XS implements an integer type as an optimization to the JavaScript Number. Number values which can be expressed as a 32-bit signed integer may be stored and operated on using the integer type. This is handled transparently to scripts, so there is no change in the language or runtime model.

XS has almost no extensions to the standard JavaScript language. The one significant exception is support for a “sandbox.” The sandbox is a way to ensure there are no collisions between properties used by the embedded device implementation and user scripts running in the same VM. It is a bit like having private fields in a C++ class. The sandbox was originally designed to support DRM implementations in JavaScript, though it is no longer used for that. The sandbox is described briefly in the XS documentation at [kinomajs/xs/doc/XS.pdf](https://github.com/Kinoma/kinomajs/tree/master/xs/doc). We are considering eliminating the sandbox in the future, which would somewhat simplify the runtime. However, the current KPR implementation depends on the sandbox.

***
##Fsk
The Fsk layer is the fundamental functional layer of KinomaJS. In the source tree, it is contained in [kinomajs/core/](https://github.com/Kinoma/kinomajs/tree/master/core). The implementation is designed to be fully portable by relying on KPL. The core functionality is minimal by most measures, to keep the footprint small. The Fsk layer includes the following functionality:

* Memory
* Lists
* Files
* Strings
* Text format conversion
* Time
* Timer callbacks
* Network - sockets, DNS, HTTP
* Synchronization primitives
* Threads
* Window system integration
* Events
* Graphics
* Digital media managers
* Instrumentation for debugging

[FskExtensions.c](https://github.com/Kinoma/kinomajs/blob/master/core/base/FskExtensions.c) implements a simple mechanism for native code plug-ins to the Fsk layer. Extensions are used by Files and Digital media managers for plug-ins. For example. support for reading files directly from compressed ZIP files is implemented by the [kinomajs/extensions/fsZip](https://github.com/Kinoma/kinomajs/tree/master/extensions/fsZip) extension.

The Fsk layer is a pure native implementation with no direct interaction with JavaScript. In KinomaJS, the KPR layer is what binds scripts through XS to the native functionality provided by Fsk. In KinomaJS, XS is hosted on top of Fsk through bindings implemented in [kinomajs/xs/sources/xslib/xs_fsk.c](https://github.com/Kinoma/kinomajs/blob/master/xs/sources/xslib/xs_fsk.c).

***
##Graphics
Graphics is part of the Fsk layer. The core graphics code is contained at [kinomajs/core/graphics](https://github.com/Kinoma/kinomajs/tree/master/core/graphics). The core graphics model is extremely simple. There are just three primitives: fill rectangle, blit bitmap, and draw text. While this may seem too limited to be useful, it is sufficient for a surprisingly large fraction of applications.

The core contains a full software implementation of these primitives, with support for a wide array of pixel sizes and formats. Text is composited by Fsk graphics. Glyphs are generated by the host OS when running on Mac OS X, iOS, and Windows; or by FreeType when running on Linux or Android.

The core also contains an optional OpenGL ES 2.0 implementation of the core graphics model. Applications render through the APIs provided by [kinomajs/core/base/FskPort.c](https://github.com/Kinoma/kinomajs/blob/master/core/base/FskPort.c), which ensures they get the same results whether using software or OpenGL rendering. The OpenGL implementation has been extensively tested for consistent results against a wide range of GPUs and operating systems.

More complex graphics operations can be implemented using the core graphics primitives. For example, KPR builds advanced text layout capabilities on top of the core text drawing capabilities.

The core graphics code implements visual effects, again both in software and OpenGL. Visual effects include box blur, colorize, erode, dilate, gaussian blur, inner & outer glow, inner & outer shadow, and shade. The fundamental effects can be combined to render more complex operations.

There is also an implementation of vector graphics based on HTML5 Canvas. The software rendering implementation is complete. Rendering performance is good though there is room for additional optimizations. An OpenGL ES 2.0 implementation of Canvas is underway, but not yet ready for use.

***
##Media
Digital media support in KinomaJS is based on extensions that plug into relatively simple managers in the core. There are media managers for audio codecs (decode and encode), video codecs (decode and encode), audio filters, media readers (demuxing), media writers (muxers), and media players. The KinomaJS contains portable implementations of many of the most commonly used of these. The audio and video codecs are not highly optimized, and even if they were, software implementations cannot compete with hardware accelerated codec implementations. When possible, KinomaJS uses the audio and video codecs provided by the host platform. This is done today on both iOS and Android.

The [FskMediaPlayer](https://github.com/Kinoma/kinomajs/blob/master/core/managers/FskMediaPlayer.c) manager allows several different media players to co-exist in KinomaJS. The current KinomaJS code contains a single media player, [FskMediaPlayerReader](https://github.com/Kinoma/kinomajs/blob/master/kinoma/mediareader/sources/FskMediaPlayerReader.c), which uses FskMediaReader extensions to demux data from files and streams, sequences them, decompresses them with FskAudioDecompress and FskVideoDecompress decoders, and then renders the video through FskPort while playing audio using FskAudio. There are media readers for AMR, ASF, AVI, FLAC, FLV, JPEG web cameras, MP3, MP4/3GPP/QuickTime, MPEG-1, MPEG-2, PCM, RTSP, and WAVE.

> Note: In the past, the FskMediaPlayer manager has been used to integrate support for DirectShow, QuickTime, and other proprietary media engines.
FskAudioDecompress implementations are provided for MP3, AAC, and Speex. FskVideoDecompress implementations are provided for MPEG-4 and AVC. In addition, the still image formats JPEG, PNG, BMP, and GIF are also implemented as FskVideoDecompress extensions.

***
##KPR
The Kinoma Platform Runtime (KPR) is the C code that implements the KinomaJS application framework. It provides the native implementation of the JavaScript objects that KinomaJS apps and shells interact with. There is extensive [documentation](../../../../develop/documentation/) for building apps in JavaScript using KinomaJS. The documentation describes in detail the containment hierarchy used to build and render the user interface of applications, and the asynchronous messaging system that is used extensively in KinomaJS for network operations, files, hardware pin access, and communication between different parts of a KinomaJS app.

KPR is designed to run efficiently on top of Fsk and XS. Where possible, it manages JavaScript objects using a technique called [volatile host instances](../../../../develop/documentation/overview/) that help reduce the size of the JavaScript memory heap, which reduces the time needed for garbage collection.

KPR has a services mechanism used to add new native services. Examples of this in the KinomaJS repository are the hardware pins service used by Kinoma Create to allow applications to script access to digital, analog, I2C, and serial pins; and the library service which provides access to the platform native media libraries on iOS and Android.

> Note: In the past, there have been other application runtimes built on top of Fsk. The KPS application runtime for the Kinoma Play app for Android, Windows Mobile, and Symbian pre-dated KPR, while building on XS and Fsk.

***
##XML
XS, Fsk, and the earlier KPS application framework made extensive use of XML. These efforts began before JSON became the de facto standard for data exchange. Because it implements JavaScript 5th Edition, XS supports JSON fully. However, XS also contains a very efficient mechanism called grammars for parsing XML documents into JavaScript objects, and serializing JavaScript objects back to XML. XS grammars and their patterns are documented in [kinomajs/xs/doc/XS.pdf](https://github.com/Kinoma/kinomajs/tree/master/xs/doc).

However, given the decline in the use of XML, we intend to make both the XML grammar and DOM support optional part of XS in the future. We still believe there are good uses of XML, but it no longer justifies a permanent place in the core.

***
##Roadmap
KinomaJS is a constantly evolving code base. Our future plans include feature enhancements, portability improvements, and some code clean ups that have been delayed longer than we’d like. The list that follows briefly describes the next steps we intend to take. This roadmap is not set in stone. Your feedback and input will help us refine this the roadmap to best meet the needs of developers using KinomaJS.

####XS (JavaScript / ECMAScript)

* ECMAScript 6th Edition
* Runtime performance improvements
* Further reduce runtime memory use

####KPR

* Update to use ECMAScript 6th Edition features
* Enhance diagnostic support for developers

####Platforms / Portability

* Enable Windows OS runtime build
* Complete Linux/GTK platform support
* Use KPL to isolate all platform dependencies
* Separate monolithic Android Java bindings out to corresponding extensions
* Use Android NDK functions where now available instead of bridging to Java
* Scrub historic platform support
* 64-bit runtime support for all platforms
* Phone conditionals update to iOS

####Rendering

* Rewrite FskWindow to simplify runtime and porting
* Formalize OpenGL initialization / termination
* OpenGL accelerated HTML5 Canvas implementation

####Documentation

* Software architecture
* Portability coding practices
More completely document build