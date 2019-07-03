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


# Importando uma imagem criptografada usando a API do SoftLayer
{: #importing-an-encrypted-image-by-using-the-softlayer-api}

É possível usar o {{site.data.keyword.slapi_short}} para importar uma imagem criptografada do {{site.data.keyword.cos_full}}
e criar um modelo de imagem. Quando o seu modelo de imagem for criado, será possível usá-lo para provisionar instâncias.
{:shortdesc}

Para limitar o acesso apenas às informações que são necessárias para concluir a tarefa de importação, autentique com um ID de serviço. O ID de serviço deve ter acesso somente à imagem criptografada no IBM Cloud Object Storage que você deseja importar e à instância do Key Protect na qual a sua chave raiz está armazenada.  

O fragmento de python a seguir mostra um exemplo de como é possível acessar a
API [SoftLayer_Virtual_Guest_Block_Device_Template_Group ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://sldn.softlayer.com/reference/services/SoftLayer_Virtual_Guest_Block_Device_Template_Group/) e usar o
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
      'crkCrn': 'crn:v1:bluemix:public:hs-crypto:us-south:a/0d06ba51fa0e431290956d1761da1b7b:5ef6cebe-26d7-4ef3-abdc-fb50f345780f:key:a9640391-aec5-4c86-8942-6e6c59bb40b5',
      'wrappedDek':'my wrapped DEK',
      }
ret = group_svc.createFromIcos(config)
print(ret)
```
{: codeblock}


Para obter mais informações sobre a localização de valores que são necessários para importar a imagem criptografada do {{site.data.keyword.cos_full_notm}}, consulte a tabela a seguir.

| Campo    | Value   |
| -------- | ------- |
| ibmApiKey | Especifique a chave de API que você anotou quando a criou. Se a chave API for perdida, uma nova chave API deverá ser criada. Para obter mais informações, consulte [Gerenciando as suas chaves de API](/docs/iam?topic=iam-userapikey#userapikey). |
| crkCrn | Especifique o [Cloud Resource Name (CRN)](/docs/overview?topic=overview-crn) para a chave raiz usada para agrupar sua chave de criptografia de dados.  Para localizar e copiar seu CRN da chave raiz, acesse a [instância de serviço do {{site.data.keyword.keymanagementserviceshort}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/resources){: new_window}, passe o mouse sobre a chave raiz, clique nas
reticências **(...)** no lado direito da tela, selecione a opção "Visualizar CRN"
e clique no ícone de cópia.  |
| wrappedDek | Especifique o texto cifrado que está associado à sua chave de criptografia de dados agrupada que você usou para criptografar a sua imagem. Para obter mais informações, consulte [Agrupando chaves usando a API](/docs/services/key-protect?topic=key-protect-wrap-keys#wrap-keys). |
{: caption="Tabela 1. Valores necessários para importar a imagem criptografada" caption-side="top"}
