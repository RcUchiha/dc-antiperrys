---
layout: default
title: Función AutoTags
parent: Karaokes
nav_order: 3
---

# Introducción a la función personalizada AutoTags y sus variantes comúnmente utilizadas

La función AutoTags se utiliza para iterar entre dos (grupos de) tags de efectos dentro de un período de tiempo continuo, como cambiar el valor de blur hacia adelante y hacia atrás, creando un efecto de parpadeo. Es muy común en muchos scripts. Para usar esta función, primero debes declararla en la línea de código.

```
function AutoTags(Intervalo,Dato1,Dato2)            local RESULTADO=""     local SUERTE = 0     local CONTADOR = 0     local ARREGLO = 0                           local count = math.ceil(line.duration/Intervalo)                         ARREGLO = {Dato1,Dato2}                            for i = 1, count do               CONTADOR = i                            if Dato1 and Dato2 then                     if  CONTADOR%2 ==0 then                                    SUERTE = ARREGLO[1]                                            else                                    SUERTE = ARREGLO[2]                            end                    end                     RESULTADO = RESULTADO .."\\t(" ..(i-1)*Intervalo.. "," ..i*Intervalo.. ",\\" ..SUERTE..")"..""                      end                 return RESULTADO                                     end
```

