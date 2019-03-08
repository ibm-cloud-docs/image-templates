---

copyright:
  years: 2018
lastupdated: "2018-08-09"

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:python: .ph data-hd-programlang='python'}
{:table: .aria-labeledby="caption"}


# Importación de una imagen cifrada mediante la API de SoftLayer

Puede utilizar la {{site.data.keyword.slapi_short}} para importar una imagen cifrada de {{site.data.keyword.cos_full}} y crear una plantilla de imagen. Cuando se crea la plantilla de imagen, puede utilizarla para suministrar instancias.
{:shortdesc}

Para limitar el acceso sólo a la información que se necesita para completar la tarea de importación, autentíquese con un ID de servicio. El ID de servicio debe tener acceso sólo a la imagen cifrada en IBM Cloud Object Storage que desea importar y a la instancia de Key Protect donde se almacena la clave raíz.  

El siguiente fragmento de código de python muestra un ejemplo de cómo puede acceder a la API [SoftLayer_Virtual_Guest_Block_Device_Template_Group ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://softlayer.github.io/reference/services/SoftLayer_Virtual_Guest_Block_Device_Template_Group/) y utilizar el método _createFromIcos_ para crear una plantilla de imagen.

```python
import SoftLayer

client = SoftLayer.create_client_from_env(username='<user>',
                        api_key='<api_key>',
                        endpoint_url='https://api.softlayer.com/rest/v3',
                        timeout=240)
group_svc = client['Virtual_Guest_Block_Device_Template_Group']
config = {'name':'my_encrypted_image',
      'note':'my note',
      'operatingSystemReferenceCode':'REDHAT_7_64',
      'uri':'cos://<region_name>/<bucket_name>/xx123.Rhel_7_encrypted.raw',
      'bootMode':'my boot mode',
      'cloud-init': True,
      'byol': True,
      'encrypted': True,
      'ibmApiKey':'<api_key>',
      'rootKeyId':'my root key ID',
      'wrappedDek':'my wrapped DEK',
      'keyProtectId':'<key_protect_instance_id>',
      }
ret = group_svc.createFromIcos(config)
print(ret)
```
{: codeblock}


Para obtener más información sobre cómo localizar valores que son necesarios para importar la imagen cifrada de {{site.data.keyword.cos_full_notm}}, consulte la tabla siguiente.

| Campo    | Valor   |
| -------- | ------- |
| ibmApiKey | Especifique la clave de API que ha anotado al crearla. Si se pierde la clave de API, deberá crear una nueva clave de API. Para obtener más información, consulte [Gestión de las claves de API](/docs/iam?topic=iam-userapikey). |
| rootKeyId | Especifique el ID de la clave raíz que se ha utilizado para envolver la clave de cifrado de datos. Para obtener más información, consulte [Visualización de claves](/docs/services/key-protect?topic=key-protect-view-keys#view-keys). |
| wrappedDek | Especifique el texto cifrado que está asociado con la clave de cifrado de datos envuelta que ha utilizado para cifrar la imagen. Para obtener más información, consulte [Envolvimiento de claves utilizando la API](/docs/services/key-protect?topic=key-protect-wrap-keys#wrap-keys). |
| keyProtectId | Puede utilizar la CLI de {{site.data.keyword.cloud_notm}} para encontrar el ID de instancia de {{site.data.keyword.keymanagementserviceshort}}. Para obtener más información, consulte [Recuperación del ID de instancia](/docs/services/key-protect?topic=key-protect-retrieve-instance-ID#retrieve-instance-ID). |
{: caption="Tabla 1. Valores necesarios para importar la imagen cifrada" caption-side="top"}
