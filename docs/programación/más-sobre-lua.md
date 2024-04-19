---
layout: default
title: Más sobre Lua
parent: Programación
nav_order: 1
---

# Más sobre la programación en Lua

Sigo escuchando a la gente decir «Necesito aprender algo de Lua» o «Necesito aprender a escribir scripts de automatización», y no muchos parecen haberse adentrado realmente en ello. Esto debería ayudarte a empezar. Deberías leer la guía de Lyger primero, porque no voy a explicar las mismas cosas de nuevo, pero quiero proporcionar algunos consejos más prácticos. En lugar de explicar Lua en sí mismo, explicaré más sobre los scripts para Aegisub específicamente.

Aprender Lua no es gran cosa. Puedes aprender todo lo que necesitas de Lua en una hora. Principalmente son solo **if**/**then**/**end**, el ciclo **for**, **gsub**, y algunas otras cosas.

Una gran parte de lo que necesitas son expresiones regulares, o más bien, la coincidencia de patrones simplificada de Lua. Eso, nuevamente, es algo que puedes aprender en una hora.

Lo que más quiero hablar es cómo trabajar con el objeto Subtitles, que no es realmente un asunto de Lua, sino más bien de Aegisub y el formato ASS. Esto se explica en el manual de Aegisub, pero como eso puede ser confuso para los principiantes, proporcionaré algunos ejemplos prácticos específicos. El objetivo es explicar cómo escribir un script de automatización básica en los términos más simples posibles. Una vez que comprendas cómo funciona un script que agrega desenfoque, agregar funciones más complejas será fácil porque eso es solo matemáticas.

Aquí está lo básico:
```lua
script_name="prueba" 
script_description="probando cosas" 
script_author="alguien" 
script_version="1"

function test(subs, sel, act) 
	-- aquí va el código 
end

aegisub.register_macro(script_name, script_description, test)
```

La última línea coloca una entrada en tu menú de automatización. Tiene 3 partes. Dos de ellas se definen al principio: script_name y script_description. El nombre aparecerá en el menú y como una entrada de deshacer cuando ejecutes el script. Puedes ver la descripción en el Administrador de Automatización. La tercera parte significa que al ejecutar este script se ejecutará una función llamada «test».

**script_author** y **script_version** no son realmente importantes, pero estoy seguro de que captas la idea.

Veamos la **función test(subs, sel, act)**. Probablemente escribí al menos 20 scripts antes de entender realmente qué es esto. Dado que esta función es referenciada por register_macro, es la función principal del script y, como tal, se le da por defecto el objeto Subtitles para trabajar. Las 3 partes — subtítulos, líneas seleccionadas y línea activa — te dan 3 cosas con las que puedes trabajar.

Puedes nombrarlas como quieras. Solo tienes que seguir con la nomenclatura. Tiendo a mantener todo corto, aunque estoy seguro de que no soy el único que usa subs/sel/act. Probablemente sea mejor usar estos incluso solo por el hecho de que otros también lo hacen, lo que facilita entender los scripts de los demás.

**subs** es todo el objeto de subtítulos. Siempre tienes que usar esto. En términos simples, es como una tabla de todas las líneas en el script ASS, incluyendo encabezados, estilos, etc.

**sel** son las líneas seleccionadas, y si quieres que tu función se aplique a todas las líneas, no tienes que usar esto. Puedes tener **function test(subs)**.

**act** es la línea activa, y probablemente no la necesitarás muy a menudo. Puedes usarla para funciones que se supone deben ejecutarse en solo una línea o leer alguna información de la línea activa. Si seleccionas una línea y usas **sel**, es prácticamente lo mismo que usar act.

La parte «aquí va el código» es donde se escribirá la función real.

Aquí tienes un ejemplo de una función simple que se ejecuta en todo el script:
```lua
function test(subs)
    for i=1,#subs do
        if subs[i].class=="dialogue" then
            line=subs[i]
            text=subs[i].text
	    
	    line.effect="test"
	    
	    line.text=text
            subs[i]=line
	end
    end
    aegisub.set_undo_point(script_name)
end
```

La parte verde es lo que normalmente tendrás en cada script que se ejecuta en todas las líneas. La parte morada es la función específica real.

**#subs** es cuántas líneas hay en **subs** (incluyendo encabezados y todo). Si el archivo ASS tiene 200 líneas, el ciclo **for** se ejecutará 200 veces. Solo quieres aplicar esto a las líneas de diálogo, no a los estilos o encabezados, así que debes especificar esta condición: **if subs[i].class=="dialogue"**.

Entonces, el iterador i va de 1 a 200, así que cuando sea, digamos, 25, **subs[i]** es **subs[25]**, o la 25ª línea en el archivo ASS. **line=subs[i]** significa que creas el elemento **line** y colocas **subs[i]** en él. Ten en cuenta que un solo = no significa «igual». Podrías leerlo como «línea ahora es subs[25]» (cuando i es 25). Luego trabajas con **line**, y para que tenga algún uso, debes volver a colocar la **line** en **subs[i]** al final. **line** es algo que creaste, **subs[i]** es la línea real en los subtítulos, así que necesitas **subs[i]=line** al final.