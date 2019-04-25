---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-19"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:deprecated: .deprecated}
{:new_window: target="_blank"}

# Exportación de una imagen a OpenStack Swift
{: #exporting-an-image-to-openstack-swift}

En la página Plantillas de imagen puede exportar una plantilla de imagen a una cuenta de [Object Storage OpenStack Swift](/docs/infrastructure/objectstorage-swift?topic=objectstorage-swift-GettingStarted#getting-started-with-object-storage-openstack-swift).
{:shortdesc}

Todas las instancias de este servicio están en desuso. Se pueden utilizar las cuentas existentes, pero desde el 10 de diciembre de 2018 no se pueden suministrar cuentas nuevas de {{site.data.keyword.objectstorageshort}} OpenStack Swift. A partir del 31 de marzo de 2019, IBM Cloud ya no admite la función de importación/exportación de plantillas de imagen con Cloud Object Storage OpenStack Swift.
{: deprecated}

El proceso de exportación toma una plantilla de imagen privada estándar preexistente, y convierte la imagen en un archivo de imagen que se almacena en una ubicación especificada en una cuenta de Object Storage OpenStack Swift. Siga los pasos siguientes para exportar una plantilla e imagen.

1. Desde el menú **Dispositivos** en el [{{site.data.keyword.slportal_full}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://control.softlayer.com/), seleccione **Gestionar > Imágenes**.
2. Pulse **Acciones** para la plantilla imagen que quiere exportar y seleccione **Exportar Imagen**. Si una plantilla de imagen con la configuración que desea aún no está disponible, consulte [Creación de una plantilla de imagen](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template).
3. En la página Exportar imagen, especifique el nombre de archivo para la imagen en el campo **Nombre de archivo**.
5. Desde la lista desplegable **Cuenta**, seleccione una **Cuenta de Object Storage**.
6. Desde la lista desplegable **Clúster**, seleccione un **Clúster de Object Storage**.
7. Desde la lista desplegable **Contenedor**, seleccione un **Contenedor de Object Storage**.
8. Haga clic en **Exportar imagen** para exportar la imagen a la ubicación específica en la Cuenta de Object Storage. Pulse **Cerrar** para cancelar la acción.

## Siguientes pasos

Después de exportar una imagen, la imagen sigue en la lista de plantillas de imagen, pero también está disponible como un archivo en la ubicación de Object Storage OpenStack Swift que se ha especificado durante el proceso de exportación. Para obtener más información sobre la visualización de un archivo que se ha exportado a una cuenta de Object Storage, consulte [Visualización y edición de detalles de archivo](/docs/infrastructure/objectstorage-swift?topic=objectstorage-swift-OSSSLPortal#viewing-and-editing-file-details). Dado que cada imagen tiene un tamaño diferente y características diferentes, el proceso de exportación puede tardar varios minutos antes de que finalice. La velocidad promedio de exportación es de 2 GB / minuto. Si un lapso de varios minutos y la imagen sigue sin estar disponible en la cuenta de Object Storage OpenStack Swift, [Póngase en contacto con el servicio de soporte](/docs/get-support?topic=get-support-getting-customer-support).
