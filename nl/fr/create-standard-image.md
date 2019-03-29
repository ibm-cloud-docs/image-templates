---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-15"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:tip: .tip}
{:new_window: target="_blank"}


# Création d'un modèle d'image
{: #creating-an-image-template}

Les modèles d'image vous permettent de répliquer diverses options de configuration pour les serveurs {{site.data.keyword.virtualmachinesshort}}.
{:shortdesc}

A tout moment pendant le cycle de vie d'un serveur virtuel, vous pouvez créer un modèle d'image. Vous pouvez ensuite l'utiliser pour répliquer rapidement des parties de sa configuration dans un autre serveur virtuel. Vous pouvez également créer des modèles d'image à partir d'un serveur virtuel, quel que soit le système d'exploitation. Lorsque votre modèle d'image est terminé, vous pouvez l'utiliser pour créer un autre serveur virtuel.

Procédez comme suit pour créer un modèle d'image d'un serveur virtuel.

1. Accédez au portail [{{site.data.keyword.slportal_full}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/){: new_window} en utilisant vos données d'identification uniques.
2. Dans le menu **Unités**, sélectionnez **Liste d'unités**.
3. Cliquez sur le serveur virtuel que vous souhaitez utiliser pour créer un modèle d'image.

  Accédez à l'onglet **Mots de passe** de la page **Détails de l'unité**. Vérifiez que les mots de passe répertoriés sur cette page**** correspondent aux mots de passe du système d'exploitation actuel et à tout autre mot de passe complémentaire de logiciel. Si les mots de passe ne correspondent pas, les serveurs virtuels créés à partir de ce modèle d'image ne fonctionnent pas.
  {:tip}

4. Dans le menu **Actions**, sélectionnez **Créer un modèle d'image**.
5. Saisissez le nouveau nom de l'image dans la zone **Nom de l'image**.
6. Entrez les éventuelles remarques nécessaires relatives à l'image dans la zone **Remarque**.
7. Cochez la case **J'accepte** une fois que toutes les informations sont saisies.
8. Cliquez sur **Créer un modèle** pour créer le modèle d'image.

## Etapes suivantes

Une fois que le modèle d'image est créé, vous pouvez l'utiliser pour créer plusieurs serveurs virtuels. Les nouveaux
serveurs virtuels ont les mêmes configurations que celles incluses dans le modèle d'image.
