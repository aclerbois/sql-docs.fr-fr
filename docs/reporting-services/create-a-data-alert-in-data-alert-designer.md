---
title: Créer une alerte de données dans le Concepteur d’alertes de données | Microsoft Docs
ms.custom: ''
ms.date: 08/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: reporting-services
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: 8464ab9d-afe1-4490-955f-9f3319bcbf8d
caps.latest.revision: 13
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 10f7e6d3f3ea4da0b9c7bb285c87c414bd1df58f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-data-alert-in-data-alert-designer"></a>Créer une alerte de données dans le Concepteur d’alertes

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../includes/ssrs-appliesto-not-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../includes/ssrs-appliesto-sharepoint-2013-2016.md)])

Vous pouvez créer les définitions d'alerte de données dans le Concepteur d'alertes de données. Après les avoir enregistrées, vous pouvez les rouvrir, les modifier, puis les enregistrer de nouveau dans le Concepteur d'alertes de données. Pour plus d’informations sur la modification des définitions d’alerte, consultez [Gérer mes alertes de données dans le Gestionnaire des alertes de données](../reporting-services/manage-my-data-alerts-in-data-alert-manager.md) et [Modifier une alerte de données dans le Concepteur d’alertes](../reporting-services/edit-a-data-alert-in-alert-designer.md).

> [!NOTE]
> L’intégration de Reporting Services à SharePoint n’est plus disponible après SQL Server 2016.

## <a name="create-a-data-alert-definition"></a>Créer une définition d’alerte de données
 
1.  Localisez la bibliothèque SharePoint qui contient le rapport pour lequel vous souhaitez créer une définition d'alerte de données.  
  
2.  Cliquez sur le rapport.  
  
     Le rapport est exécuté. Si le rapport est paramétré, vérifiez qu'il affiche les données au sujet desquelles vous souhaitez recevoir des messages d'alerte. Si vous ne voyez pas les colonnes ou les valeurs qui vous intéressent, vous pouvez exécuter de nouveau le rapport en utilisant des valeurs de paramètre différentes.  
  
    > [!NOTE]  
    >  Les valeurs de paramètre choisies pour exécuter le rapport sont enregistrées dans la définition d'alerte et seront utilisées lorsque le rapport sera exécuté une nouvelle fois dans le cadre d'une étape de traitement de la définition d'alerte. Pour utiliser des valeurs de paramètre différentes, vous devez créer une nouvelle définition d'alerte.  
  
3.  Dans le menu **Actions** , cliquez sur **Nouvelle alerte de données**.  
  
     L’image suivante affiche le menu **Actions** .  
  
     ![Ouvrir le Concepteur d’alertes à partir de la bibliothèque SharePoint](../reporting-services/media/rs-openalertdesigneriw.gif "Ouvrir le Concepteur d’alertes à partir de la bibliothèque SharePoint")  
  
     Le Concepteur d'alertes de données s'ouvre et affiche les 100 premières lignes du premier flux de données que le rapport génère dans une table.  
  
    > [!NOTE]  
    >  Si vous ne voyez pas l’option **Nouvelle alerte de données** , le service d’alerte n’est pas configuré sur le site SharePoint ou l’édition d’ [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] n’inclut pas les alertes de données. Pour plus d’informations, consultez [Service Reporting Services SharePoint et applications de service](../reporting-services/report-server-sharepoint/reporting-services-sharepoint-service-and-service-applications.md).  
    >   
    >  Si l’option **Nouvelle alerte de données** est grisée, la source de données du rapport est configurée pour utiliser les informations d’identification de sécurité intégrée ou pour demander les informations d’identification. Pour rendre l’option **Nouvelle alerte de données** disponible, vous devez mettre à jour la source de données afin d’utiliser les informations d’identification stockées ou aucune information d’identification.  
  
     Le nom du flux de données s’affiche dans la liste déroulante **Nom des données du rapport** .  
  
4.  Éventuellement, sélectionnez un flux de données différent dans la liste déroulante **Nom des données du rapport** .  
  
     Si aucun flux de données n'est généré à partir du rapport, vous ne pouvez pas créer de définition d'alerte pour le rapport. La mise en page du rapport détermine le contenu de chaque flux de données. Pour plus d’informations, consultez [Génération de flux de données à partir de rapports &#40;Générateur de rapports et SSRS&#41;](../reporting-services/report-builder/generating-data-feeds-from-reports-report-builder-and-ssrs.md).  
  
5.  Éventuellement, dans la zone de texte **Nom de l’alerte** , modifiez le nom par défaut pour qu’il soit plus explicite.  
  
     Le nom par défaut de la définition d'alerte est le nom du rapport. Les noms des définitions d'alerte ne sont pas nécessairement uniques ; par conséquent, ils peuvent être difficiles à distinguer lorsque vous consulterez ultérieurement la liste de vos alertes dans le Gestionnaire des alertes de données. Il est recommandé d'utiliser des noms explicites et uniques pour vos définitions d'alerte.  
  
6.  Éventuellement, modifiez l’option des données par défaut en remplaçant l’option **toutes les données dans le flux de données ont** par l’option **aucune donnée dans le flux de données n’a**.  
  
7.  Cliquez sur **Ajouter une règle**.  
  
     La liste des colonnes dans le flux de données s'affiche.  
  
8.  Dans la liste, sélectionnez la colonne que vous souhaitez utiliser dans la règle, puis sélectionnez un opérateur de comparaison et entrez la valeur de seuil.  
  
     Selon le type de données de la colonne sélectionnée, des opérateurs de comparaison différents apparaissent. Si la colonne a un type de données « date », l'icône du calendrier s'affiche en regard de la valeur de seuil pour la règle. Vous pouvez entrer des données en cliquant sur une date dans le calendrier ou en tapant la date.  
  
     Le Concepteur d’alertes de données fournit deux modes de comparaison : **Mode de saisie de valeur** et **Mode de sélection de champ**. Le mode par défaut est **Mode de saisie de valeur**. Vous pouvez ajouter des clauses OR uniquement quand vous travaillez en **Mode de saisie de valeur** et vous utilisez la comparaison **is** .  
  
9. Pour ajouter une clause OR, cliquez sur la flèche vers le bas, puis sur **Mode de saisie de valeur**.  
  
10. Tapez la valeur de comparaison.  
  
11. Éventuellement, recliquez sur le bouton de sélection **(…)** .  
  
     Les points de suspension **(…)** apparaissent sur la ligne qui contient la première clause.  
  
     La clause OR est ajoutée au-dessous et dans la règle AND.  
  
12. Éventuellement, cliquez sur la flèche vers le bas, sélectionnez **Mode de sélection de champ**, puis sélectionnez une colonne dans la liste.  
  
     Vous remarquerez que les points de suspension **(…)** qui vous permettent d’ajouter des clauses OR ont disparu.  
  
13. Éventuellement, recliquez sur **Ajouter une règle** pour ajouter des règles supplémentaires.  
  
     Les règles sont combinées par l'opérateur logique AND.  
  
14. Sélectionnez une option dans la liste de périodicité. Selon le type de périodicité, entrez un intervalle.  
  
15. Éventuellement, cliquez sur **Avancé**.  
  
16. Éventuellement, modifiez la date de début du message d'alerte en tapant une date différente ou en ouvrant le calendrier et en cliquant sur une date.  
  
     La date de début par défaut est la date courante.  
  
17. Éventuellement, cochez la case située en regard de **Arrêter l’alerte le**, puis choisissez une date pour arrêter le message d’alerte.  
  
     Par défaut, un message d'alerte n'a pas de date d'arrêt.  
  
    > [!NOTE]  
    >  L'arrêt d'un message d'alerte ne supprime pas la définition d'alerte. Une fois que vous avez arrêté un message d'alerte, vous pouvez le redémarrer en mettant à jour les dates de début et de fin. Pour plus d’informations sur la suppression des définitions d’alerte, consultez [Gérer mes alertes de données dans le Gestionnaire des alertes de données](../reporting-services/manage-my-data-alerts-in-data-alert-manager.md).  
  
18. Éventuellement, décochez la case **Envoyer un message seulement si les résultats changent** .  
  
     Si vous paramétrez fréquemment des messages d'alerte, les informations redondantes peuvent être agaçantes ; dans ce cas, ne désactivez pas cette case à cocher.  
  
19. Entrez les adresses de messagerie des destinataires du message d'alerte. Séparez les adresses par des points-virgules.  
  
     Si l’adresse e-mail de la personne qui a créé la définition d’alerte est disponible, elle est ajouté à la zone **Destinataire** .  
  
20. Éventuellement, dans la zone de texte **Objet** , mettez à jour la ligne Objet du message d’alerte.  
  
     L’objet par défaut est **Alerte de données pour \<nom de l’alerte de données>**.  
  
21. Éventuellement, dans la zone de texte **Description** , tapez une description pour le message d’alerte.  
  
22. Cliquez sur **Enregistrer**.  

## <a name="see-also"></a> Voir aussi

[Concepteur d'alertes de données](../reporting-services/data-alert-designer.md)   
[Gestionnaire des alertes de données pour les administrateurs d'alertes](../reporting-services/data-alert-manager-for-alerting-administrators.md)   
[Alertes de données Reporting Services](../reporting-services/reporting-services-data-alerts.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
