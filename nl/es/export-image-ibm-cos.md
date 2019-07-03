---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-18"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:tip: .tip}

# Exportación de una imagen a IBM Cloud Object Storage
{: #exporting-an-image-to-ibm-cloud-object-storage}

En la página Plantillas de imagen, puede exportar una plantilla de imagen a una cuenta de
[{{site.data.keyword.cos_full}}](/docs/cloud-object-storage?topic=cloud-object-storage-about).
{:shortdesc}

El proceso de exportación toma una plantilla de imagen privada estándar o una plantilla de imagen cifrada preexistentes y convierte la imagen en un archivo de imagen que se almacena en una ubicación especificada en una cuenta de {{site.data.keyword.cos_full_notm}}.

*Nota:* Si ha importado una imagen VMDK, puede exportar la imagen en formato VHD o VMDK. Debido a las diferencias entre formatos de imagen, se puede producir una pérdida de datos. Para proteger los datos en caso de pérdida de datos, se retiene el archivo VHD original.

## Antes de empezar
En primer lugar, acceda al menú de dispositivo y asegúrese de que tiene los permisos de cuenta correctos para completar las tareas.

* Acceda al menú del dispositivo de la consola. Para obtener más información, consulte
[Navegación a dispositivos](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices).
* Asegúrese de que tiene permiso de escritura en {{site.data.keyword.cos_full_notm}}. Solo el propietario de cuenta o un usuario con el permiso de infraestructura clásico **Gestionar usuarios**
puede ajustar los permisos.

Para obtener más información sobre los permisos, consulte [Permisos clásicos de infraestructura](/docs/iam?topic=iam-infrapermission#infrapermission) y [Gestión de acceso a dispositivos](/docs/vsi?topic=virtual-servers-managing-device-access).

Si tiene pensado exportar esta plantilla de imagen a {{site.data.keyword.cos_full_notm}}, asegúrese de que su nombre no contiene caracteres que puedan ser problemáticos en una dirección web. Por ejemplo, ?, =, < y
otros caracteres especiales podrían dar lugar a un comportamiento no deseado si no está codificada como URL.
{: tip}

## Exportación de una imagen a IBM Cloud Object Storage

Utilice los pasos siguientes para exportar una plantilla de imagen a IBM Cloud Object Storage.

1. Desde el menú **Dispositivos**, seleccione **Gestionar > Imágenes**.
2. Pulse **Acciones** para la plantilla imagen que quiere exportar y seleccione **Exportar imagen a IBM COS**. Si una plantilla de imagen con la configuración que desea aún no está disponible, consulte [Creación de una plantilla de imagen](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template#creating-an-image-template).
3. Complete los campos necesarios (consulte la tabla 1).
4. Pulse **Aceptar** para exportar la imagen a la ubicación especificada en la cuenta de {{site.data.keyword.cos_full_notm}}.

| Campo | Valor |
| ----- | ----- |
| Nombre del archivo | Especifique el nombre de archivo para la imagen. |
| {{site.data.keyword.cos_full_notm}} | Seleccione una cuenta de {{site.data.keyword.cos_full_notm}}. |
| Ubicación | Seleccione la región geográfica específica en la que desea que se almacene la plantilla de imagen. |
| Grupo | Seleccione el grupo de {{site.data.keyword.cos_full_notm}} en el que desea que se almacene la plantilla de imagen. Solo son válidos los grupos que existen en la ubicación regional que ha seleccionado. Recibirá un mensaje de error si especifica un grupo que no existe en la ubicación seleccionada. |
| Clave de API | Especifique la clave de API de ID de servicio que tiene acceso de escritura a {{site.data.keyword.cos_full_notm}}. Para obtener más información, consulte [Gestión de las claves de API de ID de servicio](/docs/iam?topic=iam-serviceidapikeys#serviceidapikeys). |
{: caption="Tabla 1. Valores de exportación de una imagen a {{site.data.keyword.cos_full_notm}}" caption-side="top"}
| Tipo de archivo de destino | Seleccione el tipo de archivo que desea exportar. Si ha importado una imagen VMDK, puede exportar la imagen en formato VHD o VMDK. Si el formato original del archivo era VHD, sólo puede exportar como VHD. |

## Siguientes pasos
{: #next-steps-exporting-an-image-to-ibm-cloud-object-storage}
Dado que cada imagen tiene un tamaño diferente y características diferentes, el proceso de exportación puede tardar varios minutos antes de que finalice.

Después de exportar una imagen, la imagen sigue en la lista de plantillas de imagen, pero también está disponible como un archivo en la ubicación de {{site.data.keyword.cos_full_notm}} que se ha especificado durante el proceso de exportación. Puede descargar el archivo de imagen desde {{site.data.keyword.cos_full_notm}}. En la Lista de recursos en la consola de {{site.data.keyword.cloud_notm}}, acceda a la instancia de servicio de Cloud Object Storage. En el grupo en el que está almacenada la imagen, seleccione los archivos que
quiera descargar y seleccione **Descargar objetos**.

Cuando la plantilla de imagen se exporta a IBM Cloud Object Storage, cada dispositivo de bloque (o disco) tiene su propio archivo asociado. Por ejemplo, si el archivo de imagen se denomina image.vhd, el primer dispositivo de bloque se exporta como image-0.vhd. El segundo dispositivo de bloque se exporta como image-1.vhd, y así sucesivamente.
{: tip}
