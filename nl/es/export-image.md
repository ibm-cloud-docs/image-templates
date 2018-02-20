---
copyright:
  years: 2014, 2018
lastupdated: "2017-10-27"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Exportación de una imagen

En la página Plantillas de imagen puede exportar una plantilla de imagen a una cuenta de almacenamiento de objetos cuenta de Object Storage. {:shortdesc}

El proceso de exportación toma una plantilla de imagen privada estándar preexistente, y convierte la imagen en un archivo de imagen que se almacena en una ubicación especificada en una cuenta de Object Storage. Siga los pasos siguientes para exportar una plantilla e imagen.

1. Desde el menú **Dispositivos** en el [Portal del cliente ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/), seleccione **Gestionar > Imágenes**.
2. Pulse **Acciones** para la plantilla imagen que quiere exportar y seleccione **Exportar Imagen**. Si una plantilla de imagen con la configuración que desea aún no está disponible, consulte [Creación de una imagen estándar](create-standard-image.html).
3. En la página Exportar imagen, especifique el nombre de archivo para la imagen en el campo **Nombre de archivo**.
5. Desde la lista desplegable **Cuenta**, seleccione una **Cuenta de Object Storage**.
6. Desde la lista desplegable **Clúster**, seleccione un **Clúster de Object Storage**.
7. Desde la lista desplegable **Contenedor**, seleccione un **Contenedor de Object Storage**.
8. Haga clic en **Exportar imagen** para exportar la imagen a la ubicación específica en la Cuenta de Object Storage. Pulse **Cerrar** para cancelar la acción.

## Siguientes pasos

Después de exportar una imagen, la imagen sigue en la lista de plantillas de imagen, pero también está disponible como un archivo en la ubicación de Object Storage que se ha especificado durante el proceso de exportación. Para obtener más información sobre la visualización de un archivo que se ha exportado a una cuenta de Object Storage, consulte [Visualización y edición de archivos Detalles de Object Storage](/docs/infrastructure/objectstorage-swift/view-and-edit-object-storage-file-details.html). Dado que cada imagen tiene un tamaño diferente y características diferentes, el proceso de exportación puede tardar varios minutos antes de que finalice. La velocidad promedio de exportación es de 2 GB / minuto. Si un lapso de varios minutos y la imagen sigue sin estar disponible en la Cuenta de Object Storage, [Póngase en contacto con soporte](/docs/get-support/howtogetsupport.html).

