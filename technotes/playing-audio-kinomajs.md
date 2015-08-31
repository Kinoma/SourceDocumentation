KinomaJS has three different ways to play audio. Each is appropriate for a different situation. This Tech Note explains the characteristics of each of the three ways to play audio with KinomaJS, and provides guidance one how to select the right one for your app. In addition, this Tech Note describes in detail the Audio hardware pin for synthesizing audio in realtime.

Kinoma Create has a built-in speaker, making it easy to add sound effects to projects. In a recent hackathon, over a third of the projects incorporated audio to deliver a more complete user experience. KinomaJS provides three different ways to play audio, to accommodate the many different ways that audio can be used in an application.

***
##Sound object

The simplest playback method is using the KinomaJS Sound object. The sound object is designed to play short sounds in response to user interactions. The constructor to the Sound object takes the URL of a sound file. The sound file can be in any format supported by KinomaJS on the host device, which includes MP3, M4A, and WAVE on Kinoma Create. Once the Sound object has been created you can play it.

	var sound = new Sound("file:///tmp/beep.wav");
	sound.play();
	
Typically an application stores its sounds inside its directory, so using a relative URL is convenient. Use mergeURI to resolve the relative path to the file.

	var sound = new Sound(mergeURI(application.url, "assets/beep.mp3"));

An application often creates the sounds objects it needs when it launches. The objects are assigned to a global variable or the application’s model, so they can be played as needed. Creating many sound objects at launch is fast because the file containing the sound is only loaded into memory the first time the sound is played. For larger sound files, this can result in a brief delay the first time the sound is played. Once the sound is played, it remains in memory until the sound instance is garbage collected.

Because the data for sounds is stored completely in memory, sounds are limited by available memory. This means that sounds objects should typically only reference small files. There are two essential ways to keep sound files small: make them short in duration or use audio compression. Keeping sounds short in duration is easy to do in any audio editor. Using audio compression means that the audio is stored in a compressed format such as MP3 or M4A. There are many tools available for compression audio, and most audio editors output in a variety of different formats.

Compressed sound have a size advantage, but they require more CPU power to play. KinomaJS sound objects decompress the audio at the time it is played. Decompressing the audio takes some CPU power. While that is typically under 25% of the available CPU, it is enough that it may slow down some animations or data processing.

The Sound object expects the audio it plays to be short. For efficiency, it only provides for playback of a single sound at a time. While a Sound object is playing, If a second Sound object attempts to play, the first Sound will be stopped. This is behavior is reasonable for user interface audio feedback, but it practically prevents the use of Sound objects to play chords or sequences of notes.

The volume of all sound objects can be changed using the Sound.volume global property. The volume level is a number from 0 to 1.0.

	Sound.volume = 1.0;			// full volume
	Sound.volume = 0.05;			// very quiet
	Sound.volume = Sound.volume / 2;	// reduce volume by half

The [Sound sample app](https://github.com/Kinoma/KPR-examples/tree/master/sound) uses the KinomaJS Sound object to play the shutter sound for a camera. The [`kprSound.c`](https://github.com/Kinoma/kinomajs/blob/master/kinoma/kpr/sources/kprSound.c) C source code that implements the KinomaJS sound object is part of the KinomaJS repository.

***
##Media object
The Media object is the media player in KinomaJS. It plays back video and audio, both from a file and streaming from the network. The Media object is designed to play long form content, such and songs and movies. A Media object is always contained within the [KinomaJS User Interface Containment Hierarchy](../../../documentation/overview/), which determines how it renders. The details of KinomaJS content is outside the scope of this Tech Note.

The Media object supports playback of a wide variety of file and streaming formats. On Kinoma Create, it supports MP3, M4A, MPV, M4V, MP4. MOV, and FLAC. Most of these file formats can be streamed over HTTP. Kinoma Create also supports streaming MP3 audio.

There are several ways to instantiate a Media object. Perhaps the easiest is calling the constructor with a dictionary.

	var video = new Media({url: "file://tmp/video.mp4", aspect: "fill",
	  top: 0, left: 0, width: 160, height: 120});
	application.add(video);

	var song = new Media({url: "http://example.com/song.mp3",
	  width: 0, height: 0});
	application.add(song);

	var backgroundMusic = new Media({width: 0, height: 0,
	  mergeURI(application.url, "assets/backgroundMusic.m4a"});
	application.add(backgroundMusic);
	
The Media instance loads the media asynchronously, so it may not be ready to play immediately after the constructor returns, especially for network sources. The Media instances invokes the onLoaded function on its behavior when the media is ready to play. The application can call play on the media anytime after that.

	function onLoaded(media) {
		media.start();
	}
	
When the Media instances reaches the end, the `onFinished` function on the behavior is invoked. The media can be stopped then.

	function onFinished(media) {
		media.stop();
	}
	
An application may want to have continuous background audio. To implement looping audio, use the `onFinished` function to rewind and restart the media.

	function onFinished(media) {
		media.stop();
		media.time = 0;
		media.start();
	}
	
More than one media object can be instantiated at a time. Several media objects can be playing simultaneously. Typically, however, only one media object is played at a time to minimize the CPU load. The media object implements playback using streaming, even for local files, so only a small portion of a typical file or stream is loaded in memory at any moment.

The Media object API contains a significant number of functions. The complete list of supported functions of the media object is in the [KinomaJS Reference](../../../documentation/javascript). The [media-player sample](https://github.com/Kinoma/KPR-examples/tree/master/media-player) application streams video over http into a simple player user interface. The [SomaFM-Player sample](https://github.com/Kinoma/KPR-examples/tree/master/somafm-player) application implements a radio tuner user interface for listening to streaming radio from SomaFM. The [`kprMedia.c`](https://github.com/Kinoma/kinomajs/blob/master/kinoma/kpr/sources/kprMedia.c) C source code that implements the KinomaJS media object is part of the KinomaJS repository.

***
##Audio output hardware pin

Playing audio with the audio output hardware pin is the most flexible and challenging way to play sound in KinomaJS. The audio output hardware pin allows applications to play uncompressed audio samples in real time. The content of the audio samples is determined by the application using an algorithm of its choosing. There may be no external audio asset files at all. Generating audio samples in realtime, synthesizing audio, is a specialized field requiring knowledge of audio algorithms and optimization. Performance is a critical concern when synthesizing audio in real time, as there will be gaps and pops in the audio if the synthesis is too slow. This Tech Note explains the process of playing synthesized audio, but does not attempt to teach audio synthesis and its optimization.

The audio output hardware pin builds on [KinomaJS Hardware Pins](../../../documentation/pins/), which are used to access sensors and actuators connected to Kinoma Create. A speaker and microphone are an actuator and sensor, so KinomaJS models audio output and input as hardware pins too. Using Hardware Pins has the advantage of running the audio generation code in the Hardware Pins thread, which is separate from the main KinomaJS application thread. This minimizes the chance for the application to block audio synthesis, which would lead to gaps and pops.

When working with Hardware Pins, the application communicates with the hardware pins using messages addressed with a URL. The audio generation code is contained in a BLL module. This Tech Note shows how to use the `synthOut.js` BLL in the [SimpleSynth](https://github.com/Kinoma/KPR-examples/tree/master/simplesynth) sample. The same techniques apply to any BLL that generates audio.

First, the application configures the `synthOut` BLL, which causes KinomaJS to load the module and invoke its configure function.

	application.invoke(new MessageWithObject("pins:configure", {
		audio: {
			require: "synthOut",
			pins: {
				speaker: {sampleRate: 8000}
			}
		}
	}));
	
The key pieces of information here are the name of the BLL file, synthOut, and the `sampleRate`. The `sampleRate` indicates the number of audio samples the synthesizer will generate per second. Higher sample rates provide the potential for better the quality but require more CPU power. For the basic speaker in Kinoma Create 8000 provides adequate quality and allows full synthesis of the audio in JavaScript.

The application then sends two more messages to the BLL. The first message tells the BLL to call its synthesize function whenever the audio output pin needs more audio samples. If that function is unable to provide the samples, the application's `/getSamples` handler is invoked.

	application.invoke(new MessageWithObject("pins:/audio/synthesize?repeat=on&timer=speaker&callback=/getSamples"));
	
Finally, the application sends a message to the BLL to begin synthesizing audio.

	application.invoke(new MessageWithObject("pins:/audio/start"));
	
Once the BLL is running, the application sends messages to the BLL to control the audio being played. The synthesizer can play up to three simultaneous notes:

	application.invoke(new MessageWithObject("pins:/audio/setFrequencies", [440]));

	application.invoke(new MessageWithObject("pins:/audio/setFrequencies", [523, 660]));

	application.invoke(new MessageWithObject("pins:/audio/setFrequencies", [523, 660, 763]));
	
Send the `silence` message to turn off all synthesis

	application.invoke(new MessageWithObject("pins:/audio/silence"));
	
To play a sequence of predefined frequencies, use the `setSequence` message.

	application.invoke(new MessageWithObject("pins:/audio/setSequence", {
		sequence: [
			{samples: 200, frequencies: [523,660]},
			{samples: 200, frequencies: [660,763]},
			{samples: 200, frequencies: [763,880]}
		]
	}));

Note that the `setFrequencies`, `setSequence`, and `silence` messages all execute immediately. Any audio that is playing is immediately stopped and replaced with the audio in the most recent message.

Inside the BLL, each time the audio hardware runs out of audio samples, the synthesize function of the BLL is invoked. The synthesize function generates more audio. There are many different ways to synthesize audio. The `synthOut` BLL uses a simple sine wave generator. The audio is generated as 16 bit signed integer samples and stored into a `chunk`. The `chunk` is then provided to the audio hardware pin.

	this.speaker.write(this.chunk);

The complete `synthOut` sample is contained in the [SimpleSynth sample application](https://github.com/Kinoma/KPR-examples/tree/master/simplesynth). All audio synthesis is contained in the BLL `synthOut.js`. The synthesizer implementation is reasonably well optimized, so it may be a little tricky to understand initially.

Hardware Pins, including the audio pins, are documented in [Programming with Hardware Pins](../../../documentation/pins/). The audio pins, both input and output, have very straightforward native implementations in [`audioIn.c`](https://github.com/Kinoma/kinomajs/blob/master/kinoma/kpr/extensions/pins/sources/audioIn.c).

***
##Choosing an audio playback API
Because KinomaJS offers several different APIs to play back audio, it can sometimes be difficult to know which API to select for a specific application. The following points summarize the characteristics of each audio API.

Choose the Sound object to play…

* Short sounds in response to user events
* Occasional sounds that are independent from one another
* Audio contained in local files
* One sound at a time
* Using the simplest audio API

Choose the Media object to play…

* Audio contained in local files or streaming sources
* Long sounds
* Looping audio
* Multiple audio content at the same time
* Using the most complete audio API

Choose the Audio out hardware pin to play…

* Audio in response to many different real time events
* The lowest latency audio
* Audio with the most control
* Using a very low level audio API with your own audio synthesis design