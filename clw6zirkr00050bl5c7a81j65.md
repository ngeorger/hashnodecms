---
title: "Kubernetes Gateway API v1.1: Service mesh, GRPCRoute y mucho más"
datePublished: Tue May 14 2024 22:48:59 GMT+0000 (Coordinated Universal Time)
cuid: clw6zirkr00050bl5c7a81j65
slug: kubernetes-gateway-api-v11-service-mesh-grpcroute-y-mucho-mas
canonical: https://sredevops.org/es/kubernetes-gateway-api-v1-1-service-mesh-grpcroute-y-mucho-mas/
cover: https://cdn.hashnode.com/res/hashnode/imageupload/v1715726938816/bdfa939e-51b9-4970-850e-227a0a1124ef.png
tags: news, opensource, kubernetes, sre, espanol

---

![Kubernetes Gateway API v1.1: Service mesh, GRPCRoute y mucho más](https://cdn.hashnode.com/res/hashnode/imageupload/v1715726932147/043e8d50-6c04-40a8-9c74-58a10e1ab6c0.png)

Después del lanzamiento GA de Gateway API el pasado octubre, Kubernetes SIG Network anuncia el lanzamiento de la versión v1.1 de [Gateway API](https://gateway-api.sigs.k8s.io/?ref=sredevops.org). En este lanzamiento, varias características pasan a formar parte del _Canal Estándar_ (GA), incluyendo el soporte para service mesh y GRPCRoute. También introducen algunas características nuevas y experimentales, como persistencia de sesión y verificación de certificados de cliente.

Novedades
---------

### Paso al Canal Estándar

Este lanzamiento incluye el paso al Canal Estándar de cuatro características muy esperadas. Esto significa que ya no son conceptos experimentales; la inclusión en el Canal Estándar denota un alto nivel de confianza en la API y proporciona garantías de compatibilidad. Por supuesto, como con cualquier otra API de Kubernetes, las características del Canal Estándar pueden seguir evolucionando con adiciones retrocompatibles y ciertamente vendrán más refinamientos y mejoras a estas nuevas características en el futuro. Para más información sobre cómo funciona todo esto, consulta la [Política de Versiones de Gateway API](https://gateway-api.sigs.k8s.io/concepts/versioning/?ref=sredevops.org).

[

Versioning - Kubernetes Gateway API

![Kubernetes Gateway API v1.1: Service mesh, GRPCRoute y mucho más](https://cdn.hashnode.com/res/hashnode/imageupload/v1715726932948/ef070c33-07cd-481d-81bb-57c6c43568bc.png)logo

![Kubernetes Gateway API v1.1: Service mesh, GRPCRoute y mucho más](https://cdn.hashnode.com/res/hashnode/imageupload/v1715726933350/a2c8bee6-608b-4b68-b66f-4083bb84ccc2.svg)

](https://gateway-api.sigs.k8s.io/concepts/versioning/?ref=sredevops.org)

#### [Soporte para Service Mesh](https://gateway-api.sigs.k8s.io/mesh/?ref=sredevops.org)

El soporte para service mesh en Gateway API permite a los usuarios utilizar la misma API para gestionar el tráfico de entrada y el _tráfico de malla,_ reutilizando las mismas interfaces de política y enrutamiento. En Gateway API v1.1, las rutas (como HTTPRoute) ahora pueden tener un Servicio como `parentRef`, para controlar cómo se comporta el tráfico a servicios específicos. Para más información, lee la [documentación de service mesh de Gateway API](https://gateway-api.sigs.k8s.io/mesh/?ref=sredevops.org) o consulta la [lista de implementaciones de Gateway API](https://gateway-api.sigs.k8s.io/implementations/?ref=sredevops.org#service-mesh-implementation-status).

[

Traefik v3.0 ya está disponible e incluye soporte para Kubernetes Gateway API, WebAssembly y más. Aquí puedes ver cómo actualizar a v3.0

Este año se cumple el noveno aniversario de Traefik y hoy se convirtió en uno de los gateways modernos más utilizados, con más de 3 mil millones de descargas y más de 750 colaboradores. Está en el Top 15 de DockerHub y tiene 47.000 estrellas en GitHub. Aquí en

![Kubernetes Gateway API v1.1: Service mesh, GRPCRoute y mucho más](https://cdn.hashnode.com/res/hashnode/imageupload/v1715726933560/8bf08a57-b1a5-49c2-bcd5-44c2d82e9f22.ico)SREDevOps.org - SRE, DevOps, Cloud, LinuxNicolás Georger

![Kubernetes Gateway API v1.1: Service mesh, GRPCRoute y mucho más](https://cdn.hashnode.com/res/hashnode/imageupload/v1715726934172/d7faad30-fc57-4cd1-914f-205643e785e0.png)

](https://sredevops.org/es/traefik-v3-ya-esta-disponible-e-incluye-soporte-para-kubernetes-gateway-api-webassembly-y-mas-aqui-puedes-ver-como-actualizar-a-v3/)

Como ejemplo, se podría hacer un despliegue Canary de una carga de trabajo (workload) con un HTTPRoute de la siguiente manera:

    apiVersion: gateway.networking.k8s.io/v1
    kind: HTTPRoute
    metadata:
      name: color-canary
      namespace: faces
    spec:
      parentRefs:
        - name: color
          kind: Service
          group: ""
          port: 80
      rules:
      - backendRefs:
        - name: color
          port: 80
          weight: 50
        - name: color2
          port: 80
          weight: 50

Esto dividiría el tráfico enviado al Servicio `color` en el namespace `faces` 50/50 entre el Servicio `color` original y el Servicio `color2`, utilizando una configuración portátil que es fácil de mover de una malla a otra.

### [GRPCRoute](https://gateway-api.sigs.k8s.io/guides/grpc-routing/?ref=sredevops.org)

Si ya estás utilizando la versión experimental de GRPCRoute, te recomendamos esperar antes de actualizar a la versión del canal estándar de GRPCRoute hasta que los controladores que estés utilizando hayan sido actualizados para soportar la versión v1 de GRPCRoute. Hasta entonces, es seguro actualizar a la versión experimental de GRPCRoute en v1.1 que incluye las versiones de API v1alpha2 y v1.

### [Puerto en ParentReference](https://gateway-api.sigs.k8s.io/reference/spec/?ref=sredevops.org#gateway.networking.k8s.io%2fv1.ParentReference)

Se agregó el campo `port` a ParentReference, lo que permite adjuntar recursos a los _**GatewayListeners**_, Servicios u otros recursos principales (dependiendo de la implementación). Vincular a un puerto también permite adjuntar a múltiples _Listeners_ a la vez.

Por ejemplo, puedes adjuntar un `HTTPRoute` a uno o más `Listeners` específicos de un Gateway según el campo `port` del Listener, en lugar del campo `name` del Listener.

Para más información, consulta [Attach to Gateways.](https://gateway-api.sigs.k8s.io/api-types/httproute/?ref=sredevops.org#attaching-to-gateways)

[

HTTPRoute - Kubernetes Gateway API

![Kubernetes Gateway API v1.1: Service mesh, GRPCRoute y mucho más](https://cdn.hashnode.com/res/hashnode/imageupload/v1715726934676/9c390414-bbcd-4d9d-abca-72e973b14b55.png)logo

![Kubernetes Gateway API v1.1: Service mesh, GRPCRoute y mucho más](https://cdn.hashnode.com/res/hashnode/imageupload/v1715726934992/38fa2d32-7c1f-4415-af29-93fb833a1231.svg)

](https://gateway-api.sigs.k8s.io/api-types/httproute/?ref=sredevops.org#attaching-to-gateways)

### [Perfiles y Reportes _Conformance_](https://gateway-api.sigs.k8s.io/concepts/conformance/?ref=sredevops.org#conformance-profiles)

La API Conformance se ha ampliado con el campo `mode` (destinado a especificar el modo de la implementación) y el `gatewayAPIChannel` (estándar o experimental). Tanto `gatewayAPIVersion` como `gatewayAPIChannel` ahora se rellenan automáticamente por la API Machinery, junto con una breve descripción del resultado de las pruebas. Los Reportes se han reorganizado de una manera más estructurada, y las implementaciones ahora pueden agregar información sobre cómo se han ejecutado las pruebas y proporcionar pasos de reproducción.

Nuevas características en el Canal Experimental
-----------------------------------------------

### [Verificación de Certificados de Cliente en Gateway](https://gateway-api.sigs.k8s.io/geps/gep-91/?ref=sredevops.org)

Los Gateways ahora pueden configurar la verificación de certificados de cliente para cada Gateway`Listener,` mediante la introducción de un nuevo campo `frontendValidation` dentro de `tls`. Este campo admite la configuración de una lista de _Certificados CA_ **(`Certificate Authority`)** que pueden utilizarse como un punto de confianza para validar los certificados presentados por el cliente.

✅ ****Ejemplo de validación de certificado de cliente en API Gateway:****  
Cómo un CACertificate almacenado en el ConfigMap `foo-example-com-ca-cert` se puede utilizar para validar los certificados presentados por los clientes que se conectan al Listener del Gateway `foo-https`.

    apiVersion: gateway.networking.k8s.io/v1
    kind: Gateway
    metadata:
      name: client-validation-basic
    spec:
      gatewayClassName: acme-lb
      listeners:
        name: foo-https
        protocol: HTTPS
        port: 443
        hostname: foo.example.com
      tls:
        certificateRefs:
          kind: Secret
          group: ""
          name: foo-example-com-cert
        frontendValidation:
          caCertificateRefs:
            kind: ConfigMap
            group: ""
            name: foo-example-com-ca-cert
    

[

Manage TLS Certificates in a Cluster

Kubernetes provides a certificates.k8s.io API, which lets you provision TLS certificates signed by a Certificate Authority (CA) that you control. These CA and certificates can be used by your workloads to establish trust. certificates.k8s.io API uses a protocol that is similar to the ACME draft. Note:Certificates created using the certificates.k8s.io API are signed by a dedicated CA. It is possible to configure your cluster to use the cluster root CA for this purpose, but you should never rely on this.

![Kubernetes Gateway API v1.1: Service mesh, GRPCRoute y mucho más](https://cdn.hashnode.com/res/hashnode/imageupload/v1715726935312/66c02e94-396c-4854-85e0-8f391420deae.png)Kubernetes

![Kubernetes Gateway API v1.1: Service mesh, GRPCRoute y mucho más](https://cdn.hashnode.com/res/hashnode/imageupload/v1715726935513/559f611f-59df-4200-b8a7-e601d1cd1a6d.png)

](https://kubernetes.io/docs/tasks/tls/managing-tls-in-a-cluster/?ref=sredevops.org)

### [Persistencia de Sesión y BackendLBPolicy](https://gateway-api.sigs.k8s.io/geps/gep-1619/?ref=sredevops.org)

[Persistencia de Sesión](https://gateway-api.sigs.k8s.io/reference/spec/?ref=sredevops.org#gateway.networking.k8s.io%2fv1.SessionPersistence) se está introduciendo en Gateway API a través de una nueva política ([BackendLBPolicy](https://gateway-api.sigs.k8s.io/reference/spec/?ref=sredevops.org#gateway.networking.k8s.io/v1alpha2.BackendLBPolicy)) para la configuración a nivel de Servicio y como campos dentro de `HTTPRoute` y `GRPCRoute` para la configuración a nivel de ruta. `BackendLBPolicy` y las API a nivel de ruta proporcionan la misma configuración de persistencia de sesión, incluyendo tiempos de espera, nombre de sesión, tipo de sesión y tiempo de vida de cookies.

✅ ****Ejemplo de**** **`**BackendLBPolicy**`******con persistencia de s****_****esión****_ __basada en cookies__ para el servicio `foo`:  
Establece el nombre de sesión en `foo-session`, define tiempos de espera (timeouts), y configura cookies como sesión:

    apiVersion: gateway.networking.k8s.io/v1alpha2
    kind: BackendLBPolicy
    metadata:
      name: lb-policy
      namespace: foo-ns
    spec:
      targetRefs:
      - group: core
        kind: service
        name: foo
      sessionPersistence:
        sessionName: foo-session
        absoluteTimeout: 1h
        idleTimeout: 30m
        type: Cookie
        cookieConfig:
          lifetimeType: Session
    

### Más detalles y extras

#### [Aclaraciones de Terminología TLS](https://gateway-api.sigs.k8s.io/geps/gep-2907/?ref=sredevops.org)

Como parte de un objetivo más amplio de hacer que la terminología sobre TLS sea más coherente en toda la API, se han algunos cambios que rompen la compatibilidad en BackendTLSPolicy. Esto ha dado lugar a una nueva versión de API (v1alpha3) y requerirá que cualquier implementación existente de esta política maneje adecuadamente la actualización de versión, por ejemplo, haciendo una copia de seguridad de los datos y desinstalando la versión v1alpha2 antes de instalar esta nueva versión.

Cualquier referencia a los campos de BackendTLSPolicy v1alpha2 deberá actualizarse a v1alpha3. Los cambios específicos en los campos incluyen:

*   `targetRef` se convierte en `targetRefs` para permitir que un BackendTLSPolicy se adjunte a múltiples objetivos
*   `tls` se convierte en `validation`
*   `tls.caCertRefs` se convierte en `validation.caCertificateRefs`
*   `tls.wellKnownCACerts` se convierte en `validation.wellKnownCACertificates`

Para obtener una lista completa de los cambios incluidos en este lanzamiento, consulta las [notas de lanzamiento de v1.1.0.](https://github.com/kubernetes-sigs/gateway-api/releases/tag/v1.1.0?ref=sredevops.org)

Desarrollo de API Gateway
-------------------------

La idea de Gateway API se propuso inicialmente [en KubeCon San Diego 2019](https://youtu.be/Ne9UJL6irXY?si=wgtC9w8PMB5ZHil2&ref=sredevops.org) como la próxima generación de la API Ingress. Desde entonces, se ha formado una increíble comunidad para desarrollar lo que probablemente se haya convertido en [la API más colaborativa en la historia de Kubernetes](https://www.youtube.com/watch?v=V3Vu_FWb4l4&ref=sredevops.org). Más de 200 personas han contribuido a esta API hasta ahora, y ese número sigue creciendo.

<iframe width="200" height="113" src="https://www.youtube.com/embed/V3Vu_FWb4l4?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen="" title="Gateway API: The Most Collaborative API in Kubernetes History Is GA - Rob Scott &amp; Nick Young"></iframe>

> Los mantenedores quieren agradecer a _todos_ los que han contribuido a Gateway API, ya sea en forma de commits al repositorio, discusiones, ideas o apoyo general. Literalmente no habríamos llegado tan lejos sin el apoyo de esta comunidad dedicada y activa.

¿Cómo utilizar y probar Kubernetes API Gateway fácil?
-----------------------------------------------------

A diferencia de otras APIs de Kubernetes, **no necesitas actualizar a la última versión de Kubernetes para obtener la última versión de Gateway API.** Siempre que estés ejecutando Kubernetes 1.26 o posterior, podrás ponerte en marcha con esta versión de Gateway API.

### Para probar la API, sigue la [Guía de Inicio Rápido](https://gateway-api.sigs.k8s.io/guides/?ref=sredevops.org):

[

Getting started - Kubernetes Gateway API

![Kubernetes Gateway API v1.1: Service mesh, GRPCRoute y mucho más](https://cdn.hashnode.com/res/hashnode/imageupload/v1715726935767/47a0e2a4-51cc-4ac3-9e46-bcdcbf2de6f8.png)logo

![Kubernetes Gateway API v1.1: Service mesh, GRPCRoute y mucho más](https://cdn.hashnode.com/res/hashnode/imageupload/v1715726935973/b06c94f8-6b49-46af-ab3d-a1e035feed31.png)

](https://gateway-api.sigs.k8s.io/guides/?ref=sredevops.org)

Participa
---------

Hay muchas oportunidades para participar y ayudar a definir el futuro de las APIs de Kubernetes, tanto para `ingress` como para `service mesh`.

*   Revisa las [guías de usuario](https://gateway-api.sigs.k8s.io/guides?ref=sredevops.org) para ver qué casos de uso se pueden abordar.
*   Prueba uno de los [controladores de Gateway existentes](https://gateway-api.sigs.k8s.io/implementations/?ref=sredevops.org) (Recientemente publicamos sobre Traefik v3, que tiene soporte para APIGateway)

[

Traefik v3.0 ya está disponible e incluye soporte para Kubernetes Gateway API, WebAssembly y más. Aquí puedes ver cómo actualizar a v3.0

Este año se cumple el noveno aniversario de Traefik y hoy se convirtió en uno de los gateways modernos más utilizados, con más de 3 mil millones de descargas y más de 750 colaboradores. Está en el Top 15 de DockerHub y tiene 47.000 estrellas en GitHub. Aquí en

![Kubernetes Gateway API v1.1: Service mesh, GRPCRoute y mucho más](https://cdn.hashnode.com/res/hashnode/imageupload/v1715726936126/0c35ae58-856c-41c5-ba0c-922a72cd99fc.ico)SREDevOps.org - SRE, DevOps, Cloud, LinuxNicolás Georger

![Kubernetes Gateway API v1.1: Service mesh, GRPCRoute y mucho más](https://cdn.hashnode.com/res/hashnode/imageupload/v1715726937419/e17dd1ac-0901-43a4-9ca5-af6b829632a3.png)

](https://sredevops.org/es/traefik-v3-ya-esta-disponible-e-incluye-soporte-para-kubernetes-gateway-api-webassembly-y-mas-aqui-puedes-ver-como-actualizar-a-v3/)

*   O [únete a la comunidad](https://gateway-api.sigs.k8s.io/contributing/?ref=sredevops.org) y colabora a construir el futuro de Kubernetes.

**Fuente:**

[

Gateway API v1.1: Service mesh, GRPCRoute, and a whole lot more

Following the GA release of Gateway API last October, Kubernetes SIG Network is pleased to announce the v1.1 release of Gateway API. In this release, several features are graduating to Standard Channel (GA), notably including support for service mesh and GRPCRoute. We’re also introducing some new experimental features, including session persistence and client certificate verification. What’s new Graduation to Standard This release includes the graduation to Standard of four eagerly awaited features.

![Kubernetes Gateway API v1.1: Service mesh, GRPCRoute y mucho más](https://cdn.hashnode.com/res/hashnode/imageupload/v1715726937969/e358fbb9-3299-42b1-878e-e3bd38160c0f.png)Kubernetes

![Kubernetes Gateway API v1.1: Service mesh, GRPCRoute y mucho más](https://cdn.hashnode.com/res/hashnode/imageupload/v1715726938141/093cd301-3a92-4c13-8b5a-3ff5d9c61dbf.svg)

](https://kubernetes.io/blog/2024/05/09/gateway-api-v1-1/?ref=sredevops.org)