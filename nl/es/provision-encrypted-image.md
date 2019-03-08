---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-21"

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}


# Uso de cifrado de extremo a extremo (E2E) para suministrar una instancia cifrada

La característica de cifrado de extremo a extremo (E2E) le permite llevar su propia imagen de sistema operativo cifrada y habilitada para cloud-init que haya cifrado utilizando una clave de cifrado de datos que posea y controle. Después de completar la configuración de entorno, puede importar la imagen cifrada en el repositorio de plantillas de imagen y utilizarla para suministrar instancias de servidor virtual cifradas. El cifrado E2E proporciona cifrado de datos en reposo para el almacenamiento que está asociado con instancias de servidor virtual suministradas. Para obtener acceso a esta característica, póngase en contacto con el servicio de soporte.
{:shortdesc}

El cifrado E2E reúne varios componentes de {{site.data.keyword.cloud}} para proporcionar una solución segura para la información crítica.

* {{site.data.keyword.keymanagementservicefull_notm}} protege las claves con módulos de seguridad de hardware (HSM) basados en la nube con certificación FIPS 140-2 Nivel 2 que impiden el robo de información.
* IBM {{site.data.keyword.iamshort}} (IAM) habilita el servicio Cloud Block Storage para acceder a {{site.data.keyword.keymanagementserviceshort}} y a la clave raíz que se utiliza para envolver la clave de cifrado de datos.
* {{site.data.keyword.cos_full_notm}} almacena de forma segura la imagen cifrada al cargarla.
* En {{site.data.keyword.slportal}}, puede importar la imagen cifrada y crear una plantilla de imagen.
* Con una plantilla de imagen cifrada disponible en el entorno de infraestructura de la consola de {{site.data.keyword.cloud_notm}}, puede suministrar instancias de servidor virtual cifradas.
* Por último, tiene la opción de auditar sucesos asociados con los servidores virtuales cifrados a través de Activity Tracker.

## Preparación del entorno

1. Debe tener una cuenta actualizada para utilizar el cifrado E2E para los servidores virtuales. Para obtener más información, consulte [Conmutación a las cuentas de IBMid y de enlace](/docs/account?topic=account-unifyingaccounts).
2. Utilice {{site.data.keyword.keymanagementservicefull_notm}} para crear y gestionar claves.
      1. Suministre el servicio de [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-provision#provision).
      2. [Cree](/docs/services/key-protect?topic=key-protect-create-root-keys#create-root-keys) o [importe](/docs/services/key-protect?topic=key-protect-import-root-keys#import-root-keys) una clave raíz (CRK) en {{site.data.keyword.keymanagementservicelong_notm}}.
      3. **Opcional**: si elige, puede [crear](/docs/services/key-protect?topic=key-protect-create-standard-keys#create-standard-keys) o [importar](/docs/services/key-protect?topic=key-protect-import-standard-keys#import-standard-keys) una clave estándar para el descifrado.
      4. Obtenga [acceso a la API {{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-set-up-api#set-up-api), de modo que pueda envolver la clave de cifrado de datos.
      5. [Envuelva la clave de cifrado de datos](/docs/services/key-protect?topic=key-protect-wrap-keys#wrap-keys) con la clave raíz. Necesitará el texto de cifrado que esté asociado a la clave de cifrado de datos envuelta (WDEK) cuando importe la imagen cifrada al {{site.data.keyword.slportal}}.
3. Desde IBM {{site.data.keyword.iamshort}} (IAM), [autorice el acceso](/docs/iam?topic=iam-serviceauth#create-an-authorization) entre **Cloud Block Storage** (servicio de origen) y {{site.data.keyword.keymanagementservicelong_notm}} (servicio de destino). Los usuarios que importan imágenes cifradas de {{site.data.keyword.cos_full_notm}} deben tener una [política de acceso definida](/docs/iam?topic=iam-userroles) para {{site.data.keyword.keymanagementservicelong_notm}} en IAM.
4. En IBM Cloud Console, cree una instancia de {{site.data.keyword.cos_full_notm}} y cree un grupo para almacenar los datos. Para obtener más información, consulte [Iniciación a {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started-console-#getting-started-console-).
      1. Cree la instancia de {{site.data.keyword.cos_full_notm}} en la misma ubicación regional en la que está suministrado el servicio de {{site.data.keyword.keymanagementserviceshort}}.
      2. Al crear el grupo, el valor de **Resiliencia** debe ser _Regional_.
      3. De forma opcional, cuando crea el grupo, puede [cifrarlo con la clave](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-manage-encryption#sse-kp).   

## Preparación de las imágenes cifradas

1. Seleccione una imagen no cifrada que funcione en el entorno de infraestructura de {{site.data.keyword.cloud_notm}} que desea cifrar. Una opción es utilizar un servidor virtual existente para [crear una imagen personalizada](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template). Para obtener más información, consulte [Trabajar con una plantilla de imagen creada a partir de un servidor virtual suministrado de cloud-init](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image#work-with-a-standard-image-created-from-a-cloud-init-provisioned-virtual-server). También puede utilizar una imagen VHD existente. Asegúrese de que la imagen cumple los [requisitos de imagen cifrados](/docs/infrastructure/image-templates?topic=image-templates-creating-an-encrypted-image#encrypted-image-reqs).
2. Si utiliza una plantilla de imagen de {{site.data.keyword.slportal}}, [exporte la imagen no cifrada](/docs/infrastructure/image-templates?topic=image-templates-exporting-to-ibm-cos) a {{site.data.keyword.cos_full_notm}}.
3. Descargue el archivo de imagen de {{site.data.keyword.cos_full_notm}} a una máquina local segura para cifrar la imagen. En el panel de control de servicio, seleccione la acción **Descargar** para recuperar el objeto del almacenamiento. Puede utilizar el plugin de transferencia de alta velocidad de Aspera para descargar imágenes de más de 200 MB.
4. Utilice el formato de cifrado de disco de LUKS para [cifrar la imagen](/docs/infrastructure/image-templates?topic=image-templates-creating-an-encrypted-image#luks-disk-encryption) utilizando QEMU y DMCrypt.
5. En {{site.data.keyword.cos_full_notm}}, vaya al grupo y pulse **Añadir objetos** para [cargar](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-upload-data#uploading-data) la imagen cifrada. Puede utilizar el plugin de transferencia de alta velocidad de Aspera para cargar imágenes de más de 200 MB.

## Importación de una imagen cifrada y solicitud de una instancia

1. Mediante IBM {{site.data.keyword.iamshort}} (IAM), cree un ID de servicio con el que autenticarse al importar la imagen cifrada en {{site.data.keyword.slportal}}.
      1. Cree un [ID de servicio](/docs/iam?topic=iam-serviceids#serviceids).
      2. Asigne una [política de acceso](/docs/iam?topic=iam-serviceidpolicy#serviceidpolicy). Asigne acceso para estos servicios: {{site.data.keyword.cos_full_notm}} y {{site.data.keyword.keymanagementservicelong_notm}}.
      3. Cree una [clave de API para un ID de servicio](/docs/iam?topic=iam-serviceidapikeys#creating-an-api-key-for-a-service-id).
      4. Para obtener más información, consulte [Introducción a las claves de API y los ID de servicio de {{site.data.keyword.cloud_notm}} IAM ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/blogs/bluemix/2017/10/introducing-ibm-cloud-iam-service-ids-api-keys/){: new_window}.
2. En {{site.data.keyword.slportal}}, [importe la imagen cifrada](/docs/infrastructure/image-templates?topic=image-templates-preparing-and-importing-images#import-icos) a la página Plantillas de imagen. (También puede importar la imagen cifrada [mediante la {{site.data.keyword.slapi_short}}](/docs/infrastructure/image-templates?topic=image-templates-importing-an-encrypted-image-by-using-the-softlayer-api)).
3. Desde la página Plantillas de imagen, puede utilizar la imagen cifrada para [solicitar](/docs/infrastructure/image-templates?topic=image-templates-ordering-an-instance-from-an-image-template) una instancia de servidor virtual.
4. Con un servidor virtual cifrado, tiene la opción de auditar [sucesos de servidor virtual](/docs/vsi?topic=virtual-servers-at_events#at_events) mediante [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov).
