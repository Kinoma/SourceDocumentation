In the "[Contemplate This](../introducing-kinomajs-dictionary-based-constructors-and-templates/)" tech note, I introduced the use of dictionary-based constructors and templates to build KinomaJS `content`, `container`, `skin`nd `style` objects. Now I will apply similar techniques to KinomaJS `behavior` objects, and present a shortcut to bind KinomaJS `handler` and `behavior` objects. There is a reference section at the end to augment KinomaJS Reference

There is a reference section at the end to augment [KinomaJS Reference](../../javascript/).

***
##Overview
KinomaJS behaviors are ordinary objects with functions corresponding to events triggered by contents or handlers. Typically frameworks use inheritance to define increasingly specialized behaviors.

For example, a framework defines a button behavior that inherits from a control behavior that inherits from `Behavior.prototype`, which is provided by KinomaJS itself.

Applications use these constructors and prototypes to create behaviors or further specialize behaviors.

As ordinary objects, behaviors can of course be coded with standard JavaScript features like `Object.create` or `Object.defineProperties`. To simplify the code, behaviors can also be created with dictionaries and specialized with templates.

The following examples present the dictionary or template based notation followed by the equivalent JavaScript notation.

***
##Dictionary-Based Constructors
To create behaviors, the `Behavior` constructor can be used as a function that takes one argument, an object with properties. The constructor uses such properties to initialize the properties of the new behavior it creates.

	var behavior = Behavior({
		onCreate: function(content) {
			debugger
		}
	});

is equivalent to

	var behavior = Object.create(Behavior.prototype, {
		onCreate: { value: function(content) {
			debugger
		}}
	});
	
####Templates
To specialize behaviors, the `Behavior` constructor also provided a `template` function. The `template` function returns a constructor and takes one argument, an object with properties. The template function uses the properties of its argument to initialize the properties of the prototype of the constructor that the `template` function returns.

	exports.ButtonBehavior = Behavior.template({
		onTap: function(content) { },
		onTouchBegan: function(content) {
			content.state = 1;
		},
		onTouchEnded: function(content) {
			content.state = 0;
			this.onTap(content);
		}
	})
	
is equivalent to

	exports.ButtonBehavior = function(content, data, dictionary) {
		Behavior.call(this, content, data, dictionary);
	};
	
	exports.ButtonBehavior.prototype = Object.create(Behavior.prototype, {
		onTap: { value: function(content) { }},
		onTouchBegan: { value: function(content) {
			content.state = 1;
		}},
		onTouchEnded: { value: function(content) {
			content.state = 0;
			this.onTap(content);
		}}
	});
	
Notice that behavior templates build a prototype hierarchy:

	Behavior.prototype
		↳ CONTROL.ButtonBehavior.prototype
			↳ URLButtonBehavior.prototype
				↳ backButtonBehavior
				
####Shortcut
Handlers bind a URL path to a behavior to receive messages, as described in [KinomaJS Overview](../../overview/), Handlers. The bind function of the Handler constructor is a shortcut:

	Handler.bind("/wow", Behavior({
		onInvoke: function(handler, message) {
			debugger
		}
	})));
	
is equivalent to

	var wowHandler = new Handler("/wow");
	wowHandler.behavior = Object.create(Behavior.prototype, {
		onInvoke: { value: function(handler, message) {
			debugger;
		}}
	})
	
	Handler.add(wowHandler);

***
##Reference
####Constructors
When called as a function (without `new`) the `Behavior` constructor is a dictionary-based constructor.

#####`Behavior(dictionary)`
|||
| :--- | :--- | :--- |
| `dictionary`    | `object`    | |

*An object with properties to initialize the result*

|||
| :--- | :--- | :--- |
| Returns    | `object`    | |

*An instance of `Behavior.prototype`*

####Templates
The `Behavior` constructor provides the `template` function.

#####`Behavior.template(dictionary)`
|||
| :--- | :--- | :--- |
| `dictionary`    | `object`    | |

*An object with properties to initialize the prototype of the result*

|||
| :--- | :--- | :--- |
| Returns    | `function`    | |

*A constructor that creates instances of a prototype that inherits from `Behavior.prototype`*

The result can also used as a dictionary-based constructor and also provides the `template` function.

####Shortcut
The constructor provides the bind function as a shortcut.

#####`Handler.bind(path, behavior)`
|||
| :--- | :--- | :--- |
| `path`    | `object`    | |

*The path of the handler object*

|||
| :--- | :--- | :--- |
| `behavior`    | `object`    | |

*The handler's behavior*

Creates a handler object with `path`; assigns its behavior with `behavior`; puts the handler into the set of active handler objects.