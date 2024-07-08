---
title: "Kueue: La forma más sencilla de gestionar colas de trabajos y recursos en Kubernetes"
datePublished: Fri Jun 14 2024 12:28:53 GMT+0000 (Coordinated Universal Time)
cuid: clxeo0pqn00030alebcfieedv
slug: kueue-la-forma-mas-sencilla-de-gestionar-colas-de-trabajos-y-recursos-en-kubernetes-1-1
canonical: https://sredevops.org/es/kueue-la-forma-mas-sencilla-de-gestionar-colas-de-trabajos-y-recursos-en-kubernetes/
cover: https://cdn.hashnode.com/res/hashnode/imageupload/v1718368132920/6d0902dd-421e-4ab8-826d-666d419c6175.svg
tags: cloud, data-science, opensource, kubernetes, devops, sre, devsecops, espanol, mlops

---

![Kueue: La forma más sencilla de gestionar colas de trabajos y recursos en Kubernetes](https://cdn.hashnode.com/res/hashnode/imageupload/v1718368131780/fede0fbe-0eb0-429b-918b-7555a9403815.svg)

Kueue es una potente herramienta diseñada para mejorar la gestión de colas de trabajos y recursos dentro de los clústeres Kubernetes. Al ofrecer un conjunto de APIs y un _controller_, Kueue te permite gestionar eficientemente la ejecución de trabajos, la asignación de recursos y el rendimiento general del clúster.

Kueue es un sistema de colas de trabajos nativo de la nube para aplicaciones por lotes, HPC, AI/ML y similares en un clúster Kubernetes.

Utiliza Kueue para construir un servicio por lotes _multi-tenant_ con cuotas y una jerarquía para compartir recursos entre equipos de tu organización. En función de las cuotas disponibles, Kueue decide cuándo los trabajos deben esperar y cuándo y dónde deben ejecutarse.

Kueue funciona en combinación con el _kube-scheduler_ estándar, el _cluster-autoscaler_ y el resto del ecosistema de Kubernetes. Esta combinación permite a Kueue ejecutarse tanto _on-prem_ como en la nube, donde los recursos pueden ser heterogéneos, fungibles y aprovisionados dinámicamente.

### Características Clave para Cargas de Trabajo Optimizadas en Kubernetes

*   **Job Management (Gestión de trabajos):** Kueue permite la puesta en cola de trabajos priorizados con estrategias flexibles como StrictFIFO y BestEffortFIFO.
*   **Resource Management (Gestión de recursos):** Logra un reparto equitativo de los recursos e implementa políticas de _preemption_ entre los distintos _tenants_.
*   **Dynamic Resource Reclaim (Recuperación dinámica de recursos):** Libera automáticamente la cuota a medida que se completan los _pods_ de trabajo, maximizando la utilización de los recursos.
*   **Resource Flavor Fungibility (Fungibilidad de los tipos de recursos):** Permite el préstamo de cuotas o _preemption_ en ClusterQueues y Cohorts para una mayor flexibilidad.
*   **Amplia gama de integraciones:** Integración perfecta con tipos de trabajo populares, como BatchJob, trabajos de entrenamiento Kubeflow, RayJob, RayCluster, JobSet y Pods simples.
*   **Información detallada del sistema:** Obtén información valiosa sobre el estado de tu sistema a través de métricas Prometheus y _Conditions_ incorporados.

### Preparación para producción y soporte robusto

Kueue cuenta con una versión API v1beta1 madura, documentación completa, amplia cobertura de pruebas y verificación de escalabilidad. Con lanzamientos regulares, seguridad robusta y una creciente lista de usuarios en entornos de producción, Kueue es una solución fiable para gestionar tus cargas de trabajo de Kubernetes.

### Instalación y uso sencillo

Kueue es compatible con Kubernetes 1.22 y superiores. Instala la última versión sin esfuerzo con un solo comando:

    # ¡Comprueba la versión si es necesario!
    
    kubectl apply --server-side -f [https://github.com/kubernetes-sigs/kueue/releases/download/v0.7.0/manifests.yaml](https://github.com/kubernetes-sigs/kueue/releases/download/v0.7.0/manifests.yaml)
    

Consulta la documentación de Kueue para obtener guías de instalación detalladas y ejemplos de uso.

[

Documentation

Powerful, extensible, and feature-packed frontend toolkit. Build and customize with Sass, utilize prebuilt grid system and components, and bring projects to life with powerful JavaScript plugins.

![Kueue: La forma más sencilla de gestionar colas de trabajos y recursos en Kubernetes](https://cdn.hashnode.com/res/hashnode/imageupload/v1718368132295/42b48e9f-3394-4c97-a8c1-fe2d78b976ac.png).cls-1{fill:none}.cls-2{fill:#fff}.cls-3{fill:#326ce5}Kueue



](https://kueue.sigs.k8s.io/docs/?ref=sredevops.org)

### Roadmap

El desarrollo de Kueue está impulsado activamente por los comentarios y contribuciones de la comunidad. En 2023, la hoja de ruta del proyecto prioriza:

*   _Preemption_ cooperativa para cargas de trabajo con _checkpointing_.
*   Estrategias de asignación de _flavors_ para optimizar la asignación de recursos.
*   Integración con el _cluster-autoscaler_ para el aprovisionamiento garantizado de recursos.
*   Mayor integración con diversas cargas de trabajo personalizadas como Kubeflow, Spark, Ray y herramientas de flujo de trabajo.

Los objetivos a largo plazo incluyen soporte presupuestario, un panel de control fácil de usar y soporte _multi-clúster_.

### Comunidad y soporte

Únete a la vibrante comunidad de Kueue para mantenerte informado, participar en debates y contribuir al desarrollo continuo del proyecto. Conéctate con los mantenedores a través de Slack y la lista de correo para obtener soporte y colaborar.

### Empieza a usar Kueue

Kueue ofrece un enfoque simplificado y eficiente para la gestión de colas de trabajos y recursos en Kubernetes. Aprovechando sus potentes funciones, puedes optimizar el rendimiento de tu clúster, garantizar una asignación equitativa de los recursos y agilizar tus procesos de gestión de cargas de trabajo. ¡Instala Kueue hoy mismo y experimenta los beneficios de primera mano!

[

GitHub - kubernetes-sigs/kueue: Kubernetes-native Job Queueing

Kubernetes-native Job Queueing. Contribute to kubernetes-sigs/kueue development by creating an account on GitHub.

![Kueue: La forma más sencilla de gestionar colas de trabajos y recursos en Kubernetes](https://cdn.hashnode.com/res/hashnode/imageupload/v1718368132498/68d0c9aa-1b73-445c-b534-ac3cbab68d96.svg)GitHubkubernetes-sigs

![Kueue: La forma más sencilla de gestionar colas de trabajos y recursos en Kubernetes](https://cdn.hashnode.com/res/hashnode/imageupload/v1718368132713/8690f8c9-d7ca-4d68-a854-15d5a5e821e0.png)

](https://github.com/kubernetes-sigs/kueue/?ref=sredevops.org)