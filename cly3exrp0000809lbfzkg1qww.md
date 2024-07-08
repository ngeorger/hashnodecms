---
title: "Vulnerabilidad crítica en OpenSSH "regreSSHion", chequea si estás en riesgo"
datePublished: Mon Jul 01 2024 20:08:54 GMT+0000 (Coordinated Universal Time)
cuid: cly3exrp0000809lbfzkg1qww
slug: vulnerabilidad-critica-en-openssh-regresshion-chequea-si-estas-en-riesgo
canonical: https://sredevops.org/es/vulnerabilidad-critica-en-openssh-regresshion-chequea-si-estas-en-riesgo/
cover: https://cdn.hashnode.com/res/hashnode/imageupload/v1719864533306/34f2d9c9-e9ac-41a8-9be3-6a2d62568234.webp
tags: linux, opensource, security, devsecops, espanol, cve

---

### TL/DR;

![Vulnerabilidad crítica en OpenSSH "regreSSHion", chequea si estás en riesgo](https://cdn.hashnode.com/res/hashnode/imageupload/v1719864531349/ba316b5e-7dd2-4c29-9f20-4fad28eba562.webp)

Una vulnerabilidad crítica ([CVE-2024-6387](https://www.qualys.com/regresshion-cve-2024-6387/?ref=sredevops.org)), apodada "regreSSHion", ha resurgido en las versiones del servidor **OpenSSH 8.5p1 a 9.8p1,** lo que podría permitir la ejecución remota de código como root sin autenticación en sistemas Linux vulnerables. Este bug, una regresión de la vulnerabilidad CVE-2006-5051 previamente parcheada, pone de manifiesto la importancia de realizar pruebas de regresión exhaustivas. **Se estima que aproximadamente 700.000 servidores OpenSSH expuestos a Internet son vulnerables.** Este artículo detalla la vulnerabilidad, su impacto y los pasos para mitigar.

[

OpenSSH Vulnerability: CVE-2024-6387 FAQs and Resources | Qualys, Inc.

Discover what the OpenSSH vulnerability, CVE-2024-6387, is as well as resources and tools to help detect and mitigate vulnerabilities in your network.

![Vulnerabilidad crítica en OpenSSH "regreSSHion", chequea si estás en riesgo](https://cdn.hashnode.com/res/hashnode/imageupload/v1719864531631/1d630ff9-9019-4c8a-9e89-6d4926f51ab7.png)Qualys

![Vulnerabilidad crítica en OpenSSH "regreSSHion", chequea si estás en riesgo](https://cdn.hashnode.com/res/hashnode/imageupload/v1719864531781/09ce2f58-587e-4c04-aaf3-76ebc69e95c4.jpeg)

](https://www.qualys.com/regresshion-cve-2024-6387/?ref=sredevops.org)

¿Qué es la vulnerabilidad regreSSHion?
--------------------------------------

La vulnerabilidad regreSSHion (CVE-2024-6387) es "_un regreso al pasado"_, una condición de carrera (race conditions, raft) de manejadores de señales en el servidor OpenSSH (sshd) que permite a los atacantes remotos ejecutar potencialmente código arbitrario con privilegios de root. El error resurgió en las versiones 8.5p1 a 9.8p1 debido a la eliminación accidental de un componente crítico del código. Esta vulnerabilidad permite a los atacantes no autenticados obtener el control total de los sistemas afectados. El nombre "regreSSHion" es un ingenioso juego de palabras que pone de manifiesto que esta vulnerabilidad es una regresión de un fallo previamente corregido, CVE-2006-5051.

¿Cómo funciona regreSSHion?
---------------------------

Esta vulnerabilidad explota una condición de carrera en la forma en que OpenSSH maneja las señales durante el proceso de autenticación. Enviando una secuencia específica de señales en el momento justo, un atacante puede forzar al servidor OpenSSH a ejecutar código malicioso con privilegios de root. Esto significa que un atacante podría potencialmente tomar el control completo de tu servidor sin necesidad de ninguna credencial de acceso válida.

¿Cuál es el impacto de regreSSHion?
-----------------------------------

La vulnerabilidad regreSSHion supone una grave amenaza para las organizaciones de todo el mundo, ya que puede provocar la completa del sistema, filtraciones de datos e interrupciones del servicio. Explotando esta vulnerabilidad, los atacantes pueden ejecutar código arbitrario con los mayores privilegios, lo que les permite:

*   **Instalar malware:** Inyectar software malicioso para robar datos, espiar o lanzar nuevos ataques.
*   **Manipular datos:** Alterar o eliminar datos confidenciales, interrumpir las operaciones y causar daños a la reputación.
*   **Crear puertas traseras:** Establecer un acceso persistente a los sistemas comprometidos para su futura explotación.
*   **Propagarse lateralmente:** Utilizar el sistema comprometido como plataforma de lanzamiento de ataques a otros sistemas de la red.

Esta vulnerabilidad es especialmente preocupante porque afecta a una tecnología muy utilizada, OpenSSH, en la que a menudo se confía para la administración remota segura y la transferencia de datos.

Cómo mitigar la vulnerabilidad regreSSHion
------------------------------------------

Abordar la vulnerabilidad regreSSHion requiere una acción inmediata. Las organizaciones deben priorizar la aplicación de parches a los sistemas afectados como principal estrategia de mitigación. A continuación, se presenta un desglose de los pasos clave:

*   **Gestión de parches:** La mitigación más eficaz es aplicar los parches oficiales publicados por OpenSSH. Actualiza OpenSSH a la versión 9.8p1 o posterior inmediatamente. Para versiones anteriores, consulta el aviso de seguridad de OpenSSH para obtener los parches específicos.
*   **Segmentación de la red:** Aísla los sistemas críticos y segmenta tu red para limitar el impacto potencial de un ataque. Esto ayuda a evitar que los atacantes se muevan lateralmente dentro de tu red si un sistema se ve comprometido.
*   **Sistemas de detección y prevención de intrusiones:** Despliega sistemas robustos de detección y prevención de intrusiones para supervisar el tráfico de red en busca de actividades sospechosas. Configura estos sistemas para detectar y bloquear los intentos de explotar la vulnerabilidad regreSSHion.
*   **Supervisión y análisis de registros:** Revisa periódicamente los registros del sistema y de seguridad en busca de cualquier indicador de compromiso. Busca intentos inusuales de conexión SSH o fallos de autenticación.
*   **Principio de mínimo privilegio:** Cumple el principio de mínimo privilegio concediendo a los usuarios y procesos sólo el nivel mínimo de acceso necesario para realizar sus tareas. Esto limita los daños potenciales que un atacante puede infligir si compromete una cuenta de usuario.

Ideas recogidas
---------------

La vulnerabilidad regreSSHion es un claro recordatorio de que incluso el software bien mantenido y ampliamente utilizado puede tener vulnerabilidades críticas. Las medidas de seguridad proactivas, como la aplicación oportuna de parches, las herramientas de seguridad robustas y la adhesión a las mejores prácticas de seguridad son esenciales para mitigar estas amenazas. Al tomar medidas rápidas para hacer frente a la vulnerabilidad regreSSHion, las organizaciones pueden reducir significativamente su riesgo de ser víctimas de ataques que exploten este fallo.

Referencias
-----------

*   [Blog de Seguridad de Qualys](https://blog.qualys.com/vulnerabilities-threat-research/2024/07/01/regresshion-remote-unauthenticated-code-execution-vulnerability-in-openssh-server?ref=sredevops.org)

[

regreSSHion: Remote Unauthenticated Code Execution Vulnerability in OpenSSH server | Qualys Security Blog

The Qualys Threat Research Unit (TRU) has discovered a Remote Unauthenticated Code Execution (RCE) vulnerability in OpenSSH’s server (sshd) in glibc-based Linux systems. CVE assigned to this…

![Vulnerabilidad crítica en OpenSSH "regreSSHion", chequea si estás en riesgo](https://cdn.hashnode.com/res/hashnode/imageupload/v1719864532034/33a51472-eb4f-48d9-bcce-589ff5a76f23.png)Qualys, Inc.Bharat Jogi

![Vulnerabilidad crítica en OpenSSH "regreSSHion", chequea si estás en riesgo](https://cdn.hashnode.com/res/hashnode/imageupload/v1719864532261/b67ab480-1ee5-4f6a-ac32-f9cd4cdb7ea8.jpeg)

](https://blog.qualys.com/vulnerabilities-threat-research/2024/07/01/regresshion-remote-unauthenticated-code-execution-vulnerability-in-openssh-server?ref=sredevops.org)

*   [Seguridad de OpenSSH](https://www.openssh.com/security.html?ref=sredevops.org)

[

OpenSSH: Security

OpenSSH advisories

![Vulnerabilidad crítica en OpenSSH "regreSSHion", chequea si estás en riesgo](https://cdn.hashnode.com/res/hashnode/imageupload/v1719864532482/70cf3fe7-ddc5-4ef0-aaa1-f8a6588c6851.ico)



](https://www.openssh.com/security.html?ref=sredevops.org)