---
title: "¿Tu app en Kubernetes falla al inicio? La propiedad minReadySeconds puede ayudarte"
datePublished: Mon Jul 01 2024 05:48:48 GMT+0000 (Coordinated Universal Time)
cuid: cly2k7owc000h0al7baro90ny
slug: tu-app-en-kubernetes-falla-al-inicio-la-propiedad-minreadyseconds-puede-ayudarte
canonical: https://sredevops.org/es/tu-app-en-kubernetes-falla-al-inicio-la-propiedad-minreadyseconds-puede-ayudarte/
cover: https://cdn.hashnode.com/res/hashnode/imageupload/v1719812927956/8c079dc0-b54c-40bb-a476-30de2edab513.png
tags: cloud, development, kubernetes, devops, containers, sre, espanol, microservicios

---

Resumen
-------

![¿Tu app en Kubernetes falla al inicio? La propiedad minReadySeconds puede ayudarte](https://cdn.hashnode.com/res/hashnode/imageupload/v1719812926810/7ccf84cb-f34f-4ba8-983d-be9ffbde8909.png)

Revisamos la propiedad `minReadySeconds` en Kubernetes y cómo puede ser tu arma secreta para implementar aplicaciones sólidas y listas para producción. Explicaremos cómo establecer un valor apropiado para `minReadySeconds` , que proporciona un período de gracia inicial para que las aplicaciones se levanten y estén listas para manejar el tráfico, garantizando una experiencia fluida para los usuarios.

Luchando contra la sobrecarga inicial: la necesidad de un período de gracia
---------------------------------------------------------------------------

Imagina que acabas de implementar una nueva versión de tu aplicación web en tu clúster de Kubernetes. Todo parece bien —los pods están iniciándose y Kubernetes está haciendo su magia. Pero espera... antes de que tu aplicación tenga la oportunidad de respirar, Kubernetes inmediatamente comienza a enrutar el tráfico hacia ella, pero el problema es que tu aplicación depende de una conexión con la base de datos y necesita unos segundos adicionales para cargar las configuraciones. Esta situación, amigos míos, es una receta para el desastre, potencialmente llevando a errores, tiempos de espera y mucha frustración para tus usuarios.

Piensa en `minReadySeconds` como esa taza de café bien merecida después de un largo descanso. Le da a tu aplicación el tiempo que necesita para ponerse en orden antes de recibir tráfico real.

### Ejemplo deployment usando minReadySeconds

    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: tests-minready
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: tests-minready
      template:
        metadata:
          labels:
            app: tests-minready
        spec:
          containers:
           - name: tests-minready
             image: nginx:stable-alpine-slim
             ports:
              - containerPort: 80
          ### Valor en segundos
          minReadySeconds: 10
    

minReadySeconds al rescate: tiempo de espera inicial como una inversión en estabilidad
--------------------------------------------------------------------------------------

Esta es la forma en que `minReadySeconds` entra en juego y salva el día; Al establecer esta propiedad en tu manifiesto de despliegue, **esencialmente le estás diciendo a Kubernetes, "¡Espera un minuto! No envíes tráfico hacia mis pods hasta que hayan tenido al menos estos segundos para ponerse en orden".**

Digamos que estableces `minReadySeconds: 10` Esto significa que Kubernetes esperará pacientemente durante 10 segundos después de que los pods informen que sus contenedores están listos antes de considerar que un pod está realmente disponible para recibir tráfico. Este período de gracia permite a tu aplicación:

*   **Establecer conexiones con la base de datos:** ¡No más intentos frenéticos de conexión mientras el tráfico ya está golpeando!
*   **Cargar configuraciones:** Asegúrate de que todas las configuraciones estén cargadas antes del primer request.
*   **Ejecutar tareas de inicio:** Completa cualquier rutina de inicialización sin la presión del tráfico entrante.

Recompensas por ir más allá: estabilidad, confiabilidad, usuarios y devs felices
--------------------------------------------------------------------------------

Al incorporar `minReadySeconds` en tu estrategia de implementación, no estás agregando solo unos segundos a tu tiempo de inicio, estás invirtiendo en una aplicación más estable y confiable. Aquí hay algunas formas en que esto puede ayudar:

*   **Menos tiempo de inactividad:** Al prevenir el enrutamiento prematuro del tráfico, `minReadySeconds` minimiza el riesgo de errores y accidentes durante la fase de inicio crítica, lo que conduce a menos tiempo de inactividad y una experiencia fluida para los usuarios.
*   **Mejora de la experiencia del usuario:** ¡Nadie disfruta de las pantallas de error! `minReadySeconds` garantiza que tu aplicación esté lista para proporcionar una experiencia perfecta desde el momento en que los usuarios llegan.
*   **Mejor confiabilidad:** Una aplicación bien inicializada es una aplicación feliz  (y un desarrollador feliz también). `minReadySeconds` agrega otra capa de confiabilidad a tus implementaciones, brindándote tranquilidad al saber que tus aplicaciones están realmente preparadas para la acción.

Más allá de lo básico: qué necesitas para un rendimiento óptimo
---------------------------------------------------------------

El valor ideal para `minReadySeconds` depende de los requisitos de inicio específicos de tu aplicación. Analiza el proceso de inicialización de tu aplicación para determinar un valor apropiado que proporcione suficiente tiempo de inicialización sin retrasar innecesariamente las implementaciones.

Recuerda, Kubernetes viene con una caja de herramientas llena de funciones potentes, y `minReadySeconds` es solo una de ellas. Investiga otras estrategias de implementación y mejores prácticas para mejorar aún más la estabilidad y resistencia de tus aplicaciones.

Disfruta kuberneteando con tus despliegues!

[

Deployments

A Deployment manages a set of Pods to run an application workload, usually one that doesn’t maintain state.

![¿Tu app en Kubernetes falla al inicio? La propiedad minReadySeconds puede ayudarte](https://cdn.hashnode.com/res/hashnode/imageupload/v1719812927433/b3d568b4-3439-4779-925c-a0ef0e3c64f9.png)Kubernetes

![¿Tu app en Kubernetes falla al inicio? La propiedad minReadySeconds puede ayudarte](https://cdn.hashnode.com/res/hashnode/imageupload/v1719812927724/02c2bcfd-98f9-4354-b13c-91caa51c6450.png)

](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/?ref=sredevops.org#min-ready-seconds)