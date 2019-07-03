---

copyright:
  years: 2018
lastupdated: "2019-05-13"

keywords:

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
{: #importing-an-encrypted-image-by-using-the-softlayer-api}

Puede utilizar la {{site.data.keyword.slapi_short}} para importar una imagen cifrada de {{site.data.keyword.cos_full}} y crear una plantilla de imagen. Cuando se crea la plantilla de imagen, puede utilizarla para suministrar instancias.
{:shortdesc}

Para limitar el acceso sólo a la información que se necesita para completar la tarea de importación, autentíquese con un ID de servicio. El ID de servicio debe tener acceso sólo a la imagen cifrada en IBM Cloud Object Storage que desea importar y a la instancia de Key Protect donde se almacena la clave raíz.  

El siguiente fragmento de código de python muestra un ejemplo de cómo puede acceder a la API [SoftLayer_Virtual_Guest_Block_Device_Template_Group ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://sldn.softlayer.com/reference/services/SoftLayer_Virtual_Guest_Block_Device_Template_Group/) y utilizar el método _createFromIcos_ para crear una plantilla de imagen.

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
      'crkCrn': 'crn:v1:bluemix:public:hs-crypto:us-south:a/0d06ba51fa0e431290956d1761da1b7b:5ef6cebe-26d7-4ef3-abdc-fb50f345780f:key:a9640391-aec5-4c86-8942-6e6c59bb40b5',
      'wrappedDek':'my wrapped DEK',
      }
ret = group_svc.createFromIcos(config)
print(ret)
```
{: codeblock}


Para obtener más información sobre cómo localizar valores que son necesarios para importar la imagen cifrada de {{site.data.keyword.cos_full_notm}}, consulte la tabla siguiente.

| Campo    | Valor   |
| -------- | ------- |
| ibmApiKey | Especifique la clave de API que ha anotado al crearla. Si se pierde la clave de API, deberá crear una nueva clave de API. Para obtener más información, consulte [Gestión de las claves de API](/docs/iam?topic=iam-userapikey#userapikey). |
| crkCrn | Especifique el nombre [Cloud Resource Name (CRN)](/docs/overview?topic=overview-crn) para la clave raíz que ha utilizado para encapsular su clave de cifrado de datos.  Para localizar y copiar
su CRN de clave raíz, acceda a la instancia de servicio de [{{site.data.keyword.keymanagementserviceshort}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/resources){: new_window},
pase el puntero del ratón sobre su clave raíz, pulse en los puntos suspensivos **(...)** en el extremo derecho de la pantalla y, a continuación, seleccione la opción
"Ver CRN" y pulse en el icono de copia.  |
| wrappedDek | Especifique el texto cifrado que está asociado con la clave de cifrado de datos envuelta que ha utilizado para cifrar la imagen. Para obtener más información, consulte [Envolvimiento de claves utilizando la API](/docs/services/key-protect?topic=key-protect-wrap-keys#wrap-keys). |
{: caption="Tabla 1. Valores necesarios para importar la imagen cifrada" caption-side="top"}
