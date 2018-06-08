---
title: Utilisation de synonymes | Documents Microsoft
ms.custom: ''
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: smo
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- synonyms [SMO]
ms.assetid: db0a9022-9549-43e5-b6b3-deb236f05fb8
caps.latest.revision: 49
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: d4971819e0731a5e72bd281f216a7afa4310dac1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-synonyms"></a>Utilisation de synonymes
[!INCLUDE[appliesto-ss-asdb-asdw-xxx-md](../../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

  Un synonyme est un alias donné à un objet dans l'étendue du schéma. Dans SMO, les synonymes sont représentés par le <xref:Microsoft.SqlServer.Management.Smo.Synonym> objet. L'objet <xref:Microsoft.SqlServer.Management.Smo.Synonym> est un enfant de l'objet <xref:Microsoft.SqlServer.Management.Smo.Database>. Cela signifie que les synonymes ne sont valides que dans le contexte de la base de données dans laquelle ils ont été définis. Toutefois, le synonyme peut faire référence à des objets sur une autre base de données, ou sur une instance distante de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 L'objet auquel est attribué un alias est l'objet de base. La propriété de nom de l'objet <xref:Microsoft.SqlServer.Management.Smo.Synonym> est l'alias attribué à l'objet de base.  
  
## <a name="example"></a>Exemple  
 Dans les exemples de code suivants, vous devez sélectionner l'environnement, le modèle et le langage de programmation à utiliser pour créer votre application. Pour plus d’informations, consultez [créer un Visual C&#35; projet SMO dans Visual Studio .NET](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md).  
  
## <a name="creating-a-synonym-in-visual-c"></a>Création d'un synonyme en Visual C#  
 L'exemple de code montre comment créer un synonyme ou un alias pour un objet compris dans l'étendue du schéma. Les applications clientes peuvent utiliser une référence unique pour l'objet de base par le biais d'un synonyme, au lieu d'employer une référence en plusieurs parties à l'objet de base.  
  
```csharp  
{  
            //Connect to the local, default instance of SQL Server.   
            Server srv = new Server();  
  
            //Reference the AdventureWorks2012 database.   
            Database db = srv.Databases["AdventureWorks2012"];  
  
            //Define a Synonym object variable by supplying the   
            //parent database, name, and schema arguments in the constructor.   
            //The name is also a synonym of the name of the base object.   
            Synonym syn = new Synonym(db, "Shop", "Sales");  
  
            //Specify the base object, which is the object on which   
            //the synonym is based.   
            syn.BaseDatabase = "AdventureWorks2012";  
            syn.BaseSchema = "Sales";  
            syn.BaseObject = "Store";  
            syn.BaseServer = srv.Name;  
  
            //Create the synonym on the instance of SQL Server.   
            syn.Create();  
        }  
```  
  
## <a name="creating-a-synonym-in-powershell"></a>Création d'un synonyme dans PowerShell  
 L'exemple de code montre comment créer un synonyme ou un alias pour un objet compris dans l'étendue du schéma. Les applications clientes peuvent utiliser une référence unique pour l'objet de base par le biais d'un synonyme, au lieu d'employer une référence en plusieurs parties à l'objet de base.  
  
```powershell  
#Get a server object which corresponds to the default instance  
$srv = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Server  
  
#And the database object corresponding to Adventureworks  
$db = $srv.Databases["AdventureWorks2012"]  
  
$syn = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Synonym `  
-argumentlist $db, "Shop", "Sales"  
  
#Specify the base object, which is the object on which the synonym is based.  
$syn.BaseDatabase = "AdventureWorks2012"  
$syn.BaseSchema = "Sales"  
$syn.BaseObject = "Store"  
$syn.BaseServer = $srv.Name  
  
#Create the synonym on the instance of SQL Server.  
$syn.Create()  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE SYNONYM &#40;Transact-SQL&#41;](../../../t-sql/statements/create-synonym-transact-sql.md)  
  
  
