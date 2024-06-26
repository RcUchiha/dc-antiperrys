---
layout: default
title: Templater de The0x
parent: Karaokes
nav_order: 2
---

# Creador de templates de The0x539

Hola, soy el creador de templates de The0x539. Realmente no tengo un nombre en sí, por lo que The0x539 escribió mis documentos en primera persona como una solución muy extraña. No aparece mucho, así que espero que no distraiga demasiado.

Si ya tienes experiencia con un creador de templates, muchos aspectos de mi funcionamiento pueden resultarte bastante familiares. Sin embargo, hay algunas diferencias clave en el uso básico que pueden causar confusión al cambiarte.
En particular, aunque comparto muchas características con otros creadores de templates, no soy compatible con templates escritos para ellos, o viceversa, aparte de casos incidentales sencillos.
En realidad, esto no es nada nuevo: los otros dos principales creadores de templates ya no son compatibles entre sí, pero parece que merece la pena dejar clara y explícita esta incompatibilidad. Cuando se escribe una template, hay que elegir a qué creador de templates dirigirse. Esto ya era así antes de que yo llegara.

# Templates
Cualquier template razonable consistirá en dos cosas, que estos documentos llamarán *components* e *input*.
A continuación se describirán en profundidad cada uno de ellos, pero brevemente, para aquellos que estén algo familiarizados con la creación de templates, un *component* es una línea de subtítulo marcada con `code`/`template`/`mixin` (más sobre esto último más adelante) en su campo de efecto, y una línea *input* es una marcada con `kara`.
Con algunas excepciones (principalmente `code once`), la idea detrás de ejecutar una template es, para cada línea de entrada, "ejecutar" cada componente contra esa línea (o contra cada char/etc dentro de ella).

## Input
Input is probably the simpler of the two parts of a template. Simply put, for a typical template, an input line consists of one line of lyrics for a song.
The lyrics may include k-timing tags, for use with `syl`-class components, as well as the standard karaskel hyphen-prefixed inline-fx tags. Details on k-timing are beyond the scope of this document.

Importantly, a line of input *must* have its effect field set to `kara` (or `karaoke`).
**Unlike other templaters, I do not treat a line as input unless it is already explicitly marked as such.**
This is not only a deliberate design decision, but also (arguably) necessary due to my more advanced conditional execution logic.

Input lines' actor fields are very useful for conditional execution purposes.
On a basic level, different actor values can be used to denote different parts of a song, applying a different set of components to each.
Getting more advanced, a template could be written to have a common `template` component apply to all the input, but select different `mixin`s based on parts of the song, perhaps to differ in color but share all other attributes.
In its most powerful form, an input line's actor field can be used as a collection of modifiers to mix and match different components on a per-line basis.
More on this later.

## Components
Components are the *fun* part of making a template. Components come in three varieties: `code`, `template`, and `mixin`.
My `code` and `template` components are, at a basic level, very similar to those of other templaters, and `mixin` components are like a more powerful form of karaOK's `template lsyl` family of components.

A component is defined by its effect field, which acts as a space-separated list of tokens.
The first token must be `code`, `template`, or `mixin`, depending on which kind of component it is.
The second token is the component's *class*, which is one of the following:

|**Class**|**Description**|
|-|-|
|`once`|Runs once at the beginning of template evaluation. Only valid for `code` components. (`template once` TBD)|
|`line`|Runs once for each line of input.|
|`syl`|Runs once for each k-timed `syl` within each line of input. Lines with no k-timing information will be treated as one big syl.|
|`word`|Like `syl`, but separated by whitespace instead of k-timing tags.|
|`char`|Runs once for each character (currently uses `unicode.chars`) within each line of input.|

Further tokens within a component's effect field, if present, are *modifiers*. More on those later.

### `code` Components
A `code` component is simple: running it executes its text as a block of Lua code.
Their most common use is `code once` components that import utility libraries, define helper functions, or set up global constants for use elsewhere in the template.
However, the other classes do occasionally prove useful, executing the code on, *e.g.*, a per-line basis.

### `template` Components
As their name might imply, `template` components are central to writing a template, as with my predecessors.
It is their sole responsibility to generate new lines of *output*. If there are no `template` components, no lines will be generated.
Generally speaking, running a `template` component entails evaluating any inline variables or inline expressions in its text, then generating a line of output consisting of the evaluated template text and the piece of text that the component's running against.
For example, consider the following simple template and the output it produces based on the given input:

```
template line: {\placeholder}
template syl: {\foo}
kara: {\k86}Word{\k41}less{\k24}ly {\k87}watch{\k33}ing {\k63}he {\k66}waits{\k25} {\k44}by {\k22}the {\k57}win{\k69}dow
kara: {\k36}And {\k38}won{\k59}ders{\k43} {\k25}at {\k16}the {\k40}emp{\k21}ty {\k49}place {\k26}in{\k85}side
fx: {\placeholder}Wordlessly watching he waits by the window
fx: {\foo}Word
fx: {\foo}less
fx: {\foo}ly
fx: {\foo}watch
fx: {\foo}ing
fx: {\foo}he
fx: {\foo}waits
fx: {\foo}
fx: {\foo}by
fx: {\foo}the
fx: {\foo}win
fx: {\foo}dow
fx: {\placeholder}And wonders at the empty place inside
fx: {\foo}And
fx: {\foo}won
fx: {\foo}ders
fx: {\foo}
fx: {\foo}at
fx: {\foo}the
fx: {\foo}emp
fx: {\foo}ty
fx: {\foo}place
fx: {\foo}in
fx: {\foo}side
```

Already this is useful for simple song styles, since you can now apply a set of tags to a set of lines, perhaps even with multiple layers, while not having to manually keep everything consistent.
Add some inline expressions to generate more complicated tags to each line and things start to get interesting.

### `mixin` Components
Mixins, as a first-class component variety, are my defining feature. They modify the output produced by `template` components, generally adding tags. If `template lsyl` from karaOK is unfamiliar to you, consider the following basic demonstration:
```
template line: {\Tpl}
mixin line: {\line}
mixin syl: {\syl}
mixin char: {\char}
kara: {\k86}Word{\k41}less{\k24}ly {\k87}watch{\k33}ing {\k63}he {\k66}waits{\k25} {\k44}by {\k22}the {\k57}win{\k69}dow
kara: {\k36}And {\k38}won{\k59}ders{\k43} {\k25}at {\k16}the {\k40}emp{\k21}ty {\k49}place {\k26}in{\k85}side
fx: {\Tpl\line\syl\char}W{\char}o{\char}r{\char}d{\syl\char}l{\char}e{\char}s{\char}s{\syl\char}l{\char}y{\char} {\syl\char}w{\char}a{\char}t{\char}c{\char}h{\syl\char}i{\char}n{\char}g{\char} {\syl\char}h{\char}e{\char} {\syl\char}w{\char}a{\char}i{\char}t{\char}s{\syl\char} {\syl\char}b{\char}y{\char} {\syl\char}t{\char}h{\char}e{\char} {\syl\char}w{\char}i{\char}n{\syl\char}d{\char}o{\char}w
fx: {\Tpl\line\syl\char}A{\char}n{\char}d{\char} {\syl\char}w{\char}o{\char}n{\syl\char}d{\char}e{\char}r{\char}s{\syl\char} {\syl\char}a{\char}t{\char} {\syl\char}t{\char}h{\char}e{\char} {\syl\char}e{\char}m{\char}p{\syl\char}t{\char}y{\char} {\syl\char}p{\char}l{\char}a{\char}c{\char}e{\char} {\syl\char}i{\char}n{\syl\char}s{\char}i{\char}d{\char}e
```

Any number of `mixin` components can "apply to" any number of `template` components using my powerful conditional execution abilities, offering much more expressive templating than the "name association" system my predecessors offer.

Mixins can be very useful for relying more heavily on `template line` than is strictly necessary, resulting in cleaner output that's also easier to make sense of.
For instance, combining `template line` with `mixin syl` allows for simple (read: doesn't involve movement or horizontal scaling) in-line highlight effects, and `mixin char` can be useful for by-character gradients.

`mixin line` is an interesting construct. It goes once at the start of each generated line, like the text of the template it's modifying, but as a mixin, it works as a modifier. Even if the template's class is `syl`, `word`, or `char`, it's appropriate to use `mixin line` to modify the entire line *of output*.
Combined with conditional execution, this behavior becomes quite useful:
- If a song style is largely uniform, but uses different colors for different groups of lines (perhaps color-coded by character), then the common tags could be put in a `template` that applies to the entire song, and there would be a `mixin` for each color, with each one set to execute when appropriate (likely using the `actor` modifier). This could also have been achieved using a sort of lookup table, but it makes for a decent example, and this approach actually scales better when space-separated actors get involved.
- If a style has some complicated stuff going on, but there is, for instance, a variety of templates meant to generate a consistent "glow" effect, then a mixin can be used to apply the same tags to all "glow" output without adding them to each template.

### Modifiers
In addition to its variety and class, a component's effect field may contain any amount of *modifiers*, which alter its behavior.
Most modifiers allow you to specify the conditions under which a component will execute based on the input or whatever else.

If there are multiple different restrictions on a component, then all restrictions must be satisfied for the component to execute.
For example, a mixin with the modifiers `actor foo actor bar layer 3 layer 5` will execute only for lines where ((actor contains `foo` OR actor contains `bar`) AND (layer is 3 or layer is 5)).

#### `style`
By default, a component is "interested" in only the style that it is itself set to. This means that it will only execute for input with a matching style.
Adding `style foo` to a template will add `foo` to its set of interested styles, so it will also execute for input with the `foo` style.
Styles with names containing spaces do not currently work with this modifier.

#### `anystyle`
Similar to the `all` modifier in other templaters, this makes the component interested in any style.

#### `actor`
Similar to the `style` modifier, each component has a set of actor values it is "interested" in.
By default, this set is empty and a component won't look at the input's actor field.
Adding one or more `actor` modifiers will restrict the component to only execute for input with matching actor values.

**Advanced usage:** The actor field of a line of input is treated not as a single string, but as a space-separated list of actors, so input can be given multiple "actor values" that the component tries to match.
If there is any intersection between the component's set of interested actors and the input's set of actor values, the component will execute.

#### `noactor`
Works basically the same way as `actor`, but adds to a list of *"disinterested"* actor values for which the component will *not* execute.
Can sort of be said to take "precedence" over `actor`, but that doesn't accurately reflect how these modifiers are implemented.

#### `t_actor`
Only valid for `mixin`s.
Very similar to `actor`, but checks the base template the mixin is running on, not the input.
For instance, if one has a `template` that generates triangular particle effects, one could put `tri` in its actor field and then use `t_actor tri` to make mixins that only apply to the particles.

#### `no_t_actor`
Given the previous three modifiers, this should be self-explanatory.

#### `sylfx` and `inlinefx`
Only valid for `syl` and `char` components.
Another "interested values" system, this time for checking the value of the current syllable's `syl_fx`/`inline_fx` field.
Useful for expressing conditional execution that changes on a per-syl basis.

#### `layer`
Only valid for `mixin`s.

Again, similar to `style` or `actor`.
By default, a mixin is "interested" in all layers.
This modifier restricts a mixin to only execute for certain layer numbers.

When checking the layer number, I look at *the output's current layer number*.
Output lines start with a layer number copied from the `template` that generated them, but the number may be changed by functions such as `relayer`.
If the layer is changed, I will "see" the updated layer number, not the original.

#### `if` and `unless`
Easily the most powerful conditional execution modifier.
Given an `if foo` modifier, I will look in the Lua environment for a value named `foo`.
When checking whether to execute, I will expect this value to be either a boolean or a function.
If it is a function, I will call it (with no arguments) and look at the result.
Either way, I then use this value as a predicate to decide whether the component should execute.

For `if`, the component will only execute if the value is `true`.
For `unless`, the component will only execute if the value is `false`.

At the moment, only one instance of the `if`/`unless` modifier is allowed per component.

`cond` is an alias for `if`.

#### `noblank`
Primarily useful for `syl` and `char` components.
Similar to the `noblank` modifier in other templaters, but also works for mixins.
This is useful for, say, preventing a `mixin char` component from pointlessly executing for spaces.

Components with the `noblank` modifier will not execute for input whose text is empty or consists entirely of whitespace.
Unlike other templaters, this will *not* filter out syls with zero duration. For that, see `nok0`.

#### `nok0`
Only valid for `syl` and `char` components.

Components with this modifier will not execute for syls with zero duration, or chars belonging to such a syl.

#### `keepspace`
Only valid for `template`s. Primarily useful for `syl` and `word`.
By default, just before inserting a line of output into the file, I will trim away any trailing whitespace.
The `keepspace` modifier will disable this behavior for an individual template, should this be desirable for any reason.

#### `nomerge`
Only valid for `template`s.
By default, just before removing trailing whitespace, I will merge consecutive tag blocks from output lines.
This is done using the primitive technique of naively removing `}{` from the output, so if this breaks anything fancy, the `nomerge` modifier can disable this behavior.

#### `notext`
Only valid for `template`s.
Equivalent to the `notext` modifier from other templaters.
By default, the last step of generating a line (i.e. after running all `mixins`) is to append the corresponding text from the source line. The `notext` modifier disables this behavior.

Useful when, say, generating particle effects with drawings.

#### `loop`
Other templaters support giving a component a `loop n` modifier, where `n` is some integer.
The component will then execute *n* times, and the current/maximum loop values can be accessed using `j` and `maxj`, available as both Lua variables and inline variables.
Loops follow the convention set by Lua, starting at 1 and including the maximum.

My implementation of this feature is similar, but I require specifying a name in the modifier, e.g., `loop foo 3`.
The loop count is then accessed using `$loop_foo` or, in Lua, `loopctx.state.foo`, and the maximum with `$maxloop_foo` or `loopctx.max.foo`.
This allows for multiple independent named loops on a single component.
Having multiple loops can be very powerful for anything that requires more than one "dimension" of repetition, like combining some frame-by-frame effect with a vertical gradient.

If there are multiple loops, they will be nested in *reverse* order of appearance.
As a demonstration of this, consider the following example:
```
template line loop foo 2 loop bar 3: {\foo$loop_foo\bar$loop_bar}
kara: {\k86}Word{\k41}less{\k24}ly {\k87}watch{\k33}ing {\k63}he {\k66}waits{\k25} {\k44}by {\k22}the {\k57}win{\k69}dow
kara: {\k36}And {\k38}won{\k59}ders{\k43} {\k25}at {\k16}the {\k40}emp{\k21}ty {\k49}place {\k26}in{\k85}side
fx: {\foo1\bar1}Wordlessly watching he waits by the window
fx: {\foo2\bar1}Wordlessly watching he waits by the window
fx: {\foo1\bar2}Wordlessly watching he waits by the window
fx: {\foo2\bar2}Wordlessly watching he waits by the window
fx: {\foo1\bar3}Wordlessly watching he waits by the window
fx: {\foo2\bar3}Wordlessly watching he waits by the window
fx: {\foo1\bar1}And wonders at the empty place inside
fx: {\foo2\bar1}And wonders at the empty place inside
fx: {\foo1\bar2}And wonders at the empty place inside
fx: {\foo2\bar2}And wonders at the empty place inside
fx: {\foo1\bar3}And wonders at the empty place inside
fx: {\foo2\bar3}And wonders at the empty place inside
```
See the `incr` method in the `loopctx` class for how this works.
Basically, usage of `loop` is treated as one big loop, where the iteration step entails treating the loopctx as a little-endian list of digits:
* Increment the first loop variable.
* If this exceeds that loop's maximum value, step forward by resetting it to 1 and incrementing the *next* loop variable.
* If the next loop variable also overflowed, step forward again in the same fashion. Repeat.
* If the final loop variable has overflowed, the "big loop" is done.

`repeat` is an alias for `loop`.

Mixins now support loops! Loop variables for mixins are namespaced separately from their template's loop variables.
As inline variables, they are accessed with `$mloop_bar` and `$maxmloop_bar`.
In Lua, they are accessed from `mloopctx` instead of `loopctx`, and the `maxmloop` function takes the place of `maxloop`.

#### `prefix`
Only valid for `mixin`s.
Mixin output is usually placed just before the text it's associated with.
The `prefix` modifier instead places it at the beginning of the line, alongside the base `template` text and any `mixin line` components.
Consider the following example, where one `mixin word` component has the `prefix` modifier and one does not:
```
template line: {\X}
mixin word prefix: {\Y(!word.text!)}
mixin word: {\Z}
kara: {\k86}Word{\k41}less{\k24}ly {\k87}watch{\k33}ing {\k63}he {\k66}waits{\k25} {\k44}by {\k22}the {\k57}win{\k69}dow
kara: {\k36}And {\k38}won{\k59}ders{\k43} {\k25}at {\k16}the {\k40}emp{\k21}ty {\k49}place {\k26}in{\k85}side
fx: {\X\Y(Wordlessly )\Y(watching )\Y(he )\Y(waits )\Y(by )\Y(the )\Y(window)\Z}Wordlessly {\Z}watching {\Z}he {\Z}waits {\Z}by {\Z}the {\Z}window
fx: {\X\Y(And )\Y(wonders )\Y(at )\Y(the )\Y(empty )\Y(place )\Y(inside)\Z}And {\Z}wonders {\Z}at {\Z}the {\Z}empty {\Z}place {\Z}inside
```

This may seem like a fairly strange modifier to offer, but I have it for a fairly specific purpose: using `\t(\clip)` in a `mixin syl prefix` component to produce behavior similar to `\kf`, but with all the flexibility of clips and transforms.

# Template Execution
TODO

## Execution Order/Semantics
```
for each line of input:
	(try to) run all `code line` components
	(try to) run all `template line` components

	for each word in the line:
		(try to) run all `code word` components
		(try to) run all `template word` components

	do the above for syls and chars
```

(mixins happen as part of "running a template")
see the `apply_templates` function for more details

TODO

## Inline Expressions
In a template's text, Lua expressions enclosed in a pair of `!` will be evaluated, with their result placed in the output text, like `!word.text!` as used in an earlier example.
Unlike other templaters, I will treat `nil` as an empty string here, which is convenient when writing functions like `retime` and `relayer` that are useful for their side effects.

Depending on the component being executed, a few tables such as `line`, `syl`, `word`, and/or `char` will be in scope, and will be populated with information such as `.text_stripped`, `.duration`, or `.width`.
More information on common fields can be found [here](https://aegisub.shinon71.moe/Automation/Lua/Modules/karaskel.lua).
The stock templater lacks full support for `word` and `char` classes, but they are somewhat similar to `syl`.
"Parent" items can be accessed where it makes sense, such as `char.syl` or `word.line`.

I populate these objects with some fields that aren't available in other templaters.
Notably, in a `syl`, the `.syl_fx` field is similar to the `.inline_fx` field, but does not "stick" to subsequent syls within a line.

For example, consider the following line: `{\k86}Word{\k41}less{\k24}ly {\k87\-foo}watch{\k33}ing {\k63}he {\k66\-bar}waits{\k25} {\k44}by {\k22\-baz}the {\k57}win{\k69}dow`, and how the fields are populated:

`syl.text_stripped` | Word | less | ly | watch | ing    | he    | waits |       | by    | the    | win    | dow
--------------------|------|------|----|-------|--------|-------|-------|-------|-------|--------|--------|-------
`syl.inline_fx`     | ""   | ""   | "" | "foo" | "foo"  | "foo" | "bar" | "bar" | "bar" | "baz"  | "baz"  | "baz"
`syl.syl_fx`        | ""   | ""   | "" | "foo" | ""     | ""    | "bar" | ""    | ""    | "baz"  | ""     | ""

TODO

## Inline Variables
In a template's text, a dollar sign followed by certain names will be expanded to certain values, like `$loop_foo` as used in an earlier example.
I don't support all the same loop variables as other templaters, and the ones I do support will often have different names.
More loop variables will probably be added in the future, since they're so convenient.
For now, see the `eval_inline_var` function for what I currently offer.

TODO

## Available Functions
If present, the `ln.kara` and `0x.color` modules will be loaded automatically and can be accessed as `ln` and `colorlib`, respectively.

Modules (code files that contain helper functions, as opposed to macros, which generally call `aegisub.register_macro` at some point) go in the `include` directory. In accordance with the standard Lua module system, those two modules must be located at `ln/kara.lua` and `0x/color.lua` respectively in order for those paths to resolve. This is completely normal; I'm not doing anything special here. Any auto4 code that loads any module follows these same rules.

Several familiar functions from other templaters, such as `retime` and `maxloop`, are available with a few enhancements, and I also offer a few new functions.

See the `util` and `template_env` declarations in my source code for my implementations of these functions, and to see what standard library functionality I expose by default.

### `print(...)`
A handy debugging function. Works like Lua's standard `print` function, but backed by `aegisub.log`.
I also offer `printf`, which is literally just an alias for `aegisub.log`.

### `retime(mode, start_offset=0, end_offset=0)`
If you have any past templating experience, this function should be familiar.
It's the most convenient way to change an output line's start/end times.

I have a handful of modes that can be pretty useful and aren't necessarily available in other templaters.
In particular, the `clamp` modes can be used to ensure other retimes don't extend the output's start/end times past the input's.

| Mode                | `line.start_time = start_offset +`    | `line.end_time = end_offset +`        |
|---------------------|---------------------------------------|---------------------------------------|
| `"syl"`             | `orgline.start_time + syl.start_time` | `orgline.start_time + syl.end_time`   |
| `"presyl"`          | `orgline.start_time + syl.start_time` | `orgline.start_time + syl.start_time` |
| `"postsyl"`         | `orgline.start_time + syl.end_time`   | `orgline.start_time + syl.end_time`   |
| `"line"`            | `orgline.start_time`                  | `orgline.end_time`                    |
| `"preline"`         | `orgline.start_time`                  | `orgline.start_time`                  |
| `"postline"`        | `orgline.end_time`                    | `orgline.end_time`                    |
| `"start2syl"`       | `orgline.start_time`                  | `orgline.start_time + syl.start_time` |
| `"syl2end"`         | `orgline.start_time + syl.end_time`   | `orgline.end_time`                    |
| `"presyl2postline"` | `orgline.start_time + syl.start_time` | `orgline.end_time`                    |
| `"preline2postsyl"` | `orgline.start_time`                  | `orgline.start_time + syl.end_time`   |
| `"delta"`           | `line.start_time`                     | `line.end_time`                       |
| `"set"`, `"abs"`    | `0`                                   | `0`                                   |

As a special case, `clamp` keeps the start and end times unchanged, unless they would fall outside the input line's start/end times (plus offsets, if applicable).
Similarly, `clampsyl` does the same, but with the original syl's start/end times.

| Mode                | `line.start_time = max(line.start_time,` | `line.end_time = min(line.end_time,` |
|---------------------|------------------------------------------|--------------------------------------|
| `"clamp"`           | `orgline.start_time + start_offset)`     | `orgline.end_time + end_offset)`     |
| `"clampsyl"`        | `syl.start_time + start_offset)`         | `syl.end_time + end_offset)`         |

### `relayer(new_layer)`
`line.layer = new_layer`. Enough said.

### `maxloop(var, val)`, `maxmloop(var, val)`
*Basically* `loopctx.max[var] = val`.
Usually useful if your loop count isn't hardcoded; perhaps it's based on the width of the syllable.
Note that this function will get called repeatedly if it's in a component that gets called repeatedly, so it's generally advisable to use a `val` that remains consistent across repeated calls.
`maxloop` is for `template` loops, while `maxmloop` is for `mixin` loops.

### `set(key, val)`
`tenv[key] = val`. For if you want to assign to a (global) variable in an inline expression, and that's about it.

### `skip()`
If this function is called while generating a line, that specific line of output will be discarded.
(The rest of the template will still be evaluated, resulting in a complete generated line - the only difference is that the line won't be inserted into the document.)

One possible use case for this is to call it in a mixin that has condition modifiers of some sort to express "If these conditions are met, *do not* generate output", since condition modifiers themselves can only express "execute only if these conditions are met".

This is similar to calling `maxloop(0)` in karaOK, except not tied to a somewhat orthogonal feature, so one could in theory skip certain iterations of a loop without prematurely ending the entire loop.

### `unskip()`
Cancels the effect of a previous `skip()` call, meaning the line will once again be included in the document.

Whichever of the two functions was called most recently takes precedence.

### `mskip()`
Skips the current iteration of the current mixin.
Works very similarly to `skip()`, but on the level of an individual mixin's evaluation.

### `unmskip()`
Take a wild guess.

### `util.tag_or_default(tag, default)`
logarithm's `ln.line.tag` is a nice function, but because of how other templaters treat inline expressions that evaluate to `nil`, it returns an empty string if it finds no tags.
The empty string is truthy in Lua, so this means that `ln.line.tag("foo") or "\\foo(bar)"` doesn't work the way one might hope.
This function arose from a frustration with having to write code to do that so many times, so I allow you to just do `util.tag_or_default("foo", "\\foo(bar)")`. Hooray!

### `util.fad(t_in, t_out)`
A specialization of `util.tag_or_default`, this function will handle formatting the `\fad` tag for you.

### `util.xf(obj=char, objs=orgline.chars, field='center')`
Most commonly called with no arguments.
Returns the position of the current character, returning 0.0 for the first char in the line and 1.0 for the final char.

Supports the passing of alternative objects and position fields, *e.g.*, `util.xf(syl, orgline.syls, 'right')`

In the beginning, there was `char.ci / #orgline.chars`, used by many a gradient-by-character script.
However, index-based calculations are flawed for variable-width fonts, and may produce lopsided gradients depending on the glyphs within the line.
Then there was `char.x / orgline.width`.
This was better, but whether looking at the left, center, or right, this approach does not reach both extremes.
Finally, there came `char.center / (final_char.center - first_char.center)`, and it was good.
It became an annoying piece of boilerplate, though, so now I offer this function to just do it.

This function was formerly known as `util.cx`.
This name now exists as an alias for backwards compatibility.

### `util.lerp(t, v0, v1)`
Straightforward linear interpolation of two numbers: `(v1 * t) + (v0 * (1 - t))`.

### `util.gbc(c1, c2, interp, t=util.xf())`
A somewhat flexible function for gradient-by-character effects.
Typical usage would go in a `mixin char` component and look something like `{\3c!util.gbc('&H0000FF&', '&HFF0000&')!}`.
If no `interp` function is provided, I will try to guess how to interpolate the two values.
At the moment, this means:
* If both values are numbers, use `util.lerp`.
* If both values are strings and the first value matches the usual format for an alpha tag (i.e., the pattern `&H[0-9a-fA-F][0-9a-fA-F]&`), use `colorlib.interp_alpha`.
* Otherwise, if they're both strings, assume they're colors and use `colorlib.interp_lch`.
* Otherwise, give up.

If this is unsatisfactory, well, that's why I expose this function's third argument: specify your own interpolation function with a signature analagous to `util.lerp`'s.

### `util.multi_gbc(cs, interp, t=util.xf())`
Like `util.gbc`, but accepts a *list* of colors, for multi-stop gradients. Nifty.

### `util.make_grad(v1, v2, dv=1, vertical=true, loopname='grad', extend=true)`
The first step to creating a clip-based gradient.

`v1` and `v2` are the min/max values for the span of the gradient.
`dv` is the "step size": each segment of the gradient will generally be this size.
`vertical` determines whether the gradient goes from top to bottom or from left to right.
Unless `extend` is false, the first and last gradient segments will have their clips extended to the edge of the screen.

This function calls `maxloop` in order to emit multiple lines; `loopname` can be used to specify an alternate name to use.

### `util.get_grad(c1, c2, interp, loopname='grad', offset=0)`
The second step to creating a clip-based gradient.
This works in largely the same way as `util.gbc`,
except that it determines the interpolation `t` value using the template loop status.

`offset` will be added to the loop state value when calculating `t`. This is useful for accurately determining the "next" or "previous" gradient value.

### `util.get_multi_grad(cs, interp, loopname='grad')`
Like `multi_gbc`, but for `get_grad`.

### `util.ftoa(n, digits=2)`
Formats `n` as a string with at most `digits` decimal digits, *without* any unwanted trailing zeroes/decimal point.
Why does no programming language ever seem to make this easy?

### `util.fbf(mode='line', start_offset=0, end_offset=0, frames=1, loopname='fbf')`
Similar to `retime` in usage, but splits a line into one copy timed to each frame of video.
If `frames` is set to a larger integer, then the line will be split into sections that each last that many frames.
Much like `util.make_grad`, this function uses regular user-accessible template loops; the loop variable can be customized using the `loopname` argument.

### `util.rand.sign()`
Either -1 or 1, randomly.

### `util.rand.item(list)`
A random element in the provided list.
Assumes a standard 1-indexed Lua list with no gaps, like any other "list"-y function.

If passed a string, returns a random UTF-8 code point from its text.

### `util.rand.bool(p=0.5)`
A random Boolean value, where `p` is the probability that the value is `true`.

### `util.rand.choice(a, b, p)`
`if util.rand.bool(p) then a else b`

### `util.math.round(n)`
Rounds `n` to the nearest integer.