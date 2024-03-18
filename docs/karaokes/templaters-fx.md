---
layout: default
title: Creadores de templates de karaoke
parent: Karaokes
nav_order: 1
---

# Creadores de templates de karaoke

Haz clic [aquí](https://github.com/TypesettingTools/arch1t3cht-Aegisub-Scripts/blob/main/doc/templaters.md#first-steps) para ir a la parte importante.

Esta no es una guía completa sobre cómo hacer KFX, sino más bien una explicación rápida de las ideas principales que deberían ayudarte lo suficiente para comenzar a leer la documentación de tu creador de templates favorito. Principalmente contiene una versión mejorada de una larga explicación que escribí en Discord en algún momento, así como algunas tablas que explican cómo convertir entre los tres creadores de templates establecidos. Básicamente, es la guía que hubiera deseado tener al aprender esto.

Si estás buscando guías reales sobre KFX, aquí tienes algunos enlaces:

- [Guía KFX de Zahuczky](https://zahuczky.com/01-kfx-guide-introduction/): Todavía en proceso de elaboración al momento que escribo esto.
- [Publicaciones de Jocko](https://jockotan.wordpress.com/2015/08/12/karaoke-templater/): También muy concisas y basadas en ejemplos, pero usa el creador de templates estándar.
- La documentación real de los diversos creadores de templates, enlazada abajo.

Además, tengo [aquí](https://github.com/TypesettingTools/arch1t3cht-Aegisub-Scripts/blob/main/doc/misc_kara.md) una colección de templates diversas que hice para la edición de carteles u otros fines que no son estilización de canciones.

---

## Creadores de Templates Existentes

Al momento de escribir esto (2022), hay tres principales creadores de templates de karaoke:

1. El creador de templates estándar que viene con Aegisub, documentado [aquí](https://aegisub.org/docs/latest/automation/karaoke_templater/) como parte de la documentación de Aegisub. La documentación es muy detallada, pero bastante técnica.
1. [KaraOK](https://github.com/logarrhythmic/karaOK), una versión modificada del creador de templates estándar junto con una biblioteca de utilidades. También se incluye por defecto con algunas versiones más nuevas de Aegisub, como AegisubDC.
1. [El Creador de Templates de The0x539](https://github.com/The0x539/Aegisub-Scripts/blob/trunk/src/0x.KaraTemplater.moon), documentado [aquí](https://github.com/The0x539/Aegisub-Scripts/blob/trunk/doc/0x.KaraTemplater.md) en su repositorio. Este es un creador de templates completamente reescrito con una lógica de ejecución más potente (mixins, condicionales más fuertes, bucles anidados, etc) y variables organizadas de manera más sensata.

Excepto por furigana y otros usos de `furi` o templates `multi`, realmente no hay nada que el creador de templates estándar o KaraOK puedan hacer que no pueda ser fácilmente portado al creador de templates de The0x. Siempre que esté instalado, el creador de templates de The0x también contiene soporte directo para la biblioteca de utilidades de KaraOK. Es por eso que será el creador de templates predeterminado para esta guía, junto con notas sobre cómo los conceptos equivalentes funcionan en otros creadores de templates. Incluso si nunca escribirás templates en otros creadores de templates, estos últimos aún pueden ser útiles para entender las templates existentes.

### Ver también

Existen otras herramientas destacadas para hacer KFX. No se explican aquí porque funcionan de manera muy diferente, pero aún así vale la pena mencionarlas.

- [KaraEffector](https://github.com/KaraEffect0r/Kara_Effector), una gran colección de plantillas existentes y formas de modificarlas.
- [PyonFX](https://github.com/CoffeeStraw/PyonFX), una biblioteca de Python para generar subtítulos `.ass`, adaptada para KFX.
- [aegsc](https://github.com/butterfansubs/aegsc), un compilador que produce scripts `.ass` (especialmente templates de karaoke) que permite escribir templates con un formato más conveniente.

---

## Empezando

Esta guía utiliza el [Creador de Templates de The0x539](https://github.com/The0x539/Aegisub-Scripts/blob/trunk/src/0x.KaraTemplater.moon), el cual necesitarás instalar primero (también te recomiendo encarecidamente que asignes su función a un atajo de teclado. Yo uso Ctrl+Alt+S). La guía intercala explicaciones semitécnicas (formateadas normalmente) con ejemplos muy simples pero concretos (formateados en cursiva). También puedes seguir solo los ejemplos en cursiva si prefieres un enfoque puramente práctico.

### Primeros Pasos

Lo siguiente es el archivo de karaoke más simple que puedas imaginar:
```
[Script Info]
ScriptType: v4.00+
WrapStyle: 0
ScaledBorderAndShadow: yes
YCbCr Matrix: TV.709
PlayResX: 1920
PlayResY: 1080

[V4+ Styles]
Format: Name, Fontname, Fontsize, PrimaryColour, SecondaryColour, OutlineColour, BackColour, Bold, Italic, Underline, StrikeOut, ScaleX, ScaleY, Spacing, Angle, BorderStyle, Outline, Shadow, Alignment, MarginL, MarginR, MarginV, Encoding
Style: Karaoke,Georgia,72,&H002A0A00,&H000019FF,&H00FFFFFF,&H00000000,0,0,0,0,100,100,0,0,1,2.5,0,8,30,30,25,1

[Events]
Format: Layer, Start, End, Style, Name, MarginL, MarginR, MarginV, Effect, Text
Comment: 0,0:00:00.00,0:00:00.00,Karaoke,,0,0,0,template line,
Comment: 0,0:00:00.00,0:00:00.00,Karaoke,,0,0,0,,
Comment: 0,0:00:20.88,0:00:26.80,Karaoke,,0,0,0,kara,{\k0}{\k39}A{\k22}to {\k54}i{\k44}chi{\k40}do {\k18}da{\k60}ke {\k12}ki{\k23}se{\k63}ki {\k39}wa {\k20}o{\k20}ko{\k39}ru {\k36}da{\k63}rou
Comment: 0,0:00:26.80,0:00:32.96,Karaoke,,0,0,0,kara,{\k36}{\k20}Ya{\k30}sa{\k70}shi{\k38}i {\k39}ko{\k21}e {\k58}de {\k21}e{\k18}ga{\k57}ku {\k23}yu{\k37}gan{\k20}da {\k41}mi{\k35}ra{\k52}i
```
(Está sincronizado con [este OP](https://animethemes.moe/anime/fatezero_2nd_season/OP-NCBD1080), por si deseas probarlo.)

Contiene:

- Un estilo.
- Algunas líneas k-timeadas comentadas que tienen este estilo, marcadas con `kara` en su campo de Efecto.
- Una única línea (en blanco, por ahora) comentada que tiene este estilo, marcada con `template line` en el campo Efecto.

*Al ejecutar la macro `The0x539's Templater`, el creador de templates verá la `template line`. Para cada línea marcada como `kara` con el mismo estilo que esta línea, generará una línea con el texto de la línea kara, sin las etiquetas de tiempo k. Estas líneas generadas serán marcadas como `fx` para que puedan ser eliminadas o reemplazadas más tarde.*

Ten en cuenta que diferentes creadores de templates utilizan palabras clave ligeramente diferentes para algunos de estos marcadores:

| Concepto                                      | En Stock Templater  | En KaraOK       | En The0x's Templater |
| --------------------------------------------- | ------------------- | --------------- | -------------------- |
| Marcador de línea de karaoke                  | `karaoke`           | `karaoke`       | `kara` or `karaoke`  |
| Template que aplica tags una vez a cada línea | `template pre-line` | `template line` | `template line`      |

Este es el principio de una lista más larga de diferencias entre los templaters. Las [tablas](#comparación) al final de esta guía muestran la lista completa de diferencias.

*A continuación, intenta escribir `{fad(150,150)}` en el texto de la línea `template line`, y aplica el template.
Esto dará a cada línea generada `fx` estos tags de desvanecimiento. Así que ya hemos creado una versión peor de HYDRA usando templates de karaoke.*

Lo que ocurre aquí es que antes de convertir una línea `kara` en una línea `fx` usando una `template line`, el creador de templates añadirá lo que la línea `template line` contenga delante del texto (eliminado) de la línea `kara`.
Ahora, la parte interesante es que esto no sólo puede contener texto estático, sino también expresiones Lua. Éstas van envueltas en signos de exclamación.

*For example, if you change the template line line's text to `{\fad(!2*50!, 150)}`, you'll get a bunch of lines with `\fad(100,150)` tags, because what was in the exclamation marks was evaluated to 100 using Lua.*

Furthermore, in this Lua environment, the templater gives you access to all necessary information about your `kara` line.
This is stored in the `orgline` table, which has the format of the standard table describing an `.ass` line, but also contains lots of additional fields added by karaskel or by The0x's templater.
The karaskel fields are documented [here](https://aegisub.org/docs/latest/automation/lua/modules/karaskel.lua/#dialogue-line-table) while The0x's fields are documented in the templater's documentation.

*For example, `orgline.duration` is the duration of the `kara` line.
So if you put `{\t(0,!orgline.duration/2!,\fscx150\fscy150)\t(!orgline.duration/2!,!orgline.duration!,\fscx100\fscy100)}` in your `template line`, this would create an effect that makes each line get bigger and smaller over its duration.
If the line is 1000ms long, this will turn into `{\t(0,500,\fscx150\fscy150)\t(500,1000,\fscx100\fscy100)}`.*

Lua eval is one of the two additional features available in templates.
The other consists of inline variables prefixed by `$`, which will be directly substituted with their value.
This happens before evaluating Lua expressions.

*For example, the inline variable `$ldur` also evaluates to the line's duration.
So the above template could also be written as `{\t(0,!$ldur/2!,\fscx150\fscy150)\t(!$ldur/2!,$ldur,\fscx100\fscy100)}`.
Note that the last occurrence of `$ldur` is not wrapped in exclamation marks, as its value can be directly substituted into the command text.
However, the previous two instances still need to happen inside of a Lua eval block to perform arithmetic operations with them afterwards.*

The inline variables for the stock templater are documented [here](https://aegisub.org/docs/latest/automation/karaoke_templater/inline_variables/).
KaraOK adds a character index variable `$ci`.
However, The0x's templater removes most of these inline variables.
You can see which ones are still available [here](https://github.com/The0x539/Aegisub-Scripts/blob/4d00d789d4897a04687fa7f66d3dce29dae64b64/src/0x.KaraTemplater.moon#L750-L775).
However, this is not really a restriction, as all of their values (and more) are accessible via `orgline` and friends.

### Syllables
Up to now, we've just added some tags to every k-timed `kara` line, without ever using the actual k-timing.
But now that we've introduced Lua eval and inline variables, we're ready to make sensible syllable-based effects.

*If we replace the effect of our `template line` line with `template syl` and apply the template again, the templater will suddenly generate multiple `fx` lines for each `kara` line - one `fx` line for each k-timed syllable of the `kara` line.
If you didn't remove the scaling effect we made before, they'll all be stacked on top of each other at the top of the screen.
If you did remove it, they'll be moved successively further down until they cover the whole screen.*

This is because, by default, the generated syllable `fx` lines don't have any formatting whatsoever, so naturally they'll just follow the alignment dictated by their style.
To position them correctly, we need to use `\an` and `\pos` tags together with some of the variables the templater gives us.
We've used `orgline` before, and `orgline.left` gives us the x position of the left edge of the `kara` line, when formatting it with whatever style it has.
Similarly, `orgline.top` gives us the y coordinate of the top edge.

To put each individual syllable where it needs to go, we can use the analogous `syl` table, which (obviously) contains similar information about the syllable.
Most importantly, `syl.left` contains the x position of the syllable's left edge, *relative to the line it's in*.

*With this in mind, we can write the following into our `template syl`: `{\an7\pos(!orgline.left+syl.left!,!orgline.top!)}`.
If we now reapply our template, all the syllables will be at the correct position.*

*Now, while `\an7` was the easiest example, but it's rarely convenient for any actual effects.
So let's instead use, `{\an5\pos(!orgline.left+syl.center!,!orgline.middle!)}` which uses the analogous variables for the x and y positions of the middle of the line or syllable.
This doesn't change the positioning, but uses `\an5` instead, which is more useful in almost all cases.*

On the stock templater or KaraOK you might also find inline variables like `$scenter` and `$lmiddle` used for these[^2].
We don't need `syl` for the y position, since the syllable's y position isn't dependent on the line with normal formatting.
In fact `syl.middle` doesn't even exist.
[^2]: I am aware that `$smiddle` also exists, but I'm trying to highlight the `$s[var]` and `$l[var]` pattern.

*With everything we know now, we can already make a simple template.
The only remaining pieces we need are the variables `syl.duration`, `syl.start_time`, and `syl.end_time`, which are the syllable's duration and start and end times in milliseconds respectively, with the latter two again being relative to the current line's start time.
With this, we can make a simple template*
```
{\an5\pos(!orgline.left+syl.center!,!orgline.middle!)
\t(!syl.start_time!,!syl.start_time+syl.duration/2!,\fscx130\fscy130)
\t(!syl.start_time+syl.duration/2!,!syl.end_time!,\fscx100\fscy100)}
```
*that highlights each syllable by making it larger and smaller again.*

*Note: This is only an example styling, designed to be as simple as possible. It is not perfect styling, nor is it styling you should copy at all. Its biggest problem is that the scaling on syl highlights follows a simple triangle curve: It grows for half of the syllable's duration, and shrinks for the other half. This will make short syllables grow and shrink very quickly, while the more dragged out syllables grow and shrink very slowly, reaching their peak size far too late. For actual song styling, you should instead make the highlight follow something resembling an [ADSR Curve](https://en.wikipedia.org/wiki/Envelope_(music)#ADSR), i.e. leave the growing and shrinking times somewhat constant and instead sustain the highlight effect for some time for longer syllables.*

**I've now explained most of the basics of templating, and hopefully you should now be able to read the documentation of the various templaters or dissect existing templates ([these](misc_kara.md), for example) to dive deeper. Read on if you want primers on some of the more specific concepts.**

### Multiline effects and mixins
Our `template syl` generates one `fx` line for each input syllable.
If we want to make an effect that generates multiple lines for each syllable we can add more `template syl` line. (For more complex effects, we can also use loops.)

*For example, let's add a blue-ish glow effect to our karaoke lines.
Take the `template syl` line we've made before, and duplicate it.
Increase the second one's layer by one, and add `\3c&HFFCCCC&\bord5\blur5` to the first one.
If you now apply the template, there will be two `fx` lines generated for each syllable - one for each `template syl`.
The `fx` lines will also have the same layers as their respective `template syl` lines.*

But if we're not careful (as happened in this example), this now duplicates a lot of our template code. If we want to change some element of the highlight effect, we'd need to change both `template syl` lines. This is where mixins are very helpful.

Mixins are a way to apply additional tags (or any text, really) to some subset of the *generated* `fx` lines. For example, by default a `mixin syl` line will add its content to every syllable in every generated line, no matter if it's from a `template line` or a `template syl`. By using modifiers like `layer`, `t_actor`, or `if` and `unless`, we can restrict what lines a mixin applies to. This is very useful both for cleaning up templates and removing duplicate code, and for conditional formatting.

*In our case, we can rewrite our template as follows:*
```
[layer 0] template syl: {\3c&HFFCCCC&\bord5\blur5}
[layer 1] template syl:
             mixin syl: {\an5\pos(!orgline.left+syl.center!,!orgline.middle!)
                         \t(!syl.start_time!,!syl.start_time+syl.duration/2!,\fscx130\fscy130)
                         \t(!syl.start_time+syl.duration/2!,!syl.end_time!,\fscx100\fscy100)}
```
*In fact, by splitting up the `mixin` into multiple parts, we can separate the different parts of this template by their purpose, and make it easier to read:*
```
[layer 0] template syl: {\3c&HFFCCCC&\bord5\blur5}
[layer 1] template syl:
             mixin syl: {\an5\pos(!orgline.left+syl.center!,!orgline.middle!)}
             mixin syl: {\t(!syl.start_time!,!syl.start_time+syl.duration/2!,\fscx130\fscy130)
                         \t(!syl.start_time+syl.duration/2!,!syl.end_time!,\fscx100\fscy100)}
```
Mixins (at least in this capacity) are a feature that's unique to The0x's templater. Read its documentation to find all of the possible modifiers for conditional execution.

### Code Lines
Now that we have all the basic functionality, we can talk about code lines. These just allow you to write Lua code which is run either once, or once for every line, syllable, word, or character.
Simple uses include defining constants or functions which compute values you need in your templates. They're pretty self-explanatory, and everything else important is explained in the documentation, so here's just a quick usage example to round off our example template:

*Right now, our template makes syllables grow larger over the first half of their duration, and grow smaller over the second half. If we want them to get larger faster, we can instead use something like `syl.start_time + 0.3 * syl.duration` in the `\t` tags instead. But now, we need to type the `0.3` twice. If we want to change the value later, we might forget one of the two and break the effect. So let's put this into a constant variable:*
```
             code once: growtime = 0.3
[layer 0] template syl: {\3c&HFFCCCC&\bord5\blur5}
[layer 1] template syl:
             mixin syl: {\an5\pos(!orgline.left+syl.center!,!orgline.middle!)}
             mixin syl: {\t(!syl.start_time!,!syl.start_time+growtime*syl.duration!,\fscx130\fscy130)
                         \t(!syl.start_time+growtime*syl.duration!,!syl.end_time!,\fscx100\fscy100)}
```
*While we're at it, let's also give some of the other constants names:*
```
             code once: growtime = 0.3; growscale = 130; glowcol = "&HFFCCCC";
[layer 0] template syl: {\3c!glowcol!\bord5\blur5}
[layer 1] template syl:
             mixin syl: {\an5\pos(!orgline.left+syl.center!,!orgline.middle!)}
             mixin syl: {\t(!syl.start_time!,!syl.start_time+growtime*syl.duration!,\fscx!growscale!\fscy!growscale!)
                         \t(!syl.start_time+growtime*syl.duration!,!syl.end_time!,\fscx100\fscy100)}
```
*We could also have a separate `code once` line for each constant, that's just a matter of taste.*

*To not need to type `!syl.start_time+growtime*syl.duration!` twice, we could also turn this into a variable. We can either do this with a `code syl`, or with the `set` function:*
```
             code once: growtime = 0.3; growscale = 130; glowcol = "&HFFCCCC";
[layer 0] template syl: {\3c!glowcol!\bord5\blur5}
[layer 1] template syl:
             code  syl: growt = syl.start_time+growtime*syl.duration
             mixin syl: {\an5\pos(!orgline.left+syl.center!,!orgline.middle!)}
             mixin syl: {\t(!syl.start_time!,!growt!,\fscx!growscale!\fscy!growscale!)
                         \t(!growt!,!syl.end_time!,\fscx100\fscy100)}
```
*or*
```
             code once: growtime = 0.3; growscale = 130; glowcol = "&HFFCCCC";
[layer 0] template syl: {\3c!glowcol!\bord5\blur5}
[layer 1] template syl:
             mixin syl: {\an5\pos(!orgline.left+syl.center!,!orgline.middle!)}
             mixin syl: {!set("growt", syl.start_time+growtime*syl.duration)!
                         \t(!syl.start_time!,!growt!,\fscx!growscale!\fscy!growscale!)
                         \t(!growt!,!syl.end_time!,\fscx100\fscy100)}
```
*The latter is probably easier to read, but the former can be cleaner if you need this variable in multiple templates or mixins.*

### Functions
The templaters provide various useful functions to further modify the current line.
They're all documented in the respective templater's documentations.
I just want to highlight the arguably most important one, called `retime`.

The `retime` function allows you to change the timing of the output line.
There isn't any magic involved here - the `line` table always contains the fields of the line currently being currently generated and can be changed by anyone, so nobody is stopping you from writing `!(function() line.start_time = 1234 end)()!` to set the generated `fx` line's start time to `1:23`.
The `retime` function just makes this much more convenient.

*For example, even with `template syl`, the generated line uses the timing of the original `kara` line by default.
If you instead want it to only appear when the syllable is being highlighted, you could add `!retime("syl")!` to your template to set its start and end time to the (absolute) start and end time of the syllable, respectively.
If you want a lead-in and lead-out of 150 and 300 milliseconds respecively, you can write `!retime("syl", -150, 300)!`.
If you instead want the syllable to disappear right when it should start getting highlighted, you can use `!retime("start2syl")!` instead.
Check the documentation for all the possible modes.*

### Loops
Finally, let's talk about loops.
You can use loops in templates to create more complex effects, like generating multiple lines from each application of any one `template` line.
You can also set the number of iterations during runtime.
With The0x's templater, you can even use multiple nested loops.
Using the `util.fbf` function, you can also easily set up a loop generating one output line for each frame.
Mixins also support loops in The0x's templater.

*For example, consider a line with the Effect `template line loop myloop 5` and the text*
```
{!relayer($maxloop_myloop - $loop_myloop)!
\an5\pos(!line.center - $loop_myloop - 1!,!line.middle + $loop_myloop - 1!)}
```
*to make something like a solid shadow. Or you could have a simple `template line` with the text*
```
{!util.fbf("line")!\an5
\pos(!line.center + 100 * math.sin(2 * (line.start_time - orgline.start_time) / 1000)!, !line.middle!)}
```
*to make the line smoothly move from side to side.*

For frame-by-frame effects not using positioning, you might also be interested in [KaraOK's wave functions](https://github.com/logarrhythmic/karaOK#wave-table---pushing-the-limits-of-ass), which are also available in The0x's templater.

### Other
Some things I haven't mentioned here are:
- Actors: Making effects behave differently (say, have different colors) depending on the Actor fields of the `kara` lines
- Inline FX: Using markers in the `kara` lines to have different effects for individual syllables
- The other very powerful conditional execution features of mixins, particularly `if` and `unless`
- Gradients using the utility library of The0x's templater, as well as the color library
- Furigana styling.

Again, read the documentation to see what is possible there.

## Comparison

The main differences between the stock or KaraOK templaters and The0x's one are:

- The `mixin` component and `word` class
- Lack of the `multi`/`furi` classes/modifiers
- Most dollar-variables need to instead be accessed through the `tenv` tables in inline lua
- Named and nestable loops
- Conditional execution (applicable to all components, not just `mixin`)

A more complete listing can be found below.

**Note that all links to code sections in the various templaters are permalinks and might be outdated.**

### Line markers
This is not a complete list. Check the documentation for all possible modifiers.
| Stock Templater | KaraOK | The0x's Templater |
| ------------------ | --------- | -------------------- |
| `karaoke` | `karaoke` | `kara` or `karaoke` |
| `fx` | `fx` | `fx`|
| `template pre-line` | `template line` | `template line` |
| `template syl` | `template syl` | `template syl` |
| `template char` or `template syl char` | `template char` | `template char` |
| not present | `template word` | `template word` |
| `template line` | `template lsyl` | `template line` + `mixin syl` |
| `template furi` | `template furi` | not present |
| not present | `template furichar` | not present |
| not present | `template lchar` | `template line` + `mixin char` |
| not present | `template lword` | `template line` + `mixin word` |
||||
| `all` (for template and code components) | `all` | `anystyle` |
| not present | `all style <stylename>` | `style <stylename>` |
||||
| not present | not present | general `mixin line` |
| not present | not present | general `mixin syl` |
| not present | not present | general `mixin char` |
| not present | not present | general `mixin word` |
||||
| `code once` | `code once` | `code once` |
| `code line` | `code line` | `code line` |
| `code syl` | `code syl` | `code syl` |
| not present | `code char` | `code char` |
| not present | `code word` | `code word` |
| `code furi` | `code furi` | not present |
||||
| `loop n` | `loop n` | `loop <loopname> n` |

### Tables in tenv
- Throughout all templaters, `orgline`, `syl`, `char`, and `word` (where present and applicable) refer to *original* objects, while `line` refers to the `fx` line that will be generated.
- In the stock templater and KaraOK, `syl` is the current syllable for `template syl` lines, but it's the current *character* for `template char` lines. In fact, `template char` just translates to a `template syl` with a per-char flag internally.
- The newer templaters add tables describing the current word or syllable for `template char` lines:
    - In the stock templater, `word` and `char` don't exist.
    - KaraOK adds tables `char`, `word`, and `syll` (the latter two are also accessible via `char.word` and `char.syll` in `template char`) referencing the current character, word, and syllable in `template char`.
    - In The0x's templater, however, `syl`, `char` and `word` exist only for `template syl`, `template char`, and `template word` respectively, and refer to the respective object. The current syllable or word can be accessed with `char.syl` and `char.word` respectively.

### Other members of tenv
Apart from the various line tables, utility functions, and loop variables, the templaters expose various different libraries and data in `tenv`, to be accessed directly from Lua eval fields:

- All three templaters provide the global environment `_G` of the calling templater to `tenv`, through which all other global variables can be accessed. For example, on the stock templater or KaraOK, Aegisub's `unicode` library can be accessed via `_G.unicode`.
- The stock templater exposed only the `string` and `math` libraries, as seen [here](https://github.com/Aegisub/Aegisub/blob/6f546951b4f004da16ce19ba638bf3eedefb9f31/automation/autoload/kara-templater.lua#L294-L300). The final member `meta` is the map of script metadata fields in the subtitle file's `[Script Info]` section, as [collected by karaskel](https://aegisub.org/docs/latest/automation/lua/modules/karaskel.lua/#karaskelcollect_head).
- In addition to this, KaraOK exposes the `math` libarary, as well as the subtitles object `subs` and various utility functions such as `ipairs` and `tostring`. The full list is [here](https://github.com/logarrhythmic/karaOK/blob/dc26cf017809b3d6ee2fabf01b454b09a6d1413a/autoload/ln.kara-templater-mod.lua#L336-L349).
- The0x's templater exposes all of the libraries that KaraOK does, together with Aegisub's libraries `unicode` and `karaskel`. When available, it also exposes KaraOK's library as `ln` (which is automatically initialized), and The0x's color library as `colorlib`.
The0x's templater does not expose all of the utility functions KaraOK does (`ipairs` being the most notable one), but exposes some other functions such as `require`. See [here](https://github.com/The0x539/Aegisub-Scripts/blob/3cd439b9d2bef2307a1c5725deb03206383b8ad3/src/0x.KaraTemplater.moon#L181) for a full list.
The templater also exposes the `subs`, `meta`, and `styles` objects, and its own utility library [here](https://github.com/The0x539/Aegisub-Scripts/blob/3cd439b9d2bef2307a1c5725deb03206383b8ad3/src/0x.KaraTemplater.moon#L240).

### Inline variables
- The following list is complete with respect to the inline variables in the stock templater and KaraOK. It's in the same order as the [documentation for inline variables in the stock templater](https://aegisub.org/docs/latest/automation/karaoke_templater/inline_variables/), so refer to that for the explanations of these variables.
    - The0x's templater contains additional inline variables for working with loops, as well as the `$env_*` variables - see the source code for these.
- The expressions in the following table are analogues of each other, but they might not be exactly equal. In particular, some values are rounded in the stock templater.
- Many of the expressions listed below for The0x's templater also work for the stock templater and KaraOK.


| Stock Templater & KaraOK | The0x's Templater |
| ------------------ | -------------------- |
| `$layer` | `orgline.layer` |
| `$lstart` | `orgline.start_time` |
| `$lend` | `orgline.end_time` |
| `$ldur` | `$ldur` or `orgline.duration` |
| `$lmid` | `orgline.start_time + 0.5 * orgline.duration` |
| `$style` | `orgline.style` |
| `$actor` | `orgline.actor` |
| `$margin_l` | `orgline.eff_margin_l` |
| `$margin_r` | `orgline.eff_margin_r` |
| `$margin_t` | `orgline.eff_margin_t` |
| `$margin_b` | `orgline.eff_margin_b` |
| `$margin_v` | `orgline.eff_margin_v` |
| `$syln` | `#orgline.syls` |
| `$li` | `$li` or `orgline.li` |
| `$lleft` | `orgline.left` |
| `$lcenter` | `orgline.center` |
| `$lright` | `orgline.right` |
| `$ltop` | `orgline.top` |
| `$lmiddle` | `orgline.middle` |
| `$lbottom` | `orgline.bottom` |
| `$lx` | not present |
| `$ly` | not present |
| `$lwidth` | `orgline.width` |
| `$lheight` | `orgline.height` |
|||
| `$sstart` | `$sylstart` or `syl.start_time` |
| `$send` | `$sylend` or `syl.end_time` |
| `$smid` | `syl.start + 0.5 * syl.duration` |
| `$sdur` | `$syldur` or `syl.duration` |
| `$si` | `$si` or `(syl or char or orgline).si` |
| `$ci` (KaraOK only) | `$ci` or `(char or syl or word or orgline).ci` |
| not present | `$wi` or `(word or char or orgline).wi` |
| not present | `$cxf` |
| not present | `$sxf` |
| not present | `$wxf` |
| `$skdur` | `$kdur` or `syl.kdur` or `syl.duration / 2` |
| `$sleft` | `orgline.left + syl.left` |
| `$scenter` | `orgline.left + syl.center` |
| `$sright` | `orgline.left + syl.right` |
| `$sbottom` | same as `$lbottom` |
| `$smiddle` | same as `$lmiddle` |
| `$stop` | same as `$ltop` |
| `$sx` | not present |
| `$sy` | not present |
| `$swidth` | `syl.width` |
| `$sheight` | `syl.height` |
|||
| Automatic variants| not present |