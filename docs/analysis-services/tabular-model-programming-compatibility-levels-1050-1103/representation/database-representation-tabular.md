---
title: Base de données (tabulaire) | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: b6a55afa2c152fe5a97c474f0a23af1fda12ff57
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="database-representationtabular"></a>Représentation de la base de données (tabulaire)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  En mode tabulaire, la base de données est le conteneur pour tous les objets dans le modèle tabulaire.  
  
## <a name="database-representation"></a>Représentation de la base de données  
 La base de données est l'emplacement où tous les objets qui forment un modèle tabulaire résident. À l'intérieur de la base de données le développeur trouve des objets tels que des connexions, des tables, des rôles et bien plus.  
  
### <a name="database-in-amo"></a>Base de données dans AMO  
 Lorsque vous utilisez AMO pour gérer une base de données de modèle tabulaire, l'objet <xref:Microsoft.AnalysisServices.Database> dans AMO a une correspondance un-à-un avec l'objet logique de base de données dans un modèle tabulaire.  
  
> [!NOTE]  
>  Pour accéder à un objet de base de données, dans AMO, l'utilisateur a besoin d'avoir accès à un objet serveur et de se connecter à celui-ci.  
  
### <a name="database-in-adomdnet"></a>Base de données dans ADOMD.Net  
 Lorsque vous utilisez ADOMD pour consulter et interroger une base de données de modèle tabulaire, la connexion à une base de données spécifique est obtenue par l'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection>.  
  
 Connectez-vous directement à une certaine base de données à l'aide de l'extrait de code suivant :  
  
```csharp  
using ADOMD = Microsoft.AnalysisServices.AdomdClient;  
…  
   ADOMD.AdomdConnection currrentCnx = new ADOMD.AdomdConnection("Data Source=<<server\instance>>;Catalog=<<database>>");  
   currrentCnx.Open();  
…  
  
```  
  
 En outre, sur un objet de connexion existant (qui n'a pas été fermé), remplacez la base de données active par une autre comme indiqué dans l'extrait de code suivant :  
  
```csharp  
currentCnx.ChangeDatabase("myOtherDatabase");  
  
```  
  
## <a name="database-in-amo"></a>Base de données dans AMO  
 Lorsque vous utilisez AMO pour gérer un objet de base de données, commencez par un objet <xref:Microsoft.AnalysisServices.Server>. Puis, recherchez votre base de données dans la collection de bases de données, ou créez une nouvelle base de données et ajoutez-la à la collection.  
  
 L'extrait de code suivant montre les étapes de connexion à un serveur et illustre comment créer une base de données vide, après avoir vérifié que la base de données n'existe pas :  
  
```  
  
AMO.Server CurrentServer = new AMO.Server();  
try  
{  
    CurrentServer.Connect(currentServerName);  
}  
catch (Exception cnxException)  
{  
    MessageBox.Show(string.Format("Error while trying to connect to server: [{0}]\nError message: {1}", currentServerName, cnxException.Message), "AMO to Tabular message", MessageBoxButtons.OK, MessageBoxIcon.Error);  
    return;  
}  
newDatabaseName = DatabaseName.Text;  
if (CurrentServer.Databases.Contains(newDatabaseName))  
{  
    return;  
}  
try  
{  
    AMO.Database newDatabase = CurrentServer.Databases.Add(newDatabaseName);  
  
    CurrentServer.Update();  
}  
catch (Exception createDBxc)  
{  
    MessageBox.Show(String.Format("Database [{0}] couldn't be created.\n{1}", newDatabaseName, createDBxc.Message), "AMO to Tabular message", MessageBoxButtons.OK, MessageBoxIcon.Error);  
    newDatabaseAvailable = false;  
}  
  
```  
  
 Pour savoir comment utiliser AMO pour créer et manipuler des représentations de base de données, consultez le code source dans l’exemple AMO 2012 tabulaire ; plus précisément, archivez le fichier source suivant : Database.cs. L'exemple est disponible sur Codeplex. Le code est fourni uniquement comme un support aux concepts logiques expliqués ici et ne doit pas être utilisé dans un environnement de production, ni à des fins autres que pédagogiques.  
  
  
