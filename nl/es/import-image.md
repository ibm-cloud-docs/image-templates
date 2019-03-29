---

copyright:
  years: 2014, 2018
lastupdated: "2018-12-17"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:screen: .screen}


# Preparación e importación de imágenes
{: #preparing-and-importing-images}

La pantalla Plantillas de imagen del {{site.data.keyword.slportal_full}} permite a los usuarios importar una imagen desde una instancia de servicio de [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage). Una vez que se ha subido una imagen a un grupo en una instancia de servicio de {{site.data.keyword.cos_full_notm}}, puede importarla al repositorio de plantillas de imagen en el {{site.data.keyword.slportal}}.
{:shortdesc}

Debe tener una cuenta actualizada para importar imágenes de {{site.data.keyword.cos_full_notm}}. Para obtener más información, consulte [Cómo cambiar a IBMid y enlazar cuentas](/docs/account?topic=account-unifyingaccounts).
{: tip}

Después de que las imágenes se importan como una plantilla de imagen, pueden utilizarse para suministrar o iniciar un servidor virtual existente. Las imágenes que se importan desde una instancia de servicio de {{site.data.keyword.cos_full_notm}} pueden ser VHD o ISO personalizadas. Las importaciones de VHD están restringidas a las siguientes sistemas operativos de 64 bit:  

* CentOS 6 y 7
* RedHat Enterprise Linux 6 y 7
* Ubuntu 14.04, y 16.04
* Microsoft Server Standard 2012, R2 2012, y 2016

Las importaciones de VHD se limitan a discos de 100 GB. Los VHD deben nombrarse según el siguiente ejemplo: filename.vhd-0.vhd.

## Conversión de imágenes VHD
{: #convert-to-vhd}

El formato VHD es el único formato de imagen soportado para los servidores virtuales. Para convertir imágenes a VHD, utilice la siguiente información:

* Se requiere Qemu-img 2.7.0 o posterior
* Convierta la imagen con el siguiente mandato:

  ```
  qemu-img convert -f <image format> <image name> -O vpc -o force_size <image name>
  ```
  {: pre}

* Mandato de ejemplo:

  ```
  prueba qemu-img convert -f qcow2 test -O vpc -o force_size
  ```
  {: pre}

Para obtener más información, consulte [Conversión de formatos de imagen
![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) en la documentación QEMU.

## Plantillas ISO
{: #iso-templates}

Sólo se pueden utilizar los sistemas operativos admitidos {{site.data.keyword.BluSoftlayer_notm}} para cargar una plantilla ISO en una VSI. Aquí podrá encontrar una lista de sistemas operativos soportados: [https://www.ibm.com/cloud/server-software ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud/server-software)

Las ISO que se importan mediante esta herramienta deben ser arrancable para que la imagen sea elegible para la importación.

## Configuración de una imagen para {{site.data.keyword.virtualmachinesshort}}
{: #config-image-vsi}

Para garantizar que una imagen pueda desplegarse correctamente en el entorno de infraestructura
de {{site.data.keyword.BluSoftlayer_notm}}, el servidor virtual de imágenes debe configurarse con las siguientes especificaciones:

* ***/boot*** debe ser la primera partición
* ***/boot*** y ***/*** debe ser el sistema de archivos ext3 o ext4
* ***/etc***  y /root debe estar en la misma partición que  ***/***
* ***/etc/fstab -> LABEL=SWAP-xvdb1 swap swap :*** para montar el disco de intercambio que está adjunto al sistema
* Debe instalarse wget
* Deben instalarse las utilidades xe-guest más actuales. Siga estos pasos:

    1. Descargue la ISO de XenServer de Citrix: [https://www.citrix.com/downloads/citrix-hypervisor/product-software/xenserver-76-free-edition.html ![Icono enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.citrix.com/downloads/citrix-hypervisor/product-software/xenserver-76-free-edition.html)

    2. Monte la ISO ejecutando el siguiente mandato:

        ```
        mount -o loop xenserver.iso /mnt
        ```
        {: pre}

    3. Ubicar la RPM para la ISO ejecutando el siguiente mandato:

        ```
        ls -l /mnt/Packages/xenserver-pv*
        ```
        {: pre}

    4. El resultado lista un RPM similar a:

        ```
        xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm
        ```
        {: screen}

     5. Puede copiar el RPM y extraer la ISO para xentools. Ejecute el siguiente mandato para crear una estructura de directorio que aloja la ISO:

        ```
        rpm2cpio <xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm> | cpio -idv
        ```
        {: pre}

     6. Desde la estructura del directorio resultante puede montar la ISO de *guest-tools* y ubicar *rpm/debs* para instalar xentools. Consulte la siguiente estructura de directorio de ejemplo:

        ```
        [root@mysystem user1]# rpm2cpio ../xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm | cpio -idv
        ./opt/xensource/bin/sr_rescan
        ./opt/xensource/libexec/unmount_halted_xstools.sh
        ./opt/xensource/packages/iso/guest-tools-7.1.0-137222c.iso
        139264 blocks
        [root@mysystem user1]#
        ```
        {: pre}

Para obtener más información acerca de las imágenes habilitadas para cloud-init, consulte [Suministrar con una imagen habilitada para cloud-init](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image).

## Subida de una imagen a {{site.data.keyword.cos_full_notm}}
{: #upload-to-ibm-cos}

Cuando la imagen esté lista, puede cargarla en {{site.data.keyword.cos_full_notm}}. Utilice un grupo en una [ubicación regional](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#select-regions-and-endpoints).

1. En {{site.data.keyword.cos_full_notm}}, vaya al grupo y pulse **Añadir objetos** para [subir](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-upload-data#uploading-data) la imagen.
2. Utilice el plugin de transferencia de alta velocidad de [Aspera](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-use-aspera-high-speed-transfer#Aspera-high-speed-transfer) para obtener las velocidades de carga más rápidas de la imagen.

Puede utilizar el SDK de COS con Aspera para iniciar la transferencia de alta velocidad dentro de las aplicaciones personalizadas al utilizar Java, Python o NodeJS. Para obtener más información, consulte [Uso de bibliotecas y SDK](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-use-aspera-high-speed-transfer#sdk).
{: tip}


## Importación de una imagen desde IBM Cloud Object Storage
{: #import-icos}

Complete los pasos siguientes para importar una imagen de {{site.data.keyword.cos_full_notm}}.

1. En el [{{site.data.keyword.slportal}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://control.softlayer.com/), acceda a la página **Plantillas de imagen** seleccionando **Dispositivos > Gestionar > Imágenes**.
2. Pulse el separador **Importar imagen de IBM COS** para abrir la herramienta de importación.
3. Complete los campos necesarios (consulte la Tabla 1).
4. Cuando la importación se haya completado desde {{site.data.keyword.cos_full_notm}}, la imagen aparecerá en la página Plantillas de imagen.

| Campo | Valor |
| ----- | ----- |
| {{site.data.keyword.cos_full_notm}} | Seleccione la instancia de servicio de {{site.data.keyword.cos_full_notm}} donde está almacenada la imagen que desea importar. |
| Ubicación | Seleccione la región geográfica específica en la que se almacena la imagen. Puede importar imágenes a las siguientes regiones y centros de datos asociados: US-South (DAL13), US-East (WDC07), EU-Great Britain (LON02), EU-Germany (FRA02), AP-Japan (TOK02). Una vez que la imagen se importa a uno de los centros de datos que aparecen en la lista, puede moverla a otro centro de datos. |
| Grupo | Seleccione el grupo de {{site.data.keyword.cos_full_notm}} en el que se almacena la imagen. Solo son válidos los grupos que existen en la ubicación regional que ha seleccionado. Recibirá un mensaje de error si selecciona un grupo que no existe en la ubicación seleccionada.|
| Archivo de imagen | Seleccione el archivo de imagen en la instancia de servicio de {{site.data.keyword.cos_full_notm}} que desea importar. Los tipos de archivos soportados son VHD, ISO y RAW. Si está importando una imagen cifrada, la imagen debe estar en formato de archivo RAW y cifrada con el cifrado de disco LUKS. |
| Nombre de imagen | Especifique un nombre descriptivo para la imagen. Esta es la imagen que utilizará para suministrar instancias de servidor virtual. |
| Clave de API | Especifique la clave de API que proporciona acceso a la instancia de servicio de {{site.data.keyword.cos_full_notm}}. Al importar una imagen cifrada, la clave de API también debe tener acceso a Key Protect. La clave de API sólo está disponible para copiarse o descargarse en el momento de la creación. Si se pierde la clave de API, deberá crear una nueva clave de API. Para obtener más información, consulte [Cómo trabajar con claves de API](/docs/iam?topic=iam-manapikey). |
| Sistema operativo | Seleccione el sistema operativo que se incluye en la imagen. En el caso de las imágenes cifradas, sólo los sistemas operativos Linux son selecciones válidas. |
| Cloud-init | Si la imagen que está importando está habilitada para cloud-init, marque este recuadro de selección. Si va a importar una imagen que tiene un sistema operativo Windows habilitado para cloud-init y selecciona esta opción, no puede especificar también **Su licencia**. Si va a importar una imagen cifrada, esta opción se selecciona de forma predeterminada y no puede editarse porque la imagen cifrada debe estar habilitada para cloud-init. |
| Su licencia | Si tiene planificado proporcionar su propia licencia del sistema operativo, marque este recuadro de selección. Si va a importar una imagen con un sistema operativo Windows, puede seleccionar esta opción si tiene previsto utilizar la imagen para suministrar [instancias de host dedicadas](/docs/vsi?topic=virtual-servers-dedicated-hosts-and-dedicated-instances#dedicated-hosts-and-dedicated-instances). Si la versión del sistema operativo Windows no es compatible con el uso de su propia licencia, esta opción queda inhabilitada. Para las imágenes de Windows, no puede seleccionar Cloud Init si especifica que va a utilizar su propia licencia. Si va a importar una imagen cifrada con Red Hat Enterprise Linux como sistema operativo, esta opción está seleccionada de forma predeterminada y no se puede editar, porque la imagen cifrada debe incluir su propia licencia de sistema operativo. |
| Modalidad de arranque | Seleccione la modalidad de arranque para la imagen. Si se establece una modalidad de arranque predeterminado para el sistema operativo que especifique, ese modo de arranque se selecciona aquí automáticamente. |
| Notas | Añada las notas relacionadas con la imagen que puedan resultar útiles para los usuarios. |
| Cifrado | La selección de este recuadro de selección viene determinada por el tipo de archivo de la imagen que se selecciona para importar. Las imágenes VHD e ISO indican que el archivo de imagen no está cifrado. Por lo tanto, el recuadro de selección no está marcado para imágenes VHD e ISO. Un archivo de imagen RAW indica que la imagen es una imagen cifrada. Si se especifica un archivo de imagen RAW, este recuadro de selección se selecciona de forma predeterminada y no se puede editar. |
{: caption="Tabla 1. Valores de importación de una imagen de IBM Cloud Object Storage" caption-side="top"}

En la tabla siguiente, se muestran campos adicionales que son aplicables sólo a la importación de imágenes cifradas. Para obtener más información sobre las imágenes cifradas, consulte [Uso de cifrado de extremo a extremo (E2E) para suministrar una instancia cifrada](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance).

Para importar una imagen cifrada, la cuenta debe tener acceso a la característica de cifrado de extremo a extremo (E2E). Para habilitar su cuenta para cifrado E2E, póngase en contacto con el servicio de soporte.
{: tip}

| Campo | Valor |
| ----- | ----- |
| ID de instancia de servicio de {{site.data.keyword.keymanagementserviceshort}} | Al importar una imagen cifrada, la instancia de servicio de {{site.data.keyword.keymanagementserviceshort}} debe estar en la misma región que el grupo de {{site.data.keyword.cos_full_notm}}. Puede utilizar la CLI de {{site.data.keyword.cloud_notm}} para encontrar el ID de instancia de {{site.data.keyword.keymanagementserviceshort}}. Para obtener más información, consulte [Recuperación del ID de instancia](/docs/services/key-protect?topic=key-protect-retrieve-instance-ID#retrieve-instance-ID). |
| Clave de cifrado de datos encapsulada | Al importar una imagen cifrada, especifique el texto de cifrado que está asociado a la clave de cifrado de datos que ha utilizado para cifrar la imagen. Para obtener más información, consulte [Envolvimiento de claves utilizando la API](/docs/services/key-protect?topic=key-protect-wrap-keys#api). |
| ID de clave raíz | Al importar una imagen cifrada, especifique el ID de la clave raíz que se ha utilizado para envolver la clave de cifrado de datos. Para obtener más información, consulte [Visualización de claves](/docs/services/key-protect?topic=key-protect-view-keys#view-keys). |
{: caption="Tabla 2. Valores para importar una imagen cifrada de IBM Cloud Object Storage" caption-side="top"}

## Siguientes pasos

Después de que comience la importación, el sistema localiza el archivo de imagen en la instancia de servicio de {{site.data.keyword.cos_full_notm}} en el grupo especificado. El archivo de imagen se importa como plantilla de imagen que después es accesible en la página Plantillas de imagen. Después de completar la importación, la imagen puede utilizarse para solicitar un dispositivo nuevo o iniciar un dispositivo existente.
Además, la plantilla de imagen puede suprimirse en cualquier momento. Los tiempos de importación de imágenes varían sobre la base del tamaño de los archivos, pero normalmente toman entre varios minutos a una hora.

Después de importar una imagen en el repositorio de plantillas de imagen, puede suprimirla de {{site.data.keyword.cos_full_notm}}. Puede continuar accediendo a la plantilla de imagen desde la página **Plantillas de imagen** y utilizarla para suministrar instancias de servidor virtual.
