---
title: Utiliser une connexion de données Office (.odc) avec les rapports | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-data
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Office Data Connection (.odc) files
- SharePoint integration [Reporting Services], shared data sources
- .odc files
ms.assetid: e8d6896d-f886-4390-8b5d-96f0a50c250c
caps.latest.revision: 13
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 83af0a0eda5039eb1e9cedb554cd425e04a03145
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="use-an-office-data-connection-odc-with-reports"></a>Utiliser une connexion de données Office (.odc) avec les rapports
  Pour certains scénarios, vous pouvez utiliser un fichier .odc (Office Data Connection) existant pour fournir des informations de connexion à un rapport [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Un fichier .odc peut être utilisé à la place d’un fichier .rsds quand vous créez une source de données partagée. Le serveur de rapports utilise le fichier .odc de la même manière qu'un fichier .rsds ; il le lit pour obtenir le type de sources de données, une chaîne de connexion et les informations d'identification.  
  
 Tous les fichiers .odc ne peuvent pas être utilisés avec un rapport [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . L'extension pour le traitement des données et les caractéristiques du rapport et du fichier .odc déterminent si un fichier .odc peut être utilisé :  
  
-   Le rapport doit être conçu pour fonctionner avec un fournisseur de données OLE DB ou ODBC. Si vous avez utilisé une autre extension de traitement des données pour créer le rapport, le rapport ou ses requêtes peuvent inclure des fonctionnalités non prises en charge par le fournisseur de données OLE DB ou ODBC.  
  
-   Le fichier .odc doit présenter les éléments et la structure attendus. Les paramètres du fournisseur de données et des informations d'identification doivent être explicitement définis dans le fichier pour que le serveur de rapports puisse les lire. La meilleure méthode pour définir ces valeurs consiste à exporter le fichier .odc avant de le télécharger vers la bibliothèque SharePoint.  
  
-   Le fichier .odc doit spécifier un type de connexion OLE DB ou ODBC.  
  
-   Le fichier .odc doit spécifier une chaîne de connexion.  
  
-   Les informations d’identification peuvent être définies avec la valeur **None**, **Stored**ou **Integrated**. Si la méthode relative aux informations d’identification a la valeur **Stored**, le serveur de rapports invite l’utilisateur à fournir ses informations d’identification au lieu d’utiliser les informations d’identification stockées. Le serveur de rapports ne peut pas utiliser les informations d'identification stockées telles qu'elles sont définies dans le fichier .odc.  
  
-   La source de données doit contenir un schéma identique à celui utilisé pour créer le rapport. Si les structures de données diffèrent, le rapport ne s'exécutera pas.  
  
-   Le fichier .odc doit être créé dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office 2007 (les versions antérieures du fichier .odc ne sont pas compatibles avec les fichiers de définitions de rapports).  
  
 Vous ne pouvez pas utiliser de fichiers .odc qui spécifient des connexions à des sources de données que le serveur de rapports ne peut pas traiter, même si les types de source de données .odc sont similaires à ceux pris en charge. En particulier, si vous avez créé un fichier .odc dans Microsoft Excel 2007 qui récupère des données de Microsoft Access, du Web ou d'un fichier texte, vous ne pouvez pas utiliser ce fichier .odc pour fournir des données à un rapport.  
  
 Les rapports et les modèles du Générateur de rapports ne fonctionnent pas avec le fichier .odc. Vous ne pouvez pas utiliser un fichier .odc pour créer un modèle, ni configurer le modèle pour qu'il utilise une source de données partagée qui se lie à un fichier .odc.  
  
 Si vous n'êtes pas familier des fichiers .odc, vous pouvez utiliser les instructions suivantes pour en créer un et l'exporter. Pour créer facilement un fichier .odc pour une source de données OLE DB, un des moyens consiste à utiliser Excel 2007 et l'Assistant Connexion de données. Notez que l'Assistant ne crée pas de source de données ; une source de données externe doit être déjà définie.  
  
 Un fichier .odc doit être utilisé uniquement s'il est totalement compatible avec le rapport et les requêtes. Si vous rencontrez des erreurs qui imposent d'importantes modifications au rapport ou au fichier .odc, vous devez créer un nouveau fichier .rsds pour le rapport. Pour plus d’informations sur la manière de créer une source de données partagée qui utilise un fichier .rsds, consultez [Créer et gérer des sources de données partagées &#40;Reporting Services en mode intégré SharePoint&#41;](http://msdn.microsoft.com/library/2d3428e4-a810-4e66-a287-ff18e57fad76).  
  
### <a name="to-create-and-export-an-odc-file"></a>Pour créer et exporter un fichier .odc  
  
1.  Démarrez Excel 2007.  
  
2.  Sous l’onglet **Données** , dans le groupe **Obtenir des données externes** , cliquez sur **À partir d’autres sources**, puis sur **Provenance : Assistant Connexion de données**.  
  
3.  Sélectionnez **Autres/Avancés**, puis cliquez sur **Suivant**.  
  
4.  Sélectionnez **Fournisseur Microsoft OLE DB pour SQL Server**, puis cliquez sur **Suivant**.  
  
5.  Entrez le nom du serveur (par défaut, il s'agit du nom réseau de l'ordinateur) et un compte d'utilisateur disposant d'autorisations valides sur la connexion et la base de données. Cliquez sur **Suivant**.  
  
6.  Sélectionnez une base de données, puis cliquez sur **OK** pour fermer la boîte de dialogue **Liaison de données** .  
  
7.  La case **Connexion à une table spécifique** est cochée par défaut. Elle sert à récupérer des données d'une table particulière. Le serveur de rapports ignore toutes les requêtes dans un fichier .odc et il n'est donc pas important que vous activiez ou non cette case à cocher. Les requêtes qui extraient des données pour un rapport sont incluses dans un fichier de définition de rapport et non dans les fichiers externes.  
  
8.  Tant que la connexion est ouverte, vous pouvez en modifier les propriétés et l'exporter. Dans le groupe **Connexions** sous l’onglet **Données** , cliquez sur **Propriétés**, puis sur le bouton **Propriétés de connexion** en regard du nom de la connexion.  
  
9. Sous l’onglet **Définition** , cliquez sur **Exporter le fichier de connexion**.  
  
10. Donnez un nom au fichier, puis cliquez sur **Enregistrer**. Fermez l'application et tous les fichiers ouverts.  
  
### <a name="to-upload-and-use-an-odc-file"></a>Pour télécharger et utiliser un fichier .odc  
  
1.  Ouvrez la bibliothèque dans laquelle télécharger le fichier de connexion.  
  
2.  Dans le menu **Télécharger** , cliquez sur **Télécharger un document**.  
  
3.  Cliquez sur **Parcourir**.  
  
4.  Sélectionnez le fichier .odc créé. Par défaut, le fichier .odc se trouve dans le dossier My Documents, dans Mes sources de données.  
  
5.  Cliquez sur **Ouvrir** pour sélectionner le fichier, puis sur **OK** pour enregistrer la sélection. La page des propriétés du nouvel élément s'ouvre automatiquement  
  
6.  Dans Type de contenu, sélectionnez **Source de données du rapport**, puis cliquez sur **OK**.  
  
7.  Pointez sur un rapport.  
  
8.  Cliquez sur la flèche orientée vers le bas et sélectionnez **Gérer les sources de données**.  
  
9. Cliquez sur le nom de la source de données.  
  
10. Si le rapport utilise des informations de source de données personnalisées, cliquez sur **Partagé**.  
  
11. Dans **Lien de source de données**, cliquez sur le bouton Parcourir (**...**).  
  
12. Sélectionnez le fichier .odc que vous venez de télécharger.  
  
13. Cliquez sur **OK** pour sélectionner le fichier, puis sur **OK** pour enregistrer les modifications.  
  
     Si vous appliquez cette procédure avec l'exemple de base de données et les exemples de rapports [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] , soyez conscient que seul le rapport Company Sales fonctionnera immédiatement avec un fichier .odc. Les autres rapports exemple contiennent des paramètres de requête et des fonctionnalités non compatibles avec le fournisseur OLE DB. Vous pouvez cependant les faire fonctionner avec le fournisseur OLE DB si vous les modifiez au préalable dans le Concepteur de rapports.  
  
## <a name="see-also"></a> Voir aussi  
 [Créer, modifier et supprimer des sources de données partagées &#40;SSRS&#41;](../../reporting-services/report-data/create-modify-and-delete-shared-data-sources-ssrs.md)  
  
  
