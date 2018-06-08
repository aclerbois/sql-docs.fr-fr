---
title: Basculer une base de données Analysis Services entre les modes ReadOnly et ReadWrite | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 0df164b267043e4784260b30b039ecb58c2c4cc7
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="switch-an-analysis-services-database-between-readonly-and-readwrite-modes"></a>Basculer une base de données Analysis Services entre les modes ReadOnly et ReadWrite
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] Les administrateurs de base de données peuvent modifier le mode lecture/écriture d’une base de données tabulaire ou multidimensionnelle dans le cadre d’une démarche plus vaste de répartition d’une charge de travail de requêtes entre plusieurs serveurs de requêtes uniquement.  
  
 Vous pouvez basculer d’un mode de base de données vers un autre de plusieurs façons. Ce document explique les scénarios courants suivants :  
  
-   Par interaction à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]  
  
-   Par programmation à l'aide d'AMO  
  
-   Par script à l’aide de XMLA ou de TMSL  
  
## <a name="switch-the-readwrite-mode-of-a-database-interactively-using-management-studio"></a>Basculer interactivement le mode lecture/écriture d’une base de données à l’aide de Management Studio  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur la base de données, puis sélectionnez **Propriétés**.  
  
     Notez l’emplacement. Un emplacement de stockage de base de données vide indique que le dossier de base de données se trouve dans le dossier de données de serveur.  
  
2.  Cliquez avec le bouton droit sur la base de données et choisissez **Détacher…**  
  
3.  Assignez un mot de passe à la base de données à détacher, puis cliquez sur **OK** pour exécuter la commande de détachement.  
  
4.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur le dossier **Bases de données** et sélectionnez **Attacher…**  
  
5.  Dans la zone de texte **dossier** , tapez l'emplacement d'origine du dossier de base de données. Vous pouvez également utiliser le bouton Parcourir (**…**) pour rechercher le dossier de base de données.  
  
6.  Sélectionnez le mode lecture/écriture pour la base de données.  
  
7.  Tapez le mot de passe, puis cliquez sur **OK** pour exécuter la commande d’attachement.  
  
## <a name="switch-the-readwrite-mode-to-a-database-programmatically-using-amo"></a>Basculer par programmation le mode lecture/écriture d’une base de données à l’aide d’AMO  
 Dans votre application C#, appelez `SwitchReadWrite()` avec les paramètres nécessaires. Compilez et exécutez le code pour déplacer la base de données.  
  
```  
private void SwitchReadWrite(Server server, string dbName, ReadWriteMode dbReadWriteMode)  
{  
    if (server.Databases.ContainsName(dbName))  
    {  
        Database db;  
        string databaseLocation;  
        db = server.Databases[dbName];  
        databaseLocation = db.DbStorageLocation;  
  
              if (databaseLocation == null)  
            {  
                 string dataDir = server.ServerProperties["DataDir"].Value;  
                 string dataDir = server.ServerProperties["DataDir"].Value;  
                 string dataDir = server.ServerProperties["DataDir"].Value;  
  
    String[] possibleFolders = Directory.GetDirectories(dataDir, string.Concat(dbName,"*"), SearchOption.TopDirectoryOnly);  
  
   if (possibleFolders.Length > 1)  
         {  
         List<String> sortedFolders = new List<string>(possibleFolders.Length);  
         sortedFolders.AddRange(possibleFolders);  
         sortedFolders.Sort();  
         databaseLocation = sortedFolders[sortedFolders.Count - 1];  
         }  
         else  
         {  
         databaseLocation = possibleFolders[0];  
          }  
        }  
    db.Detach();  
    server.Attach(databaseLocation, dbReadWriteMode);  
    }  
}  
  
```  
  
## <a name="switch-the-readwrite-mode-to-a-database-by-script-using-xmla"></a>Basculer par script le mode lecture/écriture d’une base de données à l’aide de XMLA  
 Les instructions ci-après s’appliquent aux bases de données multidimensionnelles et tabulaires présentant le mode de compatibilité 1050, 1100 ou 1103.  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur la base de données, puis sélectionnez **Propriétés**.  
  
     Notez l’emplacement. Un emplacement de stockage de base de données vide indique que le dossier de base de données se trouve dans le dossier de données de serveur.  
  
2.  Cliquez avec le bouton droit sur la base de données et choisissez **Détacher…**  
  
3.  Ouvrez un nouvel onglet XMLA dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
4.  Copiez le modèle de script suivant pour XMLA :  
  
    ```  
    <Detach xmlns="http://schemas.microsoft.com/analysisservices/2003/engine">  
       <Object>  
          <DatabaseID>%dbName%</DatabaseID>  
          <Password>%password%</Password>  
       </Object>  
    </Detach>  
    ```  
  
5.  Remplacez `%dbName%` par le nom de la base de données et `%password%` par le mot de passe. Les caractères % font partie du modèle et doivent être supprimés.  
  
6.  Exécutez la commande XMLA.  
  
7.  Copiez le modèle de script suivant pour XMLA dans un nouvel onglet XMLA.  
  
    ```  
    <Attach xmlns="http://schemas.microsoft.com/analysisservices/2003/engine">  
       <Folder>%dbFolder%</Folder>  
       <ReadWriteMode xmlns="http://schemas.microsoft.com/analysisservices/2008/engine/100">%ReadOnlyMode%</ReadWriteMode>  
    </Attach>  
    ```  
  
8.  Remplacez `%dbFolder%` par le chemin UNC complet du dossier de base de données, `%ReadOnlyMode%` par la valeur **ReadOnly** ou **ReadWrite**correspondante, et `%password%` par le mot de passe. Les caractères % font partie du modèle et doivent être supprimés.  
  
9. Exécutez la commande XMLA.  
  
## <a name="see-also"></a>Voir aussi  
 <xref:Microsoft.AnalysisServices.Database.Detach%2A>   
 [Haute disponibilité et extensibilité dans Analysis Services](../../analysis-services/instances/high-availability-and-scalability-in-analysis-services.md)   
 [Attacher et détacher des bases de données Analysis Services](../../analysis-services/multidimensional-models/attach-and-detach-analysis-services-databases.md)   
 [Emplacement de stockage de base de données](../../analysis-services/multidimensional-models/database-storage-location.md)   
 [Base de données ReadWriteModes](../../analysis-services/multidimensional-models/database-readwritemodes.md)   
 [Élément Attach](../../analysis-services/xmla/xml-elements-commands/attach-element.md)   
 [Élément Detach](../../analysis-services/xmla/xml-elements-commands/detach-element.md)   
 [Élément ReadWriteMode](../../analysis-services/xmla/xml-elements-properties/readwritemode-element.md)   
 [Élément DbStorageLocation](../../analysis-services/xmla/xml-elements-properties/dbstoragelocation-element.md)  
  
  
