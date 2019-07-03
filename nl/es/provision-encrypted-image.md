---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-13"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}


# Uso de cifrado de extremo a extremo (E2E) para suministrar una instancia cifrada
{: #using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance}

La característica de cifrado de extremo a extremo (E2E) le permite llevar su propia imagen de sistema operativo cifrada y habilitada para cloud-init que haya cifrado utilizando una clave de cifrado de datos que posea y controle. Después de completar la configuración de entorno, puede importar la imagen cifrada en el repositorio de plantillas de imagen y utilizarla para suministrar instancias de servidor virtual cifradas. El cifrado E2E proporciona cifrado de datos en reposo para el almacenamiento que está asociado con instancias de servidor virtual suministradas.
{:shortdesc}

El cifrado E2E reúne varios componentes de {{site.data.keyword.cloud}} para proporcionar una solución segura para la información crítica.

* Un servicio de gestión de claves de IBM, como {{site.data.keyword.keymanagementservicelong_notm}} o {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}} para proteger sus claves de cifrado (ver Tabla 1).
* IBM {{site.data.keyword.iamshort}} (IAM) habilita el servicio Cloud Block Storage para acceder a su sistema de gestión de claves y a la clave raíz que se utiliza para envolver la clave de cifrado de datos.
* {{site.data.keyword.cos_full_notm}} almacena de forma segura la imagen cifrada al cargarla.
* En la consola de {{site.data.keyword.cloud_notm}} puede importar la imagen cifrada y crear una plantilla de imagen.
* Con una plantilla de imagen cifrada disponible en el entorno de infraestructura de la consola de {{site.data.keyword.cloud_notm}}, puede suministrar instancias de servidor virtual cifradas.
* Por último, puede auditar sucesos asociados con los servidores virtuales cifrados a través de [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov#activity_tracker_ov).

## Servicios de gestión de claves de cifrado

{{site.data.keyword.keymanagementserviceshort}} y {{site.data.keyword.hscrypto}} (ya disponible en ciertas [áreas geográficas](/docs/services/hs-crypto?topic=hs-crypto-regions#regions)) utilizan una API de proveedor de claves común para proporcionar un enfoque coherente para la gestión de claves de cifrado.  Los centros de datos de
{{site.data.keyword.cloud_notm}} proporcionan un módulo de seguridad de hardware dedicado (HSM) para proteger sus claves.  Puede elegir las opciones siguientes: 

| Servicio de gestión de claves | Certificación de cifrado HSM |
| ----- | ----- |
| [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect/concepts?topic=key-protect-getting-started-tutorial#getting-started-tutorial) | FIPS 140-2 *Nivel 2* de conformidad |
| [{{site.data.keyword.hscrypto}}](/docs/services/hs-crypto?topic=hs-crypto-get-started#get-started) | FIPS 140-2 *Nivel 4* de conformidad |
{: caption="Tabla 1. Opciones de servicio de gestión de claves disponibles" caption-side="top"}

## Preparación del entorno

1. Debe tener una cuenta actualizada para utilizar el cifrado E2E para los servidores virtuales. Para obtener más información, consulte [Cómo cambiar a IBMid y enlazar cuentas](/docs/account/softlayerlink.html).
2. Utilice el servicio de gestión de claves para crear y gestionar claves.  Los pasos del ejemplo siguiente
son específicos de {{site.data.keyword.keymanagementserviceshort}}, pero el flujo general también se aplica a {{site.data.keyword.hscrypto}}. Si está
utilizando {{site.data.keyword.hscrypto}}, consulte la [documentación](/docs/services/hs-crypto?topic=hs-crypto-get-started#get-started) para dicho servicio, para ver las instrucciones correspondientes.
      1. Suministre el servicio [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-provision#provision).
      2. [Cree](/docs/services/key-protect?topic=key-protect-create-root-keys) o [importe](/docs/services/key-protect?topic=key-protect-import-root-keys#import-root-keys) una clave raíz (CRK) en {{site.data.keyword.keymanagementservicelong_notm}}.
      3. **Opcional**: si elige, puede [crear](/docs/services/key-protect?topic=key-protect-create-standard-keys#create-standard-keys) o [importar](/docs/services/key-protect?topic=key-protect-import-standard-keys#import-standard-keys) una clave estándar para el descifrado.      
      4. [Configure el plugin de la CLI Key Protect de {{site.data.keyword.cloud_notm}}](/docs/services/key-protect?topic=key-protect-set-up-cli) para poder [encapsular la clave de cifrado de datos](/docs/services/key-protect?topic=key-protect-cli-reference#kp-wrap) que tiene pensado utilizar para encriptar su imagen VHD con la clave raíz. Necesita el texto de cifrado que esté asociado a la clave de cifrado de datos envuelta (WDEK) cuando importe la imagen cifrada a la consola de {{site.data.keyword.cloud_notm}}.  
         
       Key Protect no guarda datos de autenticación adicionales (AAD), pero los puede utilizar para
proteger aún más una clave con hasta 255 series, cada una de ellas delimitada por comas, y que contienen hasta
255 caracteres.  Si proporciona AAD para el encapsulado de clave, guarde los datos en una ubicación segura
a la que pueda acceder y proporcione los mismos AAD en futuras solicitudes de desencapsulado de claves.
       {: tip}
      
3. En IBM {{site.data.keyword.iamshort}} (IAM), [autorice acceso](/docs/iam?topic=iam-serviceauth#create-auth) entre su **almacenamiento de bloques de nube** (servicio de origen) y su **Servicio de gestión de claves** (servicio de destino). Si importa imágenes cifradas de {{site.data.keyword.cos_full_notm}}, debe tener una [política de acceso definida](/docs/iam?topic=iam-userroles#userroles) para su servicio de gestión de claves en IAM.
4. En IBM Cloud Console, cree una instancia de {{site.data.keyword.cos_full_notm}} y cree un grupo para almacenar los datos. Para obtener más información, consulte la [guía de aprendizaje Cómo empezar de {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started)
      1. Cree la instancia de {{site.data.keyword.cos_full_notm}} en la región en la que se suministra su servicio de gestión de claves.
      2. Al crear el grupo, el valor de **Resiliencia** debe ser _Regional_.
      3. De forma opcional, cuando crea el grupo, puede [cifrarlo con la clave](/docs/services/cloud-object-storage?topic=cloud-object-storage-encryption#encryption-kp).   

## Preparación de las imágenes cifradas

1. Seleccione una imagen no cifrada que funcione en el entorno de infraestructura de {{site.data.keyword.cloud_notm}} que desea cifrar. Una opción es utilizar un servidor virtual existente para [crear una plantilla de imagen](/docs/infrastructure/image-templates/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template#creating-an-image-template). Para obtener más información, consulte [Trabajar con una plantilla de imagen creada a partir de un servidor virtual suministrado de cloud-init](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image#work-with-an-image-template-created-from-a-cloud-init-provisioned-virtual-server). También puede utilizar una imagen VHD existente. Asegúrese de que la imagen cumple los [requisitos de imagen cifrados](/docs/infrastructure/image-templates?topic=image-templates-encrypted-image-reqs#encrypted-image-reqs).
2. Si utiliza una plantilla de imagen de {{site.data.keyword.slportal}}, [exporte la imagen no cifrada](/docs/infrastructure/image-templates?topic=image-templates-exporting-an-image-to-ibm-cloud-object-storage) a {{site.data.keyword.cos_full_notm}}.
3. Descargue el archivo de imagen de {{site.data.keyword.cos_full_notm}} a una máquina local segura para cifrar la imagen. En el panel de control de servicio, seleccione la acción **Descargar** para recuperar el objeto del almacenamiento. Puede utilizar el plugin de transferencia de alta velocidad de Aspera para descargar imágenes de más de 200 MB.
4. Utilice la herramienta vhd-util para [cifrar la imagen VHD](/docs/infrastructure/image-templates?topic=image-templates-create-encrypted-image).
5. En {{site.data.keyword.cos_full_notm}}, vaya al grupo y pulse **Añadir objetos** para [cargar](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload) la imagen cifrada. Puede utilizar el plugin de transferencia de alta velocidad de Aspera para cargar imágenes de más de 200 MB.

## Importación de una imagen cifrada y solicitud de una instancia

1. Mediante IBM {{site.data.keyword.iamshort}} (IAM), cree un ID de servicio con el que autenticarse al importar la imagen cifrada en la consola de {{site.data.keyword.cloud_notm}}.
      1. Cree un [ID de servicio](/docs/iam?topic=iam-serviceids#serviceids).
      2. Asigne una [política de acceso](/docs/iam?topic=iam-serviceidpolicy#serviceidpolicy). Asignar acceso para estos servicios:
{{site.data.keyword.cos_full_notm}} y la gestión de llaves.
      3. Cree una [clave de API para un ID de servicio](/docs/iam?topic=iam-serviceidapikeys#create_service_key).
      4. Para obtener más información, consulte [Introducción a las claves de API y los ID de servicio de {{site.data.keyword.cloud_notm}} IAM ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud/blog/introducing-ibm-cloud-iam-service-ids-api-keys){: new_window}.
2. Desde la consola de {{site.data.keyword.cloud_notm}}, [importe la imagen cifrada](/docs/infrastructure/image-templates?topic=image-templates-import-icos#import-icos) a la página Plantillas de imagen.
3. Desde la página Plantillas de imagen, puede utilizar la imagen cifrada para [solicitar](/docs/infrastructure/image-templates?topic=image-templates-ordering-an-instance-from-an-image-template#ordering-an-instance-from-an-image-template) una instancia de servidor virtual.
4. Con un servidor virtual cifrado, tiene la opción de auditar [sucesos de servidor virtual](/docs/vsi?topic=virtual-servers-at_events#at_events) mediante [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov#activity_tracker_ov).
