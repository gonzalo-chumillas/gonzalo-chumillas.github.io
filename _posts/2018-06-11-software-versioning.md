---
title: Versionado de software
---

El versionado permite etiquetar cada fase de desarrollo del software, aportando información relevante al usuario. Y es curioso que a día de hoy no exista un consenso universal y cada empresa, organización o profesional adopten estrategias distintas a la hora de etiquetar cada fase del desarrollo.

Personalmente prefiero usar un sistema simple, por dos motivos: **un sistema simple es fácil de compartir y de usar.**

Y en concreto me gusta usar una versión simplificada del sistema propuesto por el grupo [PEAR](https://pear.php.net/manual/en/guide.users.concepts.version.php).

Veamos cómo funciona.

**Qué significan los números x.y.z?**

El primer dígito se refiere a la versión liberada y está estrechamente relacionada con la "compatibilidad". Por ejemplo, la versión 1.0.0 de un software no tendría por qué ser compatible con la versión 2.0.0. Usar una versión posterior podría significar tener que modificar nuestro código para adaptarlo a la nueva API.

El segundo dígito se refiere a las "funcionalidades" o "features". Por ejemplo, la versión 1.**2**.0 contiene más "funcionalidades" que la versión 1.**1**.0 del mismo software, aunque ambas sigan siendo compatibles.

Y finalmente, el tercer dígito se refiere a las incidencias solucionadas. Siguiendo con los ejemplos anteriores, la versión 1.1.**7** contendría más incidencias solucionas que la versión 1.1.**0** del mismo software. Y en cualquier caso, ambas versiones seguirían siendo compatibles.

**Pre-release 0.1.0**

Según la convención del grupo PEAR, la primera etiqueta debería ser la 0.1.0. Y el primer dígito indicaría que se trata de una versión en fase de prueba. Es decir, no apta para su uso en producción.

**First release 1.0.0**

La primera versión estable debería empezar por el 1.0.0 en adelante.
