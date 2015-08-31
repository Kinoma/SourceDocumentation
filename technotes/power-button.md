![](http://localhost/create/user-guide/img/firstRun03.png)

Kinoma Create has just one button: the Power Button. Yes, you use the power button to turn Kinoma Create on. But, the power button has other useful functions too.

The power button is located on the back of Kinoma Create, at the top above the built-in holders for the removable white legs.

Once Kinoma Create has booted, tap the power button to bring up its menu. The menu always includes Shutdown, Sleep, and Cancel buttons.

*** 
![](http://localhost/develop/documentation/technotes/images/shutdown-menu01.png)

**Shutdown** is the safest way to turn off Kinoma Create. It ensures that all file system caches have been flushed to Flash memory and the SD Card. Shutting down usually take just a few seconds, longer when a large file system cache flush is pending. If you do not turn off Kinoma Create using the Shutdown command, there is a chance that the file system will be corrupted which could lead to erratic behavior or failure to boot. Shutdown invokes onQuitting on the current application’s behavior before shutting down.

**Sleep** puts Kinoma Create into a lower power mode. The CPU, Wi-Fi, speaker, microphone, touch sensor, screen, and many other components are completely powered off. Sleep mode does use more power than turning Kinoma Create off, so use Shutdown if you do not expect to use Kinoma Create for an extended period of time. After selecting sleep, the screen will dim before turning off. To wake from sleep, tap the power button. Kinoma Create takes a few seconds to wake from sleep. It has fully woken and is ready for use when the screen un-dims.

> Note: Sleep is a relatively new feature in Kinoma Create so there may be some issues. Please report any problems you notice after waking from Sleep so they can be fixed in a software update.

**Cancel** removes the power button menu from the screen. You can also cancel by tapping in the area outside the power button menu.

*** 
![](http://localhost/develop/documentation/technotes/images/shutdown-menu02.png)

When an application is running, the power button menu adds a Home item. Tap it to return to Home from any application running on Kinoma Create. When returning to Home this way, the Shell first invokes onQuitting on the application’s behavior, so it has a chance to clean-up, flush preferences, etc.

***
![](http://localhost/develop/documentation/technotes/images/back-to-home-gesture.png)

If Kinoma Create is not responding, you can try using the Home gesture. This gesture will return Kinoma Create to home, just as if the Home button had been pressed. It runs in a background daemon, so it is able to respond in some situations (such as when the application is in an infinite loop) where the power button cannot.

If Kinoma Create is on but not responding to the power button press or Home Gesture, force Kinoma Create to turn off by pressing and holding the power button for about 8 seconds. Using this approach to shutdown Kinoma Create is not recommended as it does not flush the file system caches.

From a KinomaJS perspective, the power button behaves like a key on a keyboard, generating both key down and key up events which are delivered to the active application. Typically applications on Kinoma Create do not watch for key events, so the key down and key up events are delivered to the shell which triggers the power button menu. Your application can monitor and handle power button presses. Here’s a trivial onKeyDown handler that disables the power button, which would be useful for installations of Kinoma Create where you don’t want the user to access the power menu.

	function onKeyDown(label, key, repeat, ticks) { 
		if (key) { 
			var code = key.charCodeAt(0); 
		if (Event.FunctionKeyPower == code) 
			return true; 
		} 
		return false; 
	}
	
> Note: The long press and hold behavior of the power button, which turns off Kinoma Create, is implemented in hardware.
> It cannot be overridden in software.
 
