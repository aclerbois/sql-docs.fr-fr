---
title: Utiliser des flux de données (PowerPivot pour SharePoint) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ppvt-sharepoint
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: ac7d32ccb99776a85d82c3bc310bc29cdd82954b
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="use-data-feeds-power-pivot-for-sharepoint"></a>Utiliser des flux de données (Power Pivot pour SharePoint)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Les flux de données comportent un ou plusieurs flux de données générés à partir d'une source de données en ligne et transmis en continu à un document ou une application de destination. Si vous utilisez [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour Excel, les flux de données peuvent vous aider à obtenir des données d’entreprise ou des données métiers existantes à partir de sources de données arbitraires pour les afficher dans la fenêtre [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] de votre classeur Excel 2010. Après avoir importé un flux dans un classeur, vous pouvez y faire référence ultérieurement dans toute opération d'actualisation des données que vous planifiez sur un serveur SharePoint.  
  
 La façon dont vous utilisez un flux varie selon que vous utilisez les fonctionnalités d'exportation intégrées dans les applications qui prennent en charge les flux Atom ou que vous créez et utilisez des services de données personnalisés. Les applications capables de publier et de lire des données XML Atom fournissent un transfert de données transparent ; la mécanique des flux et services de données est invisible pour l'utilisateur. À ses yeux, un utilisateur ne fait que déplacer des données d'une application vers un autre.  
  
 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] et Microsoft SharePoint 2010 fournissent des flux de données qui peuvent être utilisés dans des classeurs [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Vous pouvez utiliser les informations de cette rubrique pour apprendre à accéder à des flux de rapports et listes que vous avez déjà.  
  
 Cette rubrique contient les sections suivantes :  
  
 [Configuration requise](#prereq)  
  
 [Créer un flux de données à partir d'une liste SharePoint](#sharepointlist)  
  
 [Créer un flux de données à partir d'un rapport Reporting Services](#rsreport)  
  
 [Créer un flux à partir d'un document de service de données](#dsdoc)  
  
##  <a name="prereq"></a> Configuration requise  
 Vous devez disposer de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour Excel pour importer un flux de données dans Excel 2010.  
  
 Vous devez disposer d'un service Web ou d'un service de données qui fournit des données au format Atom 1.0. [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] et SharePoint 2010 peuvent fournir des données à ce format.  
  
 Avant de pouvoir exporter une liste SharePoint sous forme de flux de données, vous devez installer ADO.NET Data Services sur le serveur SharePoint. Pour plus d’informations, voir [Installation d’ADO.NET Data Services pour prendre en charge les exportations de flux de données des listes SharePoint](http://msdn.microsoft.com/en-us/f32527ae-f623-4e08-adfb-6d3262f5c2ac).  
  
##  <a name="sharepointlist"></a> Créer un flux de données à partir d'une liste SharePoint  
 Dans une batterie de serveurs SharePoint 2010, une liste SharePoint présente un bouton Exporter en tant que flux de données dans le ruban Liste. Vous pouvez cliquer sur ce bouton pour exporter la liste en tant que flux. Pour vous offrir les meilleurs résultats, votre station de travail doit être équipée d’Excel 2010 et de l’application cliente [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . L’application cliente [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] est lancée en réponse à l’exportation du flux de données et crée une table [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] qui contient la liste.  
  
1.  Ouvrez la liste sur votre site SharePoint.  
  
2.  Dans Outils de liste, cliquez sur **Liste**.  
  
3.  Dans Se connecter et exporter, cliquez sur **Exporter en tant que flux de données**.  
  
    > [!NOTE]  
    >  Le bouton **Exporter en tant que flux de données** est ajouté à SharePoint par le biais de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]. Si vous n'avez pas installé [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint ou que vous n’avez pas activé la fonctionnalité [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , ce bouton n’est pas disponible.  
  
4.  Cliquez sur **Ouvrir** si [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour Excel est installé localement, ou cliquez sur **Enregistrer** pour enregistrer le document .atomsvc sur votre disque dur pour les opérations d’importation ultérieures.  
  
5.  Si vous avez choisi **Ouvrir**, utilisez l'Assistant Importation de table pour importer le flux dans une feuille de travail. Le flux de données est ajouté en tant que nouvelle table dans la fenêtre [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
 Une erreur se produira si ADO.NET Data Services 3.5.1 n'est pas installé sur le serveur SharePoint. Pour plus d’informations sur cette erreur et sur sa résolution, voir [Installation d’ADO.NET Data Services pour prendre en charge les exportations de flux de données des listes SharePoint](http://msdn.microsoft.com/en-us/f32527ae-f623-4e08-adfb-6d3262f5c2ac).  
  
##  <a name="rsreport"></a> Créer un flux de données à partir d'un rapport Reporting Services  
 Si vous avez un déploiement de SQL Server 2008 R2 Reporting Services, vous pouvez utiliser la nouvelle extension de rendu Atom pour générer un flux de données à partir d'un rapport existant. Pour vous offrir les meilleurs résultats, votre station de travail doit être équipée d’Excel 2010 et de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour Excel. L’application cliente [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] est lancée en réponse à l’exportation du flux de données et ajoute et met automatiquement en relation les tables et colonnes à mesure qu’elles sont transmises.  
  
 Pour obtenir des instructions sur l’exportation d’un flux de données à partir d’un rapport, consultez [Générer des flux de données à partir d’un rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/generate-data-feeds-from-a-report-report-builder-and-ssrs.md) dans le [fichier d’aide du Générateur de rapports](http://go.microsoft.com/fwlink/?LinkId=154494).  
  
> [!NOTE]  
>  Pour configurer une planification périodique d’actualisation des données qui réimporte les données de rapport dans un classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] publié dans une bibliothèque SharePoint, le serveur de rapports doit être configuré pour l’intégration SharePoint. Pour plus d’informations sur l’utilisation de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint avec Reporting Services, consultez [Configuration et administration d’un serveur de rapports &#40;mode SharePoint de Reporting Services&#41;](../../reporting-services/report-server-sharepoint/configuration-and-administration-of-a-report-server.md).  
  
##  <a name="dsdoc"></a> Créer un flux à partir d'un document de service de données  
 Si vous avez un service de données personnalisé qui génère des flux Atom, vous pouvez configurer un document de service de données en tant que méthode pour rendre les données accessibles aux utilisateurs et aux applications. Un fichier de *document de service de données* (.atomsvc) spécifie une ou plusieurs connexions à des sources en ligne qui publient des données au format câble Atom. Les documents de service de données peuvent être créés dans une *bibliothèque de flux de données*, qui est une bibliothèque à usage spécifique qui fournit un point d’accès commun pour l’exploration de documents de service de données publiés sur un serveur SharePoint. Les travailleurs de l'information sont autorisés à accéder aux documents de service de données dans la bibliothèque de flux peuvent faire référence à l'URL SharePoint du document pour importer les flux dans leurs classeurs et applications.  
  
1.  Ouvrez une bibliothèque de flux créée par votre administrateur de site. Pour plus d’informations, consultez [Créer ou personnaliser une bibliothèque de flux de données &#40;Power Pivot pour SharePoint&#41;](../../analysis-services/power-pivot-sharepoint/create-or-customize-a-data-feed-library-power-pivot-for-sharepoint.md).  
  
2.  Dans Outils de bibliothèque, cliquez sur **Documents**.  
  
3.  Cliquez sur **Nouveau document**.  
  
4.  Fournissez un nom de fichier et une description.  
  
5.  Spécifiez une ou plusieurs URL qui fournissent le flux :  
  
    1.  L'**URL de base** est facultative. Vous devez la spécifier si un document de service de données fournit plusieurs flux. L'URL de base doit spécifier la partie de l'URL qui est commune à tous les flux (par exemple, le nom du serveur et le site). Si vous créez un document de service de données pour un rapport Reporting Services, l'URL de base correspond à l'URL du serveur de rapports et au rapport.  
  
    2.  L'**URL du service Web** est requise. Sans l’URL de Base, cette valeur doit inclure `http://` ou `https://` dans l’adresse. Si vous spécifiez une URL de base, l'URL du service Web est la partie qui suit l'URL de base. Par exemple, si l’URL complète est `http://adventure-works/inventory/today.aspx`, l’URL de Base serait `http://adventure-works/inventory`, et l’URL du service Web est/Today.aspx.  
  
         L'URL du service Web peut inclure des paramètres qui filtrent ou sélectionnent un sous-ensemble de données. L'application ou le service qui fournit le flux doit prendre en charge les paramètres que vous spécifiez dans l'URL.  
  
6.  Entrez le **Nom de la table**, une table pour chaque flux. Cette valeur est requise. Le nom de la table est utilisé par une application cliente qui consomme le flux. Dans [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour Excel, le nom de la table est utilisé pour nommer les tables dans la fenêtre [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] qui contiendra les données importées.  
  
## <a name="see-also"></a>Voir aussi  
 [Activer l’intégration des fonctionnalités Power Pivot pour des collections de sites dans l’Administration centrale](../../analysis-services/power-pivot-sharepoint/activate-power-pivot-integration-for-site-collections-in-ca.md)   
 [Partager des flux de données à l’aide d’une bibliothèque de flux de données &#40;Power Pivot pour SharePoint&#41;](../../analysis-services/power-pivot-sharepoint/share-data-feeds-using-a-data-feed-library-power-pivot-for-sharepoint.md)  
  
  
