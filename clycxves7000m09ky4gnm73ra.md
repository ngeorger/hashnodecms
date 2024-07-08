---
title: "Chequea Kubernetes con Popeye! Seguridad, configs, problemas y más con Popeye CLI (Además es open source y liviano!)"
datePublished: Mon Jul 08 2024 12:08:52 GMT+0000 (Coordinated Universal Time)
cuid: clycxves7000m09ky4gnm73ra
slug: chequea-kubernetes-con-popeye-seguridad-configs-problemas-y-mas-con-popeye-cli-ademas-es-open-source-y-liviano
canonical: https://sredevops.org/es/chequea-kubernetes-con-popeye-seguridad-configs-problemas-y-mas-con-popeye-cli-ademas-es-open-source-y-liviano/
cover: https://cdn.hashnode.com/res/hashnode/imageupload/v1720440531670/98dd7b94-5bb2-420d-b0d4-4b74cf40351a.png
tags: apps, opensource, security, kubernetes, devops, sre, devsecops, espanol, cve

---

TL/DR;
------

![Chequea Kubernetes con Popeye! Seguridad, configs, problemas y más con Popeye CLI (Además es open source y liviano!)](https://cdn.hashnode.com/res/hashnode/imageupload/v1720440529024/de54b5ec-ce57-4a59-abc5-3fcde9ddf509.png)

¿Cansado de revisar manualmente tu clúster de Kubernetes para encontrar problemas? Popeye es como un chequeo de salud para tu clúster, encontrando posibles problemas con tus configuraciones y uso de recursos. Es una herramienta de línea de comandos que **escanea tu clúster _en vivo_**, no solo archivos estáticos, y señala cosas como errores de configuración, recursos no utilizados e incluso posibles sobreasignaciones de recursos. **Es de solo lectura, por lo que no tocará tu clúster**, solo te dará un informe amigable _(o tal vez no tan amigable_ 🙃_, dependiendo de la salud de tu clúster)_. Incluso puedes **_ponerte fancy_** con diferentes formatos de salida (JSON, HTML, lo que sea), **enviar informes a S3 e integrarlo con Prometheus y Grafana para monitoreo continuo.**

![Chequea Kubernetes con Popeye! Seguridad, configs, problemas y más con Popeye CLI (Además es open source y liviano!)](https://cdn.hashnode.com/res/hashnode/imageupload/v1720440529466/1689ff42-1657-420c-95a2-a539c7b0bb64.png)

¿Por qué necesitas un linter de Kubernetes como Popeye?
-------------------------------------------------------

Seamos realistas, Kubernetes es increíble para orquestar tus aplicaciones en containers. Sin embargo, a medida que tus implementaciones crecen, también lo hace la complejidad. De repente, te estás ahogando en un mar de archivos YAML, preguntándote si ese `Service` en el `default` namespace está _realmente_ hablando con tu `Pod`, o si ese `PersistentVolumeClaim` de un proyecto eliminado todavía está dando vueltas como un mal olor.

**Ahí es donde entra Popeye, flexionando sus músculos con poder de espinaca para darle a tu clúster un chequeo completo.**

> **🎶🎶🎤 ¡Popeye el marino soy!...**

### ¡Popeye al rescate!

Popeye se sumerge en tu clúster _en vivo_, inspeccionando tus recursos mientras se ejecutan. Esta no es solo una herramienta de análisis estático de prueba en seco. Es la cosa real, buscando problemas comunes que pueden hacerte tropezar:

*   **Error de configuraciones:** ¿Tus asignaciones de puertos de contenedor son correctas? ¿Tus etiquetas `Pod` coinciden con tus selectores `Service`?
*   **Uso de recursos:** Popeye incluso puede acceder a tu servidor de métricas (si estás usando uno) y advertirte sobre posibles sobreasignaciones de CPU o memoria _antes_ de que tu clúster _tire la toalla._
*   **Recursos obsoletos:** ¿Recuerdas ese `Namespace` que pensaste que eliminaste hace meses? Popeye lo encontrará. ¿Esos `Secrets` sin usar? Sí, también los marcará.
*   **Mejores prácticas de seguridad:** Popeye puede ayudarte a detectar cosas como Pods que se ejecutan como root, límites de recursos faltantes y otras malas prácticas básicas de seguridad.

![Chequea Kubernetes con Popeye! Seguridad, configs, problemas y más con Popeye CLI (Además es open source y liviano!)](https://cdn.hashnode.com/res/hashnode/imageupload/v1720440529977/fce78b43-c4a3-4221-b923-b08c4c8182a3.png)

**Instalación**
---------------

 ¡Tienes opciones! _(¡diVeRsIoNN! ¡fuN!)_ Descarga binarios, usa `brew install`, o `go install` si eres un aficionado a Go.

    brew install derailed/popeye/popeye
    

    go install github.com/derailed/popeye@latest
    

Comenzando con Popeye
---------------------

**Interpretando el informe:** Popeye codifica por colores sus hallazgos para darte una imagen clara de la salud de tu clúster:

*   **✅ OK:** ¡Todo parece estar bien!
*   **🔊 Info:** Solo algunos mensajes de información.
*   **😱 Warn:** Posibles problemas que podrías querer investigar.
*   **💥 Error:** ¡Se requiere acción! Estos son problemas que necesitan ser solucionados.

**Sube de nivel con Prometheus y Grafana:** Integra Popeye con Prometheus para recopilar métricas y visualizar la salud de tu clúster con el tiempo en Grafana. Incluso puedes configurar alertas para que se te notifique cuando Popeye encuentre algo sospechoso.

![Chequea Kubernetes con Popeye! Seguridad, configs, problemas y más con Popeye CLI (Además es open source y liviano!)](https://cdn.hashnode.com/res/hashnode/imageupload/v1720440530536/bc36ef06-c332-4f11-904d-78d81e31a499.png)

### **Personalizando escaneos (¿Espinacas, alguien?)**

Puedes ajustar el comportamiento de Popeye usando un archivo de configuración `spinach.yaml`. ¿Quieres ajustar los umbrales de utilización de recursos, excluir recursos específicos o incluso anular la gravedad de ciertas verificaciones? ¡Spinach te tiene cubierto!

    popeye:
       allocations:
          cpu:
             overPercUtilization: 70 # Trigger a warning if CPU utilization goes above 70%
    

**Ejecuta el escaneo:** Popeye funciona de inmediato. Solo apúntalo a tu clúster:

    popeye
    

¿Quieres escanear un namespace específico? No hay problema:

    popeye -n my-awesome-app
    

### Popeye en acción: Un ejemplo práctico

Digamos que estás ejecutando una aplicación web en tu clúster. Tienes un `Deployment`, un `Service`, y algunos otros recursos. Ejecutas Popeye, y este arroja lo siguiente:

    😱  WARN  po  Pods  default/my-awesome-app-7c94985768-x5fzk  Container 'my-awesome-app' has no resource requests or limits defined!
    

¡Uy! Parece que olvidaste establecer los límites de recursos en tu `Pod`. Esto significa que tu aplicación podría consumir potencialmente todos los recursos de tu nodo, dejando a otras aplicaciones sin nada. ¡Es hora de actualizar ese archivo YAML!

Mantén tu clúster saludable con Popeye
--------------------------------------

Popeye es una herramienta esencial para cualquiera que ejecute Kubernetes. Es como tener un experto en Kubernetes constantemente mirándote por encima del hombro, señalando posibles problemas antes de que se conviertan en grandes dolores de cabeza. Entonces, ¡agrega Popeye a tu caja de herramientas y comienza a darle a tu clúster los chequeos que se merece! _(O tal vez no...)_

Enlaces útiles
--------------

*   [Popeye GitHub Repository](https://github.com/derailed/popeye?ref=sredevops.org)

[

GitHub - derailed/popeye: 👀 A Kubernetes cluster resource sanitizer

👀 A Kubernetes cluster resource sanitizer. Contribute to derailed/popeye development by creating an account on GitHub.

![Chequea Kubernetes con Popeye! Seguridad, configs, problemas y más con Popeye CLI (Además es open source y liviano!)](https://cdn.hashnode.com/res/hashnode/imageupload/v1720440530944/332cd12d-ddc1-43bd-b3af-9c53d9a72de1.svg)GitHubderailed

![Chequea Kubernetes con Popeye! Seguridad, configs, problemas y más con Popeye CLI (Además es open source y liviano!)](https://cdn.hashnode.com/res/hashnode/imageupload/v1720440531091/ee4cd79f-b9c4-4310-af26-fcf382dba61b.png)

](https://github.com/derailed/popeye?ref=sredevops.org)

*   [Official website](https://popeyecli.io/?ref=sredevops.org)

[

popeye

👀 A Kubernetes cluster resource sanitizer

popeye

![Chequea Kubernetes con Popeye! Seguridad, configs, problemas y más con Popeye CLI (Además es open source y liviano!)](https://cdn.hashnode.com/res/hashnode/imageupload/v1720440531517/443cb366-ff12-41de-8f5e-6c8bdaf638b2.png)

](https://popeyecli.io/?ref=sredevops.org)

### Algunos checks de ejemplo

K8s Resource

Linters

Aliases

🛀

Node

no

Conditions ie not ready, out of mem/disk, network, pids, etc

Pod tolerations referencing node taints

CPU/MEM utilization metrics, trips if over limits (default 80% CPU/MEM)

🛀

Namespace

ns

Inactive

Dead namespaces

🛀

Pod

po

Pod status

Containers statuses

ServiceAccount presence

CPU/MEM on containers over a set CPU/MEM limit (default 80% CPU/MEM)

Container image with no tags

Container image using `latest` tag

Resources request/limits presence

Probes liveness/readiness presence

Named ports and their references

🛀

Service

svc

Endpoints presence

Matching pods labels

Named ports and their references

🛀

ServiceAccount

sa

Unused, detects potentially unused SAs

🛀

Secrets

sec

Unused, detects potentially unused secrets or associated keys

🛀

ConfigMap

cm

Unused, detects potentially unused cm or associated keys

🛀

Deployment

dp, deploy

Unused, pod template validation, resource utilization

🛀

StatefulSet

sts

Unused, pod template validation, resource utilization

🛀

DaemonSet

ds

Unused, pod template validation, resource utilization

🛀

PersistentVolume

pv

Unused, check volume bound or volume error

🛀

PersistentVolumeClaim

pvc

Unused, check bounded or volume mount error

🛀

HorizontalPodAutoscaler

hpa

Unused, Utilization, Max burst checks

🛀

PodDisruptionBudget

Unused, Check minAvailable configuration

pdb

🛀

ClusterRole

Unused

cr

🛀

ClusterRoleBinding

Unused

crb

🛀

Role

Unused

ro

🛀

RoleBinding

Unused

rb

🛀

Ingress

Valid

ing

🛀

NetworkPolicy

Valid, Stale, Guarded

np

🛀

PodSecurityPolicy

Valid

psp

🛀

Cronjob

Valid, Suspended, Runs

cj

🛀

Job

Pod checks

job

🛀

GatewayClass

Valid, Unused

gwc

🛀

Gateway

Valid, Unused

gw

🛀

HTTPRoute

Valid, Unused

gwr