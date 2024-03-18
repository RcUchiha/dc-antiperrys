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

## Creadores de Templates Existentes

Al momento de escribir esto (2022), hay tres principales creadores de templates de karaoke:

1. El creador de templates estándar que viene con Aegisub, documentado [aquí](https://aegisub.org/docs/latest/automation/karaoke_templater/) como parte de la documentación de Aegisub. La documentación es muy detallada, pero bastante técnica.
1. [KaraOK](https://github.com/logarrhythmic/karaOK), una versión modificada del creador de templates estándar junto con una biblioteca de utilidades. También se incluye por defecto con algunas versiones más nuevas de Aegisub, como AegisubDC.
1. [El Creador de Templates de The0x539](https://github.com/The0x539/Aegisub-Scripts/blob/trunk/src/0x.KaraTemplater.moon), documentado [aquí](https://github.com/The0x539/Aegisub-Scripts/blob/trunk/doc/0x.KaraTemplater.md) en su repositorio. Este es un creador de templates completamente reescrito con una lógica de ejecución más potente (mixins, condicionales más fuertes, bucles anidados, etc) y variables organizadas de manera más sensata.

Excepto por furigana y otros usos de furi o templates multi, realmente no hay nada que el creador de templates estándar o KaraOK puedan hacer que no pueda ser fácilmente portado al creador de templates de The0x. Siempre que esté instalado, el creador de templates de The0x también contiene soporte directo para la biblioteca de utilidades de KaraOK. Es por eso que será el creador de templates predeterminado para esta guía, junto con notas sobre cómo los conceptos equivalentes funcionan en otros creadores de templates. Incluso si nunca escribirás templates en otros creadores de templates, estos últimos aún pueden ser útiles para entender las templates existentes.