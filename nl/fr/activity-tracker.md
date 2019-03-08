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
{:table: .aria-labeledby="caption"}

# Audit d'événements de serveur virtuel avec Activity Tracker

Vous pouvez effectuer l'audit d'une instance de serveur virtuel tout au long de son cycle de vie en utilisant [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov). Vous devez disposer d'une instance d'Activity Tracker avec le plan de service Premium mis à disposition dans la région Sud des Etats-Unis. Le plan Premium vous permet d'accéder au tableau de bord Kibana incluant des options supplémentaires pour le tri et la recherche dans les journaux d'audit.

Le propriétaire du compte {{site.data.keyword.cloud}} peut voir les journaux dans la section des journaux du compte. Il peut également accéder aux journaux suivants :
* Evénements déclenchés par le propriétaire du compte.
* Evénements déclenchés pas les utilisateurs liés au compte.

Vous pouvez effectuer l'audit des événements d'instance de serveur virtuel suivants en utilisant Activity Tracker :
* Mise à disposition d'une instance de serveur virtuel
* Désactivation d'un port pour une interface privée ou publique
* Activation d'un port pour une interface privée ou publique
* Définition de la vitesse de port
* Création d'un modèle d'image
* Désactivation d'une instance de serveur virtuel
* Activation d'une instance de serveur virtuel
* Redémarrage d'une instance de serveur virtuel
* Attribution d'un nouveau nom à une instance de serveur virtuel
* Passage au mode récupération pour une instance de serveur virtuel
* Ajout d'un groupe de sécurité à une interface publique ou privée d'une instance de serveur virtuel
* Retrait d'un groupe de sécurité dans une interface publique ou privée d'une instance de serveur virtuel
* Chargement d'une instance de serveur virtuel à partir d'une image
* Initialisation d'une instance de serveur virtuel à partir d'une image
* Annulation d'une unité d'instance de serveur virtuel

Pour les événements associés à une interface réseau, le journal d'audit n'indique pas si l'événement a eu lieu sur une interface publique ou privée.
{: tip}
