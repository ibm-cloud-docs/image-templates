---

copyright:
  years: 2018
lastupdated: "2018-08-09"

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


# Importando uma imagem criptografada usando a API do SoftLayer
{: #importing-an-encrypted-image-by-using-the-softlayer-api}

É possível usar o {{site.data.keyword.slapi_short}} para importar uma imagem criptografada do {{site.data.keyword.cos_full}}
e criar um modelo de imagem. Quando o seu modelo de imagem for criado, será possível usá-lo para provisionar instâncias.
{:shortdesc}

Para limitar o acesso apenas às informações que são necessárias para concluir a tarefa de importação, autentique com um ID de serviço. O ID de serviço deve ter acesso somente à imagem criptografada no IBM Cloud Object Storage que você deseja importar e à instância do Key Protect na qual a sua chave raiz está armazenada.  

O fragmento de python a seguir mostra um exemplo de como é possível acessar a
API [SoftLayer_Virtual_Guest_Block_Device_Template_Group ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://softlayer.github.io/reference/services/SoftLayer_Virtual_Guest_Block_Device_Template_Group/) e usar o
método _createFromIcos_ para criar um modelo de imagem.

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


Para obter mais informações sobre a localização de valores que são necessários para importar a imagem criptografada do {{site.data.keyword.cos_full_notm}}, consulte a tabela a seguir.

| Campo    | Value   |
| -------- | ------- |
| ibmApiKey | Especifique a chave de API que você anotou quando a criou. Se a chave API for perdida, uma nova chave API deverá ser criada. Para obter mais informações, consulte [Gerenciando as suas chaves de API](/docs/iam?topic=iam-userapikey). |
| rootKeyId | Especifique o ID da chave raiz que foi usada para agrupar a chave de criptografia de dados. Para obter mais informações, consulte [Visualizando chaves](/docs/services/key-protect?topic=key-protect-view-keys#view-keys). |
| wrappedDek | Especifique o texto cifrado que está associado à sua chave de criptografia de dados agrupada que você usou para criptografar a sua imagem. Para obter mais informações, consulte [Agrupando chaves usando a API](/docs/services/key-protect?topic=key-protect-wrap-keys#wrap-keys). |
| keyProtectId | É possível usar a CLI do {{site.data.keyword.cloud_notm}} para localizar o seu ID da instância do {{site.data.keyword.keymanagementserviceshort}}. Para obter mais informações, consulte [Recuperando o seu ID da instância](/docs/services/key-protect?topic=key-protect-retrieve-instance-ID#retrieve-instance-ID). |
{: caption="Tabela 1. Valores necessários para importar a imagem criptografada" caption-side="top"}
