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


# Importation d'une image chiffrée en utilisant l'API SoftLayer

Vous pouvez utiliser l'API {{site.data.keyword.slapi_short}} pour importer une image chiffrée à partir d'{{site.data.keyword.cos_full}}
et créer un modèle d'image. Une fois que votre modèle d'image est créé, vous pouvez l'utiliser pour mettre des instances à disposition.
{:shortdesc}

Pour limiter l'accès uniquement aux informations requises pour la tâche d'importation, authentifiez-vous avec un ID de service. L'ID de service doit uniquement avoir accès à l'image chiffrée dans IBM Cloud Object Storage que vous souhaitez importer et à l'instance Key Protect dans laquelle votre clé racine est stockée.  

Le fragment Python suivant affiche un exemple d'accès à l'API
[SoftLayer_Virtual_Guest_Block_Device_Template_Group ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://softlayer.github.io/reference/services/SoftLayer_Virtual_Guest_Block_Device_Template_Group/) et d'utilisation
de la méthode _createFromIcos_ pour créer un modèle d'image.

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


Pour savoir où se trouvent les valeurs nécessaires à l'importation de l'image chiffrée à partir d'{{site.data.keyword.cos_full_notm}}, consultez le tableau suivant.

| Zone    | Valeur   |
| -------- | ------- |
| ibmApiKey | Indiquez la clé d'API que vous avez notée à sa création. Si la clé d'API est perdue, vous devez en créer une autre. Pour plus d'informations, voir [Gestion des clés d'API d'utilisateur](/docs/iam?topic=iam-userapikey). |
| rootKeyId | Indiquez l'ID de la clé racine utilisée pour l'encapsulation de la clé de chiffrement de données. Pour plus d'informations, voir la rubrique relative à l'[affichage des clés](/docs/services/key-protect?topic=key-protect-view-keys#view-keys). |
| wrappedDek | Indiquez le texte chiffré associé à votre clé de chiffrement de données encapsulée et utilisée pour le chiffrement de votre image. Pour plus d'informations, voir [Encapsulage de clés](/docs/services/key-protect?topic=key-protect-wrap-keys#wrap-keys). |
| keyProtectId | Vous pouvez utiliser l'interface CLI {{site.data.keyword.cloud_notm}} pour trouver votre ID d'instance {{site.data.keyword.keymanagementserviceshort}}. Pour plus d'informations, voir [Extraction de l'ID d'instance](/docs/services/key-protect?topic=key-protect-retrieve-instance-ID#retrieve-instance-ID). |
{: caption="Tableau 1. Valeurs nécessaires pour l'importation de l'image chiffrée" caption-side="top"}
