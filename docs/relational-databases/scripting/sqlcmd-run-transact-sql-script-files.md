---
title: Exécuter des fichiers de script Transact-SQL à l’aide de sqlcmd | Microsoft Docs
ms.custom: ''
ms.date: 07/15/2016
ms.prod: sql
ms.technology: scripting
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- transact sql scripts
ms.assetid: 90067eb8-ca3e-44e8-bb1a-bf7d1a359423
caps.latest.revision: 42
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: ee3e53e4d6cfeba2e1cedf2a2c6e3a5500f0e633
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="sqlcmd---run-transact-sql-script-files"></a>sqlcmd - Exécuter des fichiers de script Transact-SQL
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
 Utilisez **sqlcmd** pour exécuter un fichier de script Transact-SQL. Un fichier de script Transact-SQL est un fichier texte qui peut contenir une combinaison d’instructions Transact-SQL, de commandes **sqlcmd** et de variables de script.  

## <a name="create-a-script-file"></a>Créer un fichier de script  
 Pour créer un fichier de script Transact-SQL simple à l’aide du Bloc-notes, procédez comme suit :  
  
1.  Cliquez sur **Démarrer**, pointez sur **Tous les programmes**, sur **Accessoires**, puis cliquez sur **Bloc-notes**.  
  
2.  Copiez et collez le code Transact-SQL suivant dans le Bloc-notes :  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT p.FirstName + ' ' + p.LastName AS 'Employee Name',  
    a.AddressLine1, a.AddressLine2 , a.City, a.PostalCode   
    FROM Person.Person AS p   
       INNER JOIN HumanResources.Employee AS e   
            ON p.BusinessEntityID = e.BusinessEntityID  
        INNER JOIN Person.BusinessEntityAddress bea   
            ON bea.BusinessEntityID = e.BusinessEntityID  
        INNER JOIN Person.Address AS a   
            ON a.AddressID = bea.AddressID;  
    GO  
    ```  
  
3.  Enregistrez le fichier sous **myScript.sql** sur le lecteur C.  
  
## <a name="run-the-script-file"></a>Exécuter le fichier de script  
  
1.  Ouvrez une fenêtre d'invite de commandes.  
  
2.  Dans la fenêtre d’invite de commandes, tapez : **sqlcmd -S myServer\instanceName -i C:\myScript.sql**  
  
3.  Appuyez sur Entrée.  
  
 La liste des noms et des adresses des employés de la société [!INCLUDE[ssSampleDBCoShort](../../includes/sssampledbcoshort-md.md)] apparaît dans la fenêtre d'invite de commandes.  

## <a name="save-the-output-to-a-text-file"></a>Enregistrer la sortie dans un fichier texte
  
1.  Ouvrez une fenêtre d'invite de commandes.  
  
2.  Dans la fenêtre d’invite de commandes, tapez : **sqlcmd -S myServer\instanceName -i C:\myScript.sql -o C:\EmpAdds.txt**  
  
3.  Appuyez sur Entrée.  
  
 Aucun résultat n'est retourné dans la fenêtre d'invite de commandes. Le résultat est au contraire envoyé dans le fichier EmpAdds.txt. Vous pouvez vérifier ce résultat en ouvrant le fichier EmpAdds.txt.  
  
## <a name="see-also"></a> Voir aussi  
 [Démarrer l'utilitaire sqlcmd](../../relational-databases/scripting/sqlcmd-start-the-utility.md)   
 [sqlcmd Utility](../../tools/sqlcmd-utility.md)  
  
  
