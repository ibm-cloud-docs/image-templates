---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:important: .important}
{:pre: .pre}
{:screen: .screen}


# Preparación e importación de imágenes
{: #preparing-and-importing-images}

La pantalla Plantillas de imagen del {{site.data.keyword.slportal_full}} le permite importar una imagen desde una instancia de servicio de [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about). Puede importar imágenes que estén en formato VHD (disco duro virtual) o VMDK (disco de máquina virtual). Tras la importación, las imágenes VMDK se convierten a VHD. Una vez que se ha subido una imagen a un grupo en una instancia de servicio de {{site.data.keyword.cos_full_notm}}, puede importarla al repositorio de plantillas de imagen en el {{site.data.keyword.slportal}}.
{:shortdesc}

Debe tener una cuenta actualizada para importar imágenes de {{site.data.keyword.cos_full_notm}}. Para obtener más información, consulte [Cómo cambiar a IBMid y enlazar cuentas](/docs/account/softlayerlink.html).
{: tip}

Para utilizar esta función de importación, debe haber solicitado una [instancia de IBM Cloud Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-provision#provision-account) a través de la consola de {{site.data.keyword.cloud_notm}} (cloud.ibm.com).  No se admite IBM Cloud Object Storage desde control.softlayer.com.
{: important}

Después de que las imágenes se importan como una plantilla de imagen, pueden utilizarse para suministrar o iniciar un servidor virtual existente. Las imágenes que se importan desde una instancia de servicio de {{site.data.keyword.cos_full_notm}} pueden ser VHD, VMDK o ISO personalizadas. Las importaciones de VHD y VMDK están restringidas a las siguientes sistemas operativos de 64 bit:  

* CentOS 6 y 7
* Microsoft Server Standard 2012, R2 2012, y 2016
* RedHat Enterprise Linux 6 y 7
* Ubuntu 14.04, y 16.04

Las importaciones se limitan a discos de 100 GB. Las imágenes deben nombrarse según el siguiente ejemplo: filename.vhd-0.vhd o filename.vmdk-0.vmdk

## Conversión de imágenes VHD
{: #convert-to-vhd}

Los formatos VHD y VMDK son los únicos formatos de imagen soportados para los servidores virtuales. Para convertir imágenes a VHD desde cualquier otro formato que no sea VMDK, utilice la siguiente información:

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

    1. Descargue la ISO de XenServer de Citrix:
[https://www.citrix.com/downloads/citrix-hypervisor/![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.citrix.com/downloads/citrix-hypervisor/)

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

Para obtener más información acerca de las imágenes habilitadas para cloud-init, consulte [Suministrar con una imagen habilitada para cloud-init](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image#provisioning-with-a-cloud-init-enabled-image).

## Preparación para importar una imagen cifrada
{: #preparing-to-import-an-encrypted-image}

Si tiene previsto importar una imagen VHD cifrada con su propia clave de cifrado de datos, debe cumplir los requisitos previos y las instrucciones de cifrado que se describen en [Uso de cifrado de extremo a extremo (E2E) para suministrar una instancia cifrada](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance#using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance).

Debe utilizar la herramienta vhd-util para cifrar su imagen, que debe estar en formato VHD. Para obtener más información, consulte [Cifrado de imágenes VHD](/docs/infrastructure/image-templates?topic=image-templates-create-encrypted-image). 
{: important}

## Subida de una imagen a {{site.data.keyword.cos_full_notm}}
{: #upload-to-ibm-cos}

Cuando la imagen esté lista, puede cargarla en {{site.data.keyword.cos_full_notm}}. Utilice un grupo en una [ubicación regional](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints#endpoints).

1. En {{site.data.keyword.cos_full_notm}}, vaya al grupo y pulse **Añadir objetos** para [subir](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload) la imagen.
2. Utilice el plugin de transferencia de alta velocidad de [Aspera](/docs/services/cloud-object-storage?topic=cloud-object-storage-aspera) para obtener las velocidades de carga más rápidas de la imagen.

Puede utilizar el SDK de COS con Aspera para iniciar la transferencia de alta velocidad dentro de las aplicaciones personalizadas al utilizar Java, Python o NodeJS. Para obtener más información, consulte [Uso de bibliotecas y SDK](/docs/services/cloud-object-storage?topic=cloud-object-storage-aspera#aspera-sdk).
{: tip}


## Importación de una imagen desde IBM Cloud Object Storage
{: #import-icos}

Complete los pasos siguientes para importar una imagen de {{site.data.keyword.cos_full_notm}}.

1. Acceda al menú de dispositivo y asegúrese de que tiene los permisos de cuenta correctos para completar las tareas.

   * Acceda al menú del dispositivo de la consola. Para obtener más información, consulte
[Navegación a dispositivos](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices).
   * Asegúrese de que tiene los permisos de cuenta y el acceso de dispositivo necesarios. Solo el propietario de cuenta o un usuario con el permiso de infraestructura clásico **Gestionar usuarios**
puede ajustar los permisos.

   Para obtener más información sobre los permisos, consulte [Permisos clásicos de infraestructura](/docs/iam?topic=iam-    infrapermission#infrapermission) y [Gestión de acceso a dispositivos](/docs/vsi?topic=virtual-servers-managing-device-access).

   Si va a importar una imagen cifrada, debe utilizar la consola de {{site.data.keyword.cloud_notm}}.
   {: important}
2. Acceda a la página **Plantillas de imagen** seleccionando **Dispositivos> Gestionar > Imágenes**.
3. Pulse el separador **Importar imagen de IBM COS** para abrir la herramienta de importación.
4. Complete los campos necesarios (consulte la Tabla 1).
5. Cuando la importación se haya completado desde {{site.data.keyword.cos_full_notm}}, la imagen aparecerá en la página Plantillas de imagen.

| Campo | Valor |
| ----- | ----- |
| {{site.data.keyword.cos_full_notm}} | Seleccione la instancia de servicio de {{site.data.keyword.cos_full_notm}} donde está almacenada la imagen que desea importar. |
| Ubicación | Seleccione la región geográfica específica en la que se almacena la imagen. Puede importar imágenes a las siguientes regiones y centros de datos asociados: US-South (DAL13), US-East (WDC07), EU-Great Britain (LON02), EU-Germany (FRA02), AP-Japan. Una vez que la imagen se importa a uno de los centros de datos que aparecen en la lista, puede moverla a otro centro de datos. |
| Grupo | Seleccione el grupo de {{site.data.keyword.cos_full_notm}} en el que se almacena la imagen. Solo son válidos los grupos que existen en la ubicación regional que ha seleccionado. Recibirá un mensaje de error si selecciona un grupo que no existe en la ubicación seleccionada.|
| Archivo de imagen | Seleccione el archivo de imagen en la instancia de servicio de {{site.data.keyword.cos_full_notm}} que desea importar. Los tipos de archivos soportados son VHD (disco duro virtual), VMDK (disco de máquina virtual) e ISO. Si está importando una imagen cifrada, la imagen debe estar en formato de archivo VHD y cifrada con la herramienta vhd-util. |
| Nombre de imagen | Especifique un nombre descriptivo para la imagen. Esta es la imagen que utilizará para suministrar instancias de servidor virtual. |
| Clave de API | Especifique la clave de API que proporciona acceso a la instancia de servicio de {{site.data.keyword.cos_full_notm}}. Al importar una imagen cifrada, la clave de API también debe tener acceso a la instancia de servicio de gestión
de claves. La clave de API sólo está disponible para copiarse o descargarse en el momento de la creación. Si se pierde la clave de API, deberá crear una nueva clave de API. Para obtener más información, consulte [Cómo trabajar con claves de API](/docs/iam?topic=iam-manapikey#manapikey). |
| Sistema operativo | Seleccione el sistema operativo que se incluye en la imagen. |
| Cloud-init | Si la imagen que está importando está habilitada para cloud-init, marque este recuadro de selección. Si va a importar una imagen que tiene un sistema operativo Windows habilitado para cloud-init y selecciona esta opción, no puede especificar al mismo tiempo **Su licencia**. Si va a importar una imagen cifrada, esta opción se selecciona de forma predeterminada y no puede editarse porque la imagen cifrada debe estar habilitada para cloud-init. |
| Su licencia | Si tiene planificado proporcionar su propia licencia del sistema operativo, marque este recuadro de selección. Si va a importar una imagen con un sistema operativo Windows, puede seleccionar esta opción si tiene previsto utilizar la imagen para suministrar [instancias de host dedicadas](/docs/vsi?topic=virtual-servers-dedicated-hosts-and-dedicated-instances#dedicated-hosts-and-dedicated-instances). Si la versión del sistema operativo Windows no es compatible con el uso de su propia licencia, esta opción queda inhabilitada. Para las imágenes de Windows, no puede seleccionar Cloud Init si especifica que va a utilizar su propia licencia. Si va a importar una imagen cifrada con Red Hat Enterprise Linux como sistema operativo, esta opción está seleccionada de forma predeterminada y no se puede editar, porque la imagen cifrada debe incluir su propia licencia de sistema operativo. |
| Modalidad de arranque | Seleccione la modalidad de arranque para la imagen. Si se establece una modalidad de arranque predeterminado para el sistema operativo que especifique, ese modo de arranque se selecciona aquí automáticamente. |
| Notas | Añada las notas relacionadas con la imagen que puedan resultar útiles para los usuarios. |
| Cifrado | Si va a importar una imagen que ha cifrado con su propia clave de cifrado de datos mediante la herramienta vhd-util, marque este recuadro de selección. |
{: caption="Tabla 1. Valores de importación de una imagen de IBM Cloud Object Storage" caption-side="top"}

En la tabla siguiente, se muestran campos adicionales que son aplicables sólo a la importación de imágenes cifradas. Para obtener más información sobre las imágenes cifradas, consulte [Uso de cifrado de extremo a extremo (E2E) para suministrar una instancia cifrada](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance#using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance).

<!--
To import an encrypted image, your account must have access to the End to End (E2E) Encryption feature. To enable your account for E2E Encryption, please contact [Support](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).
{: tip}
-->

| Campo | Valor |
| ----- | ----- |
| Clave de cifrado de datos encapsulada | Al importar una imagen cifrada, especifique el texto de cifrado que está asociado a la clave de cifrado de datos que ha utilizado para cifrar la imagen. Para obtener más información, consulte [Envolvimiento de claves utilizando la API](/docs/services/key-protect?topic=key-protect-wrap-keys#api). |
| Instancia del servicio de gestión de claves | Seleccione en la cuenta una instancia del servicio de gestión de claves en la lista desplegable. La instancia del servicio de gestión de claves debe incluir la clave raíz de cliente que haya utilizado para envolver la clave de cifrado de datos. |
| Nombre de clave | Seleccione el nombre de la clave raíz en la instancia del servicio de gestión de claves que ha utilizado para
encapsular a su clave de cifrado de datos. Para obtener más información, consulte [Visualización de claves](/docs/services/key-protect?topic=key-protect-view-keys#view-keys). |
{: caption="Tabla 2. Valores para importar una imagen cifrada de IBM Cloud Object Storage" caption-side="top"}

## Siguientes pasos

Después de que comience la importación, el sistema localiza el archivo de imagen en la instancia de servicio de {{site.data.keyword.cos_full_notm}} en el grupo especificado. El archivo de imagen se importa como plantilla de imagen que después es accesible en la página Plantillas de imagen. Después de completar la importación, la imagen puede utilizarse para solicitar un dispositivo nuevo o iniciar un dispositivo existente.
Además, la plantilla de imagen puede suprimirse en cualquier momento. Los tiempos de importación de imágenes varían sobre la base del tamaño de los archivos, pero normalmente toman entre varios minutos a una hora.

Después de importar una imagen en el repositorio de plantillas de imagen, puede suprimirla de {{site.data.keyword.cos_full_notm}}. Puede continuar accediendo a la plantilla de imagen desde la página **Plantillas de imagen** y utilizarla para suministrar instancias de servidor virtual.
