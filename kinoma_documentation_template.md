#Kinoma Documentation Template


##1	About This Document
This document provides a starting point for creating drafts of Kinoma technical documentation in the proper Markdown format.


The formatting conventions are first described in general and then presented in terms of how to apply them in different contexts, such as in the Object Reference section of the *KinomaJS JavaScript API Reference* document.


**Note:** Writers of documents in the KinomaJS documentation suite should be sure to also read the *KinomaJS Documentation Writer’s Guide*, which focuses on the process and content rather than the format.


If starting from scratch, you should work from a copy of this document, retaining the original for later reference. In the copy, delete whatever parts of it don’t apply in your case (including explanatory text like this, once you no longer need it) and replace stub or filler text with the appropriate text for the documentation you’re writing.


This document assumes you already know how to use Markdown.


##2	Section Header (##)
Since # (level-1 header) is used for the title of the document, the headers within the document start at level 2.


###2.1	Subsection Header (###)
Content of subsection follows here.


####2.1.1	Sub-subsection Heading (####)

<!-- Last sentence in this paragraph needs reexamining/updating for Markdown. -->
Unless absolutely necessary, subsections should not go lower than this level. The style for the next level down, level 5, is intended for other purposes, such as for headings on examples and glossary entries (see later).



###2.2	Body Text
Paragraphs within sections have no special formatting. To create bulleted and numbered lists, use Markdown’s list syntax, including * for an unordered list and 1., 2., etc., for a numbered list (where order matters).


Format characters within body text as follows where necessary:


> •	For emphasis and most other uses of italic, use underscores (as in the next sentence). For example: A *host object* is a special kind of object with data that can be directly accessed only in C.


> **Note:** Note that bulleted items should ideally be only one paragraph long, but in the few cases where an item needs to run longer, just manually format the additional paragraphs as illustrated here.


> •	Use double asterisks for bold, including in Notes (as above) and when referring to a user interface element, as in “Press **Menu**.”



###2.2.1	Cross-References and Links
For cross-references to section headers and titles or figures or tables, use the format shown in this example:

> See the section “Applying the Formats.”

When changing the wording of any header, be sure to check for and change any cross-references to it.

Links to Kinoma documents do not need to be explicitly shown; for example, [KinomaJS Overview](http://kinoma.com/develop/documentation/overview/), [KinomaJS JavaScript API Reference](http://kinoma.com/develop/documentation/javascript/), and [KinomaJS XML API Reference](http://kinoma.com/develop/documentation/xml/).


For external links, show the link separately, as in this example:


> A module in KinomaJS is a JavaScript module as specified by CommonJS (wiki.commonjs.org/wiki/Modules/1.1).

###2.3	Code
For the “computer voice” font within body text, use back-ticks (\`). For example, refer to an element named foo as “the `foo` element.”


For blocks of code, use three back ticks (```). Do not use tab characters in code; instead, enter three spaces per “tab.” An example follows.


	src = new File(argv[2], "r");
	dst = new File(argv[3], "w");
	while (buf = src.getLine())
      dst.putLine(buf.toLowercase());

   
Show missing code (omitted for brevity) with “...”; this notation is acceptable on a line by itself (appropriately indented) or within a line, as in the following example.

	<header> ... </header>

<!-- This next bit needs reexamining/updating for Markdown. -->
Where code appears with a heading above it (as shown below, for instance), use Code Line for all code lines, even the first line.

**Example (Heading 5)**  

`xsResult = xsParse(aFile, fgetc, aPath, aLine /* Code Line */`   
`sSourceFlag | xsNoErrorFlag | xsNoWarningFlag); /* Code Line */`   

         
Reference sections typically describe many different code entities, using special context-specific formats that are too numerous to describe here; for more information, see the section “Applying the Formats.”


###2.4	Figures
Format figures and their captions as shown below. Create (or crop) all figures so that there’s no blank space on any of the four sides.


**Figure 1.** Figure Caption Goes Here  
![](http://kinoma.com/create/img/open-sensor-illus.png)
   
When adding a figure, remember to also update the numbering of any figures past that point in the document.


###2.5	Tables
To create a table, use the following as a starting point. In (relatively rare) cases where a caption is not appropriate, precede the table with a blank paragraph in the Normal style.


**Table 1.** Table Caption Goes Here

| **Column Head** | **Column Head** |
| ------------ | ------------- |
| Table entry	 | Table entry	  |
| Table entry	 | Table entry	  |
| Table entry	 | Table entry	  |


When adding a table, remember to also update the numbering of any figures past that point in the document.


##3	Applying the Formats
This section shows how the aforementioned styles (plus some others) are applied in specific contexts, in subsections with headings like this:


###Document Title: Context
These examples from existing documents may also help with figuring out how to apply the styles in other, similar contexts.


###3.1	*KinomaJS JavaScript API Reference:* Object Reference
The *KinomaJS JavaScript API Reference* document describes JavaScript objects in subsections like those shown here (although with different-level heads).


**Important:** Before creating subsections like these, be sure to read the current introduction to the object reference section of *KinomaJS* JavaScipt API *Reference*; the content of your descriptions should be consistent with what it says.
Here are some general rules for what not to do when writing descriptions in this Reference document:


•	Do not refer to the *KinomaJS Overview* document; all the basic information (without much elaboration) should be in the reference subsection itself. 


•	Do not state any default that’s being assumed per the introduction to the Reference section (see in particular **Default values** and **Pixels** in the introduction). 


•	Do not include graphics or code examples. 


###3.1.1	Object-Name Object
[Optional] Full sentences *briefly* describing the object and/or providing other details about the object. If what the object represents is not obvious, describe it here. Any general details that are useful for reference can be stated here (though the main introduction and discussion should be in the *KinomaJS Overview* document). Try to keep it no longer than one short paragraph.


Next, include only whichever of the following lower-level sections apply. The style of the first paragraph after every heading below is called Reference; names of other styles not previously introduced are explicitly noted. Square brackets indicate optional parts within subsections, to include only if applicable.


**Coordinates**  
Full sentences providing coordinates information.


**Object Description**  
Object inherits from Other-object-name.protoype.


[Object is sealed.]


The **Object Description** section is currently used only for the application, DOM, and shell objects. The rest of the section is the same as for **Prototype Description**, later, but without the `.prototype.`

**Constructor Description**  
Object-name(param1, param2)  

||||
| --- | --- | --- |
| `param1`| `type`| [required] |

> Parameter details, as:
> Sentence fragment[. Optional additional sentences.]
> List parameter names in order of their appearance.
Returns  

||||
| --- | --- | --- |
| `param2`| `type`| [required] |

> Parameter details

||||
| --- | --- | --- |
| Returns| `type`| &nbsp; |

> Return value details  

> Optionally describe what the function does, using this format whenever feasible:  
> [Sentence fragment[. Optional additional sentences.]  
> Omit this description if the function is self-explanatory—in particular, if all that needs to be said is what the function returns (above).  
> The style used for lines showing parameters (and any return value) is called Parameter; its default font is the computer voice font, so the Body Text Char is used for the comment (or for “Returns”). The style used for parameter details and return value details is called Details.  
> Some Constructor Description subsections also describe value properties; see Prototype Description, next, for the format of value property descriptions.

**Prototype Description**  
Prototype inherits from `Other-object-name.protoype.`

[Instances are sealed [and volatile].]

After the text shown above, describe all the value properties and function properties (combined, in alphabetical order). Multiple properties that have the same explanation may be grouped together, with only that one explanation following all of them.

Describe value properties in this format:

||||
| --- | --- | --- |
| `Object-name.prototype.property-name` | `type`| [read only] |
> Follow with property details, as:  
> Sentence fragment[. Optional additional sentences.]  
> Exception: Whenever possible, describe a boolean with a complete sentence beginning with “If `true`,”  
> The style used for the value property details is called Function Description (originally its sole use, but now used here for consistent alignment with descriptions of function properties).

Describe function properties in this format:

`Object-name.prototype.function-name(param1, param2)`

||||
| --- | --- | --- |
| `param1` | `type`| [required] |
> Parameter details (similar to value property details, above)  
> Show optional extra parameters as “...” with type * and description “Zero or more extra parameters”.  
> List parameter names in order of their appearance in the function.

||||
| --- | --- | --- |
| `param2` | `type`| [required] |

> Parameter details

||||
| --- | --- | --- |
| Returns | `type`|  |

> Return value details

> Optionally describe what the function does, using this format whenever feasible:
> [Sentence fragment[. Optional additional sentences.]
> Omit the description if the function is self-explanatory—in particular, if all that needs to be said is what the function returns (above).

**Events**  
[Same events as for other-object-name instances (cross-reference)[, plus:]]  
The cross-reference in parentheses above has this format:

> see “Events” in the section “Other-object-name Object”

where the two headings and the heading number are Word cross-references.
`name(param1, param2)`

||||
| --- | --- | --- |
| `param1` | `type`|  |

> Parameter details, as:  
> Sentence fragment[. Optional additional sentences.]  
> List parameter names in order of their appearance in the function.  

||||
| --- | --- | --- |
| `param2` | `type`|  |

> Parameter details  
Event description, as:  
This event happens when ….

###3.2	KinomaJS XML API Reference: XML Element Reference
The KinomaJS XML API Reference document provides details on the elements that make up the XML API, in subsections like those shown here (analogous to Object Reference sections shown in the preceding section).
3.2.1	Element-Name Element
[Optional] Full sentences briefly describing the element and/or providing other details about the element. If what the element represents is not obvious, describe it here. Any general details that are useful for reference can be stated here (though the main introduction and discussion should be in the KinomaJS Overview document). Try to keep it no longer than one short paragraph.
Next, include only whichever of the following lower-level sections apply. Square brackets indicate optional parts within subsections, to include only if applicable.
Tag
name
Attributes
name	type	[comment]
where type can be any of the allowed attribute types, followed by “(n or more, separated by commas)” if applicable (where n is the appropriate starting number), and the optional comment can be one of the following:
required
required *
0 or more
0 or 1
End with an explanation in this format: 
Sentence fragment[. Optional additional sentences.]
Exception: Whenever possible, describe a boolean with a complete sentence of the form “If true, …”
If “required *” (which indicates that a related note follows), add a line beginning with “* ” that provides the note.
List attribute names in alphabetical order.
Elements
name	comment
where the comment indicates how many of this element. For example:
0 or more
0 or 1
1
Follow with an explanation (unless none is needed) in this format where feasible: 
Sentence fragment[. Optional additional sentences.]
List element names in alphabetical order (one per line).
Several elements that have the same explanation may be grouped together, with only that one explanation following all of them.
CDATA
The words “JavaScript code” [optionally followed by further description]
3.3	XS: XS Elements
In the XS document, XS elements are typically first discussed in a tutorial manner, with usage guidance and examples, and then described in a manner appropriate for later reference. The formatting used for each of these presentation types is discussed here.
To the extent reasonable, the section heading for a tutorial discussion should be explanatory (such as “Defining Build Targets”), whereas the heading for a reference section should be the element name (as in “xs:target”).
3.3.1	Discussing XS Elements
In sections discussing XS property elements and the like (including patterns), examples are organized under the following Heading 5 heads:
•	Grammar — The grammar itself.
•	Prototype — The ECMAScript instructions corresponding to what the grammar prototypes. This and the grammar take the form of code in XML and ECMAScript. For the sake of terseness, the XML declarations and the package elements are omitted.
•	XML document — An XML document that conforms to the grammar (present only if applicable).
•	Instance — The ECMAScript expression corresponding to what the grammar instantiates from the XML document when the document is parsed (present only if applicable). This and the XML document are provided only to explain what the grammar prototypes and instantiates. They are not examples of code; sometimes they are just comments because ECMAScript has no corresponding instructions or expressions.
An example follows.
Grammar
<object name="colorIO">
   <function name="parse" params="text" c="colorIOParse"/>
   <function name="serialize" params="color" c="colorIOSerialize"/>
</object>
<custom name="color" value="#000000" pattern="/color" io="colorIO"/>
Prototype
color = { r: 0, g: 0, b: 0 }
XML document
<color>#FF8000</color>
Instance
{ r: 255, g: 128, b: 0 }
3.3.2	XS Element Reference
In sections in the XS document that describe XS elements and the like for reference purposes, a brief description of the element is followed by information under the following Heading 5 heads:
•	Attributes — All the attributes of the element along with an indication of whether they are required, optional, or inherited from the prototype of the instance that owns the property. If optional and a default value applies, or if inherited but there is no prototype, the default value is shown.
•	Elements — Which elements, if any, this element can contain.
•	Text — What text, if any, the element can (or must) contain.
The information below these heads is formatted as illustrated below.
Attributes
attribute1, required
Explanation here. (The line above has the Code Entry paragraph style; the Body Text Char style is used to change characters in such lines to the body text font where needed.)
attribute2, optional, default=""
Explanation here.
attribute2, inherited, default=""
Explanation here.
Elements
None (or enter a description if applicable)
Text
None (or enter a description if applicable)
3.4	XS Chunks: ECMAScript Objects
In the XS Chunks document, the documentation for ECMAScript objects created in XS typically includes descriptions of the constructor, prototype, and XS element corresponding to the object. The XS element is documented as described in section 3.4, “XS: XS Elements”; the constructor and prototype are documented as shown here. 
Note: A more advisable format has since evolved for describing constructors and prototypes; see section 3.1, “KinomaJS Reference: KinomaJS Object Reference.”
3.4.1	Constructor Description
After a brief introduction to the constructor, all forms of the constructor are listed and described as illustrated below.
Object()
Explanation.
Object(param)
Explanation of the function and its parameter(s).
Example
/* An example (or examples) if applicable. */
3.4.2	Prototype Description
After a brief introduction to the prototype, all properties of the prototype are listed and described as illustrated below.
Object.prototype.value-property
Explanation.
Object.prototype.function-property(param)
Explanation of the function and its parameter(s).
Example
/* An example (or examples) if applicable. */
3.5	XS in C: C API
This section describes the formatting used to present C macros in the document XS in C.
After a brief introduction to the macro, parameters of the macro (if any) are listed and described as shown below.
xsType xsMacro(xsType param1, xsType param2)  /* Code Entry style */
      /* Use the Code Line style for any additional lines (like these) */
      /* in the the macro syntax. */
param1	Brief parameter description (Parameter Def style)
param2	Brief parameter description (potentially, though rarely,  more than one line long)
Returns	Brief return value description
In cases where the first column in the Parameter Def style is not wide enough, the indents or tabs are adjusted accordingly in those lists.
Additional discussion of the macro sometimes follows (in the Normal style), and an example formatted as shown below may also be provided.
Example
/* Include example if applicable. */
If this structure needs to be extended for other C API documentation, consider adding Heading 5 heads (such as a “See also” head) to introduce additional types of information.
4	Glossary
In any document where reasonable, end with a Glossary section in which each item has the following format. (No introductory text is needed, so delete this paragraph in the actual document.)
glossary item (Heading 5)
Definition. (Explanation style)
 
Copyright © 2015 Marvell. All rights reserved.
Marvell and Kinoma are registered trademarks of Marvell. All other products and company names mentioned in this document may be trademarks of their respective owners.
