This tutorial explains the Kinoma Create Command Line Tool (`kct`), used to install, delete and run JavaScript applications on Kinoma Create. [You can download kct from GitHub here](https://github.com/Kinoma/kct).

*** 
##Installation

`kct` requires node.js so, if you do not have it already, go to [http://nodejs.org](https://nodejs.org/) and install the current version of `node.js`.

`kct` is provided as JavaScript source code and must be transformed into an executable. To transform `kct.js` into an executable file, go to the directory where you downloaded kct and execute `make`

	> cd ~/kct
	> make
	
`kct` uses `kpr2js` to process KPR XML files into JavaScript files, and xsc to compile JavaScript files into byte code. Both `kpr2js` and `xsc` are included in Kinoma Studio. Install the current version of Kinoma Studio from [http://kinoma.com/studio/](http://kinoma.com/studio/). `kpr2js` and `xsc` are in `/Applications/Kinoma Studio/Plugins/com.marvell.kinoma.kpr.sdk.macosx_1.3.25.1/sdk/frameworks/kpr/tools/`. The path may change in the future based on the Kinoma Studio version number (`1.3.25.1`). Copy `kpr2js`, `xsc`, and `kct` to a directory that contains other tools (for instance /usr/local/bin ) or add their directories to the PATH environment variable.

Copy `kpr2js`, `xsc` and `kct` to a directory that contains other tools (for instance `/usr/local/bin` ) or add their directories to the `PATH` environment variable.

***
##Host and Password

To access your Kinoma Create, `kct` needs its IP address and password. The IP address of your Kinoma Create is displayed on the Wi-Fi tile on the home screen. You can change the password in the Settings application using the Debugging item.

For `kct`, you can define environment variables with the IP address and the password:

	> setenv KINOMA_CREATE_HOST 198.162.0.22
	> setenv KINOMA_CREATE_PASSWORD wow

Or you can pass the IP address and the password to `kct` with the `-h` and `-p` options:

	> kct ping -h 198.162.0.22 -p wow
	
The default IP address is the IP address of your computer, which is useful when testing with the Kinoma Create simulator. The default password is `kinoma`, which is the default password of the Kinoma Create shell.

***
##Application

Kinoma Create sample applications are available on GitHub at [https://github.com/Kinoma/KPR-examples](https://github.com/Kinoma/KPR-examples)

To execute `kct`, go to the directory of the application you want to install, delete or run.

	> cd ~/KPR-examples/balls
	
The directory must contain a file named application.xml that defines the application id, the path to the program to execute when launching the application, and the title of the application.

	> cat application.xml
	<?xml version="1.0" encoding="utf-8"?>
	<application xmlns=http://www.kinoma.com/kpr/application/1
		id="balls.example.kinoma.marvell.com"
		program="src/balls"
		title="Balls Example">
		
On your Kinoma Create, applications can be cached or installed:

* A cached application has no tile on the Kinoma Create home screen and will be automatically deleted when your Kinoma Create is rebooted.
* An installed application has a tile on the Kinoma Create home screen, persists across reboots, and can be deleted using the Settings application.

***
##Actions

The first parameter of kct is the action. The following actions are supported by `kct`: `ping`, `close`, `install`, `delete`, and `run`.

	> kct ping
	
Check if `kct` can access your Kinoma Create. All actions but `clean` do that first.

	> kct close
	
Close the running application. All actions except `ping` and `clean` do that next.

	> kct install
	
Install the application. Sources are processed, compiled and uploaded to your Kinoma Create. Assets are uploaded to your Kinoma Create. A tile is added to the Kinoma Create home screen.

	> kct delete
	
Delete the application. Remove the tile from the Kinoma Create home screen. Delete all files and directories corresponding to the application on your Kinoma Create.

	> kct run

Run the application. Modified sources are processed, compiled and uploaded to your Kinoma Create. Modified assets are uploaded to your Kinoma Create.

If you did not `install` the application, the application is cached on your Kinoma Create so the run action is always incremental.

	> kct clean

To process and compile sources, `kct` uses temporary files and directories. The `clean` action deletes all temporary files and directories corresponding to the application.