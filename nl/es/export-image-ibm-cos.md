---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-02"

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
{: #exporting-to-ibm-cos}

En la página Plantillas de imagen puede exportar una plantilla de imagen a una cuenta de [IBM Cloud Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage).
{:shortdesc}

El proceso de exportación toma una plantilla de imagen privada estándar o una plantilla de imagen cifrada preexistentes y convierte la imagen en un archivo de imagen que se almacena en una ubicación especificada en una cuenta de IBM Cloud Object Storage.

Utilice los pasos siguientes para exportar una plantilla de imagen a IBM Cloud Object Storage.

1. Realice la autenticación en {{site.data.keyword.slportal}} con un ID de servicio que tenga acceso de escritura a IBM Cloud Object Storage.
2. Desde el menú **Dispositivos** en el [{{site.data.keyword.slportal_full}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://control.softlayer.com/), seleccione **Gestionar > Imágenes**.
3. Pulse **Acciones** para la plantilla imagen que quiere exportar y seleccione **Exportar imagen a IBM COS**. Si una plantilla de imagen con la configuración que desea aún no está disponible, consulte [Creación de una plantilla de imagen](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template).
4. Complete los campos necesarios (consulte la tabla 1).
5. Pulse **Aceptar** para exportar la imagen a la ubicación especificada en la cuenta de IBM Cloud Object Storage.

| Campo | Valor |
| ----- | ----- |
| Nombre del archivo | Especifique el nombre de archivo para la imagen. |
| {{site.data.keyword.cos_full_notm}} | Seleccione una cuenta de {{site.data.keyword.cos_full_notm}}. |
| Ubicación | Seleccione la región geográfica específica en la que desea que se almacene la plantilla de imagen. |
| Grupo | Seleccione el grupo de {{site.data.keyword.cos_full_notm}} en el que desea que se almacene la plantilla de imagen. Solo son válidos los grupos que existen en la ubicación regional que ha seleccionado. Recibirá un mensaje de error si especifica un grupo que no existe en la ubicación seleccionada. |
| Clave de API | Especifique la clave de API de ID de servicio que tiene acceso de escritura a {{site.data.keyword.cos_full_notm}}. Para obtener más información, consulte [Gestión de las claves de API de ID de servicio](/docs/iam?topic=iam-serviceidapikeys). |
{: caption="Tabla 1. Valores de exportación de una imagen a IBM Cloud Object Storage" caption-side="top"}

## Siguientes pasos
Dado que cada imagen tiene un tamaño diferente y características diferentes, el proceso de exportación puede tardar varios minutos antes de que finalice.

Después de exportar una imagen, la imagen sigue en la lista de plantillas de imagen, pero también está disponible como un archivo en la ubicación de IBM Cloud Object Storage que se ha especificado durante el proceso de exportación.

Cuando la plantilla de imagen se exporta a IBM Cloud Object Storage, cada dispositivo de bloque (o disco) tiene su propio archivo asociado. Por ejemplo, si el archivo de imagen se denomina image.vhd, el primer dispositivo de bloque se exporta como image-0.vhd. El segundo dispositivo de bloque se exporta como image-1.vhd, y así sucesivamente.
{: tip}
