#Kinoma Documentation Template

##About This Document
This document provides a starting point for creating drafts of Kinoma technical documentation in the proper Markdown format.
The formatting conventions are first described in general and then presented in terms of how to apply them in different contexts. 

Looking at this template in GitHub's edit view will show how the formatting in it was specified (along with related comments); looking at it in GitHub's preview will show a close approximation of the final web page, but the latter will look different in some places due to CSS styling.

If starting from scratch, please work from a copy of this template, deleting whatever parts of it don’t apply in your case (including explanatory text like this, once you no longer need it) and replacing stub or filler text with the appropriate text for the documentation you’re writing.

###Related Reading
<!--From CR: The doc titles in the next paragraph should be links—but what are the (ultimately) appropriate URLs?-->
<!--From CK: Added link code and will post other pages-->

For more details about Markdown, see its documentation on the web. 

> **Note:** There are alternatives to some of the formatting comventions described in this template, but the conventions described here are preferred.

See also the editorial style guidelines in the [_Kinoma Documentation Style Sheet._](http://http://www.kinoma.com/develop/documentation/doc-stylesheet/index-md.php) Writers of documents in the KinomaJS documentation suite should also read the [_KinomaJS Documentation Writer’s Guide,_](http://http://www.kinoma.com/develop/documentation/doc-writers-guide/index-md.php) which focuses on the process and content rather than the format or editorial style.

##Section Heading (##)
Since # (level-1 heading) is used for the title of the document, the headings within the document start at level 2.

###Subsection Heading (###)
Content of subsection follows here.

####Sub-subsection Heading (####)

Unless absolutely necessary, subsections should not go lower than this level. 

#####Special Low-Level Heading (#####)
Use a level-5 heading for occasional headings on code examples, terms in the glossary, and the like. (Examples follow later.)

###Body Text
Paragraphs within sections have no special formatting. The sections below describe formatting for certain characters.

Although entering characters outside the basic ASCII character set—such as the em dash, used here—will work fine, you can stick with the basic set; it may not look right in Github's preview, but CSS styling will make it look right on the web page. For example, you can type the default "straight" quotation marks and, except in code, they will be converted to the "curly" kind (“ ” and ‘ ’) on the web page. Likewise, two hyphens in a row will appear as an em dash on the web page.

####Italic (\_)
For emphasis and other uses of italic (including document titles), use underscores, as done _here._

####Bold (**)
Use double asterisks for bold—for example, when referring to a user interface element as in "Press **Menu**."

####Escape Character (\\)
To prevent Markdown from processing a character as a command, precede the character with backslash (as done in the heading above). Do not use it in code, however, because everything entered as code is taken literally (not interpreted as a command).

###Notes and Other Indentation (>)
Use Notes formatted like the one below—including blockquote (>) for indentation—for incidental, noncritical information.

> **Note:** Adding a space after the angle bracket makes the text stand out better as a blockquote in GitHub's edit view (making the quoted text appear in color). This is an example of how incidental, noncritical Notes should be formatted.

<!--From CR re blockquote: Blockquoted text has more blank space above and below in the web output than normal paragraphs do; can that be changed to look like the normal spacing?-->
<!--From CK Fixed, see the page on the website http://staging2.kinoma.com/develop/documentation/doc-template/index-md.php#applying-the-formats-->

To emphasize important, critical information, use red as illustrated below (but note, the red will not show up in Markdown's preview). These should be rare.

> <span style="color: red">**Important:** To emphasize important, critical information, use this format.</span>  

Although uncommon (outside of reference sections), indentation can be useful for setting off things other than special notes. In such cases, just use blockquote by itself, as done here:  

> GitHub's preview will show blockquoted text in gray and with a bar in the margin, but CSS styling will remove that on the web page.

###Lists (- and N.)
Precede each item in a bulleted list with a hyphen and in an ordered (numbered) list with "N.", where N is the number for that item (though it can be any number at all, as Markdown always numbers sequentially from 1). Be sure to follow the hyphen or "N." with a space.

If all or most of the listed items are very short, don't separate them with blank lines, as illustrated here:

<!--From CR: The following list has no blank space between entries in preview but does have it on the web page; no big deal, but I mention it in case there's a workaround. Please advise. (If not, I'll change this discussion accordingly.) Also note that the web output of the following list doesn't have the smaller font that all other lists have, which would mean we'd have to warn against using this format.-->

<!--From CK: Yes, this was a bug where the markdown was adding paragraph tags to most of the list items and thus affecting their size. Fixed for documentation pages. All lists should use standard body copy size. I think the spaces above/below list items are good for readablity especially when the list items are multiline. We can discuss if you are dissatisfied. -->

- First short item  
- Second short item  

If all or most of the listed items are long, separate them with blank lines.
 
1. First long item. The purpose of this example is to show how a list should have blank lines between the entries whenever the listed items are long.
 
2. Second long item. The purpose of this example is to show how a list should have blank lines between the entries whenever the listed items are long.

Items in a list would ideally be only one paragraph long, but in some cases an item will need to run to two paragraphs of text, or may be followed by a figure (or something else) before the next item in the list; in those cases, precede the continuation of the item with three space, as illustrated here:

- List item

   Continuation of list item above (preceded by three spaces in Markdown)

The following example illustrates the preferred format for beginning list items with a term or phrase that is then elaborated on.

- **Bold term or phrase** — An elaboration on the term or phrase follows here. An em dash surrounded by spaces precedes the elboration.

- **Next such item** — And so on.

###Cross-References and Links
When referring to another Kinoma document (or any Kinoma web page), do not explicitly show the link; for example, [_KinomaJS Overview_](http://kinoma.com/develop/documentation/overview/), [_KinomaJS JavaScript API Reference_](http://kinoma.com/develop/documentation/javascript/), and [_KinomaJS XML API Reference_](http://kinoma.com/develop/documentation/xml/). When referring to anything else on the web, show the link explicitly, as in this example:

> A _module_ in KinomaJS is a JavaScript module as specified by CommonJS ([wiki.commonjs.org/wiki/Modules/1.1](http://wiki.commonjs.org/wiki/Modules/1.1)).

If the title of any Kinoma document changes, check for references to it in other documents and update them.

For cross-references to headings (items with one or more # in front), use an anchor link, as illustrated below. The words after the # in Markdown are the exact heading, except: with hyphens where any spaces were; excluding any punctuation in the heading; and not case-sensitive. It will link to the first heading with that name.

> See the section "[Applying the Formats](#applying-the-formats)." 

When changing the wording of any heading, check for references to it in the document and update them. (Kinoma documents should not cross-reference each other's headings, so you only need to check for this in the current document.)

###Code (`)
For the "computer voice" font within body text, use back-ticks (\`). For example, refer to an element named `foo` as "the `foo` element."

For code blocks—that is, one or more code lines that are separate from body text—place a line of three back-ticks (```) above and below the code block. For indentation within code, either use a tab character or enter three spaces per "tab." An example (using the tab character) follows. Note that color highlighting in code will show up only on the web page and not in GitHub's preview.

>**Note:** Even a single line of code should be formatted this way (not with just single back-ticks around it on the same line), otherwise the color highlighting will not appear on the web page.

```
src = new File(argv[2], "r");
dst = new File(argv[3], "w");
while (buf = src.getLine())
	dst.putLine(buf.toLowercase());
```  
Show missing code (omitted for brevity) with `...`; this notation is acceptable on a line by itself (appropriately indented) or within a line, as in the following example.

```
<header> ... </header>
```

Code will occasionally have a heading above it—for example, to distinguish its context from that of a similar example that follows immediately. Make such headings level 5, as illustrated below.

#####AppBehaviors.js  
```
//@module   
var CB = require("SharedBehaviors");
```

Reference sections typically describe many different code entities, using special context-specific formats; for more information, see the section "[Applying the Formats](#applying-the-formats)."

###Figures
Format figures with numbered captions as shown below. Numbers and captions need not be included where doing so would be overkill, such as in numbered steps where every figure obviously demonstrates what the step above it described. 

**Figure 1.** Figure Caption Goes Here  
![](http://kinoma.com/create/img/open-sensor-illus.png)
   
Create (or crop) all figures so that there’s no blank space on any of the four sides.

When adding, deleting, or moving a captioned figure, remember to also update the numbering of any other affected figures.

###Tables and Other Columnar Formats
To create a table, use the following as a starting point (or, for more complicated tables, use HTML).

<!--From CR: Do I understand correctly that tables will look different on the web page depending on whether they're ordinary tables or used for special formatting in reference sections? If so, please explain.-->

<!--From CK: All tables in set up like below will not show borders, backgrounds, etc. If an ordinary table is needed we need to use standard HTML table markup -->

**Table 1.** Table Caption Goes Here

| **Column Head** | **Column Head** |
| ------------ | ------------- |
| Table entry | Table entry |
| Table entry | Table entry |
| Table entry | Table entry |  

When adding, deleting, or moving a table, remember to also update the numbering of any other affected tables.

Especially in reference sections, it can be useful to arrange information in a simple columnar format, without a table caption or column heads; the next section includes many examples of this.

###Comments
Comments or queries from the writer or a reviewer of the document should be placed above the paragraph that each comment refers to and should indicate who the comment is from. An example (visible only in edit view) follows here.

<!--From CR: Format comments like this, with your name/initials after "From:".-->

> <span style="color: red">**Important:** Do not type a space after or before the double hyphens in the comment syntax, and always follow the comment with a blank line; it will look OK in preview but may seriously mess up the web page.</span>

##Applying the Formats
This section shows how the aforementioned styles (plus some others) are applied in specific contexts, in subsections with headings like this:

**_Document Title:_ Context**

These examples from existing documents may also help with figuring out how to apply the styles in other, similar contexts.

###_KinomaJS JavaScript API Reference:_ Object Reference
The [_KinomaJS JavaScript API Reference_](http://kinoma.com/develop/documentation/javascript/) document describes JavaScript objects in subsections like those shown here (although with different-level heads). Before creating subsections like these, be sure to read the current introduction to the object reference section of the [_KinomaJS JavaScript API Reference_](http://kinoma.com/develop/documentation/javascript/) document; the content of your descriptions should be consistent with what it says.

Here are some general rules for what not to do when writing descriptions in this Reference document:

- Do not refer to the [_KinomaJS Overview_](http://kinoma.com/develop/documentation/overview/) document; all the basic information (without much elaboration) should be in the reference subsection itself. 

- Do not state any default that's being assumed per the introduction to the Reference section (see in particular **Default values** and **Pixels** in the introduction). 

- Do not include graphics or code examples. 

####Object-Name Object
[Optional] Full sentences _briefly_ describing the object and/or providing other details about the object. If what the object represents is not obvious, describe it here. Any general details that are useful for reference can be stated here (though the main introduction and discussion should be in the [_KinomaJS Overview_](http://kinoma.com/develop/documentation/overview/) document). Try to keep it no longer than one short paragraph.

Next, include only whichever of the following lower-level sections apply. Square brackets indicate optional parts within subsections, to include only if applicable.

#####Coordinates  
Full sentences providing coordinates information.

#####Object Description  
Object inherits from `Other-object-name.prototype`.

[Object is sealed.]

The "Object Description" section is currently used only for the application, DOM, and shell objects. The rest of the section is the same as for "[Prototype Description](#Prototype-Description)," later, but without the `.prototype`.

#####Constructor Description  
`Object-name(param1, param2)`  

||||
| --- | --- | --- |
| `param1`| `type`| [required] |

> Indented (blockquoted) parameter details, as:

> Sentence fragment[. Optional additional sentences.]

> List parameter names in order of their appearance.

||||
| --- | --- | --- |
| `param2`| `type`| [required] |

> Parameter details

<!--From CR: The web output for the following table illustrates the problem that when there is no entry in the 3rd column, the 2nd column isn't lined up correctly. Note this wil also happen when the optional 3rd entry is omitted in the param descriptions above. You alluded to another possible solution for this; can we take a look at that? Note that in general in such 3-column reference formats, the 1st column's entry could be as long as `Style.prototype.horizontalAlignment`.-->

<!--From CK: Fixed as dicussed via CSS see: http://staging2.kinoma.com/develop/documentation/doc-template/index-md.php#applying-the-formats-->

||||
| --- | --- | --- |
| Returns| `type`|  |

> Return value details  

Optionally describe (without indentation) what the function does, using this format whenever feasible:  

[Sentence fragment[. Optional additional sentences.]  

Omit this description if the function is self-explanatory—in particular, if all that needs to be said is what the function returns (above).  

Some "Constructor Description" sections also describe value properties; see "Prototype Description," next, for the format of value property descriptions.

#####Prototype Description  
Prototype inherits from `Other-object-name.prototype`.

[Instances are sealed [and volatile].]

After the text shown above, describe all the value properties and function properties (combined, in alphabetical order). Multiple properties that have the same explanation may be grouped together, with only that one explanation following all of them.

Describe value properties in this format:

<!--From CR: Note, adding a heading below for every value property is a big change, which (as I mentioned in an earlier email) is a way to have all properties (whether value properties or function properties) align the same on the left. What do you think?-->

<!--From CK: If this works for you I am fine. Assume you will just be copy pasting properly formatted snippets and updating the content -->

`Object-name.prototype.property-name`

||||
| --- | --- | --- |
| `Object-name.prototype.property-name` | `type`| [read only] |

> Follow with property details, as:  

> Sentence fragment[. Optional additional sentences.] 

> Exception: Whenever possible, describe a boolean with a complete sentence beginning with "If `true`,"  

Describe function properties in this format:

`Object-name.prototype.function-name(param1, param2)`

||||
| --- | --- | --- |
| `param1` | `type`| [required] |

> Parameter details (similar to value property details, above)

> Show optional extra parameters as `...` with type * and description "Zero or more extra parameters".  

> List parameter names in order of their appearance in the function.

||||
| --- | --- | --- |
| `param2` | `type`| [required] |

> Parameter details

||||
| --- | --- | --- |
| Returns | `type`|  |

> Return value details

Optionally describe (without indentation) what the function does, using this format whenever feasible:

[Sentence fragment[. Optional additional sentences.]

Omit the description if the function is self-explanatory—in particular, if all that needs to be said is what the function returns (above).

#####Events  
[Same events as for `other-object-name` instances (cross-reference)[, plus:]]

> The cross-reference in parentheses above has this format:

> see "Events" in the section "[Other-object-name Object](#other-object-name-object)."

> where the section heading in the cross-reference is a link.

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

Event description (without indentation), beginning with "This event happens when"

###_KinomaJS XML API Reference:_ Element Reference
The [_KinomaJS XML API Reference_](http://kinoma.com/develop/documentation/xml/) document provides details on the elements that make up the XML API, in subsections like those shown here (analogous to object reference sections shown above in this template).

####Element-Name Element
[Optional] Full sentences _briefly_ describing the element and/or providing other details about the element. If what the element represents is not obvious, describe it here. Any general details that are useful for reference can be stated here (though the main introduction and discussion should be in the [_KinomaJS Overview_](http://kinoma.com/develop/documentation/overview/) document). Try to keep it no longer than one short paragraph.

Next, include only whichever of the following lower-level sections apply. Square brackets indicate optional parts within subsections, to include only if applicable. (Not shown here is the special "Same as..." format used in some "Attributes" and "Elements" section in the document.)

> **Note:** The value in the "Tag" section below is formatted as a single-column table so that it will line up with the slight indentation of the (tabular) contents of the sections that follow it.

#####Tag 
||
| --- |
| `name` |

#####Attributes  

||||
| --- | --- | --- |
| `name` | `type`| [comment] |

> where `type` can be any of the allowed attribute types, followed by "(N or more, separated by commas)" if applicable (where N is the appropriate starting number), and the optional comment can be one of the following:  

> required  
> required \*  
> 0 or more  
> 0 or 1  

> End with an (indented) explanation in this format:   

> Sentence fragment[. Optional additional sentences.] 

> Exception: Whenever possible, describe a boolean with a complete sentence of the form "If `true`, ..."  

> If "required \*" (which indicates that a related note follows), add a paragraph beginning with * (followed by a space) that provides the note.
> List attribute names in alphabetical order.

#####Elements

||||
| --- | --- | --- |
| `name` | comment |  |
> where the comment indicates how many of this element. For example: 

> 0 or more  
> 0 or 1  
> 1  

> Follow with an explanation (unless none is needed) in this format where feasible:  

> Sentence fragment[. Optional additional sentences.]

> List element names in alphabetical order (one per line).

> Several elements that have the same explanation may be grouped together, with only that one explanation following all of them.

#####CDATA
The words “JavaScript code” [optionally followed by further description]

###_XS_: XS Elements
In the _XS_ document, XS elements are typically first discussed in a tutorial manner, with usage guidance and examples, and then described in a manner appropriate for later reference. The formatting used for each of these presentation types is discussed here.

>**Note:** None of the XS documents have been publicly posted yet.

To the extent reasonable, the section heading for a tutorial discussion should be explanatory (such as "Defining Build Targets"), whereas the heading for a reference section should be the element name (as in "xs:target").

####Discussing XS Elements
In sections discussing XS property elements and the like (including patterns), examples are organized under the following level-5 headings:

- **Grammar** — The grammar itself.

- **Prototype** — The ECMAScript instructions corresponding to what the grammar prototypes. This and the grammar take the form of code in XML and ECMAScript. For the sake of terseness, the XML declarations and the `package` elements are omitted.

- **XML document** — An XML document that conforms to the grammar (present only if applicable).

- **Instance** — The ECMAScript expression corresponding to what the grammar instantiates from the XML document when the document is parsed (present only if applicable). This and the XML document are provided only to explain what the grammar prototypes and instantiates. They are not examples of code; sometimes they are just comments because ECMAScript has no corresponding instructions or expressions.

An example follows.

#####Grammar
```
<object name="colorIO">
   <function name="parse" params="text" c="colorIOParse"/>
   <function name="serialize" params="color" c="colorIOSerialize"/>
</object>
<custom name="color" value="#000000" pattern="/color" io="colorIO"/>
```

#####Prototype
```
color = { r: 0, g: 0, b: 0 }
```

#####XML document
<!--From CR:  everything following "<color>" in the web output for the following is in italic and gray; Why?-->
<!--From CK:  this is a tricky one the code highlighter thinks it is a comment since there is a # in the code line. I will have to come up with a fix but will take a bit. -->
```
<color>#FF8000</color>
```

#####Instance
```
{ r: 255, g: 128, b: 0 }
```

####XS Element Reference
In sections in the _XS_ document that describe XS elements and the like for reference purposes, a brief description of the element is followed by information under the following level-5 headings:

- **Attributes** — All the attributes of the element along with an indication of whether they are required, optional, or inherited from the prototype of the instance that owns the property. If optional and a default value applies, or if inherited but there is no prototype, the default value is shown.

- **Elements** — Which elements, if any, this element can contain.

- **Text** — What text, if any, the element can (or must) contain.

The information below these headings is formatted as illustrated below.

#####Attributes

||||
| --- | --- | --- |
| `attribute1` | required | |
  
> Explanation here.

||||
| --- | --- | --- |
| `attribute2` | optional | default=`"[whatever]"` |

> Explanation here.

||||
| --- | --- | --- |
| `attribute3` | inherited | default=`"[whatever]"` |

> Explanation here.

#####Elements

None (or enter a description if applicable)

#####Text

None (or enter a description if applicable)

###_XS Chunks:_ ECMAScript Objects
In the _XS Chunks_ document, the documentation for ECMAScript objects created in XS typically includes descriptions of the constructor, prototype, and XS element corresponding to the object. The XS element is documented as described in the section "[_XS:_ XS Elements](#XS-XS-Elements)"; the constructor and prototype are documented as shown here. 

>**Note:** A more advisable format has since evolved for describing constructors and prototypes; see the section "[_KinomaJS JavaScript API Reference:_ Object Reference](KinomaJS-JavaScript-API-Reference-Object-Reference)."

####Constructor Description
After a brief introduction to the constructor, all forms of the constructor are listed and described as illustrated below.

`Object()`  
> Explanation.

`Object(param)`  
> Explanation of the function and its parameter(s).

#####Example  
```
An example (or examples) if applicable.
```

####Prototype Description
After a brief introduction to the prototype, all properties of the prototype are listed and described as illustrated below.

`Object.prototype.value-property`  
> Explanation.

`Object.prototype.function-property(param)`  
> Explanation of the function and its parameter(s).

#####Example  
```
An example (or examples) if applicable.
```

###_XS in C:_ C API
This section describes the formatting used to present C macros in the document _XS in C._

After a brief introduction to the macro, parameters of the macro (if any) are listed and described as shown below.

```
xsType xsMacro(xsType param1, xsType param2)
```

|||
| --- | --- |
| `param1` | Brief parameter description (potentially, though rarely, more than one line long) |
| `param2` | Brief parameter description|
| Returns | Brief return value description |


Additional discussion of the macro sometimes follows (in the normal body text style), and an example formatted as shown below may also be provided.

#####Example
```
Include example if applicable.
```

If this structure needs to be extended for other C API documentation, consider adding level-5 headings (such as a "See also" heading) to introduce additional types of information.

##Glossary
In any document where reasonable, end with a "Glossary" section in which each item has the following format. (No introductory text is needed, so delete this paragraph in the actual document.)

#####glossary item (#####)
> Definition. (>)

---
Copyright © 2015 Marvell. All rights reserved.
Marvell and Kinoma are registered trademarks of Marvell. All other products and company names mentioned in this document may be trademarks of their respective owners.
