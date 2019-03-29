---

copyright:
  years: 2014, 2017
lastupdated: "2017-10-03"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Acerca de las plantillas de imagen
{: #about-image-templates}

Las plantillas de imagen estándares proporcionan una opción de creación de imágenes para todos los {{site.data.keyword.BluVirtServers_short}}, independientemente del sistema operativo.
{:shortdesc}

Con las plantillas de imagen estándar, puede capturar una imagen de un servidor virtual existente y crear uno nuevo basado en la imagen capturada. Las plantillas de imágenes estándares no son compatibles con los servidores nativos.

## Cómo funcionan las plantillas de imágenes
Las dos acciones principales para cualquier plantilla de imagen son crear y desplegar. Cuando se solicita la creación de una imagen, el sistema automatizado de {{site.data.keyword.BluSoftlayer_full}} coloca el servidor fuera de línea. Mientras el servidor está fuera de línea, se crea una copia de seguridad comprimida de los datos, la información de configuración se registra y la plantilla de imagen se almacena en la SAN de {{site.data.keyword.BluSoftlayer_notm}}. Durante la etapa de despliegue de la plantilla de imagen, el sistema automático construye un nuevo servidor basado en los datos recopilados de la imagen seleccionada. El proceso de despliegue realiza ajustes en cuanto a volumen, restaura los datos copiados y luego realiza cambios finales en la configuración (por ejemplo, configuraciones de red) para el nuevo host.

## Imágenes privadas

Las imágenes privadas son imágenes que crea un usuario en la cuenta o imágenes que se crean en otra cuenta y se comparten. De forma predeterminada, todas las imágenes que se crean son privadas.

## Imágenes públicas

Las imágenes públicas son servidores virtuales preconfigurados que publica {{site.data.keyword.BluSoftlayer_notm}} o clientes y que están disponibles para el uso de todos los clientes de {{site.data.keyword.BluSoftlayer_notm}}. Los servidores virtuales que se suministran a través de plantillas de imágenes públicas suelen estar configurados para un rendimiento óptimo, con combinaciones específicas de diversas especificaciones de configuración.
