---
title: "Un exploit en GitHub permitía inyectar archivos en -casi- cualquier repositorio y distribuir malware "legítimo""
datePublished: Wed Apr 24 2024 05:30:59 GMT+0000 (Coordinated Universal Time)
cuid: clveimtbo000109mk5usf4hgu
slug: un-exploit-en-github-permitia-inyectar-archivos-en-casi-cualquier-repositorio-y-distribuir-malware-legitimo-1
canonical: https://sredevops.org/es/un-exploit-en-github-permitia-inyectar-archivos-en-casi-cualquier-repositorio-y-distribuir-malware-legitimo/
cover: https://cdn.hashnode.com/res/hashnode/imageupload/v1714005521557/3ec64396-4b06-4a52-aad7-5b8190beb340.jpeg
tags: news, security, videos, devsecops, espanol

---

<iframe width="200" height="113" src="https://www.youtube.com/embed/nsm7tiyA3TA?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen="" title="Github's Hilariously Simple Malware Exploit"></iframe>

Una vulnerabilidad de seguridad en GitHub permitía a atacantes inyectar malware en repositorios legítimos.
----------------------------------------------------------------------------------------------------------

![Un exploit en GitHub permitía inyectar archivos en -casi- cualquier repositorio y distribuir malware "legítimo"](https://cdn.hashnode.com/res/hashnode/imageupload/v1714005521298/b48dcf6d-4d27-4f1c-b6cd-f8915783a9ab.jpeg)

El atacante podría subir un archivo malicioso como comentario en un issue o pull request. Incluso si el comentario se eliminaba, el archivo malicioso seguía siendo accesible a través de una URL pública. Esto se debe a que la URL del archivo se generaba en base al nombre del repositorio y el nombre del archivo, y no requería que el comentario estuviera presente.

Este exploit se podía utilizar para engañar a los usuarios para que descarguen malware que parecía provenir de una fuente confiable. Por ejemplo, un atacante podría subir un ejecutable malicioso al repositorio de instalación de drivers de un fabricante popular de tarjetas gráficas. El malware se podía disfrazar como un nuevo driver que soluciona problemas en un juego popular.

Brodie Robertson, el creador del video, sugiere que GitHub debería implementar un escaneo más estricto de los archivos subidos a través de comentarios y pull requests. También sugiere que GitHub debería dificultarles a los atacantes la explotación de este tipo de vulnerabilidad cambiando la forma en que se generan las URL para los archivos.