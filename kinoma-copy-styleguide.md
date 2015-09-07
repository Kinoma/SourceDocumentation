#Style Sheet for Kinoma Documentation
---
##Introduction
This style sheet covers issues related to the editorial style used in Kinoma’s technical documentation and web content. It supplements the following guides:

* _Marvell Technical Communication Style Guide_ 
* _Marvell Acronyms, Abbreviations, and Terminology_ 

In some cases this style sheet repeats guidelines in the documents listed above, as a reminder or for emphasis. Where there are conflicts between this style sheet and the guidelines in those documents, the differences in this style sheet are intentional.

Some of these guidelines are specific to past documentation, some of which may be obsolete; they are left in just in case they should come up again. The guidelines are alphabetically ordered and not meant to be read sequentially; just look up the issue of concern to you.

For related conventions, see the _Kinoma Documentation Template._

##Style Guidelines
**5-way (adj)**  
Not *five-way*. Use as in *5-way control pad* (not as a noun by itself).

**and/or**  
Avoid; usually *or* alone suffices.

**audiobook**  
One word.

**baby cam**  
Use the more inclusive *kid cam* instead.

**bestseller**  
One word, no hyphen.

**boolean (adj)**  
Do not capitalize or use as a noun.

**boot (v)**  
Not *boot up.*

**cam**  
A separate word when preceded by *kid, pet, security, traffic,* and the like (but not a separate word in *webcam*); not hyphenated (for example, *pet cam*).

**cancel**  
Use *cancelled* and *cancelling* (double /).

>**Note:** This convention was changed (from single /) because of the spelling of the `onTouchCancelled` event in the KinomaJS API.

**Capitalization**  
When capitalizing titles and the like (called “title capitalization” in contrast with “sentence capitalization”), do not capitalize the following:

* Articles (*a, the*)

* Conjunctions (*and, or*)

* Short prepositions (less than five characters long, such as *with, to*) except in the rare case when it is the last word of the text being capitalized.
 
Do not capitalize physical button names (like "power button") unless the button is labeled as such (with that capitalization) on the device.

**Captions**  
Per Marvell’s preferred style, use title capitalization rather than sentence capitalization for figure and table captions (as in “Parts of the User Interface”). However, use special conventions, such as for code terms, the same as you would in body text (as in “The `fillImage` Operation”).

**checkbox**  
One word.

**Code Examples**  
Do not use tab characters in code examples; instead, enter three spaces per “tab.”

Use the same conventions throughout a document for where to add optional spaces and line breaks in code. The same conventions should also be used across a set of related documents.

Show missing code (omitted for brevity) with “...”; this notation is acceptable on a line by itself (appropriately indented) or within open and closing tags, as in the following example.

>`<layout> ... </layout>`

**Code Entities in Body Text**  
In any body text referring to entities that would appear exactly that way in code, use the same font as in code examples. For example, you would refer to the element illustrated under **Code Examples** above as “the `layout` element.” Note too that (at least starting with the KinomaJS documentation) instances and objects related to the constructor `Foo` in JavaScript should be referred to as “`foo` instance” and “`foo` object.”

Do not refer to a code term as a noun by itself, as in “the `port.`”

Do not start sentences with a code term that begins in lowercase. The workaround is not to capitalize the term but instead to rewrite the sentence to avoid the problem. For example:

>`column` elements juxtapose their contents vertically.&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_(Not OK)_   
>The `column` element juxtaposes its contents vertically.&nbsp;&nbsp;&nbsp;_(OK)_ 

However, if a heading must unavoidably begin with such a term, capitalize it (and use the normal heading font).

When referring to a string or boolean value in body text, do not include quotation marks.

>The default value of the `color` parameter is `gray`.  
>The default value of the `shared` parameter is `false`.

When referring in body text to the value of a numeric code entity, do not put the number in the code font.

>The default value of `length` parameter is 0.

See also **Captions** and **File Names/Extensions**.

**Comma**  
See **Numbers** and **Serial Commas**.

**content**  
Avoid referring to “a content”; use “content” alone (or plural) if meaning is still clear.

**Contractions**  
Do not use these (for example, *don’t*) in technical documentation.

**Cross-References**  
Refer to another section as in this example:

>See the section “Introduction.”

Wherever it might not be completely obvious, state what information will be found at the cross-referenced location.

**current playlist**  
Not _active playlist._

**Dash**  
Do not add a space on either side of an em dash (—) when used to set off something in a sentence. If necessary to prevent an awkward line break, feel free to force a break before or (ideally) after a dash.

Add a space on either side of an em dash that separates an item from its description in a bulleted list, as in the following example.

>* **Get Info** — Displays detailed information about the current item.

**data**  
Always use in the singular, as in “The data is …” (and similarly for *metadata*).

**dialog**  
Not *dialog box*.

**e.g.**  
Spell out (“for example”) unless space is at a premium (such as in a table).

**email**  
No hyphen.

**etc.**  
Spell out (“and so on”) unless space is at a premium (such as in a table).

**File Names/Extensions**  
Use a monospaced font (whatever is used for code examples) for file names and extensions. For extensions, assume the “dot” is pronounced, as in “a `.xs` extension” (not “an `.xs` extension”).

**Fsk**  
Capitalize only the first letter.

**function**  
Use this term to refer to any function (even if a command). Functions, not events, are overridden (events “are triggered” or “happen”).

**garbage-collect**  
Always hyphenated.

**grow**  
Avoid using as a transitive verb. For example, a framework grows; you do not grow a framework. 

**hard-code (v)**  
Hyphenated; not *hardcode*.

**ID**  
Use instead of “id” to refer to an identifier in text. Use all-lowercase only when referring to a code entity, such as a parameter named `id`.

**i.e.**  
Spell out (“that is”) unless space is at a premium (such as in a table).

**index**  
Always use *indexes*, not *indices*, as the plural.

<!-- From Caroline: Re: "internet" below
I’ve resisted changing to all-lowercase, but you seem not to be capitalizing it, hence this change. OK as is now? -->
**internet**  
Do not capitalize, except in “Internet of Things.”

**kid cam**  
Two words, no hyphen.

**Kinoma Create**  
Do not precede with *the* (but OK to precede with *your.*)

**Kinoma Forum**  
Capitalize *Forum*, and precede with *the*.

**Kinoma Guide**  
Not *Kinoma Media Guide*, and not preceded by *the*.

**Kinoma Play**  
Do not abbreviate to *KP*.

**Labels in Forms**  
For labels on fields in forms, use sentence capitalization, as in “Your email address:”.

**Li-ion**  
Capitalize only "Li," not "ion."

**Links**  
For links to web pages, explicitly show the corresponding URL (but without the leading <http://>, if any); for example, “See our support page, [kinoma.com/support/](http://www.kinoma.com/support/).” If the URL ends in “.com”, dispense with the trailing slash; otherwise, follow the usual convention of ending in a slash unless the URL leads to a file rather than a folder.

**log in, log out**  
Not *log on* or *log off*. Use *log in to*, not *log into*.

**media**  
Do not refer to “a media.” For example, rather than “A picture behaves like a media” in KinomaJS, you could say “Picture content behaves like media content.”

**menu pod**  
Do not capitalize.

**Menus**  
Users *choose from* menus. This is contrary to the Marvell (and Microsoft) guideline to use *select on*, but it is necessary because of the meaning *select* has in Kinoma Play Script terminology (where, strictly speaking, choosing a menu item not only selects but also opens it).

See also **User Interface Elements**.

**Micro USB**
Not hyphenated. Always capitalize "Micro."

**microSD**  
Always capitalize this way (never initial cap, so avoid using at beginning of sentence).

**MIME type**  
Note the capitalization.

**Mini USB**
Not hyphenated. Always capitalize "Mini."

**Modes**  
Capitalize the picture viewer modes Browse, Zoom & Rotate, and Pan.

**New Terms**  
When introducing a new term, italicize it and, in documents with a glossary, include the term in the glossary. Exception: For terms that are too general or inconsequential to include in the glossary, introduce them in quotation marks instead of italics.

**non- (prefix)**  
Generally, words beginning with this prefix are not hyphenated (look for the word in question, or a similar word, in the dictionary).

**Numbers**  
Except where inappropriate for technical reasons (for example, because of the format required in a code context), include commas in numbers four or more digits long, as in *4,000*.

**offscreen, onscreen**  
Not *off-screen* or *on-screen*.


**open source**  
Not hyphenated.

**Palm Powered**  
Unusual capitalization (and no hyphen) because it is a Palm trademark.

**pet cam**  
Two words, no hyphen.

**photostream**  
One word.

**play, playlist**  
These apply not only to audio and video but also to pictures.

**plug-in**  
Hyphenated.

**preceding**  
Preferred to *previous* when referring to the immediately preceding section.

**purchase key**  
Not *product key*.

**re- (prefix)**  
Generally, words beginning with this prefix are not hyphenated (look for the word in question, or a similar word, in the dictionary); however, hyphenate *re-download* for better readability.

**runtime (adj, n)**  
One word.

**(s)**  
Avoid using this notation to denote an optional plural; explicitly use or instead, as in “one or more modules” or “the module or modules.”

**Screens**  
Capitalize phone screen names as in *Main screen*, *Home screen*, and *Today screen*.

**scripting language**  
Not *script language*.

**section**  
Always use, even for the top level of a document; do not use *chapter*. Do not use *subsection*; for example, a subsection should refer to the next subsection as the next *section*. Use section only to refer to a section as a whole (including all it subsections) when it is clear from the context, such as in a brief introduction to a section.

**security cam**  
Two words.

**Serial Commas**  
Use them, as in “a, b, and c” (not “a, b and c”).

**should**  
Do not use _should_ when you mean _must._

**slideshow**  
One word.

**so**  
If you mean “consequently,” precede _so_ with a comma.

>He is going to the store, so he can pick up something for you if you like.

If you are instead stating a purpose or intention, use _so that._

>He is going to buy more milk so that we will not run out of it.

**soft key**  
Two words, no hyphen. Precede with *left* or *right* as needed.

**standalone**  
Not hyphenated.

**sync, syncing**  
No *h*.

**tech note**  
Not technical note.

**text**  
Do not refer to “a text” or “texts.” For example, say “A text is content that ….” (not “A text is a content that …”) and “Pictures and text are rendered …” (not “Pictures and texts are rendered…”).

**third party (n), third-party (adj)**  
Do not use *3rd*.

**this**  
Avoid using by itself; always try to follow with a noun or phrase, to prevent ambiguity.

**toolbar**  
One word.

**touch pad**  
Two words.

**touch screen**  
Two words.

**traffic cam**  
Two words.

**tune**  
Follow with *into*, not *in to*, as in “Tune into digital radio.”

**URLs**  
See **Links**.

**User Interface Elements**  
Use bold when referring to onscreen user interface elements such as names of menus, menu items, buttons, dialogs, and dialog options; do likewise when referring to functions attributed to soft keys. Follow this convention regardless of whether the context is Kinoma-specific.

Because of the various possible input methods (onscreen user interface, physical hardware keys, touch screen), refer wherever possible to the operation/command being performed rather than the physical action that triggers it; for example, say “Press **Menu**” rather than “Press the **Menu** soft key.”

See also **Menus**.

**Versions**  
Use *later* (not *newer, higher*, or *above*) and earlier (not *older, lower,* or *below*) when referring to versions—for example, “Make sure you have version 4.4.1 or later.”

**web**  
Do not capitalize.

**web page**  
Two words, not capitalized.

**webcam**  
One word, not capitalized.

**website**  
One word, not capitalized.

**Wi-Fi**  
Note the capitalization.

**wish**  
Avoid; *want* is preferred.

**XML document**  
Not *XML file*. 

**XS**
Not lowercase _xs_.

**xsc**  
Always `xsc`. 

**ZIP file**  
All caps *ZIP*.
