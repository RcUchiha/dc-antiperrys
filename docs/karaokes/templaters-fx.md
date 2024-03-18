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