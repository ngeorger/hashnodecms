---
title: "Kubernetes v1.30: Un adelanto a las mejoras y cambios importantes que debes saber"
datePublished: Mon Mar 18 2024 22:35:04 GMT+0000 (Coordinated Universal Time)
cuid: cluttxiqk000908ic0pgt1xbi
slug: kubernetes-v130-un-adelanto-a-las-mejoras-y-cambios-importantes-que-debes-saber
canonical: https://sredevops.org/es/kubernetes-v1-30-un-adelanto-a-las-mejoras-y-cambios-importantes-que-debes-saber/
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1712754706348/41064dbd-e852-48c0-96fc-8717749c8784.png
tags: news, linux, opensource, kubernetes, espanol

---

> **_Un adelanto de Kubernetes v1.30_**

Una mirada rápida: cambios interesantes en Kubernetes v1.30
-----------------------------------------------------------

![Kubernetes v1.30: Un adelanto a las mejoras y cambios importantes que debes saber](https://cdn.hashnode.com/res/hashnode/image/upload/v1712754699782/41163b9a-6e3f-4178-8f4c-c2c9abe6c02b.png)

Es un nuevo año y una nueva versión de Kubernetes está en camino. Estamos a mitad del ciclo de lanzamiento y tenemos una gran cantidad de mejoras interesantes y emocionantes que llegarán en la v1.30. Desde características completamente nuevas en fase alfa, hasta características establecidas que se gradúan a estables, y mejoras muy esperadas, ¡este lanzamiento tiene algo para que todos los entusiastas de Kubernetes, Linux y el cloud computing presten atención!

Para mantenerte al tanto hasta el lanzamiento oficial, ¡aquí tienes un adelanto de las mejoras que más nos entusiasman en este ciclo!

Cambios importantes para Kubernetes v1.30
-----------------------------------------

### Parámetros estructurados para la asignación dinámica de recursos (KEP-4381)

[

DRA: structured parameters · Issue #4381 · kubernetes/enhancements

Enhancement Description One-line enhancement description (can be used as a release note): The original dynamic resource allocation (DRA) uses claim and class parameters that are opaque to Kubernete…

![Kubernetes v1.30: Un adelanto a las mejoras y cambios importantes que debes saber](https://github.githubassets.com/assets/pinned-octocat-093da3e6fa40.svg)GitHubkubernetes

![Kubernetes v1.30: Un adelanto a las mejoras y cambios importantes que debes saber](https://cdn.hashnode.com/res/hashnode/image/upload/v1712754701231/1c96934a-f312-40ff-96b6-9c8891fe1650.png)

](https://github.com/kubernetes/enhancements/issues/4381?ref=sredevops.org)

La asignación dinámica de recursos se agregó a Kubernetes como una característica alfa en la v1.26. Define una alternativa a la API tradicional de complementos de dispositivos para solicitar acceso a recursos de terceros. Por diseño, la asignación dinámica de recursos utiliza parámetros para recursos que son completamente opacos para el núcleo de Kubernetes. Este enfoque plantea un problema para el Cluster Autoscaler (CA) o cualquier controlador de nivel superior que necesite tomar decisiones para un grupo de pods. No puede simular el efecto de asignar o desasignar reclamaciones a lo largo del tiempo. Solo los controladores DRA de terceros tienen la información disponible para hacer esto.

Los parámetros estructurados para la asignación dinámica de recursos son una extensión de la implementación original que aborda este problema al construir un marco para permitir que estos parámetros de reclamación sean menos opacos. En lugar de manejar la semántica de todos los parámetros de reclamación ellos mismos, los controladores podrían administrar los recursos y describirlos utilizando un "modelo estructurado" específico predefinido por Kubernetes. Esto permitiría a los componentes conscientes de este "modelo estructurado" tomar decisiones sobre estos recursos sin depender de algún controlador de terceros. Por ejemplo, el scheduler podría asignar reclamaciones rápidamente sin necesidad de una comunicación de ida y vuelta con los controladores de asignación dinámica de recursos. El trabajo realizado para este lanzamiento se centra en definir el marco necesario para habilitar diferentes "modelos estructurados" y en implementar el modelo de "recursos con nombre". Este modelo permite enumerar instancias de recursos individuales y, en comparación con la API tradicional de complementos de dispositivos, agrega la capacidad de seleccionar esas instancias individualmente a través de atributos.

### Soporte de intercambio de memoria (swap) en nodos (KEP-2400)

[

Node memory swap support · Issue #2400 · kubernetes/enhancements

Enhancement Description One-line enhancement description (can be used as a release note): Kubernetes nodes support swap memory. Kubernetes Enhancement Proposal: https://github.com/kubernetes/enhanc…

![Kubernetes v1.30: Un adelanto a las mejoras y cambios importantes que debes saber](https://github.githubassets.com/assets/pinned-octocat-093da3e6fa40.svg)GitHubkubernetes

![Kubernetes v1.30: Un adelanto a las mejoras y cambios importantes que debes saber](https://cdn.hashnode.com/res/hashnode/image/upload/v1712754702906/9c0b8427-c976-4cb4-936f-bab1ca10ff83.png)

](https://github.com/kubernetes/enhancements/issues/2400?ref=sredevops.org)

En Kubernetes v1.30, el soporte de intercambio de memoria (swap) en nodos Linux obtiene un gran cambio en la forma en que funciona, con un fuerte énfasis en mejorar la estabilidad del sistema. En versiones anteriores de Kubernetes, la puerta de enlace de características NodeSwap estaba deshabilitada de forma predeterminada y, cuando se habilitaba, utilizaba el comportamiento UnlimitedSwap como comportamiento predeterminado. Para lograr una mejor estabilidad, el comportamiento UnlimitedSwap (que podría comprometer la estabilidad del nodo) se eliminará en la v1.30.

El soporte actualizado y aún en beta para el intercambio de memoria (swap) en nodos Linux estará disponible de forma predeterminada. Sin embargo, el comportamiento predeterminado será ejecutar el nodo configurado en modo NoSwap (no Unlimited

Original: _Martes, 12 de marzo de 2024_

**Autores:** Amit Dsouza, Frederick Kautz, Kristin Martin, Abigail McCarthy, Natali Vlatko

Fuente:

[

A Peek at Kubernetes v1.30

Authors: Amit Dsouza, Frederick Kautz, Kristin Martin, Abigail McCarthy, Natali Vlatko A quick look: exciting changes in Kubernetes v1.30 It’s a new year and a new Kubernetes release. We’re halfway through the release cycle and have quite a few interesting and exciting enhancements coming in v1.30. From brand new features in alpha, to established features graduating to stable, to long-awaited improvements, this release has something for everyone to pay attention to!

![Kubernetes v1.30: Un adelanto a las mejoras y cambios importantes que debes saber](https://cdn.hashnode.com/res/hashnode/image/upload/v1712754704077/146928a9-8be6-4054-abd3-ebafbbdb2524.png)Kubernetes

![Kubernetes v1.30: Un adelanto a las mejoras y cambios importantes que debes saber](https://cdn.hashnode.com/res/hashnode/image/upload/v1712754705208/70578260-77f1-442e-8667-709e137941bd.png)

](https://kubernetes.io/blog/2024/03/12/kubernetes-1-30-upcoming-changes/?ref=sredevops.org)