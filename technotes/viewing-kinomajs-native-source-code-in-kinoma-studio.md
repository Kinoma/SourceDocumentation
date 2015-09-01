Sometimes when debugging a KinomaJS app in Kinoma Studio, an exception is thrown somewhere in the native source code. Until recently, there was no way to look at the code where the exception occurred. Now with the open source release of KinomaJS, you can download the native source code and link to it in Kinoma Studio, for a more complete debugging experience.

This tech note demonstrates an example app that causes an exception to be thrown in native code, and explains how to configure the debugging source path to locate the native code from the KinomaJS open source repository downloaded from GitHub.

***
##A Broken Example
The following simple program causes an exception to be thrown in the main program:

	//@program
	require( "Foo" );
	
When we run the program, it throws an exception because it can’t find the module “Foo”. Even though the debugger is smart enough to find the line of code in the application where module is being attempted to load, you will notice in the debug view that the top of the stack is a function in the file “kinoma/kpr/kpr.c” called “require”.

![](../images/view-kinomajs-studio/01.png)

***
Clicking on the require function in the debug view will attempt to load the file “kpr.c” into the editor, but fails because it can’t be located in the source lookup path.

![](../images/view-kinomajs-studio/02.png)

***
##Setting the source lookup path
Fortunately, there is now a solution allowing you to link to the actual native source code being referenced in the debugger! Download the [KinomaJS repository](https://github.com/kinoma/kinomajs). Then, click on the “Edit Source Lookup Path…” button to add the repository root directory to your source lookup path:

![](../images/view-kinomajs-studio/03.png)

***
Click on the “Add…” button to add a new lookup path directory:

![](../images/view-kinomajs-studio/04.png)

***
Next, select the “File System Directory” item in the list and click “OK”.

![](../images/view-kinomajs-studio/05.png)

***
Finally, browse to the directory of your KinomaJS repository downloaded from GitHub or enter its path in the dialog’s text field and click “OK”.

You should now see the native source code in the Kinoma Studio editor. Single stepping works between calls to the XS virtual machine, so you may be able to step through code as it exits out of the exception.

![](../images/view-kinomajs-studio/06.png)

***
##Modifying the source lookup path
There is an alternative, and not so intuitive way to modify the source lookup path, which may be useful (in particular if you want to modify or remove the path entry you just added). If you right-click on a stack frame item in the Debug view, you can select the “Edit Source Lookup…” menu item from the context menu to modify the source lookup path.

![](../images/view-kinomajs-studio/07.png)
