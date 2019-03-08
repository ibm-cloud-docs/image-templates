---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-15"

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}


# Suministro con una imagen habilitada con cloud-init

Cuando ordena un servidor virtual, muchos sistemas operativos utilizan ahora una imagen de cloud-init
para optimizar el tiempo de suministro. También puede importar una imagen personalizada que ha habilitado para cloud-init.
{:shortdesc}

Los siguientes sistemas operativos ahora son predeterminados para una imagen habilitada para cloud-init cuando solicita un servidor virtual sin complementos. (Los complementos incluyen software adicional, scripts posteriores al suministro y supervisión avanzada).
* CentOS 7
* Debian 8, 9
* Ubuntu 16.04, 18.04
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

Cuando ordena un servidor virtual con un sistema operativo cloud-init puede añadir los datos de usuario o metadatos con guiones de suministro personalizado. En el campo de datos de usuario en el formulario de pedido, especifique datos o metadatos de usuario opcional cloud-init para el servidor.

## Importar una imagen cloud-init personalizada

Si ha creado una imagen personalizada que está habilitada para cloud-init, puede designarla como una imagen de cloud-init en la página Importar imagen del {{site.data.keyword.slportal_full}}.

Para acceder la página Importar imagen de Plantillas de imagen y marcar una imagen como habilitada para cloud-init, complete los siguientes pasos:
1. Desde el menú **Dispositivos** seleccione **Gestionar** > **Imágenes**.
2. Pulse el separador **Importar Imagen**.
3. Complete la información necesaria para importar su imagen habilitada para cloud-init, y seleccione el recuadro de selección **Cloud init**, que se muestra cerca del recuadro desplegable **Sistema operativo**. Para obtener más información acerca de la importación de imágenes, consulte Importar una imagen.

## Marque una plantilla de imagen como habilitada para cloud-init

Si tiene una plantilla de imagen VHD que esté habilitada para cloud-init, puede designarla como habilitada para cloud-init en la página de detalles de la plantilla de imagen.

Para acceder una plantilla de imagen y marcarla como habilitada como cloud-init, siga estos pasos:
1. Desde el menú **Dispositivos** seleccione **Gestionar** > **Imágenes**.
2. En la lista de plantillas, pulse el nombre de la plantilla de imagen que desea actualizar.
3. En la página Detalles de la plantilla de imagen, seleccione el recuadro de selección **Habilitado** en la cabecera **Cloud Init**, y pulse **Actualizar**.

## Trabajar con una plantilla de imagen creada a partir de un servidor virtual suministrado de cloud-init

Cloud-init normalmente sólo se ejecuta una vez. Sin embargo, si suministra un servidor virtual desde una imagen habilitada para cloud-init y después crea una plantilla de imagen desde un servidor virtual, se registra el UUID. Si se utiliza esa plantilla de imagen para crear otro servidor virtual, vuelve a ejecutarse cloud-init.

## Cree plantillas de imagen que estén habilitadas para cloud-init

Para obtener más información acerca de la configuración de imágenes, consulte la [documentación de cloud-init ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloudinit.readthedocs.io/en/latest/).

Para obtener información sobre los orígenes de datos, consulte [Orígenes de datos ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://cloudinit.readthedocs.io/en/latest/topics/datasources.html). Las imágenes cloud-init de {{site.data.keyword.cloud_notm}} se crean para el entorno mediante el [origen de datos de Config Drive ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://cloudinit.readthedocs.io/en/latest/topics/datasources/configdrive.html) - Versión 2 para suministrar los metadatos.

### Requisitos de Linux
* Cloud-init versión 0.7.7 o superior

### Requisitos de Windows
* El Servicio de metadatos Cloudbase-init para dar soporte a la red pública y privada en la infraestructura de {{site.data.keyword.cloud_notm}}. El servicio también actualiza el portal de clientes con las credenciales del servidor virtual de Windows. Puede acceder al servicio en [https://github.com/softlayer/bluemix-cloudbase-init ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/softlayer/bluemix-cloudbase-init).
* Si utiliza un Vyatta en su entorno, debe configurar Vyatta para permitir llamadas API para que la API cargue equilibradores. Para obtener más información, consulte [Guía de configuración de Brocade vRouter (Vyatta) para VMware Environments con almacenamiento de archivos ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/infrastructure/FileStorage?topic=FileStorage-configureVyatta#setting-up-brocade-vrouter-vyatta-for-vmware-environments-with-file-storage).
