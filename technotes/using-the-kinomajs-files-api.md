KinomaJS Files provides a simple set of APIs that provide platform independent access to the host file system. Use the KinomaJS Files APIs to read and write files, iterate directories, rename files, and so on. All the KinomaJS Files APIs and properties are accessed from the global Files object. This tech note provides a summary of the available Files APIs and properties.

> Note: The [files-chunks](https://github.com/Kinoma/KPR-examples/tree/master/files-chunks) sample app on [GitHub](https://github.com/Kinoma/KPR-examples/) demonstrates how to use each KinomaJS Files API.

***
##Pathnames

Pathnames in KinomaJS use the [file URI](https://en.wikipedia.org/wiki/File_URI_scheme) scheme for both files and directories. Pathnames corresponding to directories must include a trailing slash. If a pathname does not include a trailing slash, then the pathname corresponds to a file:

	file://path/to/directory/
	file://path/to/file.txt

***
##Predefined directories
KinomaJS Files provides access to predefined directories on the host file system. These directories contain specific types of files:

* The `documentsDirectory` stores general-purpose document files that persist across application launches.
* The `picturesDirectory` corresponds to the host directory where picture files are stored.
* The `preferencesDirectory` stores application preference files that persist across application launches.
* The `temporaryDirectory` stores temporary files. Temporary files do not persist across application launches. The Kinoma Create temporaryDirectory lives on a RAM disk that's erased on power cycles and reboots.

The `KinomaJS` global `mergeURI` function is used to construct a URI from a base and partial URL. The partial URL is merged with the base URL to arrive at the full URI.

Using mergeURI we construct full URIs from the predefined directories:

	// Write the my_app_prefs.json file to the host preferences directory
	var uri = mergeURI(Files.preferencesDirectory, "my_app_prefs.json");
	Files.writeJSON(uri, {color: "green"});

	// Write the string "Hello world" into the "hello.txt" file contained in the host documents directory
	var uri = mergeURI(Files.documentsDirectory, "hello.txt");
	var text = "Hello world";
	Files.writeText(uri, text);

***
##Chunks

KinomaJS Files APIs which read and write binary data use a `Chunk` object to hold the data. A `Chunk` is a data type that provides similar functionality to the `Uint8Array`. A chunk instance is created using the `Chunk` constructor. This section provides a basic overview of how to work with chunks. We'll cover the KinomaJS File APIs to read and write chunks later in this document.

When the `Chunk` constructor is invoked without an argument, it returns a new chunk whose length is 0:

	var chunk = new Chunk();
	
When the argument is a number, it returns a new chunk whose length is number:

	var chunk = new Chunk(10);
	
If the argument is a *String*, the constructor returns a new chunk whose length is the number of bytes necessary to decode the string as Base64 text:

	var chunk = new Chunk("VHJpcGVsIFdlc3RtYWxsZQ==");
	
The chunk length is read and set using the `length` property:

	var chunk = new Chunk(25);
	trace("Chunk length is " + chunk.length); // "Chunk length is 25"
	chunk.length = 12;
	trace("Chunk length is " + chunk.length); // "Chunk length is 12"
	
Use subscript notation to read and write chunk bytes:

	var chunk = new Chunk(3);
	chunk[0] = 1;
	chunk[1] = 2;
	chunk[2] = 3;
	for (var i = 0, c = chunk.length; i < c; ++i)
		if (chunk[i] != i + 1)
			throw("chunk data invalid!");
			
Use `free` to recover the memory space used by a *`Chunk`* immediately instead of waiting for the garbage collector to invoke the destructor.

	var chunk = new Chunk(1024);
	…
	chunk.free();
	
***
##Reading and Writing Files

The KinomaJS File I/O APIs can be used to read and write a variety of data types, often eliminating the need for the application to convert the data after reading or before writing. Unless otherwise noted the File write APIs overwrite any existing file.

> Note: The KinomaJS Files APIs throw exceptions on failures. It is therefore recommended to bracket calls to Files APIs with try/catch blocks when there's the potential for errors.

Use `readText` and `writeText` to read and write *`String`* data:

	var uri = mergeURI(application.url, "assets/license.txt");
	var licenseText = Files.readText(uri);
	var textContainer = new Text( {left:0, right:0, string:licenseText });

	var text = container.field.string;
	var uri = mergeURI(Files.preferencesDirectory, "input.txt");
	Files.writeText(uri, text);

Use `appendText` to append *`String`* data to an existing file:

	var uri = mergeURI(Files.temporaryDirectory, "hello.txt");
	Files.writeText(uri, "Hi");
	Files.appendText(uri, " There");  // File contains the text "Hi There"
	 
Use `readChunk` and `writeChunk` to read and write *`Chunk`* data:

	// Validate PNG file by looking for "PNG" chars in header bytes 1-3
	var uri = mergeURI(application.url, "assets/balls.png");
	var chunk = Files.readChunk(uri);
	if (chunk[1] != 0x50 || chunk[2] != 0x4E || chunk[3] != 0x47)
		throw("Invalid PNG file!");

	// Write the "photo.jpg" asset file to the host pictures directory
	var uri = mergeURI(application.url, "assets/photo.jpg");
	var chunk = Files.readChunk(uri);
	var uri = mergeURI(Files.picturesDirectory, "photo.jpg");
	Files.writeChunk(uri, chunk);
	
Use `appendChunk` to append *`Chunk`* data to an existing file:

	// Write chunk1 followed by chunk2 into the data/results file in the host documents directory
	var uri = mergeURI(Files.temporaryDirectory, "test_results_1");
	var chunk1 = Files.readChunk(uri);
	var uri = mergeURI(Files.temporaryDirectory, "test_results_2");
	var chunk2 = Files.readChunk(uri);
	var uri = mergeURI(Files.documentsDirectory, "data/results");
	Files.writeChunk(uri, chunk1);
	Files.appendChunk(uri, chunk2);
	chunk1.free();
	chunk2.free();

Use `readJSON` and `writeJSON` to read and write *`JavaScript`* objects:

	var preferences = { name:"Brian", city:"Del Mar", food:"tacos" };
	var filename = "user.json";
	var uri = mergeURI(Files.preferencesDirectory, application.di + "." + filename);
	Files.writeJSON(uri, preferences);
	var user = Files.readJSON(uri);
	trace(user.name + " likes to eat " + user.food + "!");
	
Use `readXML` and `writeXML` to read and write *`XML`* *`DOM`* data:

	var uri = mergeURI(application.url, "./assets/person.xml");
	var document = Files.readXML(uri);
	var items = document.getElementsByTagName("person");
	if (items.length > 0) {
		var node = items.item(0);
		var name = node.getAttribute("name");
	if (!name || (name != "Brian"))
		throw("Unexpected person!");
	}

	var uri = mergeURI(Files.documentsDirectory, "empty.html");
	var document = DOM.implementation.createDocument("", "html");
	var html = document.documentElement;
	html.setAttribute("lang", "en");
	var head = document.createElement("head");
	html.appendChild(head);
	var meta = document.createElement("meta");
	head.appendChild(meta);
	var title = document.createElement("title");
	head.appendChild(title);
	var body = document.createElement("body");
	html.appendChild(body);
	Files.writeXML(uri, document);
	
***
##Directory and File Utilities
KinomaJS Files provides a number of common file and directory utility functions.

Use `deleteFile` and `deleteDirectory` to delete files and directories. The `deleteDirectory` function requires that the directory is empty. To recursively delete a directory tree and it's contained files, set the optional `deleteDirectory` second parameter:

	// Delete the "download.txt" file from the host temporary directory
	var uri = mergeURI(Files.temporaryDirectory, "download.txt");
	Files.deleteFile(uri);

	// Delete the empty "kinoma" directory from the host preferences directory
	var uri = mergeURI(Files.preferencesDirectory, "kinoma/");
	Files.deleteDirectory(uri);

	// Delete the "kinoma" directory tree from the host documents directory
	var uri = mergeURI(Files.documentsDirectory, "kinoma/");
	Files.deleteDirectory(uri, true);
	
Use `ensureDirectory` to ensure that a directory exists. The directory (tree) is created if it doesn't already exist:

	// Ensure there's a kinoma/apps directory in the host preferences directory
	var uri = mergeURI(Files.preferencesDirectory, "kinoma/apps/");
	Files.ensureDirectory(uri);
	
The `exists` function tests whether or not a file or directory exists:

	if (Files.exists("file:///log.txt"))
		trace("The log.txt file is in the root directory");

	// Clear out the "destination" directory
	if (Files.exists(destination)) {
		Files.deleteDirectory(destination, true);
		Files.ensureDirectory(destination);
	}

Use `renameFile` and `renameDirectory` to rename files and directories. Note these functions are never used to move a file or directory to another volume or directory:

	// Rename the "log.txt" file in the kinoma/data directory to "data.txt"
	var uri = mergeURI(Files.documentsDirectory, "kinoma/data/log.txt");
	Files.renameFile(uri, "data.txt");

	// Rename the "data" directory in the "kinoma" directory to "results"
	var uri = mergeURI(Files.documentsDirectory, "kinoma/data/");
	Files.renameDirectory(uri, "results");
	
The `getInfo` function provides information about the file or directory. The info object returned contains the following properties:

*	type – Set to `Files.directoryType` for directories and `Files.fileType` for files.
* 	created – File creation date in milliseconds from the Epoch.
* 	date – File modification date in milliseconds from the Epoch (when available).
* 	size – File size in bytes.

<!-- -->

	var uri = mergeURI(Files.picturesDirectory, "photo.jpg");
	var info = Files.getInfo(uri);
	if (info.type != Files.fileType)
		throw("photo.jpg is not a file!");
	trace("photo.jpg file size is " + info.size + " bytes and was created on " + new Date(info.created));
	
***
##Directory Iterator
KinomaJS Files provides functions for iterating directories. Create the directory iterator using the `Files.Iterator` constructor and then call `iterator.getNext` repeatedly to iterate the contents of the directory. The info object returned contains a path property corresponding to the name of the item.

	// Recursively iterate through the kinoma/apps directory 	    
	var iterateDirectory = function(path) {
		var info, iterator = new Files.Iterator(path);
		while (info = iterator.getNext()) {
			trace(path + info.path + "\n");
			if (Files.directoryType == info.type)
				iterateDirectory(path + info.path + "/");
			}
	}

	var path = mergeURI(Files.preferencesDirectory, "kinoma/apps/");
	iterateDirectory(path);