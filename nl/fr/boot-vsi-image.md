---

copyright:
  years: 2014, 2018
lastupdated: "2019-06-11"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}

# Initialisation d'une VSI à partir d'une image
{: #booting-a-vsi-from-an-image}

La fonction Initialiser à partir d'une image démarre une instance de serveur virtuel (VSI) à l'aide d'un modèle ISO importé d'un compte de stockage d'objets.
{:shortdesc}

L'initialisation d'un serveur virtuel à partir d'une image met en ligne l'unité en toute sécurité, afin de résoudre les problèmes. Dans la plupart des cas, la fonction Initialiser à partir d'une image permet de traiter les incidents dans un environnement qui ne présente pas de risque de perte des données importantes, comme cela peut se produire après un rechargement du système d'exploitation. Bien que le risque de perte des données importantes soit moins probable qu'avec un rechargement du système d'exploitation, il est conseillé de sauvegarder l'unité avant de lancer l'initialisation.

## Avant de commencer
Tout d'abord, accédez au menu Unité et assurez-vous de disposer des droits de compte appropriés pour exécuter les tâches. 

* Accédez au menu Unité de votre console. Pour plus d'informations, voir [Accès aux unités](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices).
* Vérifiez que vous disposez des droits de compte et accès aux unités requis. Seul le propriétaire de compte ou un utilisateur disposant de droit d'infrastructure classique **Gérer les utilisateurs** peut modifier les droits. 

Pour plus d'informations sur les droits, voir [Droits d'infrastructure classique](/docs/iam?topic=iam-infrapermission#infrapermission) et [Gestion de l'accès aux unités](/docs/vsi?topic=virtual-servers-managing-device-access).

## Initialisation d'une VSI à partir d'une image

Procédez comme suit pour démarrer un serveur virtuel à partir d'une image.

1. Sauvegardez toutes les données de l'unité.
2. Dans le menu **Unités**, sélectionnez **Liste d'unités**.
3. Dans la liste des unités, cliquez sur le serveur virtuel que vous souhaitez démarrer à partir d'un modèle ISO.
4. Sur la page Détails de l'unité du serveur virtuel, sélectionnez **Actions > Initialiser à partir d'une image**.
5. Cliquez sur **Initialiser à partir de cette image** pour l'image souhaitée.

  Basculez entre les images publiques et privées à l'aide de la fonction de liste déroulante située au-dessus de la liste d'images.
  {:tip}

6. Cliquez sur **Initialiser à partir d'une image** pour initialiser l'unité à l'aide de l'image sélectionnée. Cliquez sur **Fermer** pour annuler l'action.

## Etapes suivantes

Une fois le processus d'initialisation lancé, l'image est mise hors tension, puis démarrée à l'aide de l'image sélectionnée. Le temps d'initialisation varie, comme la taille et le type de chaque image. Une fois que le serveur virtuel est démarré à l'aide de l'image sélectionnée, il peut être accessible en tant qu'unité initialisée normalement, mais il est configuré en fonction de l'image. Redémarrez le serveur virtuel pour mettre l'unité hors tension et restaurer son état d'origine.
