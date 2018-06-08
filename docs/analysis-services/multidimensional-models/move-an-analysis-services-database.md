---
title: Déplacer un Analysis Services de base de données | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 984430962e9df6c3efdb04d66ef255baed814a0b
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="move-an-analysis-services-database"></a>Déplacer une base de données Analysis Services
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Il existe souvent des situations où un administrateur de base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] souhaite déplacer une base de données de modèle multidimensionnel ou tabulaire à un emplacement différent. Ces cas sont souvent motivés par des impératifs d’exploitation, tels que le déplacement de la base de données vers un autre disque afin d’obtenir de meilleures performances, le gain de place afin de permettre la croissance de la base de données, ou la mise à niveau d'un produit.  
  
 Une base de données peut être déplacée de différentes façons. Ce document explique les scénarios courants suivants :  
  
-   Par interaction à l'aide de SSMS  
  
-   Par programmation à l'aide d'AMO  
  
-   Par script à l'aide de XMLA  
  
 Tous les scénarios requièrent que l'utilisateur accède au dossier de base de données et utilise une méthode pour déplacer les fichiers vers la destination finale souhaitée.  
  
> [!NOTE]  
>  Détacher une base de données sans lui assigner un mot de passe laisse la base de données dans un état non protégé. Nous recommandons d'assigner un mot de passe à la base de données pour protéger les informations confidentielles. De même, la sécurité d'accès correspondante doit s'appliquer au dossier de base de données, aux sous-dossiers et aux fichiers pour empêcher leur accès non autorisé.  
  
## <a name="procedures"></a>Procédures  
  
#### <a name="moving-a-database-interactively-using-ssms"></a>Déplacement interactif d'une base de données à l'aide de SSMS  
  
1.  Localisez la base de données à déplacer dans le volet gauche ou droit de SSMS.  
  
2.  Cliquez avec le bouton droit sur la base de données et choisissez **Détacher...**  
  
3.  Attribuez un mot de passe à la base de données à détacher, puis cliquez sur **OK** pour exécuter la commande de détachement.  
  
4.  Utilisez tout mécanisme du système d'exploitation ou votre méthode standard de déplacement des fichiers pour déplacer le dossier de base de données vers le nouvel emplacement.  
  
5.  Recherchez le dossier **Bases de données** dans le volet gauche ou droit de SSMS.  
  
6.  Cliquez avec le bouton droit sur le dossier **Bases de données** et sélectionnez **Attacher...**  
  
7.  Dans la zone de texte **dossier** , tapez le nouvel emplacement du dossier de base de données. Vous pouvez également utiliser le bouton Parcourir (**...**) pour rechercher le dossier de base de données.  
  
8.  Sélectionnez le mode **lecture/écriture** pour la base de données.  
  
9. Entrez le mot de passe utilisé dans l'étape 3 et cliquez sur **OK** pour exécuter la commande d'attachement.  
  
#### <a name="moving-a-database-programmatically-using-amo"></a>Déplacement par programmation d'une base de données à l'aide d'AMO  
  
1.  Dans votre application C#, adaptez l'exemple de code suivant et complétez les tâches indiquées.  
  
 `private void MoveDb(Server server, string dbName,`  
  
 `string dbInitialLocation, string dbFinalLocation,`  
  
 `string dbPassword, ReadWriteMode dbReadWriteMode)`  
  
 `{`  
  
 `//Verify dbInitialLocation exists before continuing`  
  
 `if (server.Databases.ContainsName(dbName))`  
  
 `{`  
  
 `Database db;`  
  
 `//Save current cursor and change cursor to Cursors.WaitCursor`  
  
 `db = server.Databases[dbName];`  
  
 `db.Detach(dbPassword);`  
  
 `//Add your own code to copy the database files to the destination where you intend to attach the database`  
  
 `//Verify dbFinalLocation exists before continuing`  
  
 `server.Attach(dbFinalLocation, dbReadWriteMode, dbPassword);`  
  
 `//Restore cursor to its original`  
  
 `}`  
  
 `}`  
  
1.  Dans votre application C#, appelez `MoveDb()` avec les paramètres nécessaires.  
  
2.  Compilez et exécutez le code pour déplacer la base de données.  
  
#### <a name="moving-a-database-by-script-using-xmla"></a>Déplacement d'une base de données par script à l'aide de XMLA  
  
1.  Ouvrez un nouvel onglet XMLA dans SSMS.  
  
2.  Copiez le modèle de script suivant pour XMLA :  
  
 `<Detach xmlns="http://schemas.microsoft.com/analysisservices/2003/engine">`  
  
 `<Object>`  
  
 `<DatabaseID>%dbName%</DatabaseID>`  
  
 `<Password>%password%</Password>`  
  
 `</Object>`  
  
 `</Detach>`  
  
1.  Remplacez `%dbName%` par le nom de la base de données et `%password%` par le mot de passe. Les caractères % font partie du modèle et doivent être supprimés.  
  
2.  Exécutez la commande XMLA.  
  
3.  Utilisez tout mécanisme du système d'exploitation ou votre méthode standard de déplacement des fichiers pour déplacer le dossier de base de données vers le nouvel emplacement.  
  
4.  Copiez le modèle de script suivant pour XMLA dans un nouvel onglet XMLA.  
  
 `<Attach xmlns="http://schemas.microsoft.com/analysisservices/2003/engine">`  
  
 `<Folder>%dbFolder%</Folder>`  
  
 `<ReadWriteMode xmlns="http://schemas.microsoft.com/analysisservices/2008/engine/100">%ReadOnlyMode%</ReadWriteMode>`  
  
 `</Attach>`  
  
1.  Remplacez `%dbFolder%` par le chemin UNC complet du dossier de base de données, `%ReadOnlyMode%` par la valeur **ReadOnly** ou **ReadWrite**correspondante, et `%password%` par le mot de passe. Les caractères % font partie du modèle et doivent être supprimés.  
  
2.  Exécutez la commande XMLA.  
  
## <a name="see-also"></a>Voir aussi  
 <xref:Microsoft.AnalysisServices.Core.Server.Attach%2A>   
 <xref:Microsoft.AnalysisServices.Database.Detach%2A>   
 [Attacher et détacher des bases de données Analysis Services](../../analysis-services/multidimensional-models/attach-and-detach-analysis-services-databases.md)   
 [Emplacement de stockage de base de données](../../analysis-services/multidimensional-models/database-storage-location.md)   
 [Base de données ReadWriteModes](../../analysis-services/multidimensional-models/database-readwritemodes.md)   
 [Élément Attach](../../analysis-services/xmla/xml-elements-commands/attach-element.md)   
 [Élément Detach](../../analysis-services/xmla/xml-elements-commands/detach-element.md)   
 [Élément ReadWriteMode](../../analysis-services/xmla/xml-elements-properties/readwritemode-element.md)   
 [Élément DbStorageLocation](../../analysis-services/xmla/xml-elements-properties/dbstoragelocation-element.md)  
  
  
