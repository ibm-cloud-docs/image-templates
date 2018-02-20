---
copyright:
  years: 2014, 2018
lastupdated: "2017-10-31"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:screen: .screen}


# Preparación e importación de imágenes

La pantalla de Plantillas de imagen en el [Portal de cliente ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://control.softlayer.com/) permite a los usuarios cargar una imagen existente de una cuenta de [Object Storage](/docs/infrastructure/objectstorage-swift/index.html).
{:shortdesc}

Después de que las imágenes se importan como una plantilla de imagen, pueden utilizarse para suministrar o iniciar un servidor virtual existente. Las imágenes que se importan desde una cuenta de Object Storage pueden ser VHD o ISO personalizadas. Las importaciones de VHD están restringidas a las siguientes sistemas operativos de 64 bit:

* CentOS 6 y 7
* RedHat Enterprise Linux 6 y 7
* Ubuntu 14.04, y 16.04
* Microsoft Server Standard 2012, R2 2012, y 2016

Las importaciones de VHD se limitan a discos de 100 GB. Los VHD deben nombrarse según el siguiente ejemplo: filename.vhd-0.vhd.

## Conversión de imágenes VHD

El formato de VHD es el único formato de soporte de imagen para {{site.data.keyword.BluVirtServers_full}}. Para convertir imágenes a VHD, utilice la siguiente información:

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

Sólo se pueden utilizar los sistemas operativos admitidos {{site.data.keyword.BluSoftlayer_notm}} para cargar una plantilla ISO en una VSI. Se puede encontrar una lista de Sistemas operativos soportados en:
[http://www.softlayer.com/services/software/ ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://www.softlayer.com/services/software/)

Las ISO que se importan mediante esta herramienta deben ser arrancable para que la imagen sea elegible para la importación.

## Configuración de una imagen para {{site.data.keyword.virtualmachinesshort}}

Para garantizar que una imagen pueda desplegarse correctamente en el entorno de infraestructura
de {{site.data.keyword.BluSoftlayer_notm}}, el servidor virtual de imágenes debe configurarse con las siguientes especificaciones:

* ***/boot*** debe ser la primera partición
* ***/boot*** y ***/*** debe ser el sistema de archivos ext3 o ext4
* ***/etc***  y /root debe estar en la misma partición que  ***/***
* ***/etc/fstab -> LABEL=SWAP-xvdb1 swap swap :*** para montar el disco de intercambio que está adjunto al sistema 
* Debe instalarse wget 
* Deben instalarse las utilidades xe-guest más actuales. Siga estos pasos:
    
    1. Descargar la ISO de XenServer ISO desde Citrix: [https://www.citrix.com/downloads/xenserver/product-software/xenserver-72-standard-edition.html ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.citrix.com/downloads/xenserver/product-software/xenserver-72-standard-edition.html)
    
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
    
Para obtener más información acerca de las imágenes habilitadas para cloud-init, consulte [Suministrar con una imagen habilitada para cloud-init](image_cloud-init.html).

## Importación de una imagen

Complete los pasos siguientes para importar una imagen en el portal del cliente.

1. Ubique y registre los siguientes detalles para la imagen desde la cuenta de {{site.data.keyword.objectstorageshort}}.  Para obtener más información, consulte
[Visualización y edición de Detalles del archivo de Object Storage](/docs/infrastructure/objectstorage-swift/view-and-edit-object-storage-file-details.html).
  * Nombre de la cuenta
  * Clúster
  * Contenedor
  * Nombre de archivo de la imagen
2. En el [Portal del cliente ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://control.softlayer.com/), acceda a la página **Plantillas de imagen** seleccionando **Dispositivos > Gestionar > Imágenes**.
3. Pulse el separador **Importar imagen** para abrir la herramienta de importación.
4. Seleccione la **Cuenta de {{site.data.keyword.objectstorageshort}}** para la imagen que desea importar en la lista desplegable **Cuenta**.
5. Seleccione el **Clúster de {{site.data.keyword.objectstorageshort}}** para la imagen que desea importar en la lista desplegable **Clúster**.
6. Seleccione el **{{site.data.keyword.objectstorageshort}} Contenedor** para la imagen que desea importar en la lista desplegable **Contenedor**.
7. Seleccione el **Nombre del archivo de imagen** tal y como aparece en {{site.data.keyword.objectstorageshort}} en la lista desplegable **Archivo de imagen**.
8. Especifique el nombre de la imagen para la nueva plantilla de imagen en el campo **Nombre de imagen**.
9. Escriba notas aplicables en el recuadro de texto **Notas**.
10. Seleccione el sistema operativo de la imagen en la lista desplegable **Sistema operativo**.

  La lista desplegable Sistema operativo está inhabilitada si la imagen para la importación es una ISO personalizada. El paso solo es necesario cuando la importación incluye un VHD.
  {:tip}

11. Si la imagen que está importando está habilitada para cloud-init, marque el recuadro de selección **Cloud Init**. Para obtener más información, consulte [Suministro con una imagen habilitada para cloud-init](image_cloud-init.html).        
12. Si tiene planificado proporcionar su propia licencia del sistema operativo, marque el recuadro de selección **Su licencia**. Para obtener más información, consulte [Uso de Red Hat Cloud Access](use-red-hat-cloud-access.html).
13. Pulse **Importar** para importar la imagen de la pantalla de Plantillas de imagen. Pulse **Cancelar** para cancelar la acción.

## Siguientes pasos

Después de que comience la importación, el sistema localice el archivo de imagen en la cuenta de {{site.data.keyword.objectstorageshort}} utilizando la vía de acceso específica (Cuenta > Clúster > Contenedor > Archivo de imagen). El archivo de imagen se importa como plantilla de imagen que después es accesible en la página Plantillas de imagen. Después de completar la importación, la imagen puede utilizarse para solicitar un dispositivo nuevo o iniciar un dispositivo existente.
Además, la imagen puede eliminarse en cualquier momento. Los tiempos de importación de imágenes varían sobre la base del tamaño de los archivos, pero normalmente toman entre varios minutos a una hora.

