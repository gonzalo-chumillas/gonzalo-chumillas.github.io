---
title: JWT (JSON Web Token)
---

En esencia JWT (JSON Web Token) es un sistema para generar "tokens de acceso" sin necesidad de almacenarlos en el propio servidor, con las ventajas que ello conlleva. Cuando el usuario accede por primera vez al sistema, normalmente indicando su usuario y contraseña, el sistema le otorga un "token" debidamente firmado. El usuario deberá guardar ese "token" para su posterior uso.

A efectos prácticos un "token" no se diferencia mucho de la pulserita que nos entregan al acceder a una discoteca. La primera vez mostramos nuestro carnet de identidad, para verificar que somos mayores de edad, y posteriormente usamos la pulserita para entrar y salir del establecimiento ágilmente. ¡Pero ojo! la pulserita (token) deberá estar debidamente sellada, no vaya a ser que se cuele algún tramposo.

Es decir, un "token" no es más que una pulserita que nos acredita para acceder a un sistema y beneficiarnos de sus servicios. **Nada nuevo bajo el sol.**

Pero como suele ocurrir, lo sencillo suele complicarse "ad infinitum". Y por ésa razón, en este breve artículo vamos a centrarnos en la versión más simple, y posiblemente más conocida, del sistema de autenticación basado en JWT. Y para ello usaremos como ejemplo el siguiente proyecto:

[github.com/gchumillas/jwtserver](https://github.com/gchumillas/jwtserver)

El proyecto anterior fue desarrollado en el lenguaje GO y tiene dos end-points:

```
[POST] mywebsite/sign/in (identificarnos en el sistema)
[GET]  mywebsite/home    (contenido de la página inicial)
```

Y en particular el script token.go posee dos funciones: New, para generar tokens y entregarlos al usuario, y Parse, para escanear tokens y verificar su autenticidad.

Los tokens se generan y escanean usando nuestra firma de seguridad (privateKey), que es secreta y nadie deberá conocer:

[github.com/gchumillas/../example.env#L12](https://github.com/gchumillas/jwtserver/blob/master/example.env#L12)

Y nada más. Básicamente necesitamos una clave privada (privateKey) y un par de funciones para generar y escanear tokens.

Tan simple! Tan sencillo! Tan bonito!
